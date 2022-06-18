---
layout : default
title : quic network simulator
parent: QUIC
grand_parent : OpenSource
---


[quic network simulator original](https://github.com/marten-seemann/quic-network-simulator)


# Network Simulator for QUIC benchmarking

이 프로젝트는 다양한 네트워크 조건에서 QUIC 구현의 성능을 벤치마킹하고 측정하는 데 사용할 수 있는 테스트 프레임워크를 구축합니다. 네트워크 조건 및 교차 트래픽을 시뮬레이션하고 실제 세계와 시뮬레이션된 세계를 연결하기 위해 [ns-3](https://www.nsnam.org/) 네트워크 시뮬레이터를 사용합니다. 클라이언트와 서버 간의 트래픽이 시뮬레이션된 네트워크를 통해 흐르도록 격리하고 강제하는 데 Docker를 사용합니다.

## Framework

프레임워크는 docker-compose를 사용하여 3개의 Docker 이미지를 구성합니다. 네트워크 시뮬레이터([sim](https://github.com/marten-seemann/quic-network-simulator/tree/master/sim) 디렉토리에 있음), 클라이언트 및 서버(개별 QUIC 구현 디렉토리 또는 simple shell에 있음) , [endpoint](https://github.com/marten-seemann/quic-network-simulator/tree/master/endpoint) 디렉토리).

프레임워크는 호스트 시스템에서 `leftnet`(IPv4 193.167.0.0/24, IPv6 fd00:cafe:cafe:0::/64) 및 `rightnet`(IPv4)의 두 네트워크를 사용합니다.
193.167.100.0/24, IPv6 fd00:cafe:cafe:100::/64). 'leftnet'은 클라이언트 Docker 이미지에 연결되고 'rightnet'은 서버에 연결됩니다. ns-3 시뮬레이션은 중간에 위치하며 `leftnet`과 `rightnet` 사이에 패킷을 전달합니다.

```
      +-----------------------+
      |      client eth0      |
      |                       |
      |     193.167.0.100     |
      | fd00:cafe:cafe:0::100 |
      +----------+------------+
                 |
                 |
      +----------+------------+
      |     docker-bridge     |
      |                       |
      |      193.167.0.1      |
      |  fd00:cafe:cafe:0::1  |
+-----------------------------------+
|     |         eth0          |     |
|     |                       |     |
|     |      193.167.0.2      |     |
|     |  fd00:cafe:cafe:0::2  |     |
|     +----------+------------+     |
|                |                  |
|                |                  |
|     +----------+------------+     |
|     |         ns3           |     |
|     +----------+------------+     |
|                |                  |
|                |                  |
|     +----------+------------+     |
|     |         eth1          |     |
|     |                       |     |
|     |     193.167.100.2     |     |
|     | fd00:cafe:cafe:100::2 |  sim|
+-----------------------------------+
      |     docker-bridge     |
      |                       |
      |     193.167.100.1     |
      | fd00:cafe:cafe:100::1 |
      +----------+------------+
                 |
                 |
      +----------+------------+
      |      server eth0      |
      |                       |
      |    193.167.100.100    |
      |fd00:cafe:cafe:100::100|
      +-----------------------+
```


## Building your own QUIC docker image

[endpoint](https://github.com/marten-seemann/quic-network-simulator/tree/master/endpoint) 디렉터리에는 endpoint Docker container에 대한 기본 Docker 이미지가 포함되어 있습니다. 이 container의 미리 빌드된 이미지는 [dockerhub](https://hub.docker.com/r/martenseemann/quic-network-simulator-endpoint)에서 사용할 수 있습니다.

고유한 QUIC 구현을 설정하려면 다음 단계를 따르세요.

1. 구현을 위한 새 디렉터리를 만듭니다(예: my_quic_impl). 아래에 설명된 대로 이 디렉토리에 `Dockerfile` 및 `run_endpoint.sh`라는 두 개의 파일을 생성합니다.

1. 아래 Dockerfile을 복사하고 명령을 추가하여 QUIC 구현을 빌드합니다.

    ```dockerfile
    FROM martenseemann/quic-network-simulator-endpoint:latest

    # download and build your QUIC implementation
    # [ DO WORK HERE ]

    # copy run script and run it
    COPY run_endpoint.sh .
    RUN chmod +x run_endpoint.sh
    ENTRYPOINT [ "./run_endpoint.sh" ]
    ```

1. 이제 아래 스크립트를 `run_endpoint.sh`에 복사하고 지시에 따라 명령어를 추가합니다. 로그는 시뮬레이션 완료 후 사용할 수 있도록 `/logs`에 기록되어야 합니다(나중에 자세히 설명).

    ```bash
    #!/bin/bash
    
    # Set up the routing needed for the simulation
    /setup.sh

    # The following variables are available for use:
    # - ROLE contains the role of this execution context, client or server
    # - SERVER_PARAMS contains user-supplied command line parameters
    # - CLIENT_PARAMS contains user-supplied command line parameters

    if [ "$ROLE" == "client" ]; then
        # Wait for the simulator to start up.
        /wait-for-it.sh sim:57832 -s -t 30
        [ INSERT COMMAND TO RUN YOUR QUIC CLIENT ]
    elif [ "$ROLE" == "server" ]; then
        [ INSERT COMMAND TO RUN YOUR QUIC SERVER ]
    fi
    ```

예를 들어 [quic-gosetup](https://github.com/marten-seemann/quic-go-docker) 또는 [quicly setup](https://github.com/h2o/h2o-qns) 를 살펴보세요.

## Running a Simulation

1. quic-network-simulator 디렉토리에서 먼저 필요한 이미지를 빌드합니다.

   ```
   CLIENT=[client directory name] \
   SERVER=[server directory name] \
   docker-compose build
   ```

   클라이언트 또는 서버 구현, `Dockerfile` 또는 `run_endpoint.sh` 파일을 변경할 때마다 이 빌드 명령을 실행해야 합니다.

   For instance:

   ```
   CLIENT="my_quic_impl" \
   SERVER="another_quic_impl" \
   docker-compose build
   ```

1. 시나리오와 함께 설정을 실행하고 싶을 것입니다. 현재 제공되는 시나리오는 다음과 같습니다.

   * [Simple point-to-point link, with configurable link properties](https://github.com/marten-seemann/quic-network-simulator/tree/master/sim/scenarios/simple-p2p)

   * [Single TCP connection running over a configurable point-to-point link](https://github.com/marten-seemann/quic-network-simulator/tree/master/sim/scenarios/tcp-cross-traffic)

   You can now run the experiment as follows:
   ```
   CLIENT=[client directory name] \
   CLIENT_PARAMS=[params to client] \
   SERVER=[server directory name] \
   SERVER_PARAMS=[params to server] \
   SCENARIO=[scenario] \
   docker-compose up
   ```

   해당 QUIC 구현에 필요하지 않은 경우 SERVER_PARAMS 및 CLIENT_PARAMS를 생략할 수 있습니다.

   예를 들어 다음 명령은 간단한 지점 간 시나리오를 실행하고 클라이언트 구현에 대해서만 명령줄 매개변수를 지정합니다.
   
   ```
   CLIENT="my_quic_impl" \
   CLIENT_PARAMS="-p /10000.txt" \
   SERVER="another_quic_impl" \
   SCENARIO="simple-p2p --delay=15ms --bandwidth=10Mbps --queue=25" \
   docker-compose up
   ```

   Endpoint에서 로그를 기록하기 위해 탑재된 디렉터리가 제공됩니다. docker-compose는 실행되는 디렉토리에서 `logs/server` 및 `logs/client` 디렉토리를 생성합니다. Docker container 내에서 디렉토리는 `/logs`로 사용할 수 있습니다.

## Debugging and FAQs

1. 서버(클라이언트의 경우와 유사)가 실행 중이고 다음을 사용하여 서버 Docker container에서 루트 셸을 가져올 수 있습니다.

   ```bash
   docker exec -it server /bin/bash
   ```