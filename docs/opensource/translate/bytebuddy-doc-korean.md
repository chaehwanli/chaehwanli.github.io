---
layout : default
title : bytebuddydockorean
parent : Translate
---


[Original Tutorial](https://bytebuddy.net/#/tutorial)


# Develop

# Getting started

먼저, 당신의 PC에 Byte Buddy의 복사본을 만들어야 합니다. [git](http://git-scm.com) 을 사용 하거나 [GitHub](https://github.com)에서 직접 복제 하여 Byte Buddy의 저장소를 복제하기만 하면 됩니다. 저장소가 복제되면, [Maven](https://maven.apache.org/)을 사용하여 프로젝트를 빌드할 수 있습니다. shell에서 다음과 같이 보일 수 있습니다.

>git clone `https://github.com/raphw/byte-buddy.git`
>
>cd byte-buddy
>
>mvn package

코드를 시작하기 전에 모든 테스트 케이스가 성공적으로 실행되는지 확인하십시오. 실행은 Byte Budd의 [master](https://github.com/raphw/byte-buddy/tree/master) branch 에서 항상 성공되어야 합니다. 실행은 현재 Byte Buddy의 최신 상태를 지속적으로 모니터링하고 있는 [Travis](https://travis-ci.org/) [build](https://travis-ci.org/raphw/byte-buddy)에서 확인되기를 바랍니다. [Travis](https://travis-ci.org/)는 [Open JDK](http://openjdk.java.net/) 버전 6 과 7 그리고 [Oracle JDK](https://www.oracle.com/java/technologies/downloads/) 7 과 8 용 프로젝트를 컴파일하고 테스트하도록 구성되어 있습니다. 일부 테스트에서는 Java 클래스를 생성 및 로드하므로 생성된 클래스가 특정 Java 버전에서 이해할 수 없는 형식을 사용하지 않도록 합니다. 생성된 바이트 코드에 직접적인 영향을 주는 Byte Buddy를 변경하는 경우 변경 사항을 커밋하기 전에 다른 Java 버전을 사용하여 빌드를 충분히 테스트해야 합니다. 이 모든 것은 Java 6 컴파일러와 호환되는 Java 코드를 작성해야 함을 의미합니다. 일반적으로 귀하의 코드는 최소 90%의 테스트 범위를 포함해야 하지만 우리는 특정 라인에만 해당하는 테스트보다 전반적인 테스트를 선호합니다. 항상 Maven의 `cobertura:cobertura goal`를 실행할수 있습니다. `cobertura:cobertura` [cobertura](https://cobertura.github.io/cobertura/)는 코드의 테스트 커버리지에 대한 보고서를 생성하는 것이 목표입니다. Byte Buddy의 현재 test coverage 는 [code coverage](https://coveralls.io/r/raphw/byte-buddy?branch=master)입니다. 보다 현대적인 대안으로 Byte Buddy는 Maven에서 실행하는 `org.itest:pitest-maven:mutationCoverage` [Mutation Test](https://pitest.org/) 실행도 지원합니다. 그러나 돌연변이 테스트를 실행하려면 라인 커버리지를 계산하는 것과 비교하여 상당한 양의 추가 런타임이 필요합니다. 마지막으로 Maven의 `통합` 프로필을 활성화하여 코드 기반에서 잠재적 오류를 검색할 수 있습니다. 이 프로파일은 지속적 통합 서버에서 항상 활성화됩니다. 반면에 `검사` 프로필은 더 빠르게 실행되는 코드 검사를 집계하고 모든 Maven 빌드에 대해 기본적으로 활성화됩니다.

>[Mutation Testing 이란](https://grapevine9700.tistory.com/386)

프로젝트는 여러 모듈들로 구성됩니다. `byte-buddy-parent`는 루트 프로젝트로 모든 Byte Buddy 모듈 구성을 위한 공통 기반 역할을 합니다. 프로젝트의 구현은 `byte-buddy-dev`모듈 에서 찾을 수 있습니다. 이 모듈은 [ASM](https://asm.ow2.io/) 라이브러리 에 대한 직접적인 종속성에 의존하도록 구성되어 있으므로 모듈 이름에 dev 한정자가 있습니다. [ASM 라이브러리는 하위 호환성을 보장하지 않기](https://asm.ow2.io/#Q15) 때문에, `byte-buddy` 모듈은 `ASM-Dependency`을 Byte Buddy의 고유한 이름 공간으로 재패키징합니다. 이렇게 하면 ASM 종속성이  `net.bytebuddy.jar.asm` 로 이동됩니다. 이 종속성 없는 모듈은 자체 소스를 정의하지 않고 `byte-buddy-dev` 모듈에 의존합니다. 빌드 시 모든 종속성이 해결되고 종속성이 없는 배포 설명자가 생성됩니다. 추가 정보는 [Byte Buddy에 대한 종속성을 정의하는 방법](https://bytebuddy.net/#/) 에 대한 섹션에서 찾을 수 있습니다. 재패킹을 활성화하려면 `추가` 프로파일을 사용하여 Maven 빌드를 실행해야 하며 이는 소스 코드 및 javadoc에 대한 산출물 생성을 더욱 유발합니다. 만약 `gpg` Maven이 모든 산출물에 서명을 시도하는 경우에, 이것이 가능하려면 서명자가 [gpg 플러그인을 올바르게 구성](https://blog.sonatype.com/2010/01/how-to-generate-pgp-signatures-with-maven/)해야 합니다. 또 다른 모듈인 byte-buddy-benchmark에는 다른 코드 생성 라이브러리와 비교하여 Byte Buddy의 런타임 동작을 측정하기 위한 [JMH](http://openjdk.java.net/projects/code-tools/jmh/) 벤치마크가 포함되어 있습니다. `byte-buddy-agent` 모듈은 Byte Buddy의 일반 API를 향상시킬 수 있는 Java agent를 제공합니다. `byte-buddy-android` 모듈에는 `ClassLoadingStrategy`가 포함되어 있습니다. 이는 `byte-buddy-android-test`가 이 전략을 적용할 테스트 애플리케이션을 정의하는 Android 플랫폼에서 작동합니다. 'android' 프로필이 활성화된 경우에만 빌드되고, 모듈을 빌드하려면 Android SDK가 필요합니다.

# Architectural overview

모든 코드 생성 프레임워크에는 type들과 해당 멤버를 반영하는 수단이 필요합니다. Byte Buddy는 `TypeDescription` interface를 통해 type 정보를 접근합니다. 이 interface는 Java reflection API의 `Class` type과 유사한 API를 제공하며 이러한 instance를 wrapping하는 `TypeDescription.ForLoadedType` 구현을 사용하여 로드된 Java class를 type 설명으로 나타낼 수 있습니다. 그러나 Byte Buddy는 클래스가 로드되기 전에 조작되어야 하는 곳에서 Java 에이전트 생성에도 사용할 될 수 있습니다. 따라서 Byte Buddy는 로드된 유형에서 작동하지 않고 항상 설명에 의존합니다. 모든 설명 인터페이스는 `net.bytebuddy.description` 패키지에 있습니다.

마찬가지로 Byte Buddy는 일반적인 type을 `TypeDescription.Generic` interface의 instance로 설명합니다. 이 interface는 Byte Buddy가 모든 종류의 일반 type을 처리하기 위한 method를 제공하는 Java의 동등한 'Type' interface보다 풍부합니다. 특정 속성을 지원하지 않는 일반적인 type에 대해 요청할 때, 특정 일반 type에서만 지원되는 기능은 예외를 throw합니다. 이 접근 방식은 Java 언어에서 요구하는 type casting에 대한 보다 간결한 대안으로 선택되었습니다. 대부분의 조작은 visitor들에 의해 수행되기 때문에 실제로는 문제가 되지 않습니다.

Byte Buddy는 [ASM byte code parser](https://asm.ow2.io/) 위에 구축되고, 이는 Java 생태계에서 byte code parsing을 위한 사실상의 표준이 되었습니다. ASM은 낮은 수준의 구성으로 작동하며 종종 이름과 설명자를 나타내는 문자열 값을 사용합니다. Byte Buddy의 설명 인터페이스와 상호 작용하고 필요한 값을 적절한 형식으로 추출하고 ASM과 상호 작용하는 `StackManipulation` interface를 Byte Buddy는 제공하고 이는 visitor 명령에 대한 type-safe하게 wrapping 함을 나타냅니다. 또한 각 스택 조작은 JVM의 피연산자 스택 크기에 미치는 영향을 알고 있습니다.
이런 식으로 여러 스택 조작을 결합하여 최소 스택 크기에 대한 공통 요구 사항을 계산할 수 있습니다. 여러 스택 조작을 그룹화하여 `ByteCodeAppender`를 나타낼 수 있습니다. `ByteCodeAppender`는 코드 블록을 나타내며 피연산자 스택을 비워 두는 데 필요합니다.
또한 코드 실행을 위해 할당한 지역 변수를 위한 공간을 노출하려면 `ByteCodeAppender`가 필요합니다. 일반적으로 `ByteCodeAppender`는 하나 이상의 스택 조작으로 구성됩니다. 코드 생성과 관련된 모든 클래스는 `net.bytebuddy.implementation.bytecode` 패키지에 모아져 있습니다.

각 `ByteCodeAppender`는 구현 중인 method를 `apply` method에 대한 argument로 수신합니다. 또한 method의 코드와 'Implementation.Context'의 instance를 등록하기 위한 ASM visitor를 수신합니다. 구현 context는 보조 type의 등록을 허용합니다. 보조 type은 method를 실행하는 데 필요한 helper type을 나타냅니다. 보조 type의 예로 `@SuperCall` annotation에 대한 `MethodDelegation` instrumentation(unit testing)에서 사용하는 instrument(unit testing) 유형의 다른 method를 호출하기 위한 proxy class가 있습니다.구현 context의 구현은 자체적으로 변경 가능한 ASM의 method visitor를 동반하므로 Byte Buddy 내에서 몇 안 되는 변경 가능한 클래스 중 하나입니다. 그러나 구현 컨텍스트는 공개 API에 노출되지 않으며 `ByteCodeAppender`에 의해 노출되어서도 안 됩니다.

method instrumentation(unit testing)을 나타내는 최상위 구성은 `Implementation` interface입니다. instrument(unit testing) type이 지정된 `ByteCodeAppender`를 생성하려면 이 interface의 instance가 필요합니다. 또한 구현에는 instrumented type의 static initializer 내에서 실행할 추가 method, field 또는 code block을 등록할 수 있는 기회가 제공됩니다. 마지막으로, 모든 구현 instance는 type이 instrumented type에 관계없이 instrumented type의 속성에 접근하는 방법을 제공하는 'Implementation.Target' instance를 수신합니다. 예를 들어, instrumented type에 대한 super method 호출을 쿼리할 수 있습니다.사용자가 subclass instrumentation을 수행하는 경우 이 쿼리는 실제 super method 호출을 반환합니다. 만약 사용자가 type-rebasement을 수행하더라도, 구현 대상 구현은 동일한 클래스의 다른 메서드에 복사된 rebased method의 원래 코드를 호출합니다.

Builder 자체는 Byte Buddy의 다른 구성 요소가 생성되기 전에 instrumented type을 반영할 수 있도록 허용하는 `InstrumentedType`의 인스턴스를 구성합니다. `InstrumentedType` 는 `TypeDescription`을 상속합니다. 또한 method 및 field 구현은 각각 'FieldRegistry' 또는 'MethodRegistry'에 등록됩니다. 마지막으로, 모든 구현은 class 파일 생성을 위해 ASM API와 상호 작용하는 'TypeWriter'에 수집된 모든 정보를 제공하는 동적 유형 빌더에 의해 일치하는 메소드 설명에 적용됩니다.

# Coding conventions

Byte Buddy는 대부분 변경 가능한 ASM 라이브러리와 상호 작용하기 위한 변경 가능한 클래스를 제외하고 완전한 불변성을 목표로 합니다. 그러나 변경 가능한 구성 요소는 ASM과 상호 작용하는 범위로 격리되어야 하며 최종 사용자에게 노출되어서는 안 됩니다. 이러한 instance는 field 값으로 저장되어서도 안 됩니다. 또한 Byte Buddy의 class loader와 같은 일부 구성 요소는 class loader의 특성으로 인해 변경 가능합니다. 그러나 Byte Buddy 내의 모든 collection들은 변경할 수 없는 것으로 간주되어야 하며 이 속성은 공개 API에서 collection이 반환될 때도 적용되어야 합니다.

object equality는 Byte Buddy의 일부 구성 요소에 대한 중요한 개념이기 때문에 모든 immutable classes는 적절한 `hashCode` 및 `equals` method를 구현해야 합니다. 예를 들어 Java에서 동일한 이름의 field를 두 번 등록하는 것은 불법이기 때문에 `Implementation` instance는 instrumented type을 한 번만 준비해야 합니다. 이를 보장하기 위해 모든 `Implementation`은 이미 instrumented type을 준비할 기회가 있었던 이전 구현과 동일한지 확인합니다. 구현이 내부적으로 다른 개체에 의존하는 경우 이러한 모든 구성 요소가 equality 조건을 이행하는 것이 중요합니다. 또한 모든 구성 요소는 특히 사용자의 스택 추적이 도움말 포럼에 게시될 때 디버깅을 개선하기 위해 적절한 `toString` method를 구현해야 합니다 . `ObjectPropertyAssertion`을 사용하여 Byte Buddy는 이러한 모든 method의 올바른 구현을 위해 unit tests를 실행합니다.

Byte Buddy는 강력한 객체 지향이지만 기능적 디자인에서 영감을 얻습니다. 불행히도 다른 많은 라이브러리 개발자가 사용하는 메타 라이브러리인 Byte Buddy는 강력한 호환성 요구 사항에 구속되며 Java 6에서 컴파일됩니다. 함수를 모방하기 위해 Byte Buddy는 종종 열거형으로 명명된 함수를 생성하는 열거형 인터페이스를 구현합니다. 마지막으로 클래스 파일은 패키지가 아닌 관련 클래스의 컨테이너로 사용됩니다. 클래스 파일 내에서, 예를 들어 내부적으로 다른 클래스를 사용하는 클래스 또는 하위 클래스에서만 볼 수 있는 `protected` 클래스를 허용함으로써 최상위 클래스보다 더 미세한 가시성 범위를 정의할 수 있습니다. 또한 리팩토링 결과 클래스가 더 이상 필요하지 않은 경우 이러한 그룹화 규칙을 통해 종속 코드를 모두 쉽게 삭제할 수 있습니다.

메타 라이브러리인 Byte Buddy는 사용 범위를 예상할 수 없는 만큼 확장에 최대한 개방된 API를 제공하려고 합니다. 가능하면 위임은 클래스 확장보다 선호되는 확장 메커니즘입니다. **모든 코드**는 자동 검사를 통해 이 속성을 [검증하기 쉽게](https://en.wikipedia.org/wiki/Broken_windows_theory) 문서화해야 합니다. Byte Buddy 내에서 null을 필드 값, 매개변수 또는 메서드 반환으로 사용하는 것은 나쁜 습관으로 간주됩니다. 예외는 `null` 값이 일반적인 Java 리플렉션 API를 모방하는 설명 유형입니다. 설명 인스턴스는 최종 사용자에게 노출되므로 일관성보다 유사성이 더 중요한 요소로 간주되기로 결정했습니다. 모든 잠재적인 `null` 값은 메서드에 문서화되어야 합니다. Byte Buddy의 경우 확인되지 않은 예외가 확인된 예외보다 우선합니다.

# Contribute

버그를 수정했다면 GitHub에서 [pull request](https://github.com/raphw/byte-buddy/pulls)를 생성하면 됩니다. nofitication을 받는 대로 최대한 빨리 문제를 조사하도록 하겠습니다. 그러나 변경 사항과 수정된 문제를 정확하게 설명했는지 확인하고 문제를 재현하고 수정 사항이 작동 중임을 증명하는 테스트 사례를 제공해 주세요. 이렇게 하면 작업이 훨씬 쉬워지고 패치를 훨씬 빨리 적용할 수 있습니다. 새로운 method, field 또는 type을 추가하는 경우 해당 목적을 설명하는 코드 내 문서를 작성해야 합니다. 성능 관련 변경 사항을 적용하는 경우 `byte-buddy-benchmark` 제품군을 사용하여 해당 변경 사항을 self-critically 로 반영하십시오. 마지막으로 Byte Buddy의 새 릴리스는 일반적으로 자체 branch에서 개발됩니다.

만약 기능을 기여하는 경우, 많은 시간을 보내기 전에 Byte Buddy의 현재 개발 상태에서 귀하의 변경 사항이 어떻게 의미가 있는지 논의할 수 있도록 연락하십시오.
Byte Buddy는 더 많은 기능을 꾸준히 제공하기 위한 것이지만 안정성과 코드 일관성을 희생하면서 기능 세트를 확장하지 않습니다. 그러나 이것에 낙담하지 마세요. 공유하고 싶은 새로운 멋진 기능을 구현할 수 있을 만큼 Byte Buddy의 소스에 대해 충분히 이해하셨다면 확실히 생각해 보셨을 것입니다. 우리는 이를 빌드에 병합하기 위해 최선을 다할 것입니다!
간단히 저희에게 말씀해 주시면 우리는 당신의 승선을 기쁘게 환영할 겁니다.

Byte Buddy의 문서, 이 웹 페이지의 설명 또는 이 웹 페이지의 구조와 디자인에 기여하고 싶다면 절대 환영합니다! 우리는 철저하고 최신의 문서화가 성공적인 프로젝트의 열쇠라고 굳게 믿고 있으며 이 신념에 부응하기 위해 최선을 다할 것입니다. Byte Buddy의 접근성이나 모양을 개선하는 한 사소한 변경이라도 환영합니다. 이 프로젝트는 결국 사용자를 위해 만들어졌기 때문입니다. 프로젝트의 [gh-pages](https://github.com/raphw/byte-buddy/tree/gh-pages) branch에서 GitHub에서 호스팅되는 이 웹 페이지를 복제하기만 하면 됩니다. 웹 페이지는 [angular.js](https://angularjs.org/)와 [Twitter's Bootstrap](https://getbootstrap.com/)을 사용하여 만들었습니다.

# Roadmap

Byte Buddy는 버전 1.0에 도달했으며 아직 지원되지 않는 두 가지 기능을 제외하고 완전한 기능으로 간주됩니다. 버전 1.0의 릴리스와 함께 라이브러리의 안정성과 성능이 크게 강조되었으며 새로운 기능이 방어적으로 추가되었습니다. 당연히 Byte Buddy의 목표는 이전 버전과 최신 버전의 Java에 대해 이전 버전과 호환되는 코드 조작 방법을 제공하는 것이고, Java 프로그래밍 언어와 바이트 코드 형식의 진화는 미래에 더 새로운 릴리스를 요구할 것입니다. Java 9 지원은 현재 아직 실험 단계입니다. Java 8부터 Byte Buddy는 현재 다음 기능을 지원하지 않습니다.

Type inference

generic type은 Java 컴파일러에서 inference할 수 있습니다. Byte Buddy는 현재 이러한 기능을 제공하지 않습니다. type inference를 지원하면 generic type의 유효성을 더 잘 확인하고 generic type 정보를 고려하는 'Assigners'를 구현할 수 있습니다. 불행히도, 이 기능은 실제적으로 거의 사용되지 않는 반면 많은 작업을 부과합니다. 이러한 이유로 현재 구현되지 않습니다.