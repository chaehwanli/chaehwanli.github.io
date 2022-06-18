---
layout : default
title : QUIC Discovery eng
parent: QUIC
grand_parent : OpenSource
---

[link](https://docs.google.com/document/d/1i4m7DbrWGgXafHxwl8SwIusY2ELUe8WX258xt2LFxPM/edit#)

QUIC 디스커버리
라이언 해밀턴 <rch@chromium.org>
2014-10-28

첫 번째 연결

Chrome이 이전에 말한 적이 없는 출처에 요청을 할 때 출처가 QUIC를 지원하는지 알지 못하므로 TCP를 통해 첫 번째 요청을 보냅니다. 서버의 응답은 Alt-Svc HTTP 응답 헤더를 통해 QUIC도 지원함을 나타냅니다. (예를 들어, 응답 "Alt-Svc: 443:quic"는 원본이 포트 443에서 QUIC를 지원한다는 것을 Chrome에 알립니다.) 이제 Chrome은 원본(구성표, 호스트, 포트)이 QUIC를 사용한다는 것을 알고 있으므로 다음 요청에 QUIC 사용을 시도할 수 있습니다. 요청이 이루어지면 Chrome은 TCP 연결 작업과 QUIC 연결 작업을 경쟁합니다. (이 작업은 연결을 설정하지만 요청을 발행하지 않습니다.) 첫 번째 요청이 TCP를 통해 나갔기 때문에 TCP 작업은 거의 즉시 경쟁에서 승리하고 두 번째 요청은 TCP를 통해 나갑니다. 어떤 후속 지점에서 QUIC 연결이 성공할 것입니다. 이러한 일이 발생하면 모든 향후 요청은 QUIC 연결을 통해 전송됩니다.

후속 연결

Chrome은 오리진이 QUIC와 QUIC의 0-RTT 핸드셰이크를 가능하게 하는 서버 구성을 지원한다는 지식을 유지합니다. Chrome이 이전에 QUIC를 사용한 출처에 요청을 하면 QUIC 작업과 함께 TCP 작업과 경쟁합니다. Chrome은 0-RTT 핸드셰이크를 수행할 수 있으므로 QUIC 작업이 즉시 승리하고 QUIC 연결에서 요청이 발행됩니다.


끊어진 연결

QUIC 핸드셰이크가 실패하는 경우(예: UDP가 차단되거나 서버가 QUIC의 호환 가능한 버전을 말하지 않는 경우) Chrome은 해당 호스트에 대해 QUIC를 깨진 것으로 표시합니다. 진행 중인 모든 요청은 TCP를 통해 다시 발행됩니다. QUIC가 호스트에 대해 중단된 것으로 표시되는 동안에는 QUIC 연결이 시도되지 않습니다. 5분이 지나면 손상된 항목이 만료됩니다. QUIC는 해당 출처에 대해 "최근 파손됨"으로 표시됩니다. 다음 요청이 오리진에 발행되면 Chrome은 TCP 작업과 QUIC 작업을 경합합니다. QUIC가 "최근에 중단"되었기 때문에 0-RTT 핸드셰이크가 비활성화됩니다. 핸드셰이크가 다시 실패하면 QUIC는 이번에도 10분 동안 해당 원점에 대해 중단된 것으로 표시되고 프로세스는 이전 기간의 두 배 동안 QUIC를 중단된 것으로 표시하는 것을 반복합니다. 핸드셰이크가 성공하면 QUIC를 통해 요청이 전송되고 QUIC는 더 이상 "최근 중단됨"으로 표시되지 않습니다.