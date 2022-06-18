---
layout : default
title : Connection Migration in QUIC korean
parent: QUIC
grand_parent : OpenSource
---

# Connection Migration in QUIC
[original](https://tools.ietf.org/id/draft-tan-quic-connection-migration-00.html)


## Abstract
이 문서는 QUIC connection migration을 탐색 및 분류하고 가능한 개선 사항을 제안합니다.

### Status of This Memo
이 인터넷 초안은 BCP 78 및 BCP 79의 규정을 완전히 준수하여 제출되었습니다.

인터넷 초안은 IETF(Internet Engineering Task Force)의 작업 문서입니다. 다른 그룹은 작업 문서를 인터넷 초안으로 배포할 수도 있습니다. 현재 인터넷 초안 목록은 https://datatracker.ietf.org/drafts/current/에 있습니다.

인터넷 초안은 최대 6개월 동안 유효한 초안 문서이며 언제든지 다른 문서로 업데이트, 대체 또는 폐기될 수 있습니다. Internet-Drafts를 참고 자료로 사용하거나 "진행 중인 작업"이 아닌 다른 방식으로 인용하는 것은 부적절합니다.

이 인터넷 초안은 2021년 4월 18일에 만료됩니다.

Copyright Notice
Copyright (c) 2020 IETF Trust and the persons identified as the document authors. All rights reserved.

이 문서는 BCP 78 및 이 문서의 발행일에 유효한 IETF 문서와 관련된 IETF Trust의 법적 조항(https://trustee.ietf.org/license-info)의 적용을 받습니다. 이 문서와 관련된 귀하의 권리 및 제한 사항을 설명하므로 이 문서를 주의 깊게 검토하십시오. 이 문서에서 추출한 코드 구성요소는 Trust Legal 조항의 섹션 4.e에 설명된 대로 Simplified BSD License 텍스트를 포함해야 하며 Simplified BSD License에 설명된 대로 보증 없이 제공됩니다.

## 1. Introduction
이 문서에서는 QUIC connection migration을 자세히 살펴보고 분류합니다.

## 2. Terminology

### 2.1. Terms Used In This Document
이 문서는 [QUIC-TRANSPORT]에 설정된 용어와 개념을 사용합니다. 독자는 위의 문서와 거기에 정의된 용어에 익숙하다고 가정합니다.

The following terms used in this document:

|Name|Description|
|---|---|
|Endpoint|QUIC 패킷을 생성, 수신 및 처리하여 QUIC 연결에 참여할 수 있는 entity입니다. QUIC에는 클라이언트와 서버의 두 가지 유형의 endpoint만 있습니다.|
|QUIC Client|QUIC 연결을 시작하는 endpoint입니다.|
|QUIC Server|QUIC 연결을 수락하는 endpoint입니다.|
|Connection ID|endpoint에서 QUIC 연결을 식별하는 데 사용되는 식별자입니다. 각 endpoint는 endpoint으로 보내는 패킷에 포함할 peer에 대해 하나 이상의 연결 ID를 선택합니다. 이 값은 peer에게 불투명합니다.|
|Connection migration|QUIC endpoint는 연결 ID를 사용하여 endpoint 주소의 변경 사항을 유지하기 위해 연결합니다.|
|Connection migration initiator|connection migration을 시작한 endpoint입니다.|
|Connection migration responder|connection migration 개시자와 통신하는 peer입니다.|
|Path validation|endpoint는 특정 로컬 주소와 특정 peer 주소 간의 연결 가능성을 테스트합니다.|
|Probing frame|PATH_CHALLENGE, PATH_RESPONSE, NEW_CONNECTION_ID 및 PADDING 프레임과 같은 탐색 기능이 있는 프레임입니다.|
|Non-probing frame|probing 프레임을 제외한 프레임.|
|Probing packet|probing 프레임만 포함하는 패킷입니다.|
|Non-probing packet|다른 non-probing 프레임을 포함하는 패킷입니다.|
|Application|QUIC를 사용하여 데이터를 보내고 받는 entity입니다.|

### 2.2. Abbreviations
The following abbreviations used in this document:

|Name|Description|
|---|---|
|CM|Connection Migration.|
|ACM|Active Connection Migration.|
|PCM|Passive Connection Migration.|
|VCM|Vertical Connection Migration.|
|HCM|Horizontal Connection Migration.|
|CCM|Client Connection Migration.|
|SCM|Server Connection Migration.|
|SPCM|Single-path Connection Migration.|
|MPCM|Multi-path Connection Migration.|

## 3. Connection Migration
QUIC 연결은 단일 네트워크 경로에 엄격하게 바인딩되지 않습니다. connection migration은 QUIC의 중요한 기능 중 하나입니다.

connection migration을 시작하는 QUIC endpoint를 connection migration 개시자(CM Initiator)라고 합니다. peer는 connection migration 응답자(CM 응답자)입니다.

connection migration은 연결 ID를 사용하여 연결을 새 네트워크 경로로 전송할 수 있도록 합니다. 연결 ID를 사용하면 그림 1과 같이 endpoint가 새 네트워크로 마이그레이션되는 경우와 같이 endpoint 주소(IP 주소 및 포트)가 변경되어도 연결을 유지할 수 있습니다.

연결 ID는 0이 아니어야 합니다.

```
       (Before connection migration)
       +--------------+   Non-probing Packet   +--------------+
       | CM Initiator |  ------------------->  |              |
       |(Source IP: 1)|  <-------------------  |              |
       +--------------+   Non-probing Packet   |              |
              |                                |              |
              |                                |              |
              |                                |              |
              v             Probing Packet     |              |
       +--------------+    (PATH_CHALLENGE)    |              |
       |              |  ------------------->  |              |
       |              |  <-------------------  |              |
       |              |     Probing Packet     |              |
       |              |     (PATH_RESPONSE)    |              |
       |              |                        | CM Responder |
       |              |     Probing Packet     |              |
       |              |    (PATH_CHALLENGE)    |              |
       | CM Initiator |  <-------------------  |              |
       |(Source IP: 2)|  ------------------->  |              |
       |              |     Probing Packet     |              |
       |              |     (PATH_RESPONSE)    |              |
       |              |                        |              |
       |              |   Non-probing Packet   |              |
       |              |  ------------------->  |              |
       |              |  <-------------------  |              |
       |              |   Non-probing Packet   |              |
       +--------------+                        +--------------+
       (After connection migration)
Figure 1: The connection migration
```

### 3.1. Initiating Connection Migration
CM 개시자는 해당 주소에서 non-probing 프레임이 포함된 패킷을 전송하여 새 로컬 주소로 연결을 마이그레이션할 수 있습니다.

### 3.2. Path Verification
endpoint는 경로 확인을 초기화하기 위해 난수를 포함하는 PATH_CHALLENGE 프레임을 보냅니다. 각 endpoint는 연결 설정 중에 peer 주소의 유효성을 검사합니다. 따라서 마이그레이션하는 endpoint는 peer가 peer의 현재 주소에서 수신할 의향이 있음을 알고 peer에 보낼 수 있습니다.

Endpoint가 경로를 확인하지 않는 한 대부분의 connection migration에 대해 경로 확인을 수행해야 합니다. 이유는 경로의 도달 가능성 및 보안을 확인하기 위한 것입니다. 그러나 일부 특별한 경우에는 connection migration 개시자가 peer의 경로 확인 없이 직접 데이터 패킷을 보낼 수 있어야 합니다. 조건은 connection migration 응답자가 수신한 패킷이 스푸핑된 소스 주소를 전달하지 않도록 하는 것입니다.

endpoint는 PATH_CHALLENGE 프레임(유형=0x1a)을 사용하여 peer에 대한 연결 가능성을 확인하고 connection migration 중 경로 유효성 검사를 수행할 수 있습니다.

PATH_CHALLENGE 프레임은 그림 2와 같이 형식이 지정됩니다.

PATH_RESPONSE 프레임(유형=0x1b)은 PATH_CHALLENGE 프레임에 대한 응답으로 전송됩니다.

PATH_RESPONSE 프레임은 그림 3과 같이 형식이 지정됩니다.

```
  +-+-+-+-+-+-+-+-+
  |      0x1a     |
  +-+-+-+-+-+-+-+-++-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
  |                           Data (64)                          |
  +-+-+-+-+-+-+-+-++-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
Figure 2: PATH_CHALLENGE Frame Format
```

```
  +-+-+-+-+-+-+-+-+
  |      0x1b     |
  +-+-+-+-+-+-+-+-++-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
  |                           Data (64)                          |
  +-+-+-+-+-+-+-+-++-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
Figure 3: PATH_RESPONSE Frame Format
```

### 3.3. Completing Connection Migration
Endpoint는 peer로부터 non-probing 프레임의 새 peer 주소가 포함된 패킷을 수신하여 connection migration 개시자가 이 주소로 마이그레이션되었음을 나타냅니다.

연결이 마이그레이션된 후 endpoint는 혼잡 제어 매개변수를 재설정해야 합니다.

### 3.4. Special Cases
connection migration을 허용하지 않는 몇 가지 상황:

   1. endpoint는 [QUIC-TLS]의 섹션 4.1.2에 정의된 대로 handshake가 확인되기 전에 connection migration을 시작해서는 안 됩니다(MUST NOT).
   1. peer가 disable_active_migration 전송 매개변수를 보낸 경우 endpoint는 connection migration을 시작해서는 안 됩니다(MUST NOT).

[QUIC-TRANSPORT] 섹션 9.1에서는 connection migration 과정에서 발생할 수 있는 몇 가지 잠재적인 보안 문제와 솔루션을 분석합니다.

## 4. Classification of Connection Migration
비즈니스 시나리오에 따라 connection migration은 다양한 구현 기술을 포함하는 여러 유형으로 나눌 수 있습니다.

### 4.1. Active Connection Migration and Passive Connection Migration
connection migration은 개시자에 따라 ACM(Active Connection Migration)과 PCM(Passive Connection Migration)으로 나눌 수 있습니다.

네트워크 모델의 관점에서 ACM은 응용 계층에서 시작된 connection migration으로 간주할 수 있으며 PCM은 IP 계층 또는 하위 계층에서 시작된 connection migration으로 볼 수 있습니다.

ACM은 사용 가능한 모든 네트워크 연결을 QUIC에 제공하기 위해 IP 계층이 필요합니다. 애플리케이션 계층 또는 QUIC는 더 나은 사용자 경험, 보안 및 경제성을 달성하기 위해 다양한 네트워크 연결의 측정 결과에 따라 효과적인 connection migration 전략을 설계합니다. 사용 가능한 마이그레이션 전략에는 임계값 기반(threshold-driven) 마이그레이션, 폴링(polling) 마이그레이션, 임의(random) 마이그레이션 등이 포함됩니다.

PCM의 목적은 모바일 또는 약한 네트워크 환경에서 QUIC 클라이언트와 QUIC 서버 간의 연결을 유지하는 것입니다. QUIC는 종종 PCM에 적응해야 합니다. 또한 NAT/NAPT는 QUIC endpoint가 강제 connection migration을 수행하도록 합니다.

### 4.2. Vertical Connection Migration and Horizontal Connection Migration
QUIC 단말이나 액세스 네트워크의 이동으로 인해 QUIC는 다중 네트워크 액세스 환경에서 connection migration을 지원해야 합니다. 따라서 이러한 상황에서의 connection migration은 VCM(수직 connection migration)과 HCM(수평 connection migration)으로 나눌 수 있습니다.

VCM은 이기종 네트워크 사이에서 발생하고 HCM은 동종 네트워크에서 발생합니다. 시간적, 공간적 변화로 인해 VCM과 HCM은 교대로 또는 동시에 발생할 수 있습니다. HCM은 동종 네트워크에서 발생하기 때문에 마이그레이션 전 네트워크는 마이그레이션 후 네트워크와 약간의 유사점이 있을 수 있습니다(MAY). 이는 QUIC가 기록 정보를 바탕으로 연결을 더 잘 유지하도록 안내합니다.

### 4.3. Client Connection Migration and Server Connection Migration
대부분의 경우 QUIC 서버는 고정되어 있습니다. QUIC connection migration은 QUIC 클라이언트 측에서 발생합니다. 그러나 P2P 네트워크, 모바일 서버 및 데이터 센터 4계층 로드 밸런싱과 같은 시나리오에서 QUIC 통신의 두 endpoint 모두 connection migration을 겪을 수 있습니다(MAY).

connection migration은 위치에 따라 CCM(client connection migration)과 SCM(server connection migration)으로 구분됩니다.

[QUIC-TRANSPORT] 섹션 9에서는 QUIC 클라이언트만 connection migration을 능동적으로 시작할 수 있다고 규정합니다. 클라이언트는 알 수 없는 서버의 패킷을 버려야 합니다(MUST). [QUIC-TRANSPORT] 섹션 9.6은 클라이언트가 처음에 여러 서버에서 공유하는 주소에 연결하지만 연결 안정성을 보장하기 위해 유니캐스트 주소를 사용하는 것을 선호하는 장면을 설명합니다. 그래서 우리는 클라이언트와 서버가 같아야 하고 SCM이 필요하다고 믿습니다.

### 4.4. Single-path Connection Migration and Multi-path Connection Migration
connection migration 시 클라이언트와 서버 간에 유지되는 연결 경로의 수에 따라 connection migration은 SPCM(single-path connection migration)과 MPCM(multi-path connection migration)으로 나눌 수 있습니다.

SPCM은 connection migration의 간단하고 일반적인 형태입니다. 그러나 미래에는 이기종 융합 네트워크의 지속적인 발전으로 엔드포인트가 다중 경로 전송을 유지하면서 connection migration을 수행하는 것도 가능합니다. MPCM과 MPQUIC[QUIC-MPQUIC] 간에 차이가 있다는 점은 주목할 가치가 있습니다. MPCM은 MPQUIC 전송 중에 발생할 수 있는 이벤트입니다. 따라서 MPCM은 SPCM보다 결정 변수가 더 많습니다.

## 5. Connection Migration Strategy
섹션 4의 connection migration 분류 외에도 QUIC는 전략에서 connection migration을 고려할 수 있습니다.

### 5.1. Failover Mode
장애 조치 모드에서 네트워킹 스택은 연결 또는 링크의 품질을 모니터링하고 정상적으로 작동하지 않을 때 QUIC 연결이 대체 경로로 마이그레이션됩니다.

QUIC는 새 경로로 마이그레이션하기 전에 패킷을 교환할 필요가 없으므로 이 마이그레이션이 원활합니다. 그러나 새 경로가 검증되지 않은 경우 작동할 것이라는 확신이 없습니다. 이 모드에서는 QUIC 연결이 일시적으로 중단될 수 있습니다.

### 5.2. Standby Mode
대기 모드에서 네트워킹 스택은 기본 경로에서 연결을 설정한 직후에 대체 경로를 설정하고 검증합니다. 링크나 연결이 나빠지면 QUIC는 대체 경로로 전환할 수 있습니다.

이는 QUIC 클라이언트와 QUIC 서버 사이에 둘 이상의 논리적 경로가 있음을 의미합니다. 이 모드에서 QUIC는 항상 이러한 경로 중 하나만 통신에 사용합니다.

### 5.3. Aggregation Mode
Aggregation 모드에서 QUIC 클라이언트(또는 QUIC 서버)는 MPQUIC [QUIC-MPQUIC] 기술을 사용하여 네트워크 처리량을 높일 수 있습니다. 이는 여러 소스 IP가 동일한 연결 ID를 공유함을 의미합니다.

### 5.4. Load Balance Mode
[QUIC-TRANSPORT] 세부 로드 밸런싱 모드의 섹션 9.6.

서버는 클라이언트가 네트워크 연결에 가장 적합한 주소를 선택할 수 있도록 두 개 이상의 주소 중 선호하는 주소를 전달할 수 있습니다(MAY). 클라이언트는 서버에서 제공한 두 개 이상의 주소 중 하나를 선택하고 경로 유효성 검사를 시작해야 합니다(SHOULD). 경로 유효성 검사가 성공하는 즉시 클라이언트는 새 연결 ID를 사용하여 새 서버 주소로 미래의 모든 패킷을 보내기 시작하고 이전 서버 주소의 사용을 중단해야 합니다(SHOULD).

## 6. Trigger Condition for Connection Migration
PCM을 제외한 다른 connection migration은 RTT, 패킷 손실률(packet loss rate), 시간 초과(timeout), ECN 또는 애플리케이션 신호에 의해 트리거될 수 있습니다.

## 7. Security Considerations
TBD.

## 8. IANA Considerations
TBD.

## 9. Acknowledgements
None.

## 10. Normative references
- [QUIC-MPQUIC]
Coninck, Q. and O. Bonaventure, "Multipath Extensions for QUIC (MP-QUIC)", August 2020, <https://tools.ietf.org/html/draft-deconinck-quic-multipath-05>.
- [QUIC-TLS]
Thomson, M. and S. Turner, "Using TLS to Secure QUIC", September 2020, <https://tools.ietf.org/html/draft-ietf-quic-tls-31>.
- [QUIC-TRANSPORT]
Iyengar, J. and M. Thomson, "QUIC: A UDP-Based Multiplexed and Secure Transport", September 2020, <https://tools.ietf.org/html/draft-ietf-quic-transport-31>.

## Authors' Addresses
Lizhuang Tan
Beijing Jiaotong University
Shangyuan Cun. 3.
Haidian, Beijing
100044
China
Email: lzhtan@bjtu.edu.cn

Xiaochuan Gao
China Unicom
Zhonghe Street. 1.
Daxing, Beijing
100176
China
Email: gaoxc50@chinaunicom.cn

Wei Su
Beijing Jiaotong University
Shangyuan Cun. 3.
Haidian, Beijing
100044
China
Email: wsu@bjtu.edu.cn

Na Li
CNCERT/SD
Jingshiyi Road. 40.
Shizhong, Jinan
250002
China
Email: lina@cert.org.cn

Wei Zhang
Shandong Computing Science Center
Keyuan Road. 19.
Lixia, Jinan
250014
China
Email: wzhang@qlu.edu.cn