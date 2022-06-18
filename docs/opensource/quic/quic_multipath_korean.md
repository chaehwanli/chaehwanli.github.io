---
layout : default
title : Multipath Extensions for QUIC korean
parent: QUIC
grand_parent : OpenSource
---

# Multipath Extensions for QUIC (MP-QUIC)
[original](https://datatracker.ietf.org/doc/html/draft-deconinck-quic-multipath-07.html)

## Abstract

이 문서는 단일 연결에 대해 여러 경로를 동시에 사용할 수 있도록 QUIC 프로토콜의 확장을 지정합니다. 이러한 확장은 단일 경로 QUIC 설계를 준수하고 QUIC 개인 정보 보호 기능을 유지합니다.

초안에 대한 토론은 QUIC IETF 메일링 리스트 quic@ietf.org 또는 초안이 포함된 GitHub 저장소(https://github.com/qdeconinck/raft-deconinck-multipath-quic)에서 권장됩니다.

##Status of This Memo

이 인터넷 초안은 BCP 78 및 BCP 79의 규정을 완전히 준수하여 제출되었습니다.

인터넷 초안은 IETF(Internet Engineering Task Force)의 작업 문서입니다. 다른 그룹은 작업 문서를 인터넷 초안으로 배포할 수도 있습니다. 현재 인터넷 초안 목록은 https://datatracker.ietf.org/drafts/current/에 있습니다.

인터넷 초안은 최대 6개월 동안 유효한 초안 문서이며 언제든지 다른 문서로 업데이트, 대체 또는 폐기될 수 있습니다. Internet-Drafts를 참고 자료로 사용하거나 "진행 중인 작업"이 아닌 다른 방식으로 인용하는 것은 부적절합니다.

이 인터넷 초안은 2021년 11월 4일에 만료됩니다.

## Copyright Notice

Copyright (c) 2021 IETF Trust and the persons identified as the document authors.  All rights reserved.

This document is subject to BCP 78 and the IETF Trust's Legal Provisions Relating to IETF Documents (https://trustee.ietf.org/license-info) in effect on the date of publication of this document. Please review these documents carefully, as they describe your rights and restrictions with respect to this document.  Code Components extracted from this document must include Simplified BSD License text as described in Section 4.e of the Trust Legal Provisions and are provided without warranty as described in the Simplified BSD License.

Table of Contents

   1.  Introduction  . . . . . . . . . . . . . . . . . . . . . . . .   3
   2.  Conventions and Definitions . . . . . . . . . . . . . . . . .   4
     2.1.  Notation Conventions  . . . . . . . . . . . . . . . . . .   5
   3.  Overview  . . . . . . . . . . . . . . . . . . . . . . . . . .   5
     3.1.  Moving from Bidirectional Paths to Uniflows . . . . . . .   5
     3.2.  Beyond Connection Migration . . . . . . . . . . . . . . .   7
     3.3.  Establishment of a Multipath QUIC Connection  . . . . . .   8
     3.4.  Architecture of Multipath QUIC  . . . . . . . . . . . . .   9
     3.5.  Uniflow Establishment . . . . . . . . . . . . . . . . . .  11
     3.6.  Exchanging Data over Multiple Uniflows  . . . . . . . . .  12
     3.7.  Exchanging Addresses  . . . . . . . . . . . . . . . . . .  14
     3.8.  Coping with Address Removals  . . . . . . . . . . . . . .  15
     3.9.  Uniflow Migration . . . . . . . . . . . . . . . . . . . .  15
     3.10. Handling Multiple Network Paths . . . . . . . . . . . . .  15
     3.11. Congestion Control  . . . . . . . . . . . . . . . . . . .  16
   4.  Mapping Uniflow IDs to Connection IDs . . . . . . . . . . . .  16
   5.  Using Multiple Uniflows . . . . . . . . . . . . . . . . . . .  16
     5.1.  Multipath Negotiation . . . . . . . . . . . . . . . . . .  17
       5.1.1.  Transport Parameter Definition  . . . . . . . . . . .  17
     5.2.  Coping with Additional Remote Addresses . . . . . . . . .  17
     5.3.  Receiving Uniflow State . . . . . . . . . . . . . . . . .  18
     5.4.  Sending Uniflow State . . . . . . . . . . . . . . . . . .  19
     5.5.  Losing Addresses  . . . . . . . . . . . . . . . . . . . .  20
   6.  New Frames  . . . . . . . . . . . . . . . . . . . . . . . . .  21
     6.1.  MP_NEW_CONNECTION_ID Frames . . . . . . . . . . . . . . .  22
     6.2.  MP_RETIRE_CONNECTION_ID Frame . . . . . . . . . . . . . .  22
     6.3.  MP_ACK Frame  . . . . . . . . . . . . . . . . . . . . . .  23
     6.4.  ADD_ADDRESS Frame . . . . . . . . . . . . . . . . . . . .  24
     6.5.  REMOVE_ADDRESS Frame  . . . . . . . . . . . . . . . . . .  25
     6.6.  UNIFLOWS Frame  . . . . . . . . . . . . . . . . . . . . .  26
   7.  Extension of the Meaning of Existing QUIC Frames  . . . . . .  27
   8.  Security Considerations . . . . . . . . . . . . . . . . . . .  27
     8.1.  Nonce Computation . . . . . . . . . . . . . . . . . . . .  27
     8.2.  Validation of Exchanged Addresses . . . . . . . . . . . .  28
   9.  IANA Considerations . . . . . . . . . . . . . . . . . . . . .  28
     9.1.  QUIC Transport Parameter Registry . . . . . . . . . . . .  28
   10. Acknowledgments . . . . . . . . . . . . . . . . . . . . . . .  29
   11. References  . . . . . . . . . . . . . . . . . . . . . . . . .  29
     11.1.  Normative References . . . . . . . . . . . . . . . . . .  29
     11.2.  Informative References . . . . . . . . . . . . . . . . .  30
   Appendix A.  Comparison with Multipath TCP  . . . . . . . . . . .  31
     A.1.  Multipath TCP Bidirectional Paths vs. QUIC Uniflows . . .  31
     A.2.  Negotiating the Multipath Extensions  . . . . . . . . . .  32
     A.3.  Uniflow Establishment . . . . . . . . . . . . . . . . . .  32
     A.4.  Exchanging Data over Multiple Uniflows  . . . . . . . . .  33
     A.5.  Advertising Host's Addresses  . . . . . . . . . . . . . .  33
     A.6.  Congestion Control  . . . . . . . . . . . . . . . . . . .  34
   Appendix B.  Change Log . . . . . . . . . . . . . . . . . . . . .  34
     B.1.  Since draft-deconinck-quic-multipath-06 . . . . . . . . .  34
     B.2.  Since draft-deconinck-quic-multipath-05 . . . . . . . . .  34
     B.3.  Since draft-deconinck-quic-multipath-04 . . . . . . . . .  34
     B.4.  Since draft-deconinck-quic-multipath-03 . . . . . . . . .  34
     B.5.  Since draft-deconinck-quic-multipath-02 . . . . . . . . .  35
     B.6.  Since draft-deconinck-quic-multipath-01 . . . . . . . . .  35
     B.7.  Since draft-deconinck-quic-multipath-00 . . . . . . . . .  35
     B.8.  Since draft-deconinck-multipath-quic-00 . . . . . . . . .  35
   Authors' Addresses  . . . . . . . . . . . . . . . . . . . . . . .  36

## 1.  Introduction

오늘날의 엔드호스트에는 여러 네트워크 인터페이스가 장착되어 있습니다. 사용자는 한 인터페이스에서 다른 인터페이스로 원활하게 전환하거나 필요할 때마다 대역폭을 집계하기 위해 동시에 사용할 수 있기를 기대합니다. 지난 몇 년 동안 전송 프로토콜에 대한 여러 다중 경로 확장이 제안되었습니다(예: [RFC8684], [MPRTP] 또는 [SCTPCMT]). 다중경로 TCP[RFC8684]가 가장 성숙한 것입니다. 이미 널리 사용되는 스마트폰에 배포되어 있지만 [RFC8041] [IETFJ]의 다른 사용 사례에도 적용됩니다.

일반 TCP 및 UDP를 사용하면 주어진 흐름에 속하는 모든 패킷이 이 흐름의 식별자 역할을 하는 동일한 4-tuple {출발지 IP 주소(source IP address), 원본 포트 번호(source port number), 대상 IP 주소(destination IP address), 대상 포트 번호(destination port number)}를 공유합니다. 이렇게 하면 이러한 흐름이 여러 경로를 사용하는 것을 방지할 수 있습니다.

QUIC [I-D.ietf-quic-transport]는 암시적 연결 식별자로 4-tuple을 사용하지 않습니다. 대신 QUIC 흐름은 연결 ID로 식별됩니다. 이를 통해 QUIC는 NAT 리바인딩 또는 IP 주소 변경과 같은 4-tuple에 영향을 미치는 이벤트에 대처할 수 있습니다. [I-D.ietf-quic-transport]의 섹션 9에 지정된 QUIC 연결 마이그레이션 기능을 사용하면 한 4-tuple에서 다른 tuple로 흐름을 마이그레이션할 수 있으며 특히 다른 네트워크 경로를 통한 연결을 유지할 수 있습니다.

그러나 단일 연결에 대해 사용 가능한 네트워크 경로를 통해 QUIC의 동시 사용을 지정하는 것은 불가능합니다. 대역폭 집계 또는 원활한 네트워크 핸드오버와 같은 사용 사례는 현재 다중 경로 TCP [RFC8041][IETFJ]와 함께 QUIC에 적용할 수 있습니다. 스마트폰에서 다중 경로 TCP를 사용한 경험은 핸드오버 동안 WLAN과 셀룰러를 동시에 사용할 수 있는 기능이 사용자가 인지하는 경험 품질을 향상시킨다는 것을 보여줍니다. 이러한 사용 사례에 대한 초기 솔루션의 성능 평가와 다중 경로 QUIC와 다중 경로 TCP 간의 비교는 [MPQUIC]에서 찾을 수 있습니다. 최근 간행물[MFQUIC]은 이 초안에 제시된 설계를 통해 구현이 위성 링크와 같은 고도로 비대칭적인 네트워크 경로를 활용할 수 있는 방법을 보여줍니다.

이 문서에서는 다중 경로 TCP 설계에서 얻은 많은 교훈과 이 문서의 첫 번째 버전에서 받은 의견을 활용하여 여러 네트워크 경로를 동시에 사용할 수 있도록 현재 QUIC 설계에 대한 확장을 제안합니다. 이 문서는 주로 endpoint로 구분할 수 있는 네트워크 경로에 중점을 둡니다.

이 문서는 다음과 같이 구성됩니다. 먼저 섹션 3에서 Multipath QUIC의 작동에 대한 개요를 제공합니다. 그런 다음 섹션 4에서 연결 ID가 사용 중인 다른 단방향 흐름(단일 흐름이라고 함)에 매핑되는 방법을 설명합니다. 섹션 5는 여러 네트워크 경로의 동시 사용을 가능하게 하기 위해 현재 QUIC 설계[I-D.ietf-quic-transport]에 필요한 변경 사항을 지정합니다. 이러한 확장은 섹션 6 및 섹션 7에서 설명하는 새로운 프레임을 도입합니다. 마지막으로 섹션 8에서는 몇 가지 보안 고려 사항에 대해 설명합니다.

## 2.  Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 [RFC2119] [RFC8174] when, and only when, they appear in all capitals, as shown here.

독자가 [I-D.ietf-quic-transport]에 사용된 용어에 익숙하다고 가정합니다. 또한 다음 용어를 정의합니다.

* Uniflow: QUIC 호스트와 해당 peer 사이의 단방향 패킷 흐름입니다. 이 흐름은 Uniflow ID라는 내부 식별자로 식별됩니다. uniflow를 통해 전송된 패킷은 연결 수명 동안 변경될 수 있는 대상 연결 ID를 사용합니다. 사용 중일 때 uniflow는 일시적으로 4-tuple(소스 IP 주소, 소스 포트 번호, 대상 IP 주소, 대상 포트 번호)에 바인딩됩니다.

* 초기 uniflow: QUIC 연결 설정을 위해 peer가 사용하는 두 개의 uniflow. 하나는 클라이언트에서 서버로의 uniflow이고 다른 하나는 서버에서 클라이언트로의 uniflow입니다. 암호화 핸드셰이크는 이러한 uniflow에서 수행됩니다. 이들은 Uniflow ID 0으로 식별됩니다.


### 2.1.  Notation Conventions

   Packet and frame diagrams use the format described in Section 12 of [I-D.ietf-quic-transport].

## 3.  Overview

QUIC[I-D.ietf-quic-transport]의 설계는 데이터 흐름의 다중화, 기밀성, 무결성 및 신뢰성을 갖춘 안정적인 전송( reliable transport with multiplexing, confidentiality, integrity, and authenticity)을 제공합니다. 오늘날 인터넷의 다양한 장치는 멀티홈입니다. 예로는 WLAN과 셀룰러 인터페이스가 모두 장착된 스마트폰과 IPv4와 IPv6을 모두 사용하는 일반 이중 스택 호스트가 있습니다.

QUIC의 현재 설계에서는 멀티홈 장치가 동시에 다른 경로를 효율적으로 사용할 수 없습니다. 이 문서는 다음과 같은 설계 목표를 가진 다중 경로 확장을 제안합니다.

* 각 호스트는 연결을 통해 사용되는 uniflow의 수를 제어합니다.

* 여러 uniflows의 동시 사용으로 인해 새로운 개인 정보 보호 문제가 발생해서는 안 됩니다.

* 호스트는 피해자를 향한 패킷 플러딩을 피하기 위해 사용하는 모든 경로가 실제로 peer에 도달하는지 확인해야 합니다.
      [I-D.ietf-quic-transport]의 섹션 21.12.3)

* 다중 경로 확장은 두 peer 간의 경로의 비대칭 특성을 처리해야 합니다.

먼저 다중 경로 확장이 QUIC에 유익한 이유를 설명한 다음 상위 수준에서 설명합니다.

### 3.1.  Moving from Bidirectional Paths to Uniflows

다중 경로 확장의 전체 아키텍처를 이해하기 위해 먼저 "경로"의 개념을 다듬겠습니다. 경로는 4-tuple(소스 IP 주소, 소스 포트 번호, 대상 IP 주소, 대상 포트 번호)로 표시될 수 있습니다. QUIC에서 이것은 로컬 호스트에서 원격 호스트로의 UDP 경로입니다. 단일 홈 서버와 상호 작용하는 스마트폰을 고려할 때 스마트폰은 WLAN 네트워크를 통해 하나의 경로를 사용하고 셀룰러를 통해 다른 경로를 사용하기를 원할 수 있습니다.
   
이러한 경로가 반드시 분리되어 있는 것은 아닙니다. 예를 들어, 듀얼 스택 서버와 상호 작용할 때 스마트폰은 WLAN을 통해 두 개의 경로를 생성할 수 있습니다. 하나는 IPv4를 통해 다른 하나는 IPv6을 통해.

일반 QUIC 연결은 두 개의 독립적인 활성 패킷 흐름으로 구성됩니다. 첫 번째 흐름은 클라이언트에서 서버로 패킷을 수집하고 다른 패킷은 서버에서 클라이언트로 수집합니다. 이를 설명하기 위해 그림 1의 예를 살펴보겠습니다. 이 예에서 클라이언트에는 IPc1과 IPc2라는 두 개의 IP 주소가 있습니다. 서버에는 IPs1이라는 단일 주소가 있습니다.

```
                     Probed flow IPc2 to IPs1
    +---------------------------------------------------------+
    |                                                         |
    |   From IPc1 to IPs1                                     v
    |   +--------+          Client to Server Flow         +--------+
    |   |        | =====================================> |        |
    +-- | Client |                                        | Server |
        |        | <===================================== |        |
        +--------+          Server to Client Flow         +--------+
                                                 From IPc1* to IPs1*

             Figure 1: Identifying Unidirectional Flows in QUIC
```

클라이언트는 서버로 패킷을 전송하여 QUIC 연결을 시작합니다. 그런 다음 서버는 연결을 수락하면 클라이언트에 응답합니다. 핸드셰이크가 성공하면 연결이 설정됩니다. 그래도 이 "경로"는 실제로 두 개의 독립적인 UDP 흐름으로 구성됩니다. 각 호스트는 (i) 패킷을 보내는 데 사용되는 4-tuple과 (ii) 패킷을 수신하는 4-tuple에 대한 고유한 보기를 가지고 있습니다. 클라이언트가 패킷을 전송하는 데 사용하는 4-tuple은 서버에서 보고 사용하는 것과 동일할 수 있지만 미들박스(예: NAT)가 4-tuple 패킷을 변경할 수 있기 때문에 항상 그런 것은 아닙니다.

이러한 흐름 비대칭성을 더욱 강조하기 위해 QUIC는 호스트가 주어진 4-tuple을 통해 peer에 도달할 수 있는지 여부를 평가하는 경로 유효성 검사 메커니즘[I-D.ietf-quic-transport]을 포함합니다. 이 프로세스는 단방향입니다. 즉, 발신자는 수신자에게 도달할 수 있는지 확인하지만 그 반대는 확인할 수 없습니다. 새로운 4-tuple에서 PATH_CHALLENGE 프레임을 수신하는 호스트는 차례로 경로 유효성 검사를 시작할 수 있지만 이것은 peer에 달려 있습니다.

QUIC 연결은 단방향 흐름(단일 흐름이라고 함) 모음입니다. 일반 QUIC 연결은 클라이언트에서 서버로의 메인 uniflow와 서버에서 클라이언트로의 또 다른 메인 uniflow로 구성됩니다. 이러한 uniflow에는 고유한 연결 ID가 있습니다. 그들은 호스트에 따라 다릅니다. 즉, A에서 B로의 단일 흐름은 B에서 A로 가는 것과 다릅니다. 이는 잠재적으로 사용할 수 없는 non-broadcast 위성 링크[RFC3077]와 같은 단방향 링크의 사용을 가능하게 합니다. TCP로.

### 3.2.  Beyond Connection Migration

TCP[RFC0793]와 달리 QUIC는 연결 수명 동안 특정 4-tuple에 바인딩되지 않습니다. QUIC 연결은 각 QUIC 패킷의 공개 헤더에 있는 (일련의) 연결 ID로 식별됩니다. 이렇게 하면 예를 들어 NAT 리바인딩으로 인해 4-tuple이 변경되더라도 호스트가 연결을 계속할 수 있습니다. 하나의 4-tuple에서 다른 4-tuple로 연결을 전환하는 이 기능을 연결 마이그레이션(Connection Migration)이라고 합니다. 사용 사례 중 하나는 사용 중인 IP 주소가 실패했지만 다른 주소를 사용할 수 있는 경우 장애 조치(failover)입니다. 예를 들어 WLAN 연결이 끊어진 스마트폰은 셀룰러 인터페이스를 통해 연결을 계속할 수 있습니다.

따라서 QUIC 연결은 초기 uniflow로 표시된 주어진 uniflow 집합에서 시작하여 다른 uniflow에서 끝날 수 있습니다. 그러나 현재 QUIC 설계[I-D.ietf-quic-transport]는 주어진 연결에 대해 한 쌍의 uniflow만 사용된다고 가정합니다. 현재 전송 사양 [I-D.ietf-quic-transport]은 경로 마이그레이션을 주어진 연결에 대해 사용 가능한 uniflow의 동시 사용과 구별하는 수단을 제공하지 않습니다.

이 문서는 그 공백을 채웁니다. 

먼저 엔드호스트 주소를 peer에 전달하는 메커니즘을 제안합니다. 그런 다음 [I-D.ietf-quic-transport]의 섹션 8에 설명된 PATH_CHALLENGE 및 PATH_RESPONSE 프레임이 있는 주소 유효성 검사 절차를 활용하여 호스트에서 보급한 추가 주소에 도달할 수 있는지 여부를 확인합니다. 이 경우 해당 주소는 이 문서의 범위를 벗어나는 패킷 스케줄링 정책에 따라 여러 네트워크 경로를 통해 패킷을 확산하기 위해 새로운 uniflow를 시작하는 데 사용할 수 있습니다.

그림 2의 예는 두 개의 패킷에 걸쳐 요청을 보내는 이중 홈 클라이언트와 단일 홈 서버 간의 데이터 교환을 보여줍니다. Uniflow ID는 각 호스트에서 독립적으로 선택됩니다. 제시된 예에서 클라이언트는 Uniflow 0에서 WLAN을 통해, Uniflow 1에서 LTE를 통해 패킷을 전송하는 반면 WLAN을 통해 서버에서 보낸 패킷은 Uniflow 2에 있고 LTE를 통해 패킷은 Uniflow 1에 있습니다.

```
   Server                        Phone                        Server
   via WLAN                                                  via LTE
   -------                      -------                        -----
     | Pkt(DCID=A,PN=5,frames=[    |                             |
     |  STREAM("Request (1/2)")])  | Pkt(DCID=B,PN=1,frames=[    |
     |<----------------------------|  STREAM("Request (2/2)")])  |
     | Pkt(DCID=E,PN=1,frames=[    |--------                     |
     |  ACK(LargestAcked=5)])      |       |----------           |
     |---------------------------->|                 |--------   |
     | Pkt(DCID=E,PN=2,frames=[    |                         |-->|
     |  STREAM("Response 1")])     | Pkt(DCID=D,PN=1,frames=[    |
     |---------------------------->|  MPACK(UID=1,LargestAck=1), |
     |                             |  STREAM("Response 2")])  ---|
     | Pkt(DCID=A,PN=6,frames=[    |                 ---------|  |
     |  MPACK(UID=2,LargestAck=2), |       ----------|           |
     |  MPACK(UID=1,LargestAck=1)])|<------|                     |
     |<----------------------------|                             |
     | Pkt(DCID=E,PN=3,frames=[    | Pkt(DCID=D,PN=2,frames=[    |
     |  STREAM("Response 3")])     |  STREAM("Response 4")])     |
     |---------------------------->|                         ----|
     |                             |                   ------|   |
     |            ...              |    ...  <---------|         |

                  Figure 2: Data flow with Multipath QUIC
```

이 섹션의 나머지 부분에서는 QUIC의 다중 경로 작업에 대한 높은 수준의 개요를 제공합니다.

### 3.3.  Establishment of a Multipath QUIC Connection

다중 경로 QUIC 연결은 일반 QUIC 연결 [I-D.ietf-quic-transport] [I-D.ietf-quic-tls]으로 시작됩니다. 이 문서에 정의된 다중 경로 확장은 "max_sending_uniflow_id" 전송 매개변수를 사용하여 협상됩니다. 이 전송 매개변수의 모든 값은 다중 경로 확장 지원을 알립니다.

다중 경로 확장을 협상할 때 호스트는 연결을 통해 사용할 전송 uniflow 수의 상한선을 설정합니다. 예를 들어 호스트가 연결 패킷을 최대 2개의 네트워크 경로로 전파하려는 경우 "max_sending_uniflow_id"가 1임을 알립니다. 두 번째 전송 uniflow 생성은 나중에 설명하는 것처럼 peer에 따라 달라집니다.

"max_sending_uniflow_id" 전송 매개변수에 대해 0 값을 광고하는 호스트는 패킷을 보내는 추가 uniflow를 원하지 않지만 여전히 다중 경로 확장을 지원한다는 것을 나타냅니다. 이러한 상황은 호스트가 패킷 전송을 위해 다중 uniflow우를 필요로 하지 않지만 peer가 도달하기 위해 다중 uniflow우를 사용하도록 하려는 경우에 유용할 수 있습니다.

### 3.4.  Architecture of Multipath QUIC

   To illustrate the architecture of a Multipath QUIC connection, consider Figure 3.

```
   +--------+          CID A - Uniflow ID 1          +--------+
   |        | =====================================> |        |
   |        |          CID B - Uniflow ID 0          |        |
   |        | =====================================> |        |
   |        |                                        |        |
   | Client |          CID C - Uniflow ID 0          | Server |
   |        | <===================================== |        |
   |        |          CID D - Uniflow ID 1          |        |
   |        | <===================================== |        |
   |        |          CID E - Uniflow ID 2          |        |
   |        | <===================================== |        |
   +--------+                                        +--------+

       Figure 3: An Example of Uniflow Distribution over a Multipath QUIC Connection
```
일단 설정되면 다중 경로 QUIC 연결은 클라이언트에서 서버로의 하나 이상의 uniflow와 서버에서 클라이언트로의 하나 이상의 uniflow로 구성됩니다. 한 방향의 uniflow 수는 다른 방향의 수와 다를 수 있습니다. 그림 3의 예는 클라이언트에서 서버로의 2개의 uniflow와 서버에서 클라이언트로의 3개의 uniflow를 보여줍니다. 최종 호스트의 관점에서 그들은 두 가지 종류의 uniflow를 관찰합니다.

* Sending uniflows: 호스트가 패킷을 보낼 수 있는 uniflows

* receiving uniflow: 호스트가 패킷을 수신할 수 있는 uniflow

그림 3의 예를 다시 살펴보면 클라이언트에는 2개의 송신 uniflow와 3개의 수신 uniflow가 있습니다. 서버에는 3개의 송신 uniflow와 2개의 수신 uniflow가 있습니다. 따라서 호스트의 송신 uniflow와 peer의 수신 uniflow 간에 일대일 매핑이 있습니다. uniflow는 발신자 관점에서 보내는 uniflow와 수신자의 관점에서 수신 uniflow로 간주됩니다.

각 uniflow는 그림 4와 같이 특정 4-tuple과 연결되고 Uniflow ID로 식별됩니다.

```
   Client state
       +-----------------------------------------------------+
       |                      Connection                     |
       |   +-----------+ +-----------+ ... +-------------+   |
       |   |  Sending  | |  Sending  | ... |   Sending   |   |
       |   | Uniflow 0 | | Uniflow 1 |     | Uniflow N-1 |   |
       |   | UCID set A| | UCID set B| ... |  UCID set C |   |
       |   |  Tuple A  | |  Tuple B  | ... |   Tuple C   |   |
       |   |   PNS A   | |   PNS B   |     |    PNS C    |   |
       |   +-----------+ +-----------+ ... +-------------+   |
       |   +-----------+ +-----------+ ... +-------------+   |
       |   | Receiving | | Receiving | ... |  Receiving  |   |
       |   | Uniflow 0 | | Uniflow 1 |     | Uniflow M-1 |   |
       |   | UCID set X| | UCID set Y| ... |  UCID set Z |   |
       |   |  Tuple X  | |  Tuple Y  | ... |   Tuple Z   |   |
       |   |   PNS X   | |   PNS Y   |     |    PNS Z    |   |
       |   +-----------+ +-----------+ ... +-------------+   |
       +-----------------------------------------------------+

   Server state
       +-----------------------------------------------------+
       |                      Connection                     |
       |   +-----------+ +-----------+ ... +-------------+   |
       |   |  Sending  | |  Sending  | ... |   Sending   |   |
       |   | Uniflow 0 | | Uniflow 1 |     | Uniflow M-1 |   |
       |   | UCID set X| | UCID set Y| ... |  UCID set Z |   |
       |   |  Tuple X* | |  Tuple Y* | ... |   Tuple Z*  |   |
       |   |   PNS X   | |   PNS Y   |     |    PNS Z    |   |
       |   +-----------+ +-----------+ ... +-------------+   |
       |   +-----------+ +-----------+ ... +-------------+   |
       |   | Receiving | | Receiving | ... |  Receiving  |   |
       |   | Uniflow 0 | | Uniflow 1 |     | Uniflow N-1 |   |
       |   | UCID set A| | UCID set B| ... |  UCID set C |   |
       |   |  Tuple A* | |  Tuple B* | ... |   Tuple C*  |   |
       |   |   PNS A   | |   PNS B   |     |    PNS C    |   |
       |   +-----------+ +-----------+ ... +-------------+   |
       +-----------------------------------------------------+

      Figure 4: Architectural view of Multipath QUIC for a host having N sending uniflows and M receiving uniflows
```

다중 경로 QUIC 연결은 각 peer에서 Uniflow ID 0으로 식별되는 두 개의 초기 Uniflow를 사용하여 시작됩니다. 그러면 패킷이 여러 uniflow에 분산될 수 있습니다. 각 uniflow에는 속한 위치를 명시적으로 표시하는 데 사용되는 UCID(Uniflow Connection ID(s)) 패킷(세트)이 있습니다. uniflow의 방향에 따라 호스트는 uniflow 소스 연결 ID(USCID, 수신 uniflow) 또는 uniflow 대상 연결 ID(USCID, 보내는 uniflow)를 유지합니다. 호스트의 송신 uniflow의 (집합) UDCID(들)은 원격 peer의 대응하는 수신 uniflow의 (집합) USCID(들)와 동일하다는 점에 유의하십시오.

다른 uniflow의 연결 가능성을 방지하는 것은 다중 경로 확장[I-D.huitema-quic-mpath-req]에 대한 중요한 요구 사항입니다. 암시적 uniflow 식별자로 UCID를 사용하여 이 문제를 해결합니다. 이것은 이 초안의 이전 버전에서와 같이 명시적 신호를 갖는 것보다 연결 가능성을 더 어렵게 만듭니다. 또한 공개 헤더 변경이 필요하지 않으므로 QUIC 불변 [I-D.ietf-quic-invariants]을 유지합니다.

uniflow가 사용 중일 때 각 엔드호스트는 이를 네트워크 경로와 연결합니다. 실제로 이것은 패킷이 송신(또는 수신) uniflow에서 송신(또는 수신)되는 특정 4-tuple로 구성됩니다. 각 엔드호스트에는 엔드호스트마다 다를 수 있는 4-tuple에 대한 특정 비전이 있습니다. 예를 들어 NAT 뒤에 위치한 클라이언트는 사설 IP 주소에서 데이터를 보내고 서버는 NAT의 공인 IP 주소에서 오는 패킷을 받습니다. uniflow가 공통 네트워크 경로를 공유할 수 있지만 이것이 필수는 아닙니다.

각 uniflow는 주어진 네트워크 경로를 통한 패킷의 독립적인 흐름입니다. Uniflow는 매우 다른 네트워크 조건(대기 시간, 대역폭, ...)을 경험할 수 있습니다. 이를 처리하기 위해 각 uniflow에는 고유한 패킷 시퀀스 번호 공간이 있습니다.

UCID, 4-tuple 및 패킷 번호 공간 외에도 각 uniflow에 대해 몇 가지 추가 정보가 유지됩니다. Uniflow ID는 프레임 수준에서 uniflow를 식별하고 nonce의 고유성을 보장하면서 동시에 사용되는 uniflow의 수를 제한합니다(자세한 내용은 섹션 8.1 참조).

### 3.5.  Uniflow Establishment

암호화 핸드셰이크 중에 교환되는 "max_sending_uniflow_id" 전송 매개변수는 호스트가 지원하려는 전송 uniflow 수의 상한선을 수정합니다. 그런 다음 호스트는 uniflow에서 사용할 peer uniflow 연결 ID를 제공합니다. 두 호스트 모두 peer가 현재 사용할 수 있는 전송 uniflow 수, 즉 peer에 제안된 서로 다른 uniflow ID의 수를 동적으로 제어합니다.
발신자가 가질 수 있는 전송 경로의 상한을 결정하는 동안 발신자는 uniflow를 사용하기 전에 수신기와 통신하는 UCID가 필요하므로 uniflow를 초기화하는 것은 수신기입니다.

peer가 "max_sending_uniflow_id" 전송 매개변수에 대해 서로 다른 값을 알릴 수 있으며, 각 호스트의 송신 및 수신 uniflow에 서로 다른 상한을 설정할 수 있습니다.

호스트는 NEW_CONNECTION_ID 프레임의 확장 버전인 MP_NEW_CONNECTION_ID 프레임(섹션 6.1 참조)을 전송하여 수신 uniflow 생성을 시작합니다. 이 프레임은 UCID를 uniflow에 연결합니다. MP_NEW_CONNECTION_ID 프레임을 수신하면 호스트는 제공된 UCID로 보낸 패킷을 표시하여 참조된 Uniflow ID를 가진 제안된 전송 uniflow 사용을 시작할 수 있습니다. 따라서 호스트는 MP_NEW_CONNECTION_ID 프레임을 전송하면 제안된 UCID를 사용하여 해당 Uniflow ID에서 패킷을 수신할 준비가 되었음을 알립니다. 프레임이 암호화됨에 따라 QUIC 연결을 통해 새로운 uniflow를 추가해도 일반 텍스트 식별자 [I-D.huitema-quic-mpath-req]가 누출되지 않습니다.

서버는 MP_NEW_CONNECTION_ID 프레임이 여러 개인 동일한 Uniflow ID에 대해 여러 Uniflow 연결 ID를 제공할 수 있습니다. 이것은 섹션 3.9에 설명된 대로 마이그레이션 사례에 대처하는 데 유용할 수 있습니다. 따라서 다중 경로 QUIC는 비대칭입니다.

### 3.6.  Exchanging Data over Multiple Uniflows

QUIC 패킷은 하나 이상의 프레임에 대한 컨테이너 역할을 합니다. 다중 경로 QUIC는 QUIC와 동일한 STREAM 프레임을 사용하여 데이터를 전송합니다. 바이트 오프셋은 데이터 페이로드와 연결됩니다. (다중 경로) QUIC의 주요 설계 중 하나는 프레임이 프레임을 운반하는 패킷과 독립적이라는 것입니다. 이것은 하나의 uniflow를 통해 전송된 프레임이 변경 없이 나중에 다른 uniflow에서 재전송될 수 있음을 의미합니다. 또한 현재의 모든 QUIC 프레임은 멱등성(idempotent)이고 여러 uniflow에서 낙관적으로 복제될 수 있습니다.

데이터가 전송되는 uniflow는 패킷 수준 정보입니다. 이것은 프레임을 운반하는 패킷의 uniflow에 관계없이 프레임을 보낼 수 있음을 의미합니다. MAX_STREAM_DATA 프레임에 의해 보급된 스트림 수신 창과 같은 다른 흐름 제어 고려 사항은 여러 전송 uniflow가 있는 경우 변경되지 않은 상태로 유지됩니다.

앞에서 설명한 것처럼 다중 경로 QUIC는 대기 시간이 다른 uniflow를 사용할 때 패킷 수준에서 재정렬에 직면할 수 있습니다. 다른 Uniflow 연결 ID의 존재는 주어진 uniflow를 통해 전송된 패킷이 단조롭게 증가하는 패킷 번호를 포함하도록 합니다. 더 많은 유연성을 보장하고 다른 네트워크 특성을 나타내는 uniflow의 대역폭을 집계할 때 (MP_)ACK 프레임의 ACK 블록 섹션을 잠재적으로 줄이기 위해 각 uniflow는 자신의 단조 증가하는 패킷 번호 공간을 유지합니다. 이것은 잠재적으로 각 uniflow우가 자체 패킷 번호 공간을 가지고 있기 때문에 QUIC 연결에서 최대 2 * 2^64 패킷을 보낼 수 있습니다(이 제한에 대한 자세한 내용은 섹션 8.1 참조).

다중 uniflow가 도입됨에 따라 서로 다른 uniflow에서 개별적으로 전송된 패킷을 승인할 필요가 있습니다. 초기 Uniflows(Uniflow ID 0)에서 전송된 패킷은 여전히 ​​일반 ACK 프레임으로 확인되므로 코어 프레임에 수정 사항이 도입되지 않습니다. 다른 uniflow의 경우 다중 경로 확장은 호스트가 패킷을 확인하는 수신 uniflow를 나타내는 Uniflow ID 필드를 ACK 프레임에 접두사로 지정하는 MP_ACK 프레임을 도입합니다. 이를 더 잘 설명하기 위해 그림 5에 나와 있는 상황을 살펴보겠습니다.

```
   Sending uniflow 0 - CID A      |    Receiving uniflow 0 - CID A
   Sending uniflow 1 - CID B      |    Receiving uniflow 1 - CID B
   Receiving uniflow 0 - CID C    |      Sending uniflow 0 - CID C
   Receiving uniflow 1 - CID D    |      Sending uniflow 1 - CID D
   Receiving uniflow 2 - CID E    |      Sending uniflow 2 - CID E

   Client                                                   Server
   ------                                                   ------
      |                                                        |
      |       Pkt(DCID=B,PN=42,frames=[STREAM("Request")])     |
      |---------------------------                             |
      |                          |---------------------------->|
      |                                                        |
      |  Pkt(DCID=E,PN=58,frames=[                             |
      |   STREAM("Response"), MP_ACK(UID=1,LargestAcked=42)])  |
      |                          ------------------------------|
      |<-------------------------|                             |
      |                                                        |

              Figure 5: Acknowledging Packets Sent on Uniflows
```

여기에서 5개의 uniflow가 사용 중이며 2개는 클라이언트에서 서버 방향으로, 3개는 반대 방향으로 사용됩니다. 클라이언트는 먼저 보내는 Uniflow 1(CID B에 연결됨)에서 패킷을 보냅니다. 서버는 수신하는 Uniflow 1에서 패킷을 수신합니다. 따라서 Uniflow ID 1에 대한 MP_ACK 프레임을 생성하여 클라이언트로 전송합니다. 서버는 이 프레임을 전송하기 위해 전송 uniflow를 선택할 수 있습니다. 제공된 상황에서 서버는 Uniflow 2에서 패킷을 보냅니다. 따라서 클라이언트는 수신 Uniflow 2에서 이 패킷을 받습니다.

유사하게, 주어진 uniflow우를 통해 전송된 패킷은 동일한 네트워크 경로를 공유하지 않는 다른 uniflow우에서 전송된 (MP_)ACK 프레임에 의해 승인될 수 있습니다. 그림 2를 다시 보면 LTE 네트워크를 사용하는 DCID D를 가진 서버의 송신 uniflow 1의 "응답 2" 패킷은 WLAN 네트워크를 사용하는 uniflow에서 수신된 MP_ACK 프레임에 의해 확인됩니다.

3.7.  Exchanging Addresses

멀티홈 장치가 IPv4 주소를 사용하여 이중 스택 서버에 연결할 때 로컬 주소(예: WLAN 및 셀룰러 주소)와 QUIC 연결을 설정하는 데 사용되는 IPv4 원격 주소를 인식합니다. 클라이언트가 새로운 uniflow를 만들고 IPv6 네트워크를 통해 사용하려는 경우 원격 peer의 다른 주소를 알아야 합니다.

이것은 현재 주소를 알리기 위해 다중 경로 QUIC 호스트에서 보낸 ADD_ADDRESS 프레임으로 가능합니다. 보급된 각 주소는 주소 ID로 식별됩니다. 호스트에 연결된 주소는 다중 경로 QUIC 연결의 수명 동안 다를 수 있습니다. 호스트가 새로운 주소를 가질 때 새로운 ADD_ADDRESS 프레임이 전송된다. 이 ADD_ADDRESS 프레임은 다른 QUIC 제어 프레임으로 보호되므로 공격자가 스푸핑할 수 없습니다. 통신된 주소는 [I-D.ietf-quic-transport]의 섹션 8에 설명된 대로 사용을 시작하기 전에 수신 호스트에서 먼저 유효성을 검사해야 합니다(MUST). 이 프로세스는 보급된 주소가 실제로 peer에 속하고 peer가 제공된 주소에서 호스트가 보낸 패킷을 수신할 수 있는지 확인합니다. 또한 호스트가 피해자 주소에 대한 증폭 공격을 시작하는 것을 방지합니다.

클라이언트가 NAT 뒤에 있는 경우 ADD_ADDRESS 프레임에서 개인 주소를 알릴 수 있습니다. 이러한 상황에서 서버는 통신된 주소를 확인할 수 없습니다. 클라이언트는 여전히 NAT 주소를 사용하여 전송 uniflow 사용을 시작할 수 있습니다. 서버가 개인 주소와 공용 주소 사이에 링크를 만들 수 있도록 하고 다른 4-tuple 보기를 조정하기 위해 Multipath QUIC는 연결된 로컬 주소 ID와 함께 현재 활성 전송 Uniflow ID를 나열하는 UNIFLOWS 프레임을 제공합니다. 호스트는 연결과 관련된 원격 IP 주소를 관찰하여 peer의 공용 주소를 발견할 수도 있습니다.

호스트가 해당 Uniflow 연결 ID를 제안하는 MP_NEW_CONNECTION_ID 프레임을 peer에 보내자마자 수신 uniflow가 활성화됩니다. 송신 uniflow는 uniflow 연결 ID를 수신하고 검증된 4-tuple에 바인딩될 때 활성화됩니다. UNIFLOWS 프레임은 uniflow가 보낸 사람의 관점에서 사용하는 로컬 주소 ID를 나타냅니다. 이 정보를 사용하여 원격 호스트는 공용 주소의 유효성을 검사하고 알려진 주소를 인식된 주소와 연결할 수 있습니다.

### 3.8.  Coping with Address Removals

QUIC 연결의 수명 동안 호스트는 주소 중 일부를 잃을 수 있습니다. 구체적인 예는 스마트폰이 WLAN 네트워크의 범위를 벗어나거나 네트워크 인터페이스 중 하나를 차단하는 것입니다. 이러한 주소 제거는 REMOVE_ADDRESS 프레임을 사용하여 알립니다. REMOVE_ADDRESS 프레임에는 ADD_ADDRESS를 통해 이전에 통신한 손실된 주소의 주소 ID가 포함됩니다. 지정된 주소 ID가 주문해야 하는 여러 이벤트(예: ADD_ADDRESS, REMOVE_ADDRESS 및 ADD_ADDRESS 다시)를 만날 수 있기 때문에 ADD_ADDRESS 및 REMOVE_ADDRESS 프레임 모두 주소 ID 관련 시퀀스 번호를 포함합니다.

### 3.9.  Uniflow Migration

주어진 시간에 다중 경로 QUIC endpoint은 각각 4-tuple과 연결된 일련의 활성 송수신 uniflow를 수집합니다. 이 연결은 변경 가능합니다. 호스트는 전송 uniflow가 사용하는 4-tuple을 언제든지 변경할 수 있으므로 QUIC가 uniflow를 한 네트워크 경로에서 다른 네트워크 경로로 마이그레이션할 수 있습니다. 그러나 주소 연결 가능성으로 인한 개인 정보 보호 문제를 해결하려면 [I-D.ietf-quic-transport]의 섹션 9.5에 설명된 대로 4-tuple이 변경될 때 호스트가 전송 uniflow에서 사용하는 것과 동일한 연결 ID를 재사용하지 않아야 합니다.

### 3.10.  Handling Multiple Network Paths

여러 전송 uniflow를 동시에 사용하면 사양이 이 문서의 범위를 벗어나는 새로운 알고리즘(패킷 스케줄링, 경로 관리)이 도입됩니다. 그럼에도 불구하고 이러한 알고리즘은 실제로 다중 경로 TCP, CMT-SCTP 및 다중 경로 DCCP와 같은 다중 경로 지원 전송 프로토콜에 존재합니다. 동반 초안[I-D.bonaventure-iccrg-schedulers]은 애플리케이션 목표에 따라 여러 범용 패킷 스케줄러를 제공합니다. 경로/uniflow 관리 고려 사항을 논의하기 위해 유사한 문서를 만들 수 있습니다.

### 3.11.  Congestion Control

QUIC 혼잡 제어 방식은 [I-D.ietf-quic-recovery]에 정의되어 있습니다. 이 혼잡 제어 방식은 여러 송신 uniflow가 활성화되어 있을 때 적합하지 않습니다. [I-D.ietf-quic-recovery]에 정의된 혼잡 제어 방식을 다중 경로 QUIC와 함께 사용하면 불공정한 결과를 초래할 수 있습니다. 다중 경로 QUIC 연결의 각 전송 uniflow는 자체 정체 제어 상태를 가져야 합니다(MUST). 다중 경로 TCP의 경우 서로 다른 전송 uniflow의 창을 함께 결합해야 합니다[RFC6356].

## 4.  Mapping Uniflow IDs to Connection IDs

개요 섹션에 설명된 대로 호스트는 uniflow 패킷이 전송되는 대상을 식별해야 합니다. 그런 다음 공용 헤더에서 Uniflow ID를 유추해야 합니다. 이것은 짧은 헤더 패킷의 대상 연결 ID 필드를 사용하여 수행됩니다.

초기 Uniflow 연결 ID는 암호화 핸드셰이크 중에 결정되며 실제로 현재 단일 경로 QUIC 설계[I-D.ietf-quic-transport]의 두 연결 ID에 모두 해당합니다. 초기 Uniflow에 대한 추가 Uniflow 연결 ID는 일반 NEW_CONNECTION_ID 프레임과 함께 제공될 수 있습니다. 다른 uniflow의 uniflow 연결 ID는 MP_NEW_CONNECTION_ID 프레임이 교환될 때 결정됩니다.

호스트는 전송한 (MP_)NEW_CONNECTION_ID 프레임에서 제안한 UCID를 사용하여 peer에서 오는 패킷을 수락하고 해당 수신 Uniflow ID와 연결해야 합니다(MUST). (MP_)NEW_CONNECTION_ID 프레임을 수신하면 호스트는 이를 확인하고 광고된 Uniflow 대상 연결 ID와 제안된 전송 uniflow의 Uniflow ID를 저장해야 합니다(MUST).

호스트는 (MP_)RETIRE_CONNECTION_ID 수신에 의해 그 동안 peer에 의해 폐기되지 않는 한 광고된 모든 Uniflow 연결 ID가 전체 연결 수명 동안 사용 가능한지 확인해야 합니다.

호스트는 peer에 의해 보급된 "max_sending_uniflow_id" 값보다 큰 Uniflow ID를 가진 MP_NEW_CONNECTION_ID 프레임을 보내지 않아야 합니다(MUST NOT).

## 5.  Using Multiple Uniflows

   This section describes in details the Multipath QUIC operations.

### 5.1.  Multipath Negotiation

다중 경로 협상은 "max_sending_uniflow_id" 전송 매개변수를 사용하여 암호화 핸드셰이크 중에 발생합니다. QUIC 연결은 처음에 QUIC에서 단일 경로입니다. 이 핸드셰이크 동안 호스트는 다중 경로 작업에 대한 지원을 알립니다. 호스트가 "max_sending_uniflow_id" 전송 매개변수에 대한 값을 알릴 때 이는 다중 경로 확장, 즉 이 문서에 정의된 확장을 지원함을 나타냅니다(로컬 다중 네트워크 경로의 가용성과 혼합되지 않음). 호스트가 "max_sending_uniflow_id" 전송 매개변수를 알리지 않으면 다중 경로 확장이 비활성화됩니다.

여러 uniflows의 사용은 동일한 QUIC 연결을 통해 여러 연결 ID를 사용하는 기능에 의존합니다. 따라서 peer가 "max_sending_uniflow_id" 전송 매개변수에 대해 0과 다른 값을 광고하는 경우 길이가 0인 연결 ID를 사용해서는 안 됩니다(MUST NOT).

### 5.1.1.  Transport Parameter Definition

호스트는 다음 전송 매개변수를 사용할 수 있습니다.

max_sending_uniflow_id(0x40): 이 문서에서 제시하는 다중경로 확장의 지원을 전달된 값과 상관없이 나타냅니다. 정수 값은 호스트가 값을 지원할 준비가 되었음을 알리는 전송 uniflow 수의 상한을 설정합니다. 없으면 호스트가 연결을 통해 다중 경로 확장을 사용하는 데 동의하지 않음을 의미합니다.

### 5.2.  Coping with Additional Remote Addresses

호스트는 ADD_ADDRESS 프레임을 수신하거나 들어오는 패킷의 4-tuple을 관찰하여 원격 주소를 학습할 수 있습니다. 호스트는 패킷을 해당 주소로 보내기 시작하기 전에 새로 학습된 원격 IP 주소를 먼저 검증해야 합니다(MUST). 이 요구 사항은 섹션 8.2에 설명되어 있습니다. 호스트는 [I-D.ietf-quic-transport]에 지정된 대로 주소 유효성 검사 절차를 시작해야 합니다(MUST).

호스트는 제한된 시간 동안 검증된 주소를 캐시할 수 있습니다(MAY).

### 5.3.  Receiving Uniflow State

peer에게 uniflow를 제안할 때 호스트는 수신 uniflow에 대한 일부 상태를 유지해야 합니다. 이 상태는 해당 Uniflow ID를 제안하는 첫 번째 MP_NEW_CONNECTION_ID 프레임을 보낼 때 생성됩니다. 이 수신 uniflow에 대해 활성 uniflow 연결 ID가 하나 있는 한(즉, MP_RETIRE_CONNECTION_ID를 사용하여 아직 폐기되지 않은 하나의 UCID) 호스트는 수신 uniflow를 통해 패킷을 수락해야 합니다(MUST). 일단 생성되면 호스트는 다음 수신 uniflow 정보를 유지해야 합니다.

uniflow ID: 연결에서 수신 uniflow를 고유하게 식별하는 정수입니다. 이 값은 변경할 수 없습니다.

Uniflow Connection IDs: 이 수신 uniflow에 속하는 패킷의 연결 ID 필드에 가능한 값입니다. 이 값은 이전에 전송된 MP_NEW_CONNECTION_ID 프레임에서 보급된 활성 UCID 시퀀스를 포함합니다. 예를 들어 모든 보급된 UCID가 peer에 의해 폐기된 경우 이 시퀀스가 ​​비어 있을 수 있습니다.

패킷 번호 공간: 이 수신 유니플로우 전용 패킷 번호 공간. [I-D.ietf-quic-transport]의 섹션 12.3에 설명된 패킷 번호 고려 사항은 지정된 수신 유니플로 내에서 적용됩니다.

Associated 4-tuple: 현재 이 uniflow를 통해 패킷을 수신하는 것으로 관찰된 tuple(sIP, dIP, sport, dport)입니다. 호스트가 이전에 기록된 것과 다른(가능한) 검증된 원격 주소 및/또는 포트를 가진 패킷을 수신할 수 있기 때문에 이 값은 변경 가능합니다. 호스트가 수신 uniflow의 4-tuple의 변경을 관찰하면 [I-D.ietf-quic-transport]의 섹션 9.5의 고려 사항을 따릅니다.

Associated local Address ID: 패킷을 수신하는 데 사용되는 로컬 주소에 해당하는 peer에서 보낸 ADD_ADDRESS 프레임에 광고된 주소 ID입니다. 이것은 uniflow와 주소 간의 매핑을 알리는 UNIFLOWS 프레임을 생성하는 데 도움이 됩니다. 연결이 설정된 주소의 주소 ID는 0입니다.

호스트는 수신된 패킷 수와 같이 uniflow 단위로 네트워크 측정값을 수집할 수도 있습니다.

### 5.4.  Sending Uniflow State

다중 경로 QUIC 연결 중에 호스트는 uniflow를 보내기 위한 일부 상태를 유지합니다. 전송 uniflow의 상태는 호스트가 저장해야 하는 정보를 결정합니다. 가능한 전송 uniflow 상태는 그림 6에 나와 있습니다.

```
         o
         |
         | receive a first MP_NEW_CONNECTION_ID
         |    with the associated Uniflow ID
         |
         v       path usage over a validated 4-tuple
   +----------+ ------------------------------------> +----------+
   |  UNUSED  |                                       |  ACTIVE  |
   +----------+ <------------------------------------ +----------+
                    address change or retired UCID

      Figure 6: Finite-State Machine describing the possible states of a sending uniflow
```

수신된 MP_NEW_CONNECTION_ID 프레임에서 peer에 의해 전송 uniflow가 제안되면 UNUSED 상태가 됩니다. 이 상황에서 호스트는 다음과 같은 전송 uniflow 정보를 유지해야 합니다.

Uniflow ID: 연결에서 보내는 uniflow를 고유하게 식별하는 정수입니다. 이 값은 변경할 수 없습니다.

Uniflow Connection IDs: 이 전송 uniflow에 속하는 패킷의 연결 ID 필드에 가능한 값입니다. 이 값은 이전에 수신된 MP_NEW_CONNECTION_ID 프레임에서 보급된 활성 UCID 시퀀스를 포함합니다. 예를 들어 광고된 모든 UCID가 폐기된 경우 이 시퀀스가 ​​비어 있을 수 있습니다.

송신 uniflow우 상태: 송신 uniflow우의 현재 상태로, 그림 6에 표시된 값 중 하나입니다.

패킷 번호 공간: 이 전송 유니플로우 전용 패킷 번호 공간. [I-D.ietf-quic-transport]의 섹션 12.3에 설명된 패킷 번호 고려 사항은 지정된 전송 유니플로 내에서 적용됩니다.

UNUSED 송신 uniflow우는 주어진 4-tuple을 검증하기 위해 proving packet을 보낼 수 있습니다.

호스트가 검증된 주소를 통해 송신 uniflow 사용을 시작하려고 할 때 송신 uniflow는 ACTIVE 상태가 됩니다. 이것은 송신 uniflow우를 사용하여 패킷을 보낼 수 있는 상태입니다. ACTIVE 상태의 uniflow를 갖는 것은 그것이 사용될 수 있다는 것을 보장할 뿐 호스트가 강제로 사용되는 것은 아닙니다. UNUSED 상태에 필요한 필드 외에도 다음 요소를 추적해야 합니다.

Associated 4-tuple: 현재 이 uniflow를 통한 패킷에 사용되는 tuple(sIP, dIP, sport, dport)입니다. 호스트가 로컬(또는 원격) 주소 및/또는 포트를 변경하기로 결정할 수 있으므로 이 값은 변경 가능합니다. 전송 uniflow의 4-tuple을 변경하는 호스트는 이를 마이그레이션해야 합니다(SHOULD).

Associated(로컬 주소 ID, 원격 주소 ID) tuple: 이러한 식별자는 전송(로컬) 및 수신(원격)된 ADD_ADDRESS에서 가져옵니다. 이것은 예를 들어 원격 주소 ID가 REMOVE_ADDRESS에서 손실된 것으로 선언될 때 호스트가 전송 uniflow 사용을 일시적으로 중지할 수 있도록 합니다. 연결이 설정된 주소는 주소 ID 0을 갖습니다. UNIFLOWS 프레임을 수신하면 호스트가 보내는 uniflow에서 사용하는 원격 주소 ID를 연결하는 데 도움이 됩니다.

혼잡 컨트롤러(Congestion controller): 송신 uniflow의 전송 속도를 제한하는 혼잡 창.

성능 메트릭(Performance metrics): 단방향 지연 또는 전송된 패킷 수와 같은 기본 통계입니다. 이 정보는 호스트가 패킷 스케줄링 결정 및 흐름 제어를 수행해야 할 때 유용할 수 있습니다.

엔드포인트 주소 중 하나를 더 이상 사용할 수 없거나 호스트가 전송 uniflow의 모든 UCID를 폐기했기 때문에 전송 경로를 일시적으로 사용할 수 없는 경우가 발생할 수 있습니다. 이러한 경우 경로는 UNUSED 상태로 돌아갑니다. UNUSED 상태로 다시 전환을 수행할 때 호스트는 ACTIVE 상태에 의해 추가된 추가 상태를 재설정해야 합니다. UNUSED 상태에서 호스트는 비proving packet을 보내지 않아야 합니다(MUST NOT). 이 상태에서 호스트는 다른 검증된 4-tuple을 통해 uniflow를 사용하여 다시 시작하여 uniflow 상태를 다시 ACTIVE 상태로 전환할 수 있습니다. 그러나 혼잡 컨트롤러 상태는 다시 시작되어야 하고 성능 메트릭은 재설정되어야 합니다(SHOULD).

### 5.5.  Losing Addresses

연결 수명 동안 호스트는 주소(예: 종료된 네트워크 인터페이스)를 잃을 수 있습니다. 해당 로컬 주소를 사용하고 있던 모든 ACTIVE 전송 uniflow는 해당 주소에서 패킷 전송을 중지해야 합니다(MUST). peer에게 주소 손실을 알리기 위해 호스트는 손실된 로컬 주소 ID를 나타내는 REMOVE_ADDRESS 프레임을 보내야 합니다. 호스트는 또한 나머지 ACTIVE uniflow의 상태를 나타내는 UNIFLOWS 프레임을 보내야 합니다.

REMOVE_ADDRESS를 수신하면 수신 호스트는 주소 제거의 영향을 받는 ACTIVE 전송 uniflow를 사용하여 중지해야 합니다(MUST).

호스트는 할당된 4-tuple을 변경하여 이러한 전송 uniflow 중 하나를 재사용할 수 있습니다(MAY). 이 경우 해당 변경을 설명하는 UNIFLOWS 프레임을 보내야 합니다.

## 6.  New Frames

To support the multipath operations, new frames have been defined to coordinate hosts.  
   
The following table summarizes the added frames.

   | Type Value  | Frame Type Name         | Definition  | Pkts | Spec |
   |---|---|---|---|---|---|
   | 0x40        | MP_NEW_CONNECTION_ID    | Section 6.1 | ___1 | P    |
   | 0x41        | MP_RETIRE_CONNECTION_ID | Section 6.2 | ___1 |      |
   | 0x42 -      | MP_ACK                  | Section 6.3 | ___1 | NC   |
   | 0x43        |                         |             |      |      |
   | 0x44        | ADD_ADDRESS             | Section 6.4 | ___1 |      |
   | 0x45        | REMOVE_ADDRESS          | Section 6.5 | ___1 |      |
   | 0x46        | UNIFLOWS                | Section 6.6 | ___1 |      |
                    Table 1: Multipath-related Frame Types

표 1은 Pkts 및 Spec 열에 대해 [I-D.ietf-quic-transport]의 표 3과 동일한 표기법을 사용합니다. 특히, 이 문서에 정의된 모든 프레임은 1-RTT 패킷으로 교환되어야 합니다(MUST). 다른 암호화 컨텍스트에서 다중 경로 관련 프레임 중 하나를 수신하는 호스트는 PROTOCOL_VIOLATION 오류와 함께 연결을 닫아야 합니다(MUST). MP_ACK 프레임을 제외한 모든 프레임은 ack-eliciting입니다. MP_ACK 프레임을 포함하는 패킷이 ACK 또는 MP_ACK 프레임만 전달하는 경우 MP_ACK 프레임은 전송 중인 바이트로 계산되지 않습니다. MP_NEW_CONNECTION_ID 프레임은 새 네트워크 경로를 조사하기 위해 보낼 수 있는 유일한 새 프레임입니다.

이 문서의 나머지 부분은 [I-D.ietf-quic-transport]에 설명된 표기법을 사용합니다.

### 6.1.  MP_NEW_CONNECTION_ID Frames

MP_NEW_CONNECTION_ID 프레임(type=0x40)은 [I-D.ietf-quic-transport]에 의해 정의된 NEW_CONNECTION_ID 프레임의 확장이다. peer에 대체 연결 ID를 제공하고 Uniflow ID를 사용하여 특정 uniflow에 연결합니다.

MP_NEW_CONNECTION_ID 프레임의 포맷은 다음과 같다.

```
   MP_NEW_CONNECTION_ID Frame {
     Type (i) = 0x40,
     Uniflow ID (i),
     Sequence Number (i),
     Retire Prior To (i),
     Length (8),
     Connection ID (8..160),
     Stateless Reset Token (128),
   }

                Figure 7: MP_NEW_CONNECTION_ID Frame Format
```

[I-D.ietf-quic-transport]에 명시된 NEW_CONNECTION_ID 프레임과 비교하여 다음 필드가 추가된다.

Uniflow ID: 제공된 연결 ID와 관련된 uniflow를 나타냅니다.

나머지 필드는 NEW_CONNECTION_ID 프레임과 동일한 의미를 유지합니다.

이 프레임은 두 호스트 모두에서 보낼 수 있습니다. 지정된 Uniflow ID가 있는 프레임을 수신하면 peer는 관련 전송 uniflow 상태를 업데이트하고 통신된 연결 ID를 저장해야 합니다(MUST).

핸드셰이크 완료 시 다중 경로 사용의 지연을 제한하기 위해 호스트는 연결 설정이 완료되는 즉시 사용을 허용하는 수신 uniflow에 대해 MP_NEW_CONNECTION_ID 프레임을 보내야 합니다(SHOULD).

연결 ID 생성은 [I-D.ietf-quic-transport]의 섹션 5.1에 제시된 것과 동일한 고려 사항을 따라야 합니다(MUST).

### 6.2.  MP_RETIRE_CONNECTION_ID Frame

MP_RETIRE_CONNECTION_ID 프레임(type=0x41)은 [I-D.ietf-quic-transport]에 의해 정의된 RETIRE_CONNECTION_ID 프레임의 확장이다. 이는 엔드 호스트가 peer에서 발행한 주어진 uniflow우와 관련된 연결 ID를 더 이상 사용하지 않음을 나타냅니다.

MP_RETIRE_CONNECTION_ID 프레임의 형식은 다음과 같습니다.

```
   MP_RETIRE_CONNECTION_ID Frame {
     Type (i) = 0x41,
     Uniflow ID (i),
     Sequence Number (i),
   }

               Figure 8: MP_RETIRE_CONNECTION_ID Frame Format
```

[I-D.ietf-quic-transport]에 명시된 RETIRE_CONNECTION_ID 프레임과 비교하여 다음 필드가 추가된다.

Uniflow ID: 연결 ID가 폐기되는 Uniflow를 나타냅니다.

프레임은 uniflow 기반으로 [I-D.ietf-quic-transport]에 설명된 RETIRE_CONNECTION_ID 프레임으로 처리됩니다.

### 6.3.  MP_ACK Frame

MP_ACK 프레임(유형 0x42 및 0x43)은 [I-D.ietf-quic-transport]에 의해 정의된 ACK 프레임의 확장입니다. 호스트가 초기가 아닌 uniflow에서 보낸 패킷을 확인할 수 있도록 합니다. 프레임 유형이 0x43인 경우 MP_ACK 프레임에는 이 지점까지 연결에서 수신된 관련 ECN 표시가 있는 QUIC 패킷의 합계도 포함됩니다.

MP_ACK 프레임의 포맷은 아래와 같다.

```
   MP_ACK Frame {
     Type (i) = 0x02..0x03,
     Uniflow ID (i),
     Largest Acknowledged (i),
     ACK Delay (i),
     ACK Range Count (i),
     First ACK Range (i),
     ACK Range (..) ...,
     [ECN Counts (..)],
   }

                       Figure 9: MP_ACK Frame Format
```

[I-D.ietf-quic-transport]에 명시된 ACK 프레임과 비교하여 다음 필드가 추가된다.

Uniflow ID: 승인된 패킷 시퀀스 번호가 관련된 uniflow를 나타냅니다.

6.4.  ADD_ADDRESS Frame

ADD_ADDRESS 프레임(유형=0x44)은 호스트에서 현재 도달 가능한 주소를 알리는 데 사용됩니다.

ADD_ADDRESS 프레임의 포맷은 아래와 같다.

```
   ADD_ADDRESS Frame {
     Type (i) = 0x44,
     Rsv (3) = 0,
     P (1),
     IP Version (4),
     Address ID (8),
     Sequence Number (i),
     Interface Type (8),
     IP Address (32/128),
     [Port (16)],
   }

                    Figure 10: ADD_ADDRESS Frame Format
```

ADD_ADDRESS 프레임에는 다음 필드가 포함됩니다.

Rsv: 이 비트는 0으로 설정되며 향후 사용을 위해 예약되어 있습니다.

P bit: 이 비트는 설정된 경우 포트 필드의 존재를 나타냅니다.

IP version: 4비트로 작성되며 IP 주소 필드에 포함된 IP 주소의 버전이 포함됩니다.

Address ID: 추적 및 제거 목적으로 광고된 주소에 대한 1바이트 고유 식별자입니다. 이것은 예를 들어 NAT가 IP 주소를 변경하여 두 호스트가 동일한 네트워크 경로에 대해 다른 IP 주소를 볼 수 있도록 할 때 필요합니다.

Sequence Number: 가변 길이 정수로 인코딩된 이벤트의 주소 ID 관련 시퀀스 번호. 시퀀스 번호 공간은 동일한 주소 ID를 언급하는 REMOVE_ADDRESS 프레임과 공유됩니다.

Interface Type: 이 주소가 바인딩된 인터페이스 유형에 대한 표시를 제공하는 1바이트 필드입니다. 다음 값이 정의됩니다.

  * 0: 고정. 기본값으로 사용됩니다.
  * 1: 무선랜(WLAN)
  * 2: 셀룰러(Cellular)

IP 주소: 네트워크 순서에 따라 보급된 IP 주소입니다.

포트: 이 선택적 필드는 보급된 IP 주소와 관련된 포트 번호를 나타냅니다. 이 필드가 있으면 uniflow가 2-tuple(IP 주소, 포트)을 사용할 수 있음을 나타냅니다.

ADD_ADDRESS 프레임을 수신하면 수신기는 향후 사용을 위해 통신된 주소를 저장해야 합니다(SHOULD).

수신자는 [I-D.ietf-quic-transport]의 8절에 명시된 대로 유효성을 검증하지 않고 통신된 주소로 유효성 검증 패킷 이외의 패킷을 보내서는 안 됩니다(MUST NOT). ADD_ADDRESS 프레임은 전역적으로 연결할 수 있는 주소를 포함해야 합니다(SHOULD). 링크 로컬 및 개인 주소는 교환하면 안 됩니다(SHOULD NOT).

### 6.5.  REMOVE_ADDRESS Frame

REMOVE_ADDRESS 프레임(type=0x45)은 이전에 발표된 주소가 손실되었음을 알리기 위해 호스트가 사용합니다.

REMOVE_ADDRESS 프레임의 형식은 다음과 같습니다.

```
   REMOVE_ADDRESS Frame {
     Type (i) = 0x45,
     Address ID (8),
     Sequence Number (i),
   }

                   Figure 11: REMOVE_ADDRESS Frame Format
```

REMOVE_ADDRESS 프레임에는 다음 필드가 포함됩니다.

Address ID: 제거할 주소의 1바이트 식별자입니다.

Sequence Number: 가변 길이 정수로 인코딩된 이벤트의 주소 ID 관련 시퀀스 번호. 시퀀스 번호 공간은 동일한 주소 ID를 언급하는 ADD_ADDRESS 프레임과 공유됩니다. 이것은 어떤 이유로 REMOVE_ADDRESS가 새로운 ADD_ADDRESS 이후에 수신기에 도달하더라도 동일한 주소 ID를 암시하는 ADD_ADDRESS 프레임 이전에 REMOVE_ADDRESS가 전송되었을 수 있음을 수신기가 파악하는 데 도움이 됩니다.

호스트는 제거된 주소를 사용하는 전송 uniflow 사용을 중지하고 UNUSED 상태로 설정해야 합니다(SHOULD). REMOVE_ADDRESS가 이전에 발표되지 않은 주소 ID를 포함하는 경우 수신기는 프레임을 조용히 무시해야 합니다(MUST).

### 6.6.  UNIFLOWS Frame

UNIFLOWS 프레임(type=0x46)은 송신 호스트의 uniflows 상태를 peer에게 전달합니다. 이를 통해 발신자는 uniflow를 통해 잠재적인 연결 문제를 감지하기 위해 활성 uniflow를 peer와 통신할 수 있습니다. 또한 호스트가 주소 ID에 영향을 미치는 미들박스(예: NAT 등)가 있을 때 본 4-tuple에 매핑할 수 있습니다.

UNIFLOWS 프레임의 형식은 다음과 같습니다.

```
   UNIFLOWS Frame {
     Type (i) = 0x46,
     Sequence Number (i),
     Receiving Uniflows (i),
     Active Sending Uniflows (i),
     Receiving Uniflow Info Section (..) ...,
     Active Sending Uniflow Info Section (..) ...,
   }

                      Figure 12: UNIFLOWS Frame Format
```

UNIFLOWS 프레임에는 다음 필드가 포함됩니다.

Sequence Number: 가변 길이 정수. 이 값은 0에서 시작하여 호스트가 보낸 각 UNIFLOWS 프레임에 대해 1씩 증가합니다. 가장 최근의 UNIFLOWS 프레임을 식별할 수 있습니다. 수신 uniflow: 발신자의 관점에서 수신 uniflow의 현재 수입니다.

Active Sending Uniflows: 발신자의 관점에서 ACTIVE 상태의 현재 송신 uniflow우 수입니다.

receive Uniflow Info Section: 수신 uniflow에 대한 정보를 포함합니다(수신 uniflow 항목이 있음).

Sending Uniflow Info Section: ACTIVE 상태의 송신 Uniflow에 대한 정보를 포함합니다(Active Sending Uniflow 항목이 있음).

Uniflow 정보 수신 및 활성 송신 Uniflow 정보 섹션은 모두 아래와 같은 형식을 공유합니다.

```
   Uniflow Info Section {
     Uniflow ID (i),
     Local Address ID (8),
   }

                   Figure 13: Uniflow Info Section Format
```

Uniflow 정보 섹션의 필드는 다음과 같습니다.

Uniflow ID: 보내는 호스트가 정보를 제공하는 uniflow의 Uniflow ID입니다.

LocAddrID: 현재 uniflow에서 사용하는 주소의 로컬 주소 ID입니다.

Uniflow Info section에는 지금까지 로컬 주소 ID만 포함되어 있지만 이 섹션은 나중에 잠재적으로 유용한 다른 정보로 확장될 수 있습니다.

## 7.  Extension of the Meaning of Existing QUIC Frames

다중 경로 확장은 기존 QUIC 프레임의 와이어 형식을 수정하지 않습니다. 그러나 이 추가 사항을 투명하고 단일 경로 QUIC 설계와 일관되게 유지하면서 몇 가지의 의미를 확장합니다. 관련 프레임(및 확장된 의미)은 다음과 같습니다.

NEW_CONNECTION_ID: Uniflow ID가 0으로 설정된 MP_NEW_CONNECTION_ID 프레임과 동일합니다.

RETIRE_CONNECTION_ID: Uniflow ID가 0으로 설정된 MP_RETIRE_CONNECTION_ID 프레임과 동일합니다.

ACK: Uniflow ID가 0으로 설정된 MP_ACK 프레임과 동일합니다.

## 8.  Security Considerations

### 8.1.  Nonce Computation

Multipath QUIC를 사용하면 각 uniflow에는 고유한 패킷 번호 공간이 있습니다. 현재 nonce 계산 [I-D.ietf-quic-tls]을 사용하면 동일한 방향의 두 개의 서로 다른 uniflow우에 대해 동일한 패킷 번호를 두 번 사용하면 동일한 암호화 nonce가 생성됩니다. 동일한 nonce를 두 번 사용하면 안 됩니다(MUST NOT). 따라서 MP-QUIC는 [I-D.ietf-quic-tls]와 다른 nonce 계산을 갖습니다.

nonce의 가장 왼쪽 비트는 최대 max_sending_uniflow_id까지 현재 uniflow를 식별하는 Uniflow ID여야 합니다. nonce의 나머지 비트는 패딩된 패킷 번호(왼쪽이 0으로 채워짐)와 패킷 보호 IV의 최하위 비트의 배타적 OR에 의해 형성됩니다. nonce는 max_sending_uniflow_id <= 2이면 왼쪽으로 채워져야 하고 max_sending_uniflow_id는 2^61보다 높아서는 안 됩니다(MUST NOT). uniflow가 2^62-max_sending_uniflow_id 패킷을 보낸 경우 동일한 nonce를 재사용하는 것을 피하기 위해 다른 uniflow를 사용해야 합니다.

### 8.2.  Validation of Exchanged Addresses

ADD_ADDRESS 프레임을 통해 peer가 통신한 주소를 사용하려면 호스트가 [I-D.ietf-quic-transport]의 섹션 8에 설명된 대로 이러한 주소에 대한 uniflow를 사용하기 전에 해당 주소를 확인해야 합니다. [I-D.ietf-quic-transport] 섹션 21.12.3은 이 프로세스에 대한 추가 동기를 제공합니다. 또한 호스트는 경로 이탈 공격을 방지하기 위해 1-RTT 프레임에서 ADD ADDRESS 프레임을 보내야 합니다.

## 9.  IANA Considerations

### 9.1.  QUIC Transport Parameter Registry

이 문서는 다중 경로 협상을 위한 새로운 전송 매개변수를 정의합니다. 표 2의 다음 항목은 "QUIC 프로토콜" 제목 아래의 "QUIC 전송 매개변수" 레지스트리에 추가되어야 합니다.

```
            +=======+========================+===============+
            | Value | Parameter Name         | Specification |
            +=======+========================+===============+
            | 0x40  | max_sending_uniflow_id | Section 5.1.1 |
            +-------+------------------------+---------------+

              Table 2: Addition to QUIC Transport Parameters Entries
```

## 10.  Acknowledgments

이 초안의 첫 번째 버전에 대해 많은 의견을 주신 Masahiro Kozuka와 Kazuho Oku에게 감사드립니다. 우리는 또한 현재 디자인으로 이어진 초기 의견에 대해 Philipp Tiesel에게 감사하고, 이후 피드백에 대해 Ian Swett에게 감사합니다. 또한 다중 경로 기능에 대한 중요한 요소를 식별하기 위한 다중 경로 요구 사항에 대한 초안을 제공한 Christian Huitema에게도 감사드립니다. Mohamed Boucadair는 이 문서의 두 번째 버전에서 유용한 정보를 많이 제공했습니다. Maxime Piraux와 Florentin Rochet는 이 초안의 마지막 버전을 개선하는 데 도움을 주었습니다. 이 프로젝트는 Walloon 정부가 자금을 지원하는 MQUIC 프로젝트에 의해 부분적으로 지원되었습니다.

## 11.  References

### 11.1.  Normative References

   [I-D.ietf-quic-invariants]
              Thomson, M., "Version-Independent Properties of QUIC", Work in Progress, Internet-Draft, draft-ietf-quic-invariants-13, 14 January 2021, <https://www.ietf.org/archive/id/draft-ietf-quic-invariants-13.txt>.

   [I-D.ietf-quic-recovery]
              Iyengar, J. and I. Swett, "QUIC Loss Detection and Congestion Control", Work in Progress, Internet-Draft, draft-ietf-quic-recovery-34, 14 January 2021, <https://www.ietf.org/archive/id/draft-ietf-quic-recovery-34.txt>.

   [I-D.ietf-quic-tls]
              Thomson, M. and S. Turner, "Using TLS to Secure QUIC", Work in Progress, Internet-Draft, draft-ietf-quic-tls-34, 14 January 2021, <https://www.ietf.org/archive/id/draft-ietf-quic-tls-34.txt>.

   [I-D.ietf-quic-transport]
              Iyengar, J. and M. Thomson, "QUIC: A UDP-Based Multiplexed and Secure Transport", Work in Progress, Internet-Draft,
              draft-ietf-quic-transport-34, 14 January 2021, <https://www.ietf.org/archive/id/draft-ietf-quic-transport-34.txt>.

   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, DOI 10.17487/RFC2119, March 1997, <https://www.rfc-editor.org/info/rfc2119>.

   [RFC8174]  Leiba, B., "Ambiguity of Uppercase vs Lowercase in RFC 2119 Key Words", BCP 14, RFC 8174, DOI 10.17487/RFC8174, May 2017, <https://www.rfc-editor.org/info/rfc8174>.

### 11.2.  Informative References

   [I-D.bonaventure-iccrg-schedulers]
              Bonaventure, O., Piraux, M., Coninck, Q. D., Baerts, M., Paasch, C., and M. Amend, "Multipath schedulers", Work in Progress, Internet-Draft, draft-bonaventure-iccrg-schedulers-01, 9 September 2020, <https://www.ietf.org/archive/id/draft-bonaventure-iccrg-schedulers-01.txt>.

   [I-D.huitema-quic-mpath-req]
              Huitema, C., "QUIC Multipath Requirements", Work in Progress, Internet-Draft, draft-huitema-quic-mpath-req-01, 7 January 2018, <https://www.ietf.org/archive/id/draft-huitema-quic-mpath-req-01.txt>.

   [IETFJ]    Bonaventure, O. and S. Seo, "Multipath TCP Deployments", IETF Journal , November 2016.

   [MFQUIC]   De Coninck, Q. and O. Bonaventure, "Multiflow QUIC: A Generic Multipath Transport Protocol", To appear in IEEE Communications Magazine. A preprint is available at https://hdl.handle.net/2078.1/243486 , May 2021.

   [MPQUIC]   De Coninck, Q. and O. Bonaventure, "Multipath QUIC: Design and Evaluation", 13th International Conference on emerging Networking EXperiments and Technologies (CoNEXT 2017). http://multipath-quic.org , December 2017.

   [MPRTP]    Singh, V., Ahsan, S., and J. Ott, "MPRTP: Multipath considerations for real-time media", Proceedings of the 4th ACM Multimedia Systems Conference , 2013.

   [OLIA]     Khalili, R., Gast, N., Popovic, M., Upadhyay, U., and J.-Y. Le Boudec, "MPTCP is not pareto-optimal: performance issues and a possible solution", Proceedings of the 8th international conference on Emerging networking experiments and technologies, ACM , 2012.

   [RFC0793]  Postel, J., "Transmission Control Protocol", STD 7, RFC 793, DOI 10.17487/RFC0793, September 1981,
              <https://www.rfc-editor.org/info/rfc793>.

   [RFC3077]  Duros, E., Dabbous, W., Izumiyama, H., Fujii, N., and Y. Zhang, "A Link-Layer Tunneling Mechanism for Unidirectional Links", RFC 3077, DOI 10.17487/RFC3077, March 2001, <https://www.rfc-editor.org/info/rfc3077>.

   [RFC6356]  Raiciu, C., Handley, M., and D. Wischik, "Coupled
              Congestion Control for Multipath Transport Protocols",
              RFC 6356, DOI 10.17487/RFC6356, October 2011,
              <https://www.rfc-editor.org/info/rfc6356>.

   [RFC8041]  Bonaventure, O., Paasch, C., and G. Detal, "Use Cases and
              Operational Experience with Multipath TCP", RFC 8041,
              DOI 10.17487/RFC8041, January 2017,
              <https://www.rfc-editor.org/info/rfc8041>.

   [RFC8684]  Ford, A., Raiciu, C., Handley, M., Bonaventure, O., and C.
              Paasch, "TCP Extensions for Multipath Operation with
              Multiple Addresses", RFC 8684, DOI 10.17487/RFC8684, March
              2020, <https://www.rfc-editor.org/info/rfc8684>.

   [SCTPCMT]  Iyengar, J., Amer, P., and R. Stewart, "Concurrent
              multipath transfer using SCTP multihoming over independent
              end-to-end paths", IEEE/ACM Transactions on networking,
              Vol. 14, no 5 , 2006.

## Appendix A.  Comparison with Multipath TCP

다중 경로 TCP[RFC8684]는 현재 인터넷에서 가장 널리 배포된 다중 경로 전송 프로토콜입니다. 설계가 QUIC 프로토콜에 대한 다중 경로 확장의 초기 버전에 영향을 주었지만 이제 두 프로토콜 간에 주요 차이점이 있습니다.

### A.1.  Multipath TCP Bidirectional Paths vs. QUIC Uniflows

TCP는 데이터 발신자에게 다시 승인을 보내 안정적인 데이터 전달을 보장합니다. 데이터는 한 방향으로 흐르고 승인은 다른 방향으로 흐르기 때문에 양방향 경로의 개념은 TCP에서 사실상의 표준이 되었습니다. 다중 경로 TCP 확장 [RFC8684]은 여러 TCP 연결을 결합하여 단일 데이터 스트림을 분산시킵니다. 따라서 다중 경로 TCP 연결의 모든 경로는 양방향이어야 합니다. 그러나 네트워킹 경험에 따르면 방향을 따르는 패킷이 항상 반대 방향의 패킷과 정확히 동일한 경로를 공유하는 것은 아닙니다. 또한 QUIC는 설계의 많은 부분이 가능한 네트워크 비대칭(단방향 연결 ID, 유도하는 PATH_CHALLENGE와 다른 네트워크 경로에서 흐를 수 있는 PATH_RESPONSE,... ).

### A.2.  Negotiating the Multipath Extensions

다중 경로 TCP 연결은 다중 경로 확장을 협상하기 위해 TCP 헤더의 TCP 옵션 필드에 의존합니다. 핸드셰이크 중에 다중 경로 TCP는 MP_CAPABLE TCP 옵션을 사용하여 추가 다중 경로 TCP 하위 흐름을 인증하고 연결을 식별하는 데 사용되는 토큰을 생성하는 데 사용되는 연결 키를 교환합니다. 그러나 TCP 옵션은 일반 텍스트로 전송됩니다. 경로상의 모든 네트워크 관찰자는 연결의 키를 기록하고 여기에 다중 경로 TCP 하위 흐름을 생성할 수 있습니다. 다중 경로 QUIC는 이 보안 문제에 직면하지 않습니다. 다중 경로 확장은 QUIC 핸드셰이크의 인증된 전송 매개변수를 사용하여 협상됩니다. 그런 다음 Multipath QUIC는 QUIC의 암호화 기능을 활용하여 네트워크 관찰자로부터 정보를 숨깁니다.

### A.3.  Uniflow Establishment

다중 경로 TCP 연결에 대한 추가 하위 흐름을 만들기 위해 호스트는 MP_JOIN TCP 옵션을 협상하여 TCP 핸드셰이크를 시작합니다. 이 옵션은 TCP 하위 흐름을 다중 경로 TCP 연결에 일치시키는 토큰과 교환된 연결의 키를 사용하여 계산된 HMAC 값을 전달합니다. 또한 연결 키가 핸드셰이크 중에 일반 텍스트로 교환되었고 토큰도 일반 텍스트로 되어 있다는 점 외에도 HMAC 값(SHA-256 사용)은 맨 왼쪽 64비트(SYN/ACK) 또는 160비트로 잘립니다. (세 번째 ACK에서) TCP 옵션 길이가 40바이트로 제한되어 있기 때문입니다. 다중 경로 QUIC는 이러한 모든 문제를 방지합니다. 추가 유니플로의 협상 및 사용은 암호화된 메시지를 사용하여 수행되며 프레임 길이는 해당 메시지를 전달하는 패킷 크기에 의해서만 제한됩니다.

또 다른 차이점은 사용 중인 경로/유니플로의 수를 제어하는 ​​데 있습니다. 다중 경로 TCP에는 해당 TCP 핸드셰이크가 성공하면 새 하위 흐름이 있습니다. 그러나 Multipath TCP 연결이 사용할 수 있는 경로의 수를 미리 제한할 수는 없습니다. 따라서 서버가 경로 수를 제어하는 ​​유일한 방법은 반응적 접근 방식을 채택하는 것입니다. 다중 경로 QUIC에서 각 호스트는 사용하려는 전송 유니플로 수에 대한 상한선을 설정하는 동시에 피어에 제공하는 전송 유니플로 수를 제어합니다. 따라서 이 경로 관리는 사전 예방적입니다.

### A.4.  Exchanging Data over Multiple Uniflows

다중 경로 TCP의 주요 설계 결정 중 하나는 모든 하위 흐름이 네트워크 간섭을 처리하기 위해 일반 TCP 연결처럼 작동해야 한다는 것입니다. 각 다중 경로 TCP 하위 흐름은 순서대로 데이터 전달을 유지해야 하며 이를 위해 TCP 시퀀스 번호를 지정해야 합니다. 다중 경로 재정렬을 처리하려면 다중 경로 TCP 수준에서 추가 데이터 시퀀스 번호가 필요합니다. 다중 경로 QUIC에서 데이터가 전송되는 유니플로는 패킷 수준 정보입니다. 이것은 프레임을 운반하는 패킷의 유니플로에 관계없이 프레임을 보낼 수 있음을 의미합니다. 또한 (STREAM) 데이터 오프셋은 프레임 수준 정보이기 때문에 유니플로 간의 재정렬에 대처하기 위해 추가 시퀀스 번호를 정의할 필요가 없습니다.

이 신호 오버헤드 외에도 다중 경로 TCP는 이 승인 제약으로 인해 성능 문제에 직면합니다. 다음 시나리오를 고려하십시오. 일부 데이터는 먼저 손실 경로 A를 통해 전송되어 피어가 수신하지 않습니다. 발신자는 작업 경로 B를 통해 동일한 데이터를 (성공적으로) 재전송할 수 있습니다(및 해당 승인을 얻음). 그러나 보낸 사람은 초기 손실 데이터가 해당 경로에서 배달되지 않는 한(TCP 동작 제약으로 인해) 경로 A에서 새 데이터를 보낼 수 없습니다. 다중 경로 QUIC 유니플로가 단일 경로 QUIC처럼 동작해야 한다는 규칙이 없기 때문에 이러한 전송 오버헤드는 다중 경로 QUIC에 존재하지 않습니다.

"다중 경로 TCP 하위 흐름은 일반 TCP 연결처럼 작동해야 함"의 또 다른 결과는 승인이 데이터에서 사용되는 것과 동일한 네트워크 경로에 있어야 한다는 것입니다. 확인 전략에 대한 이러한 제약은 프레임이 패킷과 독립적이므로 다중 경로 QUIC에는 존재하지 않으며 거의 ​​시행할 수 없습니다. 다중 경로 QUIC는 대기 시간이 긴 경로의 이점을 더 잘 활용하고 단방향 네트워크를 사용할 수 있습니다.

### A.5.  Advertising Host's Addresses

다중 경로 TCP를 사용하면 호스트가 ADD_ADDR TCP 옵션을 사용하여 로컬 IP 주소를 피어와 통신할 수 있습니다. 유사하게, REMOVE_ADDR TCP 옵션은 로컬 주소의 손실을 피어에게 알리는 데 사용됩니다. 그러나 이러한 옵션은 일반 텍스트로 전달되어 호스트의 IP 주소가 네트워크에 누출될 수 있으므로 사용 시 보안 문제가 발생할 수 있습니다. 이 모든 정보는 프레임에서 암호화되므로 이 보안 문제는 Multipath QUIC에 영향을 미치지 않습니다.

또 다른 점은 주소 광고의 신뢰성과 관련이 있습니다. 다중 경로 TCP에서 ADD_ADDR 및 REMOVE_ADDR 옵션은 불안정하게 전송됩니다. 즉, 수신에 대한 승인 메커니즘이 없습니다. 다중 경로 TCP[RFC8684]는 ADD_ADDR에 "에코" 메커니즘을 제공하지만 REMOVE_ADDR에 해당하는 메커니즘은 없습니다. 다중 경로 QUIC에서 ADD_ADDRESS 및 REMOVE_ADDRESS 프레임은 확인을 유도하므로 신뢰할 수 있습니다.

### A.6.  Congestion Control

다중경로 TCP는 [RFC6356]에 명시된 LIA 혼잡 제어 방식을 사용합니다. 이 방식은 Multipath QUIC에 즉시 적용할 수 있습니다. [OLIA]와 같은 다중 경로 TCP에 대해 다른 결합된 혼잡 제어 방식이 제안되었습니다.

## Appendix B.  Change Log

### B.1.  Since draft-deconinck-quic-multipath-06

   *  Link with recent publication

### B.2.  Since draft-deconinck-quic-multipath-05

   *  Summarize frame types in a table

   *  Update frame format to structure style

   *  Update text to match draft-ietf-quic-transport-32

   *  Link to scheduling companion draft

   *  Remove dangling to-dos

### B.3.  Since draft-deconinck-quic-multipath-04

   *  Mostly editorial and reference fixes

### B.4.  Since draft-deconinck-quic-multipath-03

   *  Clarify the notion of asymmetric paths by introducing uniflows

   *  Remove the PATH_UPDATE frame

   *  Rename PATHS frame to UNIFLOWS frame and adapt its content

   *  Add a sequence number to frames involving Address ID events (#4)

   *  Disallow Zero-length connection ID (#2)

   *  Correctly handle nonce computation (thanks to Florentin Rochet)

   *  Focus on the core concepts of multipath and delegate algorithms to
      companion drafts

   *  Updated text to match draft-ietf-quic-transport-27

### B.5.  Since draft-deconinck-quic-multipath-02

   *  Consider asymmetric paths

### B.6.  Since draft-deconinck-quic-multipath-01

   *  Include path policies considerations

   *  Add practical considerations thanks to Mohamed Boucadair inputs

   *  Adapt the RETIRE_CONNECTION_ID frame

   *  Updated text to match draft-ietf-quic-transport-18

### B.7.  Since draft-deconinck-quic-multipath-00

   *  Comply with asymmetric Connection IDs

   *  Add max_paths transport parameter to negotiate initial number of
      active paths

   *  Path ID as a regular varint

   *  Remove max_path_id transport parameter

   *  Updated text to match draft-ietf-quic-transport-14

### B.8.  Since draft-deconinck-multipath-quic-00

   *  Added PATH_UPDATE frame

   *  Added MAX_PATHS frame

   *  No more packet header change

   *  Implicit Path ID notification using Connection ID and
      NEW_CONNECTION_ID frames

   *  Variable-length encoding for Path ID

   *  Updated text to match draft-ietf-quic-transport-10

   *  Fixed various typos


## Authors' Addresses

   Quentin De Coninck
   UCLouvain

   Email: quentin.deconinck@uclouvain.be


   Olivier Bonaventure
   UCLouvain

   Email: olivier.bonaventure@uclouvain.be

