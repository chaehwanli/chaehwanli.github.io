---
layout : default
title : Reactive Streams 번역
parent : Study
grand_parent : OpenSource
---


[Reactive Streams](https://github.com/reactive-streams/reactive-streams-jvm#readme)

# Reactive Streams #

Reactive Streams의 목적은 non-blocking backpressure로 비동기 스트림 처리를 위한 표준을 제공하는 것입니다.

최신 릴리스는 Maven Central에서 다음과 같이 사용할 수 있습니다.

```xml
<dependency>
  <groupId>org.reactivestreams</groupId>
  <artifactId>reactive-streams</artifactId>
  <version>1.0.4</version>
</dependency>
<dependency>
  <groupId>org.reactivestreams</groupId>
  <artifactId>reactive-streams-tck</artifactId>
  <version>1.0.4</version>
  <scope>test</scope>
</dependency>
```

## Goals, Design and Scope ##
데이터 스트림, 특히 볼륨이 미리 결정되지 않은 "라이브" 데이터를 처리하려면 비동기식 시스템에서 특별한 주의가 필요합니다. 가장 두드러진 문제는 빠른 데이터 소스가 스트림 대상을 압도하지 않도록 리소스 소비를 신중하게 제어해야 한다는 것입니다. 단일 시스템 내에서 협업하는 네트워크 호스트 또는 여러 CPU 코어에서 컴퓨팅 리소스의 병렬 사용을 가능하게 하려면 비동기가 필요합니다.

Reactive Streams의 주요 목표는 비동기 경계를 넘어 스트림 데이터 교환을 제어하는 ​​것입니다. 다른 스레드나 스레드 풀로 요소를 전달하는 것을 생각해보세요. 동시에 수신 측이 임의의 양의 데이터를 버퍼링하지 않도록 하는 것입니다. 다시 말해서, backpressure(역압)은 스레드 사이를 중재하는 큐가 제한될 수 있도록 하기 위해 이 모델의 필수적인 부분입니다. backpressure(역압) 신호가 동기식인 경우 비동기식 처리의 이점이 무효화되므로([Reactive Manifesto](https://www.reactivemanifesto.org/) 참조), Reactive Streams 구현의 모든 측면에서 완전히 비차단 및 비동기식 동작을 의무화하도록 주의를 기울였습니다.

이 사양의 의도는 규칙을 준수함으로써 원활하게 상호 운용할 수 있고 스트림 애플리케이션의 전체 처리 그래프에서 앞서 언급한 이점과 특성을 보존할 수 있는 많은 준수 구현의 생성을 허용하는 것입니다.

스트림 조작(transformation(변환), splitting(분할), merging(병합), etc.)의 정확한 특성은 이 사양에서 다루지 않습니다. Reactive Streams는 서로 다른 API 구성 요소 간의 데이터 스트림 중재에만 관심이 있습니다. 스트림을 결합하는 모든 기본 방법이 표현될 수 있도록 개발에 주의를 기울였습니다.

요약하면, Reactive Streams는 JVM용 스트림 지향 라이브러리의 표준 및 사양입니다.

- process a potentially unbounded number of elements(잠재적으로 무한한 수의 요소 처리)
- in sequence(순서대로),
- asynchronously passing elements between components(구성 요소 간에 비동기식으로 요소 전달),
- with mandatory non-blocking backpressure(필수 비차단 배압 포함).

Reactive Streams 사양은 다음 부분으로 구성됩니다.

API는 Reactive Streams를 구현하고 서로 다른 구현 간의 상호 운용성을 달성하기 위한 유형을 지정합니다.

TCK(Technology Compatibility Kit)는 구현의 적합성 테스트를 위한 표준 테스트 제품군입니다.

구현은 API 요구 사항을 준수하고 TCK의 테스트를 통과하는 한 사양에서 다루지 않는 추가 기능을 자유롭게 구현할 수 있습니다.

### API Components ###

API는 Reactive Stream 구현에서 제공해야 하는 다음 구성 요소로 구성됩니다.

1. Publisher
2. Subscriber
3. Subscription
4. Processor

*Publisher*는 잠재적으로 무한한 수의 시퀀스 요소를 제공하여 구독자로부터 받은 요구에 따라 게시합니다.

`Publisher.subscribe(Subscriber)`에 대한 호출에 대한 응답으로 `Subscriber`의 메서드에 대한 가능한 호출 시퀀스는 다음 프로토콜에 의해 제공됩니다.

```
onSubscribe onNext* (onError | onComplete)?
```

즉, 'onSubscribe'는 항상 신호를 받고, 그 다음에는 ('구독자'가 요청한 대로) 무한한 수의 'onNext' 신호가 오고, 실패가 있으면 'onError' 신호가 오고, 더 이상 사용할 수 있는 요소가 없으면 'onComplete' 신호가 옵니다. '구독'은 취소되지 않습니다.

#### NOTES

- 아래 사양은 https://www.ietf.org/rfc/rfc2119.txt 에서 대문자로 된 바인딩 단어를 사용합니다.

### Glossary

| Term                      | Definition                                                                                             |
| ------------------------- | ------------------------------------------------------------------------------------------------------ |
| <a name="term_signal">Signal</a> | 명사: `onSubscribe`, `onNext`, `onComplete`, `onError`, `request(n)` 또는 `cancel` 방법 중 하나입니다. 동사로: 신호 호출/호출. |
| <a name="term_demand">Demand</a> |게시자가 아직 제공(이행)하지 않은 구독자가 요청한 요소의 명사로 집계된 수입니다. 동사로서 더 많은 요소를 '요청'하는 행위. |
| <a name="term_sync">Synchronous(ly)</a> | 호출 스레드에서 실행. |
| <a name="term_return_normally">Return normally</a> | 호출자에게 선언된 유형의 값만 반환합니다. '구독자'에게 실패를 알리는 유일한 합법적인 방법은 'onError' 메서드를 사용하는 것입니다.|
| <a name="term_responsivity">Responsivity</a> |준비/대응 능력. 이 문서에서는 서로 다른 구성 요소가 서로의 응답 능력을 손상시키지 않아야 함을 나타내는 데 사용됩니다.|
| <a name="term_non-obstructing">Non-obstructing</a> | 호출 스레드에서 가능한 한 빨리 실행되는 메서드를 설명하는 품질입니다. 이는 예를 들어 호출자의 실행 스레드를 지연시키는 과도한 계산 및 기타 작업을 피한다는 것을 의미합니다.|
| <a name="term_terminal_state">Terminal state</a> | 게시자의 경우: `onComplete` 또는 `onError`가 신호를 받았을 때. 구독자의 경우: `onComplete` 또는 `onError`가 수신된 경우.|
| <a name="term_nop">NOP</a> | 호출 스레드에 감지할 수 있는 영향이 없고 여러 번 안전하게 호출될 수 있는 실행입니다.|
| <a name="term_serially">Serial(ly)</a> |[Signal](#term_signal) 컨텍스트에서 겹치지 않습니다. JVM 컨텍스트에서 개체의 메서드 호출은 해당 호출 간에 발생 이전 관계가 있는 경우에만 직렬입니다(호출이 겹치지 않음을 의미함). 호출이 비동기식으로 수행되는 경우 발생 이전 관계를 설정하기 위한 조정은 atomic, 모니터 또는 잠금과 같은 기술을 사용하여 구현되어야 합니다.|
| <a name="term_thread-safe">Thread-safe</a> | 프로그램 정확성을 보장하기 위해 외부 동기화를 요구하지 않고 동기식 또는 비동기식으로 안전하게 호출할 수 있습니다. |

### SPECIFICATION

#### 1. Publisher ([Code](https://github.com/reactive-streams/reactive-streams-jvm/blob/v1.0.4/api/src/main/java/org/reactivestreams/Publisher.java))

```java
public interface Publisher<T> {
    public void subscribe(Subscriber<? super T> s);
}
````

| ID                        | Rule                                                                                                   |
| ------------------------- | ------------------------------------------------------------------------------------------------------ |
| <a name="1.1">1</a>       | '게시자'가 '구독자'에게 보내는 'onNext'의 총 수는 항상 해당 '구독자'의 '구독'이 요청한 총 요소 수보다 작거나 같아야 합니다(MUST). |
| [:bulb:](#1.1 "1.1 explained") | *이 규칙의 목적은 게시자가 구독자가 요청한 것보다 더 많은 요소를 알릴 수 없음을 명확히 하는 것입니다. 이 규칙에는 암시적이지만 중요한 결과가 있습니다. 수요가 수신된 후에만 충족될 수 있기 때문에 요청 요소와 수신 요소 사이에 '발생 이전' 관계가 있습니다.*|
| <a name="1.2">2</a>       |'게시자'는 요청된 것보다 적은 'onNext' 신호를 보내고 'onComplete' 또는 'onError'를 호출하여 '구독'을 종료할 수 있습니다(MAY).|
| [:bulb:](#1.2 "1.2 explained") |*이 규칙의 목적은 게시자가 요청된 요소 수를 생성할 수 있다고 보장할 수 없음을 명확히 하는 것입니다. 단순히 모든 것을 생산하지 못할 수도 있습니다. 실패한 상태일 수 있습니다. 비어 있거나 이미 완료되었을 수 있습니다.*|
| <a name="1.3">3</a>       |'구독자'에게 시그널링된 'onSubscribe', 'onNext', 'onError' 및 'onComplete'는 반드시 [직렬](#term_serially)로 시그널링되어야 합니다.|
| [:bulb:](#1.3 "1.3 explained") |*이 규칙의 목적은 각 신호 사이에 발생 전 관계가 설정된 경우에만 신호의 신호(다중 스레드 포함)를 허용하는 것입니다.*|
| <a name="1.4">4</a>       |'게시자'가 실패하면 'onError' 신호를 보내야 합니다(MUST).|
| [:bulb:](#1.4 "1.4 explained") |*이 규칙의 목적은 게시자가 진행할 수 없음을 감지한 경우 구독자에게 알릴 책임이 있음을 명확히 하기 위한 것입니다. 구독자는 리소스를 정리하거나 게시자의 실패를 처리할 기회가 제공되어야 합니다.*|
| <a name="1.5">5</a>       |'Publisher'가 성공적으로 종료되면(유한 스트림) 'onComplete' 신호를 보내야 합니다(MUST).|
| [:bulb:](#1.5 "1.5 explained") |*이 규칙의 목적은 게시자가 구독자에게 [터미널 상태](#term_terminal_state)에 도달했음을 알릴 책임이 있음을 명확히 하는 것입니다. 그러면 구독자는 이 정보에 따라 조치를 취할 수 있습니다. 자원 등을 정리합니다.*|
| <a name="1.6">6</a>       | '게시자'가 '구독자'에 대해 'onError' 또는 'onComplete' 신호를 보내는 경우 해당 '구독자'의 '구독'은 취소된 것으로 간주되어야 합니다(MUST). |
| [:bulb:](#1.6 "1.6 explained") | *이 규칙의 목적은 구독이 취소되었거나 게시자가 onError 또는 onComplete 신호를 보냈는지 여부에 관계없이 구독이 동일하게 취급되도록 하는 것입니다.* |
| <a name="1.7">7</a>       | 일단 [터미널 상태](#term_terminal_state)가 신호를 받으면(`onError`, `onComplete`) 더 이상 신호가 발생하지 않아야 합니다. |
| [:bulb:](#1.7 "1.7 explained") | *이 규칙의 목적은 onError 및 onComplete가 게시자와 구독자 쌍 간의 상호 작용의 최종 상태인지 확인하는 것입니다.* |
| <a name="1.8">8</a>       | '구독'이 취소되면 '구독자'는 결국 신호를 중지해야 합니다. |
| [:bulb:](#1.8 "1.8 explained") | *이 규칙의 목적은 Subscription.cancel()이 호출되었을 때 게시자가 구독 취소 요청을 게시자가 존중하도록 하는 것입니다. **결국**의 이유는 신호가 비동기식이므로 전파 지연이 있을 수 있기 때문입니다.* |
| <a name="1.9">9</a>       | 'Publisher.subscribe'는 제공된 '구독자'가 'null'인 경우를 제외하고 해당 '구독자'에게 다른 신호를 보내기 전에 제공된 '구독자'에서 'onSubscribe'를 호출해야 하고 [정상적으로 반환](#term_return_normally)해야 합니다. 호출자에게 `java.lang.NullPointerException`을 던져야 하는 경우(MUST), 다른 모든 상황에서 실패를 알리는(또는 `구독자`를 거부하는) 유일한 법적 방법은 `onError`를 호출하는 것입니다(`onSubscribe` 호출 후). |
| [:bulb:](#1.9 "1.9 explained") | *이 규칙의 목적은 'onSubscribe'가 항상 다른 신호보다 먼저 신호를 보내도록 하여 신호가 수신될 때 구독자가 초기화 논리를 실행할 수 있도록 하는 것입니다. 또한 'onSubscribe'는 최대 한 번만 호출되어야 합니다([2.12](#2.12) 참조]. 제공된 `Subscriber`가 `null`이면 호출자에게만 신호를 보낼 수 있는 곳이 없으므로 `java.lang.NullPointerException`이 발생해야 합니다. 가능한 상황의 예: 상태 저장 게시자는 압도되거나 제한된 수의 기본 리소스에 의해 제한되거나 소진되거나 [터미널 상태](#term_terminal_state)에 있을 수 있습니다.* |
| <a name="1.10">10</a>     | `Publisher.subscribe`는 원하는 만큼 호출될 수 있지만 매번 다른 `Subscriber`와 함께 있어야 합니다[[2.12](#2.12) 참조]. |
| [:bulb:](#1.10 "1.10 explained") | *이 규칙의 목적은 `subscribe`의 호출자가 일반 게시자와 일반 구독자가 여러 번 연결되는 것을 지원한다고 가정할 수 없다는 것을 인식하도록 하는 것입니다. 또한 '구독'의 의미 체계는 호출 횟수에 관계없이 유지되어야 합니다.* |
| <a name="1.11">11</a>     | '게시자'는 여러 '구독자'를 지원할 수 있으며 각 '구독'이 유니캐스트인지 멀티캐스트인지 결정합니다. |
| [:bulb:](#1.11 "1.11 explained") | *이 규칙의 목적은 게시자 구현에 지원하는 구독자 수(있는 경우)와 요소 배포 방법을 결정할 수 있는 유연성을 제공하는 것입니다.* |

#### 2. Subscriber ([Code](https://github.com/reactive-streams/reactive-streams-jvm/blob/v1.0.4/api/src/main/java/org/reactivestreams/Subscriber.java))

```java
public interface Subscriber<T> {
    public void onSubscribe(Subscription s);
    public void onNext(T t);
    public void onError(Throwable t);
    public void onComplete();
}
````

| ID                        | Rule                                                                                                   |
| ------------------------- | ------------------------------------------------------------------------------------------------------ |
| <a name="2.1">1</a>       | '구독자'는 'onNext' 신호를 수신하기 위해 'Subscription.request(long n)'를 통해 요구 신호를 보내야 합니다(MUST). |
| [:bulb:](#2.1 "2.1 explained") | *이 규칙의 목적은 언제 얼마나 많은 요소를 수신할 수 있고 수신할 의사가 있는지를 결정하는 것이 구독자의 책임임을 확립하는 것입니다. 재진입 구독 방법으로 인한 신호 재정렬을 방지하려면 동기 구독자 구현이 신호 처리의 맨 끝에서 구독 방법을 호출하는 것이 좋습니다. 한 번에 하나의 요소만 요청하면 본질적으로 비효율적인 "정지 후 대기" 프로토콜이 발생하므로 가입자는 처리할 수 있는 것의 상한선을 요청하는 것이 좋습니다.* |
| <a name="2.2">2</a>       | `Subscriber`가 신호 처리가 `Publisher`의 책임에 부정적인 영향을 미칠 것이라고 의심하는 경우 신호를 비동기식으로 발송하는 것이 좋습니다. |
| [:bulb:](#2.2 "2.2 explained") | *이 규칙의 의도는 구독자가 실행 관점에서 게시자의 진행을 [방해하지 않아야](#term_non-obstructing) 하는 것입니다. 즉, 구독자는 게시자가 CPU 주기를 수신하지 못하도록 굶겨서는 안 됩니다.* |
| <a name="2.3">3</a>       | 'Subscriber.onComplete()' 및 'Subscriber.onError(Throwable t)'는 '구독' 또는 '게시자'에 대한 메서드를 호출하지 않아야 합니다(MUST NOT). |
| [:bulb:](#2.3 "2.3 explained") | *이 규칙의 목적은 완료 신호를 처리하는 동안 게시자, 구독 및 구독자 간의 주기 및 경쟁 조건을 방지하는 것입니다.* |
| <a name="2.4">4</a>       | `Subscriber.onComplete()` 및 `Subscriber.onError(Throwable t)`는 신호를 수신한 후 구독이 취소된 것으로 간주해야 합니다(MUST). |
| [:bulb:](#2.4 "2.4 explained") | *이 규칙의 목적은 구독자가 게시자의 [터미널 상태](#term_terminal_state) 신호를 준수하는지 확인하는 것입니다. onComplete 또는 onError 신호가 수신된 후에는 구독이 더 이상 유효하지 않습니다.* |
| <a name="2.5">5</a>       | '구독자'는 이미 활성 '구독'이 있는 경우 'onSubscribe' 신호 이후에 주어진 '구독'에 대해 'Subscription.cancel()'을 호출해야 합니다(MUST). |
| [:bulb:](#2.5 "2.5 explained") | *이 규칙의 목적은 둘 이상의 별도 게시자가 동일한 구독자와 상호 작용을 시도하는 것을 방지하는 것입니다. 이 규칙을 적용하면 추가 구독이 취소되므로 리소스 누수가 방지됩니다. 이 규칙을 준수하지 않으면 게시자 규칙 1 등을 위반할 수 있습니다. 이러한 위반은 진단하기 어려운 버그로 이어질 수 있습니다.* |
| <a name="2.6">6</a>       | '구독자'는 '구독'이 더 이상 필요하지 않은 경우 'Subscription.cancel()'을 호출해야 합니다(MUST). |
| [:bulb:](#2.6 "2.6 explained") | *이 규칙의 목적은 구독자가 구독이 더 이상 필요하지 않을 때 구독을 버릴 수 없도록 하고 해당 구독이 보유한 리소스를 안전하고 시기 적절하게 회수할 수 있도록 '취소'를 호출해야 함을 설정하는 것입니다. 예를 들면 특정 요소에만 관심이 있는 구독자가 구독을 취소하여 게시자에게 완료 신호를 보내는 경우입니다.* |
| <a name="2.7">7</a>       | 구독자는 구독 요청 및 취소 메서드에 대한 모든 호출이 [직렬로](#term_serialally) 수행되도록 해야 합니다. |
| [:bulb:](#2.7 "2.7 explained") | *이 규칙의 목적은 각 호출 간에 [serial](#term_serially) 관계가 설정된 경우에만 요청 및 취소 메서드(여러 스레드 포함) 호출을 허용하는 것입니다.* |
| <a name="2.8">8</a>       | '구독자'는 요청된 요소가 보류 중인 경우 'Subscription.cancel()'을 호출한 후 하나 이상의 'onNext' 신호를 수신할 준비가 되어 있어야 합니다[[3.12](#3.12) 참조]. 'Subscription.cancel()'은 기본 청소 작업을 즉시 수행한다고 보장하지 않습니다. |
| [:bulb:](#2.8 "2.8 explained") | *이 규칙의 목적은 '취소'를 호출하는 것과 게시자가 해당 취소를 관찰하는 사이에 지연이 있을 수 있음을 강조하는 것입니다.* |
| <a name="2.9">9</a>       | '구독자'는 선행 'Subscription.request(long n)' 호출이 있거나 없는 'onComplete' 신호를 수신할 준비가 되어 있어야 합니다(MUST). |
| [:bulb:](#2.9 "2.9 explained") | *이 규칙의 목적은 완료가 수요 흐름과 관련이 없음을 확인하는 것입니다. 이렇게 하면 스트림이 일찍 완료되고 완료를 *폴링*할 필요가 없습니다.* |
| <a name="2.10">10</a>     | '구독자'는 선행 'Subscription.request(long n)' 호출이 있거나 없는 'onError' 신호를 수신할 준비를 해야 합니다(MUST). |
| [:bulb:](#2.10 "2.10 explained") | *이 규칙의 목적은 게시자 실패가 신호된 수요와 완전히 관련이 없을 수 있음을 확인하는 것입니다. 즉, 구독자는 게시자가 요청을 이행할 수 없는지 알아보기 위해 폴링할 필요가 없습니다.* |
| <a name="2.11">11</a>     | '구독자'는 해당 신호를 처리하기 전에 [signal](#term_signal) 메서드에 대한 모든 호출이 발생하는지 확인해야 합니다(MUST). 즉. 가입자는 신호를 처리 논리에 적절하게 게시해야 합니다. |
| [:bulb:](#2.11 "2.11 explained") | *이 규칙의 목적은 신호의 비동기 처리가 스레드로부터 안전한지 확인하는 것이 구독자 구현의 책임임을 확인하는 것입니다. [섹션 17.4.5의 Happens-Before에 대한 JMM 정의](https://docs.oracle.com/javase/specs/jls/se8/html/jls-17.html#jls-17.4.5)를 참조하세요.* |
| <a name="2.12">12</a>     | `Subscriber.onSubscribe`는 주어진 `Subscriber`에 대해 최대 한 번만 호출되어야 합니다(객체 동등성에 기반). |
| [:bulb:](#2.12 "2.12 explained") | *이 규칙의 목적은 동일한 구독자가 최대 한 번만 구독할 수 있다고 가정해야 함을 설정하는 것입니다. `객체 동등성`은 `a.equals(b)`입니다.* |
| <a name="2.13">13</a>     | 'onSubscribe', 'onNext', 'onError' 또는 'onComplete'를 호출하면 제공된 매개변수가 'null'인 경우를 제외하고는 반드시 [정상적으로 반환](#term_return_normally)해야 합니다. 다른 모든 상황에서 '구독자'가 실패를 알리는 유일한 법적 방법은 '구독'을 취소하는 것입니다. 이 규칙을 위반하는 경우 '구독자'에 대한 모든 관련 '구독'은 취소된 것으로 간주되어야 하고 호출자는 런타임 환경에 적합한 방식으로 이 오류 조건을 발생시켜야 합니다(MUST). |
| [:bulb:](#2.13 "2.13 explained") | *이 규칙의 목적은 구독자의 메서드에 대한 의미 체계와 이 규칙을 위반하는 경우 게시자가 수행할 수 있는 작업을 설정하는 것입니다. «런타임 환경에 적합한 방식으로 이 오류 조건을 올리십시오.»는 오류를 기록하는 것을 의미할 수 있습니다. 그렇지 않으면 오류가 결함이 있는 구독자에게 신호를 보낼 수 없기 때문에 누군가 또는 무언가가 상황을 알 수 있습니다.* |

#### 3. Subscription ([Code](https://github.com/reactive-streams/reactive-streams-jvm/blob/v1.0.4/api/src/main/java/org/reactivestreams/Subscription.java))

```java
public interface Subscription {
    public void request(long n);
    public void cancel();
}
````

| ID                        | Rule                                                                                                   |
| ------------------------- | ------------------------------------------------------------------------------------------------------ |
| <a name="3.1">1</a>       | `Subscription.request` 및 `Subscription.cancel`은 `Subscriber` 컨텍스트 내에서만 호출되어야 합니다(MUST). |
| [:bulb:](#3.1 "3.1 explained") | *이 규칙의 목적은 구독이 구독자와 게시자 간의 고유한 관계를 나타내도록 설정하는 것입니다[[2.12](#2.12) 참조]. 구독자는 요소가 요청되는 시기와 더 이상 요소가 더 이상 필요하지 않은 시기를 제어합니다.* |
| <a name="3.2">2</a>       | '구독'은 '구독자'가 'onNext' 또는 'onSubscribe' 내에서 동기적으로 'Subscription.request'를 호출하도록 허용해야 합니다(MUST). |
| [:bulb:](#3.2 "3.2 explained") | *이 규칙의 목적은 `request`와 `onNext` 사이의 상호 재귀(결국 `onComplete` / `onError`)의 경우 스택 오버플로를 피하기 위해 `request`의 구현이 재진입해야 한다는 것을 분명히 하는 것입니다. . 이는 게시자가 '동기식'일 수 있음을 의미합니다. 즉, '요청'을 호출하는 스레드에서 'onNext'를 시그널링할 수 있습니다.* |
| <a name="3.3">3</a>       | 'Subscription.request'는 'Publisher'와 'Subscriber' 사이의 가능한 동기 재귀에 상한선을 지정해야 합니다(MUST). |
| [:bulb:](#3.3 "3.3 explained") | *이 규칙의 의도는 `request`와 `onNext`(그리고 결국 `onComplete` / `onError`) 사이의 상호 재귀에 상한선을 두어 [[3.2](#3.2) 참조]를 보완하는 것입니다. 스택 공간을 절약하기 위해 이 상호 재귀를 깊이 '1'(ONE)로 제한하도록 구현하는 것이 좋습니다. 바람직하지 않은 동기 개방 재귀의 예는 Subscriber.onNext -> Subscription.request -> Subscriber.onNext -> ...입니다. 그렇지 않으면 호출 스레드의 스택이 끊어지기 때문입니다.* |
| <a name="3.4">4</a>       | `Subscription.request`는 적시에 응답하여 응답해야 합니다(SHOULD). |
| [:bulb:](#3.4 "3.4 explained") | *이 규칙의 목적은 '요청'이 [비 방해](#term_non-obstructing) 메서드가 되도록 하고 호출 스레드에서 가능한 한 빨리 실행되어야 한다는 것을 설정하는 것입니다. 따라서 호출자의 실행 스레드를 지연시키는 과도한 계산 및 기타 작업을 피하십시오.* |
| <a name="3.5">5</a>       | `Subscription.cancel`은 적시에 반환하여 호출자의 응답을 존중해야 하며(MUST) 멱등적이어야 하고 [스레드 안전](#term_thread-safe)이어야 합니다(MUST). |
| [:bulb:](#3.5 "3.5 explained") | *이 규칙의 목적은 `cancel`이 [비장애](#term_non-obstructing) 메서드가 되도록 하고 호출 스레드에서 가능한 한 빨리 실행되어야 하므로 과도한 계산을 피하고 호출자의 실행 스레드를 지연시키는 다른 것들. 또한 부작용 없이 여러 번 호출할 수 있는 것도 중요합니다.* |
| <a name="3.6">6</a>       | `Subscription`이 취소된 후 추가 `Subscription.request(long n)`는 [NOP](#term_nop)여야 합니다(MUST). |
| [:bulb:](#3.6 "3.6 explained") | *이 규칙의 목적은 구독 취소와 추가 요소 요청의 후속 비작동 사이의 인과 관계를 설정하는 것입니다.* |
| <a name="3.7">7</a>       | '구독'이 취소된 후 추가 'Subscription.cancel()'은 [NOP](#term_nop)여야 합니다(MUST). |
| [:bulb:](#3.7 "3.7 explained") | *이 규칙의 의도는 [3.5](#3.5)로 대체됩니다.* |
| <a name="3.8">8</a>       | `Subscription`이 취소되지 않은 동안 `Subscription.request(long n)`는 생성될 추가 요소의 주어진 수를 해당 가입자에게 등록해야 합니다(MUST). |
| [:bulb:](#3.8 "3.8 explained") | *이 규칙의 목적은 '요청'이 추가 작업인지 확인하고 요소 요청이 게시자에게 전달되도록 하는 것입니다.* |
| <a name="3.9">9</a>       | `Subscription`이 취소되지 않은 동안 `Subscription.request(long n)`는 인수가 <= 0인 경우 `java.lang.IllegalArgumentException`과 함께 `onError`에 신호를 보내야 합니다(MUST). 원인 메시지는 긍정적이지 않은 요청을 설명해야 합니다(SHOULD). 신호는 불법입니다. |
| [:bulb:](#3.9 "3.9 explained") | *이 규칙의 목적은 잘못된 구현이 예외가 발생하지 않고 작업을 진행하는 것을 방지하는 것입니다. 요청은 가산적이기 때문에 음수 또는 0개의 요소를 요청하는 것은 구독자를 대신한 잘못된 계산의 결과일 가능성이 큽니다.* |
| <a name="3.10">10</a>     | `Subscription`이 취소되지 않은 동안 `Subscription.request(long n)`는 이(또는 다른) 가입자에 대해 `onNext`를 동기적으로 호출할 수 있습니다(MAY). |
| [:bulb:](#3.10 "3.10 explained") | *이 규칙의 목적은 동기 게시자, 즉 호출 스레드에서 논리를 실행하는 게시자를 만들 수 있도록 설정하는 것입니다.* |
| <a name="3.11">11</a>     | `Subscription`이 취소되지 않은 동안 `Subscription.request(long n)`는 이(또는 다른) 가입자에 대해 `onComplete` 또는 `onError`를 동기적으로 호출할 수 있습니다(MAY). |
| [:bulb:](#3.11 "3.11 explained") | *이 규칙의 목적은 동기 게시자, 즉 호출 스레드에서 논리를 실행하는 게시자를 만들 수 있도록 설정하는 것입니다.* |
| <a name="3.12">12</a>     | '구독'이 취소되지 않은 동안 'Subscription.cancel()'은 '게시자'에게 결국 '구독자' 신호를 중지하도록 요청해야 합니다(MUST). 작업은 '구독'에 즉시 영향을 미칠 필요가 없습니다. |
| [:bulb:](#3.12 "3.12 explained") | *이 규칙의 목적은 신호가 수신되기까지 다소 시간이 걸릴 수 있음을 인정하고 구독 취소 의사가 궁극적으로 게시자에 의해 존중되도록 설정하는 것입니다.* |
| <a name="3.13">13</a>     | '구독'이 취소되지 않은 동안, 'Subscription.cancel()'은 '게시자'에게 결국 해당 구독자에 대한 참조를 삭제하도록 요청해야 합니다(MUST). |
| [:bulb:](#3.13 "3.13 explained") | *이 규칙의 목적은 구독이 더 이상 유효하지 않은 후 구독자가 적절하게 가비지 수집될 수 있도록 하는 것입니다. 동일한 Subscriber 객체로 다시 구독하는 것은 권장되지 않습니다[[2.12](#2.12) 참조]. 그러나 이 사양은 이전에 취소된 구독을 무기한 저장해야 한다는 것을 의미하기 때문에 허용되지 않도록 규정하지 않습니다.* |
| <a name="3.14">14</a>     | '구독'이 취소되지 않은 동안, 'Subscription.cancel'을 호출하면 이 시점에 다른 '구독'이 존재하지 않는 경우 '게시자'가 스테이트풀(stateful)인 경우 '종료' 상태로 전환될 수 있습니다([1.9] 참조). (#1.9)]. |
| [:bulb:](#3.14 "3.14 explained") | *이 규칙의 목적은 게시자가 기존 구독자의 취소 신호에 대한 응답으로 새 구독자의 'onSubscribe' 다음에 'onComplete' 또는 'onError' 신호를 보낼 수 있도록 하는 것입니다.* |
| <a name="3.15">15</a>     | 'Subscription.cancel'을 호출하면 [정상적으로 반환](#term_return_normally)해야 합니다(MUST). |
| [:bulb:](#3.15 "3.15 explained") | *이 규칙의 목적은 '취소' 호출에 대한 응답으로 구현에서 예외를 throw하는 것을 허용하지 않는 것입니다.* |
| <a name="3.16">16</a>     | 'Subscription.request' 호출은 [정상적으로 반환](#term_return_normally)해야 합니다(MUST). |
| [:bulb:](#3.16 "3.16 explained") | *이 규칙의 목적은 '요청' 호출에 대한 응답으로 구현에서 예외를 throw하는 것을 허용하지 않는 것입니다.* |
| <a name="3.17">17</a>     | '구독'은 '요청'에 대한 무제한 호출을 지원해야 하고 최대 2^63-1('java.lang.Long.MAX_VALUE')의 요구를 지원해야 합니다(MUST). 2^63-1(`java.lang.Long.MAX_VALUE`)보다 크거나 같은 요구는 `게시자`가 "효과적으로 제한이 없는" 것으로 간주할 수 있습니다(MAY). |
| [:bulb:](#3.17 "3.17 explained") | *이 규칙의 목적은 구독자가 '요청'을 여러 번 호출할 때 0보다 큰 [[3.9](#3.9) 참조] 제한 없는 수의 요소를 요청할 수 있도록 설정하는 것입니다. 2^63-1의 수요를 충족하기 위해 합리적인 시간(나노초당 1개의 요소는 292년이 소요됨) 내에 현재 또는 예상되는 하드웨어로 도달할 수 없기 때문에 게시자가 이 시점에서 수요 추적을 중지할 수 있습니다.* |

'구독'은 이 쌍 간의 데이터 교환을 중재하기 위해 정확히 하나의 '게시자'와 1명의 '구독자'가 공유합니다. 이것이 `subscribe()` 메소드가 생성된 `Subscription`을 반환하지 않고 대신 `void`를 반환하는 이유입니다. '구독'은 'onSubscribe' 콜백을 통해서만 '구독자'에게 전달됩니다.

#### 4.Processor ([Code](https://github.com/reactive-streams/reactive-streams-jvm/blob/v1.0.4/api/src/main/java/org/reactivestreams/Processor.java))

```java
public interface Processor<T, R> extends Subscriber<T>, Publisher<R> {
}
````

| ID                       | Rule                                                                                                   |
| ------------------------ | ------------------------------------------------------------------------------------------------------ |
| <a name="4.1">1</a>      | '프로세서'는 '구독자'와 '게시자' 모두인 처리 단계를 나타내며 두 계약을 모두 준수해야 합니다. |
| [:bulb:](#4.1 "4.1 explained") | *이 규칙의 목적은 프로세서가 게시자 및 구독자 사양 모두에 의해 작동하고 이에 구속되도록 설정하는 것입니다.* |
| <a name="4.2">2</a>      | '프로세서'는 'onError' 신호를 복구하도록 선택할 수 있습니다. 그렇게 하기로 선택하면 '구독'이 취소된 것으로 간주해야 하며, 그렇지 않으면 'onError' 신호를 즉시 구독자에게 전파해야 합니다(MUST). |
| [:bulb:](#4.2 "4.2 explained") | *이 규칙의 목적은 구현이 단순한 변환 이상일 수 있음을 알리는 것입니다.* |

의무 사항은 아니지만, 마지막 '구독자'가 '구독'을 취소할 때 '프로세서'의 업스트림 '구독'을 취소하여 취소 신호가 업스트림으로 전파되도록 하는 것이 좋습니다.

### Asynchronous vs Synchronous Processing ###

Reactive Streams API는 요소(`onNext`) 또는 종료 신호(`onError`, `onComplete`)의 모든 처리가 `Publisher`를 *차단*해서는 안 된다고 규정합니다. 그러나 각각의 'on*' 핸들러는 이벤트를 동기식 또는 비동기식으로 처리할 수 있습니다.

Take this example:

```
nioSelectorThreadOrigin map(f) filter(p) consumeTo(toNioSelectorOutput)
```

비동기 원본과 비동기 대상이 있습니다. 출발지와 목적지가 모두 선택기 이벤트 루프라고 가정해 보겠습니다. `Subscription.request(n)`은 목적지에서 출발지로 연결되어야 합니다. 여기에서 각 구현이 이를 수행하는 방법을 선택할 수 있습니다.

다음은 파이프 `|` 문자를 사용하여 비동기 경계(대기열 및 일정)를 알리고 `R#`을 사용하여 리소스(스레드 가능)를 나타냅니다.

```
nioSelectorThreadOrigin | map(f) | filter(p) | consumeTo(toNioSelectorOutput)
-------------- R1 ----  | - R2 - | -- R3 --- | ---------- R4 ----------------
```

이 예에서 3명의 소비자인 `map`, `filter` 및 `consumeTo`는 각각 작업을 비동기식으로 예약합니다. 동일한 이벤트 루프(트램펄린), 별도의 스레드에 있을 수 있습니다.

```
nioSelectorThreadOrigin map(f) filter(p) | consumeTo(toNioSelectorOutput)
------------------- R1 ----------------- | ---------- R2 ----------------
```

여기서는 NioSelectorOutput 이벤트 루프에 작업을 추가하여 비동기식으로 예약하는 마지막 단계일 뿐입니다. `map` 및 `filter` 단계는 원본 스레드에서 동기적으로 수행됩니다.

또는 다른 구현이 작업을 최종 소비자와 융합할 수 있습니다.

```
nioSelectorThreadOrigin | map(f) filter(p) consumeTo(toNioSelectorOutput)
--------- R1 ---------- | ------------------ R2 -------------------------
```

이러한 모든 변형은 "비동기 스트림"입니다. 그것들은 모두 각자의 위치가 있으며 성능 및 구현 복잡성을 포함하여 서로 다른 절충점이 있습니다.

Reactive Streams 계약을 통해 구현은 리소스와 일정을 관리하고 비차단, 비동기, 동적 푸시풀 스트림의 범위 내에서 비동기 및 동기 처리를 혼합할 수 있습니다.

참여하는 모든 API 요소(`Publisher`/`Subscription`/`Subscriber`/`Processor`)의 완전한 비동기식 구현을 허용하기 위해 이러한 인터페이스에 의해 정의된 모든 메소드는 `void`를 반환합니다.

### Subscriber controlled queue bounds ###

기본 설계 원칙 중 하나는 모든 버퍼 크기가 제한되어야 하며 이러한 경계는 구독자가 *알고* *제어*해야 한다는 것입니다. 이러한 경계는 *요소 수*(이는 차례로 onNext의 호출 수로 변환됨)로 표현됩니다. 무한 스트림(특히 높은 출력 속도 스트림)을 지원하는 것을 목표로 하는 모든 구현은 메모리 부족 오류를 방지하고 일반적으로 리소스 사용을 제한하기 위해 모든 과정에서 경계를 적용해야 합니다.

역압은 필수이므로 무제한 버퍼의 사용을 피할 수 있습니다. 일반적으로 대기열이 제한 없이 커질 수 있는 유일한 경우는 게시자 측이 장기간 구독자보다 높은 비율을 유지하지만 이 시나리오는 대신 백프레셔에 의해 처리됩니다.

대기열 경계는 적절한 수의 요소에 대한 수요 신호를 보내는 가입자에 의해 제어될 수 있습니다. 가입자는 언제든지 다음 사항을 알고 있습니다.

 - the total number of elements requested: `P`
 - the number of elements that have been processed: `N`

그러면 게시자에게 더 많은 수요가 있다는 신호를 받을 때까지 도착할 수 있는 최대 요소 수는 'P - N'입니다. 구독자가 입력 버퍼에 있는 요소 B의 수를 알고 있는 경우 이 경계를 'P - B - N'으로 조정할 수 있습니다.

이러한 경계는 그것이 나타내는 소스가 역압될 수 있는지 여부와 상관없이 게시자가 준수해야 합니다. 클럭 틱 또는 마우스 움직임과 같이 생산 속도에 영향을 줄 수 없는 소스의 경우 게시자는 부과된 경계를 준수하기 위해 요소를 버퍼링하거나 삭제하도록 선택해야 합니다.

요소 수신 후 한 요소에 대한 요구를 신호하는 가입자는 요구 신호가 승인과 동일한 Stop-and-Wait 프로토콜을 효과적으로 구현합니다. 여러 요소에 대한 수요를 제공함으로써 승인 비용은 상각됩니다. 구독자는 언제든지 수요 신호를 보낼 수 있으므로 게시자와 구독자 사이의 불필요한 지연을 방지할 수 있습니다(즉, 전체 왕복을 기다리지 않고 입력 버퍼가 채워진 상태로 유지).

## Legal

이 프로젝트는 Kaazing, Lightbend, Netflix, Pivotal, Red Hat, Twitter 및 기타 여러 엔지니어 간의 공동 작업입니다. 이 프로젝트는 MIT No Attribution(SPDX: MIT-0)에 따라 사용이 허가되었습니다.