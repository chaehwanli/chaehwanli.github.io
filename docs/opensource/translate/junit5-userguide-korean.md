---
layout : default
title : junit5 user guide korean
parent: Translate
---

[Original Junit5 User Guide](https://junit.org/junit5/docs/current/user-guide/)

# 1. Overview
이 문서의 목표는 테스트를 작성하는 프로그래머, 확장 작성자 및 엔진 작성자는 물론 빌드 도구 및 IDE 공급업체를 위한 포괄적인 참조 문서를 제공하는 것입니다.

이 문서는 [PDF eng](https://junit.org/junit5/docs/current/user-guide/junit-user-guide-5.8.2.pdf) 다운로드로도 제공됩니다.

## 1.1. What is JUnit 5?

이전 버전의 JUnit과 달리 JUnit 5는 세 가지 다른 subprojects의 여러 모듈로 구성됩니다.

*JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage*

JUnit Platform은 JVM에서 `testing framework를 시작`하기 위한 기반 역할을 합니다. 또한 플랫폼에서 실행되는 testing framework를 개발하기 위한 [TestEngine API](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/TestEngine.html)를 정의합니다. 또한 플랫폼은 명령줄에서 플랫폼을 시작하는 `console launcher`와 플랫폼에서 하나 이상의 테스트 엔진을 사용하여 사용자 지정 테스트 제품군을 실행하기 위한 `JUnit Platform Suite Engine`을 제공합니다. JUnit 플랫폼에 대한 일급 지원은 널리 사용되는 IDE(`IntelliJ IDEA`, `Eclipse`, `NetBeans` 및 `Visual Studio Code` 참조) 및 빌드 도구(`Gradle`, `Maven` 및 `Ant` 참조)에도 존재합니다.

JUnit Jupiter는 JUnit 5에서 테스트 및 확장을 작성하기 위한 새로운 `프로그래밍 모델`과 `확장 모델`의 조합입니다. Jupiter 하위 프로젝트는 플랫폼에서 Jupiter 기반 테스트를 실행하기 위한 TestEngine을 제공합니다.

JUnit Vintage는 플랫폼에서 JUnit 3 및 JUnit 4 기반 테스트를 실행하기 위한 TestEngine을 제공합니다. 클래스 경로 또는 모듈 경로에 JUnit 4.12 이상이 있어야 합니다.

## 1.2. Supported Java Versions

JUnit 5는 런타임에 Java 8(또는 그 이상)이 필요합니다. 그러나 이전 버전의 JDK로 컴파일된 코드는 계속 테스트할 수 있습니다.

## 1.3. Getting Help
[Stack Overflow](https://stackoverflow.com/questions/tagged/junit5)에서 JUnit 5 관련 질문을 하거나 [Gitter](https://gitter.im/junit-team/junit5)에서 커뮤니티와 이야기하세요.

## 1.4. Getting Started

### 1.4.1. Downloading JUnit Artifacts

다운로드하여 프로젝트에 포함할 수 있는 artifact를 알아보려면 `Dependency Metadata`를 참조하세요. 빌드에 대한 종속성 관리를 설정하려면 `빌드 지원` 및 `예제 프로젝트`를 참조하십시오.

### 1.4.2. JUnit 5 Features

JUnit 5에서 사용할 수 있는 기능과 사용 방법을 알아보려면 주제별로 구성된 이 사용자 가이드의 해당 섹션을 읽으십시오.

- `Writing Tests in JUnit Jupiter`
- `Migrating from JUnit 4 to JUnit Jupiter`
- `Running Tests`
- `Extension Model for JUnit Jupiter`
- Advanced Topics
    - `JUnit Platform Launcher API`
    - `JUnit Platform Test Kit`

### 1.4.3. Example Projects

복사하고 실험할 수 있는 완전한 프로젝트 예제를 보려면 [junit5-samples](https://github.com/junit-team/junit5-samples) repository를 시작하는 것이 좋습니다. [junit5-samples](https://github.com/junit-team/junit5-samples) 저장소는 JUnit Jupiter, JUnit Vintage 및 기타 testing framework를 기반으로 하는 샘플 프로젝트 모음을 호스팅합니다. 예제 프로젝트에서 적절한 빌드 스크립트(예: `build.gradle`, `pom.xml` 등)를 찾을 수 있습니다. 아래 링크는 선택할 수 있는 몇 가지 조합을 강조 표시합니다.

- For Gradle and Java, check out the [junit5-jupiter-starter-gradle](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-gradle) project.
- For Gradle and Kotlin, check out the [junit5-jupiter-starter-gradle-kotlin](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-gradle-kotlin) project.
- For Gradle and Groovy, check out the [junit5-jupiter-starter-gradle-groovy](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-gradle-groovy) project.
- For Maven, check out the [junit5-jupiter-starter-maven](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-maven) project.
- For Ant, check out the [junit5-jupiter-starter-ant](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-ant) project.

# 2. Writing Tests

다음 예제는 JUnit Jupiter에서 테스트를 작성하기 위한 최소 요구 사항을 보여줍니다. 이 장의 다음 섹션에서는 사용 가능한 모든 기능에 대해 자세히 설명합니다.

*A first test case*

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import example.util.Calculator;
import org.junit.jupiter.api.Test;

class MyFirstJUnitJupiterTests {

    private final Calculator calculator = new Calculator();

    @Test
    void addition() {
        assertEquals(2, calculator.add(1, 1));
    }

}
```

JUnit Jupiter는 테스트 구성 및 framework 확장을 위해 다음 annotation들을 지원합니다.
달리 명시되지 않는 한 모든 core annotation들은 `junit-jupiter-api` 모듈의 [org.junit.jupiter.api](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/package-summary.html) 패키지에 있습니다.

## 2.1. Annotations

|Annotation|Description|
|---|---|
|@Test|method가 테스트 메소드임을 나타냅니다. JUnit 4의  `@Test`  annotation과 달리 이 annotation은 속성을 선언하지 않습니다. JUnit Jupiter의 테스트 확장은 자체 전용 annotation을 기반으로 작동하기 때문입니다. 이러한 method는 *재정의*되지 않는 한 *상속*됩니다.|
|@ParameterizedTest|method가 `parameterized test`임을 나타냅니다. 이러한 method는 *재정의*되지 않는 한 *상속*됩니다.|
|@RepeatedTest|method가 `repeated test`를 위한 테스트 템플릿임을 나타냅니다. 이러한 method는 *재정의*되지 않는 한 *상속*됩니다.|
|@TestFactory|method가 `dynamic test`를 위한 테스트 팩토리임을 나타냅니다. 이러한 method는 *재정의*되지 않는 한 *상속*됩니다.|
|@TestTemplate|method는 등록된 providers가 반환한 호출 컨텍스트의 수에 따라 여러 번 호출되도록 설계된 `테스트 케이스의 템플릿`임을 나타냅니다. 이러한 method는 *재정의*되지 않는 한 *상속*됩니다.|
|@TestClassOrder|annotation이 달린 테스트 클래스에서 `@Nested` 테스트 클래스에 대한 `테스트 클래스 실행 순서`를 구성하는 데 사용됩니다. 이러한 annotation은 상속됩니다.|
|@TestMethodOrder|annotation이 달린 테스트 클래스에 대한 `테스트 메소드 실행 순서`를 구성하는 데 사용됩니다. JUnit 4의 `@FixMethodOrder`와 유사합니다. 이러한 annotation은 상속됩니다.|
|@TestInstance|annotation이 달린 테스트 클래스의 `테스트 인스턴스 수명 주기`를 구성하는 데 사용됩니다. 이러한 annotation은 상속됩니다.|
|@DisplayName|테스트 클래스 또는 테스트 메소드에 대한 사용자 지정 `display name`을 선언합니다. 이러한 annotation은 *상속*되지 않습니다.|
|@DisplayNameGeneration|테스트 클래스에 대한 사용자 지정 `display name 생성기`를 선언합니다. 이러한 annotation은 *상속*됩니다.|
|@BeforeEach|현재 클래스의 각 `@Test`, `@RepeatedTest`, `@ParameterizedTest` 또는 `@TestFactory` 메소드보다 먼저 annotation이 달린 메소드가 실행되어야 함을 나타냅니다. JUnit 4의 @Before와 유사합니다. 이러한 method는 *재정의*되지 않는 한 *상속*됩니다.|
|@AfterEach|현재 클래스의 `@Test`, `@RepeatedTest`, `@ParameterizedTest` 또는 `@TestFactory` 메소드마다 annotation이 달린 method가 실행되어야 함을 나타냅니다. JUnit 4의 @After와 유사합니다. 이러한 method는 *재정의*되지 않는 한 *상속*됩니다.|
|@BeforeAll|현재 클래스의 모든 `@Test`, `@RepeatedTest`, `@ParameterizedTest` 및 `@TestFactory` 메소드보다 먼저 annotation이 달린 메소드를 실행해야 함을 나타냅니다. JUnit 4의 `@BeforeClass`와 유사합니다. 이러한 method는 상속되며(숨겨지거나 재정의되지 않는 한) 정적이어야 합니다("클래스별" `테스트 인스턴스 수명 주기`가 사용되지 않는 한).|
|@AfterAll|현재 클래스의 모든 `@Test`, `@RepeatedTest`, `@ParameterizedTest` 및 `@TestFactory` 메소드 후에 annotation이 달린 메소드가 실행되어야 함을 나타냅니다. JUnit 4의 `@AfterClass`와 유사합니다. 이러한 method는 상속되며(숨겨지거나 재정의되지 않는 한) 정적이어야 합니다("클래스별" `테스트 인스턴스 수명 주기`가 사용되지 않는 한).|
|@Nested|annotation이 달린 클래스는 정적이 아닌 `중첩 테스트 클래스`임을 나타냅니다. `@BeforeAll` 및 `@AfterAll` method는 "클래스별" `테스트 인스턴스 수명 주기`가 사용되지 않는 한 `@Nested` 테스트 클래스에서 직접 사용할 수 없습니다. 이러한 annotation은 *상속*되지 않습니다.|
|@Tag|클래스 또는 메소드 수준에서 `테스트 필터링을 위한 태그`를 선언하는 데 사용됩니다. TestNG의 테스트 그룹이나 JUnit 4의 카테고리와 유사합니다. 이러한 annotation은 클래스 수준에서 상속되지만 메소드 수준에서는 상속되지 않습니다.|
|@Disabled|테스트 클래스 또는 테스트 메소드를 `비활성화`하는 데 사용됩니다. JUnit 4의 `@Ignore`와 유사합니다. 이러한 annotation은 *상속*되지 않습니다.|
|@Timeout|실행이 지정된 기간을 초과하는 경우 테스트, 테스트 팩토리, 테스트 템플릿 또는 수명 주기 메소드를 실패하는 데 사용됩니다. 이러한 annotation은 *상속*됩니다.|
|@ExtendWith|확장을 `선언적으로 등록`하는 데 사용됩니다. 이러한 annotation은 *상속*됩니다.|
|@RegisterExtension|필드를 통해 `프로그래밍 방식으로 확장을 등록`하는 데 사용됩니다. 이러한 필드는 음영 처리되지 않는 한 상속됩니다.|
|@TempDir|수명 주기 메소드 또는 테스트 메소드에서 필드 주입 또는 매개 변수 주입을 통해 임시 디렉터리를 제공하는 데 사용됩니다. `org.junit.jupiter.api.io` 패키지에 있습니다.|

>일부 annotation들은 현재 실험적일 수 있습니다. 자세한 내용은 `Experimental API`의 표를 참조하세요.

### 2.1.1. Meta-Annotations and Composed Annotations

JUnit Jupiter annotation은 meta-annotation으로 사용할 수 있습니다. 즉, meta-annotation의 의미를 자동으로 상속하는 자신만의 합성 annotation을 정의할 수 있습니다.

예를 들어, 코드 기반 전체에서 `@Tag("fast")`를 복사하여 붙여넣는 대신(`태그 지정 및 필터링` 참조) 다음과 같이 @Fast라는 사용자 지정 합성 annotation을 만들 수 있습니다. `@Fast`는 `@Tag("fast")`에 대한 drop-in 교체로 사용할 수 있습니다.

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.junit.jupiter.api.Tag;

@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Tag("fast")
public @interface Fast {
}
```

다음 `@Test` 메소드는 `@Fast` annotation의 사용법을 보여줍니다.

```java
@Fast
@Test
void myFastTest() {
    // ...
}
```

`@Tag("fast")` 및 `@Test`에 대한 drop-in 교체로 사용할 수 있는 사용자 정의 `@FastTest` annotation을 도입하여 한 단계 더 나아갈 수도 있습니다.

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.Test;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Tag("fast")
@Test
public @interface FastTest {
}
```

JUnit은 다음을 "fast"로 태그가 지정된 `@Test` 메소드로 자동 인식합니다.

```java
@FastTest
void myFastTest() {
    // ...
}
```

## 2.2. Test Classes and Methods

테스트 클래스: 모든 최상위 클래스, 정적 멤버 클래스 또는 하나 이상의 테스트 메소드를 포함하는 `@Nested` 클래스.

테스트 클래스는 `추상적`이지 아니어야 하며 단일 생성자가 있어야 합니다.

테스트 방법: `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory` 또는 `@TestTemplate`으로 직접 annotation 처리되거나 meta-annotation 처리된 모든 인스턴스 메소드.

수명 주기 방법: `@BeforeAll`, `@AfterAll`, `@BeforeEach` 또는 `@AfterEach`로 직접 annotation 처리되거나 meta-annotation 처리된 모든 방법.

테스트 메소드 및 수명 주기 method는 현재 테스트 클래스 내에서 로컬로 선언되거나 슈퍼클래스에서 상속되거나 인터페이스에서 상속될 수 있습니다(`Test Interfaces and Default Methods(테스트 인터페이스 및 기본 메소드)` 참조). 또한 테스트 메소드 및 수명 주기 method는 `추상적`이지 아니어야 하며 값을 반환하지 않아야 합니다(값을 반환하는 데 필요한 `@TestFactory` 메소드 제외).

>테스트 클래스, 테스트 메소드 및 수명 주기 method는 `public`일 필요는 없지만 `private`일 수는 없습니다.

>테스트 클래스가 다른 패키지의 테스트 클래스에 의해 확장되는 경우와 같이 기술적인 이유가 없는 한 일반적으로 테스트 클래스, 테스트 메소드 및 수명 주기 메소드에 대해 `public` 한정자를 생략하는 것이 좋습니다. 클래스와 메소드를 공개하는 또 다른 기술적 이유는 Java Module System을 사용할 때 모듈 경로에 대한 테스트를 단순화하기 위함입니다.

다음 테스트 클래스는 `@Test` 메소드와 지원되는 모든 수명 주기 메소드의 사용을 보여줍니다. 런타임 시맨틱에 대한 자세한 내용은 `Test Execution Order(테스트 실행 순서)` 및 `Wrapping Behavior of Callbacks(콜백의 래핑 동작)`을 참조하세요.

*A standard test class*
```java
import static org.junit.jupiter.api.Assertions.fail;
import static org.junit.jupiter.api.Assumptions.assumeTrue;

import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

class StandardTests {

    @BeforeAll
    static void initAll() {
    }

    @BeforeEach
    void init() {
    }

    @Test
    void succeedingTest() {
    }

    @Test
    void failingTest() {
        fail("a failing test");
    }

    @Test
    @Disabled("for demonstration purposes")
    void skippedTest() {
        // not executed
    }

    @Test
    void abortedTest() {
        assumeTrue("abc".contains("Z"));
        fail("test should have been aborted");
    }

    @AfterEach
    void tearDown() {
    }

    @AfterAll
    static void tearDownAll() {
    }

}
```

## 2.3. Display Names

테스트 클래스와 테스트 method는 `@DisplayName` (공백, 특수 문자, 이모티콘 포함  포함)을 통해 사용자 지정 display name을 선언할 수 있으며, 이 이름은 테스트 보고서와 테스트 실행기 및 IDE에 표시됩니다.

```java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

@DisplayName("A special test case")
class DisplayNameDemo {

    @Test
    @DisplayName("Custom test name containing spaces")
    void testWithDisplayNameContainingSpaces() {
    }

    @Test
    @DisplayName("╯°□°）╯")
    void testWithDisplayNameContainingSpecialCharacters() {
    }

    @Test
    @DisplayName("😱")
    void testWithDisplayNameContainingEmoji() {
    }

}
```

### 2.3.1. Display Name Generators

JUnit Jupiter는 `@DisplayNameGeneration` annotation을 통해 구성할 수 있는 사용자 지정 display name 생성기를 지원합니다. `@DisplayName` annotation을 통해 제공된 값은 항상 `DisplayNameGenerator`에 의해 생성된 display name보다 우선합니다.

`DisplayNameGenerator`를 구현하여 생성기를 만들 수 있습니다. 다음은 Jupiter에서 사용할 수 있는 몇 가지 기본 항목입니다.

|DisplayNameGenerator|Behavior|
|---|---|
|Standard|JUnit Jupiter 5.0이 릴리스된 이후의 표준 display name 생성 동작과 일치합니다.|
|Simple|매개변수가 없는 메소드의 후행 괄호를 제거합니다.|
|ReplaceUnderscores|밑줄을 공백으로 바꿉니다.|
|IndicativeSentences|테스트 이름과 포함하는 클래스를 연결하여 완전한 문장을 생성합니다.|

`IndicativeSentences`의 경우 다음 예제와 같이 `@IndicativeSentencesGeneration`을 사용하여 구분 기호와 기본 생성기를 사용자 지정할 수 있습니다.

```java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.DisplayNameGeneration;
import org.junit.jupiter.api.DisplayNameGenerator;
import org.junit.jupiter.api.IndicativeSentencesGeneration;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

class DisplayNameGeneratorDemo {

    @Nested
    @DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class)
    class A_year_is_not_supported {

        @Test
        void if_it_is_zero() {
        }

        @DisplayName("A negative value for year is not supported by the leap year computation.")
        @ParameterizedTest(name = "For example, year {0} is not supported.")
        @ValueSource(ints = { -1, -4 })
        void if_it_is_negative(int year) {
        }

    }

    @Nested
    @IndicativeSentencesGeneration(separator = " -> ", generator = DisplayNameGenerator.ReplaceUnderscores.class)
    class A_year_is_a_leap_year {

        @Test
        void if_it_is_divisible_by_4_but_not_by_100() {
        }

        @ParameterizedTest(name = "Year {0} is a leap year.")
        @ValueSource(ints = { 2016, 2020, 2048 })
        void if_it_is_one_of_the_following_years(int year) {
        }

    }

}
```

```
+-- DisplayNameGeneratorDemo [OK]
  +-- A year is not supported [OK]
  | +-- A negative value for year is not supported by the leap year computation. [OK]
  | | +-- For example, year -1 is not supported. [OK]
  | | '-- For example, year -4 is not supported. [OK]
  | '-- if it is zero() [OK]
  '-- A year is a leap year [OK]
    +-- A year is a leap year -> if it is divisible by 4 but not by 100. [OK]
    '-- A year is a leap year -> if it is one of the following years. [OK]
      +-- Year 2016 is a leap year. [OK]
      +-- Year 2020 is a leap year. [OK]
      '-- Year 2048 is a leap year. [OK]
```

### 2.3.2. Setting the Default Display Name Generator

`junit.jupiter.displayname.generator.default` 구성 `매개변수`를 사용하여 기본적으로 사용하려는 `DisplayNameGenerator`의 정규화된 클래스 이름을 지정할 수 있습니다. `@DisplayNameGeneration annotation`을 통해 구성된 display name 생성기와 마찬가지로 제공된 클래스는 `DisplayNameGenerator` 인터페이스를 구현해야 합니다. `@DisplayNameGeneration` annotation이 둘러싸는 테스트 클래스 또는 테스트 인터페이스에 없는 경우 기본 display name 생성기가 모든 테스트에 사용됩니다. `@DisplayName` annotation을 통해 제공된 값은 항상 `DisplayNameGenerator`에 의해 생성된 display name보다 우선합니다.

예를 들어, 기본적으로 `ReplaceUnderscores` display name 생성기를 사용하려면 구성 매개변수를 해당하는 정규화된 클래스 이름으로 설정해야 합니다(예: `src/test/resources/junit-platform.properties`에서).

```xml
junit.jupiter.displayname.generator.default = \
    org.junit.jupiter.api.DisplayNameGenerator$ReplaceUnderscores
```

마찬가지로 `DisplayNameGenerator`를 구현하는 모든 사용자 지정 클래스의 정규화된 이름을 지정할 수 있습니다.

요약하면 테스트 클래스 또는 메소드의 display name은 다음 우선 순위 규칙에 따라 결정됩니다.

1. `@DisplayName` annotation의 값(이 있는 경우)
1. `@DisplayNameGeneration` annotation에 지정된 `DisplayNameGenerator`를 호출하여(있는 경우)
1. 존재하는 경우 구성 매개변수를 통해 구성된 기본 `DisplayNameGenerator`를 호출하여
1. `org.junit.jupiter.api.DisplayNameGenerator.Standard`를 호출하여

## 2.4. Assertions

JUnit Jupiter는 JUnit 4에 있는 많은 assertion method와 함께 제공되며 Java 8 lambds와 함께 사용하기에 적합한 몇 가지를 추가합니다. 모든 JUnit Jupiter assertion은 `org.junit.jupiter.api.Assertions` 클래스의 `static` 메소드입니다.

```java
import static java.time.Duration.ofMillis;
import static java.time.Duration.ofMinutes;
import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.junit.jupiter.api.Assertions.assertTimeout;
import static org.junit.jupiter.api.Assertions.assertTimeoutPreemptively;
import static org.junit.jupiter.api.Assertions.assertTrue;

import java.util.concurrent.CountDownLatch;

import example.domain.Person;
import example.util.Calculator;

import org.junit.jupiter.api.Test;

class AssertionsDemo {

    private final Calculator calculator = new Calculator();

    private final Person person = new Person("Jane", "Doe");

    @Test
    void standardAssertions() {
        assertEquals(2, calculator.add(1, 1));
        assertEquals(4, calculator.multiply(2, 2),
                "The optional failure message is now the last parameter");
        assertTrue('a' < 'b', () -> "Assertion messages can be lazily evaluated -- "
                + "to avoid constructing complex messages unnecessarily.");
    }

    @Test
    void groupedAssertions() {
        // In a grouped assertion all assertions are executed, and all
        // failures will be reported together.
        assertAll("person",
            () -> assertEquals("Jane", person.getFirstName()),
            () -> assertEquals("Doe", person.getLastName())
        );
    }

    @Test
    void dependentAssertions() {
        // Within a code block, if an assertion fails the
        // subsequent code in the same block will be skipped.
        assertAll("properties",
            () -> {
                String firstName = person.getFirstName();
                assertNotNull(firstName);

                // Executed only if the previous assertion is valid.
                assertAll("first name",
                    () -> assertTrue(firstName.startsWith("J")),
                    () -> assertTrue(firstName.endsWith("e"))
                );
            },
            () -> {
                // Grouped assertion, so processed independently
                // of results of first name assertions.
                String lastName = person.getLastName();
                assertNotNull(lastName);

                // Executed only if the previous assertion is valid.
                assertAll("last name",
                    () -> assertTrue(lastName.startsWith("D")),
                    () -> assertTrue(lastName.endsWith("e"))
                );
            }
        );
    }

    @Test
    void exceptionTesting() {
        Exception exception = assertThrows(ArithmeticException.class, () ->
            calculator.divide(1, 0));
        assertEquals("/ by zero", exception.getMessage());
    }

    @Test
    void timeoutNotExceeded() {
        // The following assertion succeeds.
        assertTimeout(ofMinutes(2), () -> {
            // Perform task that takes less than 2 minutes.
        });
    }

    @Test
    void timeoutNotExceededWithResult() {
        // The following assertion succeeds, and returns the supplied object.
        String actualResult = assertTimeout(ofMinutes(2), () -> {
            return "a result";
        });
        assertEquals("a result", actualResult);
    }

    @Test
    void timeoutNotExceededWithMethod() {
        // The following assertion invokes a method reference and returns an object.
        String actualGreeting = assertTimeout(ofMinutes(2), AssertionsDemo::greeting);
        assertEquals("Hello, World!", actualGreeting);
    }

    @Test
    void timeoutExceeded() {
        // The following assertion fails with an error message similar to:
        // execution exceeded timeout of 10 ms by 91 ms
        assertTimeout(ofMillis(10), () -> {
            // Simulate task that takes more than 10 ms.
            Thread.sleep(100);
        });
    }

    @Test
    void timeoutExceededWithPreemptiveTermination() {
        // The following assertion fails with an error message similar to:
        // execution timed out after 10 ms
        assertTimeoutPreemptively(ofMillis(10), () -> {
            // Simulate task that takes more than 10 ms.
            new CountDownLatch(1).await();
        });
    }

    private static String greeting() {
        return "Hello, World!";
    }

}
```

>*Preemptive Timeouts with* `assertTimeoutPreemptively()`(`assertTimeoutPreemptively()`를 사용한 Timeout 선점)
`선언적 시간 제한`과 달리 `Assertions` 클래스의 다양한 `assertTimeoutPreemptively()` method는 호출 코드와 다른 스레드에서 제공된 `실행 파일`이나 `공급자`를 실행합니다. `실행 파일` 또는 `공급자` 내에서 실행되는 코드가 `java.lang.ThreadLocal` 저장소에 의존하는 경우 이 동작으로 인해 바람직하지 않은 부작용이 발생할 수 있습니다.

>이에 대한 한 가지 일반적인 예는 Spring Framework의 트랜잭션 테스트 지원입니다. 특히, Spring의 테스트 지원은 테스트 method가 호출되기 전에 트랜잭션 상태를 현재 스레드(`ThreadLocal`을 통해)에 바인딩합니다. 결과적으로 `assertTimeoutPreemptively()`에 제공된 `실행 파일`이나 `공급자`가 트랜잭션에 참여하는 Spring 관리 구성 요소를 호출하는 경우 해당 구성 요소에서 수행한 모든 작업은 테스트 관리 트랜잭션과 함께 롤백되지 않습니다. 반대로 테스트 관리 트랜잭션이 롤백되더라도 이러한 작업은 영구 저장소(예: 관계형 데이터베이스)에 커밋됩니다.

>`ThreadLocal` 저장소에 의존하는 다른 프레임워크에서도 유사한 부작용이 발생할 수 있습니다.

### 2.4.1. Kotlin Assertion Support

JUnit Jupiter는 또한 [Kotlin](https://kotlinlang.org/)에서 사용하기에 적합한 몇 가지 assertion method와 함께 제공됩니다. 모든 JUnit Jupiter Kotlin assertion은 `org.junit.jupiter.api` 패키지의 최상위 기능입니다.

```java
import example.domain.Person;
import example.util.Calculator;
import java.time.Duration;
import org.junit.jupiter.api.Assertions.assertEquals;
import org.junit.jupiter.api.Assertions.assertTrue;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.assertAll;
import org.junit.jupiter.api.assertDoesNotThrow;
import org.junit.jupiter.api.assertThrows;
import org.junit.jupiter.api.assertTimeout;
import org.junit.jupiter.api.assertTimeoutPreemptively;

class KotlinAssertionsDemo {

    private val person = Person("Jane", "Doe")
    private val people = setOf(person, Person("John", "Doe"))

    @Test
    fun `exception absence testing`() {
        val calculator = Calculator()
        val result = assertDoesNotThrow("Should not throw an exception") {
            calculator.divide(0, 1)
        }
        assertEquals(0, result)
    }

    @Test
    fun `expected exception testing`() {
        val calculator = Calculator()
        val exception = assertThrows<ArithmeticException> ("Should throw an exception") {
            calculator.divide(1, 0)
        }
        assertEquals("/ by zero", exception.message)
    }

    @Test
    fun `grouped assertions`() {
        assertAll("Person properties",
            { assertEquals("Jane", person.firstName) },
            { assertEquals("Doe", person.lastName) }
        )
    }

    @Test
    fun `grouped assertions from a stream`() {
        assertAll("People with first name starting with J",
            people
                .stream()
                .map {
                    // This mapping returns Stream<() -> Unit>
                    { assertTrue(it.firstName.startsWith("J")) }
                }
        )
    }

    @Test
    fun `grouped assertions from a collection`() {
        assertAll("People with last name of Doe",
            people.map { { assertEquals("Doe", it.lastName) } }
        )
    }

    @Test
    fun `timeout not exceeded testing`() {
        val fibonacciCalculator = FibonacciCalculator()
        val result = assertTimeout(Duration.ofMillis(1000)) {
            fibonacciCalculator.fib(14)
        }
        assertEquals(377, result)
    }

    @Test
    fun `timeout exceeded with preemptive termination`() {
        // The following assertion fails with an error message similar to:
        // execution timed out after 10 ms
        assertTimeoutPreemptively(Duration.ofMillis(10)) {
            // Simulate task that takes more than 10 ms.
            Thread.sleep(100)
        }
    }
}
```

### 2.4.2. Third-party Assertion Libraries

JUnit Jupiter가 제공하는 assertion 기능은 많은 테스트 시나리오에 충분하지만 더 많은 성능과 matcher와 같은 추가 기능이 필요하거나 필요한 경우가 있습니다. 이러한 경우 JUnit 팀은 [AssertJ](https://joel-costigliola.github.io/assertj/), [Hamcrest](https://hamcrest.org/JavaHamcrest/), [Truth](https://truth.dev/) 등과 같은 third-party assertion 라이브러리의 사용을 권장합니다. 따라서 개발자는 선택한 assertion 라이브러리를 자유롭게 사용할 수 있습니다.

예를 들어, matcher와 유창한 API의 조합을 사용하여 assertion을 보다 설명적이고 읽기 쉽게 만들 수 있습니다. 그러나 JUnit Jupiter의 `org.junit.jupiter.api.Assertions` 클래스는 Hamcrest [Matcher](https://junit.org/junit4/javadoc/latest/org/hamcrest/Matcher.html)를 허용하는 JUnit 4의 org.junit.Assert 클래스에서 볼 수 있는 것과 같은 `assertThat()` 메소드를 제공하지 않습니다. 대신 개발자는 third-party assertion 라이브러리에서 제공하는 matcher에 대한 기본 제공 지원을 사용하는 것이 좋습니다.

다음 예제는 JUnit Jupiter 테스트에서 Hamcrest의 `assertThat()` 지원을 사용하는 방법을 보여줍니다. Hamcrest 라이브러리가 클래스 경로에 추가되어 있으면 `assertThat()`, `is()` 및 `equalTo()`와 같은 메소드를 정적으로 가져온 다음 아래의 `assertWithHamcrestMatcher()` 메소드와 같은 테스트에서 사용할 수 있습니다.

```java
import static org.hamcrest.CoreMatchers.equalTo;
import static org.hamcrest.CoreMatchers.is;
import static org.hamcrest.MatcherAssert.assertThat;

import example.util.Calculator;

import org.junit.jupiter.api.Test;

class HamcrestAssertionsDemo {

    private final Calculator calculator = new Calculator();

    @Test
    void assertWithHamcrestMatcher() {
        assertThat(calculator.subtract(4, 1), is(equalTo(3)));
    }

}
```

당연히 JUnit 4 프로그래밍 모델을 기반으로 하는 레거시 테스트는 `org.junit.Assert#assertThat`을 계속 사용할 수 있습니다.

## 2.5. Assumptions

JUnit Jupiter는 JUnit 4가 제공하는 assumpton method의 subset과 함께 제공되며 Java 8 lambda expressions 및 메소드 참조와 함께 사용하기에 적합한 몇 가지를 추가합니다. 모든 JUnit Jupiter assumption은 `org.junit.jupiter.api.Assumptions` 클래스의 static method입니다.

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assumptions.assumeTrue;
import static org.junit.jupiter.api.Assumptions.assumingThat;

import example.util.Calculator;

import org.junit.jupiter.api.Test;

class AssumptionsDemo {

    private final Calculator calculator = new Calculator();

    @Test
    void testOnlyOnCiServer() {
        assumeTrue("CI".equals(System.getenv("ENV")));
        // remainder of test
    }

    @Test
    void testOnlyOnDeveloperWorkstation() {
        assumeTrue("DEV".equals(System.getenv("ENV")),
            () -> "Aborting test: not on developer workstation");
        // remainder of test
    }

    @Test
    void testInAllEnvironments() {
        assumingThat("CI".equals(System.getenv("ENV")),
            () -> {
                // perform these assertions only on the CI server
                assertEquals(2, calculator.divide(4, 2));
            });

        // perform these assertions in all environments
        assertEquals(42, calculator.multiply(6, 7));
    }

}
```

>JUnit Jupiter 5.4부터는 assumption을 위해 JUnit 4의 `org.junit.Assume` 클래스에서 메소드를 사용할 수도 있습니다. 특히 JUnit Jupiter는 JUnit 4의 `AssumptionViolatedException`을 지원하여 테스트가 실패로 표시되는 대신 중단되어야 한다는 신호를 보냅니다.

## 2.6. Disabling Tests

전체 테스트 클래스 또는 개별 테스트 방법은 `@Disabled` annotation, `조건부 테스트 실행`에서 논의된 annotation 중 하나 또는 사용자 정의 `ExecutionCondition`을 통해 *비활성화*될 수 있습니다.

Here’s a `@Disabled` test class.

```java
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

@Disabled("Disabled until bug #99 has been fixed")
class DisabledClassDemo {

    @Test
    void testWillBeSkipped() {
    }

}
```

And here’s a test class that contains a `@Disabled` test method.

```java
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

class DisabledTestsDemo {

    @Disabled("Disabled until bug #42 has been resolved")
    @Test
    void testWillBeSkipped() {
    }

    @Test
    void testWillBeExecuted() {
    }

}
```

>`@Disabled`는 이유를 제공하지 않고 선언될 수 있습니다. 그러나 JUnit 팀은 개발자가 테스트 클래스 또는 테스트 메소드가 비활성화된 이유에 대한 간단한 설명을 제공할 것을 권장합니다. 결과적으로 위의 예에서는 `@Disabled`(`"bug #42가 해결될 때까지 비활성화됨"`)과 같이 이유 — 의 사용을 보여줍니다. 일부 개발 팀은 automated traceability 등을 위해  issue tracking number의 존재를 요구하기도 합니다.

## 2.7. Conditional Test Execution

JUnit Jupiter의 `ExecutionCondition 확장 API`를 사용하면 개발자가 컨테이너를 활성화 또는 비활성화하거나 특정 조건에 따라 프로그래밍 방식으로 테스트할 수 있습니다. 이러한 조건의 가장 간단한 예는 `@Disabled` annotation을 지원하는 built-in [DisabledCondition](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-engine/src/main/java/org/junit/jupiter/engine/extension/DisabledCondition.java)입니다(테스트 비활성화 참조). `@Disabled` 외에도 JUnit Jupiter는 `org.junit.jupiter.api.condition` 패키지에서 여러 다른 annotation 기반 조건을 지원하여 개발자가 선언적으로 컨테이너 및 테스트를 활성화 또는 비활성화할 수 있도록 합니다. 여러 ExecutionCondition 확장이 등록되면 조건 중 하나가 비활성화됨을 반환하는 즉시 컨테이너 또는 테스트가 비활성화됩니다. 비활성화될 수 있는 이유에 대한 세부 정보를 제공하려는 경우 이러한 기본 제공 조건과 관련된 모든 annotation에는 해당 용도로 사용할 수 있는 `disabledReason` 속성이 있습니다.

자세한 내용은 `ExecutionCondition` 및 다음 섹션을 참조하세요.

>Composed Annotations
다음 섹션에 나열된 조건부 annotation은 사용자 정의 작성된 annotation을 생성하기 위해 meta-annotation으로 사용될 수도 있습니다. 예를 들어 `@EnabledOnOs` 데모의 `@TestOnMac` annotation은 `@Test`와 `@EnabledOnO`를 재사용 가능한 단일 annotation으로 결합하는 방법을 보여줍니다.

>달리 명시되지 않는 한 다음 섹션에 나열된 각 조건부 annotation은 지정된 테스트 인터페이스, 테스트 클래스 또는 테스트 메소드에서 한 번만 선언될 수 있습니다. 조건부 annotation이 주어진 요소에 직접 존재하거나 간접적으로 존재하거나 여러 번 meta-present하는 경우 JUnit에서 발견한 첫 번째 annotation만 사용됩니다. 추가 선언은 자동으로 무시됩니다. 그러나 각 조건부 annotation은 `org.junit.jupiter.api.condition` 패키지의 다른 조건부 annotation과 함께 사용될 수 있습니다.

### 2.7.1. Operating System Conditions

컨테이너 또는 테스트는 `@EnabledOnOs` 및 `@DisabledOnOs` annotation을 통해 특정 운영 체제에서 활성화 또는 비활성화될 수 있습니다.

```java
@Test
@EnabledOnOs(MAC)
void onlyOnMacOs() {
    // ...
}

@TestOnMac
void testOnMac() {
    // ...
}

@Test
@EnabledOnOs({ LINUX, MAC })
void onLinuxOrMac() {
    // ...
}

@Test
@DisabledOnOs(WINDOWS)
void notOnWindows() {
    // ...
}

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Test
@EnabledOnOs(MAC)
@interface TestOnMac {
}
```

### 2.7.2. Java Runtime Environment Conditions

`@EnabledOnJre` 및 `@DisabledOnJre` annotation을 통해 JRE(Java Runtime Environment)의 특정 버전에서 또는 @EnabledForJreRange 및 @DisabledForJreRange annotation을 통해 [JRE](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/condition/JRE.html)의 특정 버전 범위에서 컨테이너 또는 테스트를 활성화하거나 비활성화할 수 있습니다. 범위는 기본적으로 [JRE](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/condition/JRE.html).JAVA_8이 아래쪽 경계(`min`)이고 [JRE](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/condition/JRE.html).OTHER가 위쪽 경계(`max`)로, 절반의 열린 범위를 사용할 수 있습니다.

```java
@Test
@EnabledOnJre(JAVA_8)
void onlyOnJava8() {
    // ...
}

@Test
@EnabledOnJre({ JAVA_9, JAVA_10 })
void onJava9Or10() {
    // ...
}

@Test
@EnabledForJreRange(min = JAVA_9, max = JAVA_11)
void fromJava9to11() {
    // ...
}

@Test
@EnabledForJreRange(min = JAVA_9)
void fromJava9toCurrentJavaFeatureNumber() {
    // ...
}

@Test
@EnabledForJreRange(max = JAVA_11)
void fromJava8To11() {
    // ...
}

@Test
@DisabledOnJre(JAVA_9)
void notOnJava9() {
    // ...
}

@Test
@DisabledForJreRange(min = JAVA_9, max = JAVA_11)
void notFromJava9to11() {
    // ...
}

@Test
@DisabledForJreRange(min = JAVA_9)
void notFromJava9toCurrentJavaFeatureNumber() {
    // ...
}

@Test
@DisabledForJreRange(max = JAVA_11)
void notFromJava8to11() {
    // ...
}
```

### 2.7.3. System Property Conditions

컨테이너 또는 테스트는 `@EnabledIfSystemProperty` 및 `@DisabledIfSystemProperty` annotation을 통해 `named` JVM 시스템 속성 값을 기반으로 활성화 또는 비활성화될 수 있습니다. `matches` attribute을 통해 제공된 값은 정규식으로 해석됩니다.

```java
@Test
@EnabledIfSystemProperty(named = "os.arch", matches = ".*64.*")
void onlyOn64BitArchitectures() {
    // ...
}

@Test
@DisabledIfSystemProperty(named = "ci-server", matches = "true")
void notOnCiServer() {
    // ...
}
```

>JUnit Jupiter 5.6부터 `@EnabledIfSystemProperty` 및 `@DisabledIfSystemProperty`는 *repeatable annotations*입니다. 결과적으로 이러한 annotation은 테스트 인터페이스, 테스트 클래스 또는 테스트 메소드에서 여러 번 선언될 수 있습니다. 특히 이러한 annotation은 주어진 요소에 직접 존재하거나 간접적으로 존재하거나 meta-present하는 경우 발견됩니다.

### 2.7.4. Environment Variable Conditions

컨테이너 또는 테스트는 `@EnabledIfEnvironmentVariable` 및 `@DisabledIfEnvironmentVariable` annotation을 통해 기본 운영 체제의 `named` 환경 변수 값을 기반으로 활성화 또는 비활성화할 수 있습니다. `matches` attribute을 통해 제공된 값은 정규식으로 해석됩니다.

```java
@Test
@EnabledIfEnvironmentVariable(named = "ENV", matches = "staging-server")
void onlyOnStagingServer() {
    // ...
}

@Test
@DisabledIfEnvironmentVariable(named = "ENV", matches = ".*development.*")
void notOnDeveloperWorkstation() {
    // ...
}
```

>JUnit Jupiter 5.6부터 `@EnabledIfEnvironmentVariable` 및 `@DisabledIfEnvironmentVariable`은 *repeatable annotations*입니다. 결과적으로 이러한 annotation은 테스트 인터페이스, 테스트 클래스 또는 테스트 메소드에서 여러 번 선언될 수 있습니다. 특히 이러한 annotation은 주어진 요소에 직접 존재하거나 간접적으로 존재하거나 meta-present하는 경우 발견됩니다.

### 2.7.5. Custom Conditions

`@EnabledIf` 및 `@DisabledIf` annotation을 통해 메소드의 boolean 반환을 기반으로 컨테이너 또는 테스트를 활성화하거나 비활성화할 수 있습니다. method는 이름을 통해 annotation에 제공됩니다. 필요한 경우 조건 method는 `ExtensionContext` 유형의 단일 매개 변수를 사용할 수 있습니다.

```java
@Test
@EnabledIf("customCondition")
void enabled() {
    // ...
}

@Test
@DisabledIf("customCondition")
void disabled() {
    // ...
}

boolean customCondition() {
    return true;
}
```

또는 조건 메소드가 테스트 클래스 외부에 있을 수 있습니다. 이 경우 다음 예와 같이 *fully qualified name(완전히 정규화된 이름)*으로 참조해야 합니다.

```java
package example;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.condition.EnabledIf;

class ExternalCustomConditionDemo {

    @Test
    @EnabledIf("example.ExternalCondition#customCondition")
    void enabled() {
        // ...
    }

}

class ExternalCondition {

    static boolean customCondition() {
        return true;
    }

}
```

>@EnabledIf 또는 @DisabledIf가 클래스 수준에서 사용되는 경우 조건 method는 항상 `static`이어야 합니다. 외부 클래스에 있는 조건 메소드도 `static`이어야 합니다. 다른 경우에는 정적 또는 인스턴스 방법을 모두 사용할 수 있습니다.

## 2.8. Tagging and Filtering

`@Tag` annotation을 통해 테스트 클래스와 메소드에 태그를 지정할 수 있습니다. 이러한 태그는 나중에 `테스트 검색 및 실행을 필터링`하는 데 사용할 수 있습니다. JUnit 플랫폼의 태그 지원에 대한 자세한 내용은 `Tags` section을 참조하십시오.

```java
import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.Test;

@Tag("fast")
@Tag("model")
class TaggingDemo {

    @Test
    @Tag("taxes")
    void testingTaxCalculation() {
    }

}
```

>태그에 대한 사용자 정의 annotation을 만드는 방법을 보여주는 예제는 `Meta-Annotation 및 Composed annotation`을 참조하십시오.

## 2.9. Test Execution Order

기본적으로 테스트 클래스와 method는 결정적이지만 의도적으로 명확하지 않은 알고리즘을 사용하여 정렬됩니다. 이것은 test suite의 후속 실행이 동일한 순서로 테스트 클래스와 테스트 메소드를 실행하도록 하여 반복 가능한 빌드를 허용합니다.

>`테스트 메소드 및 테스트 클래스`의 정의는 테스트 클래스 및 메소드를 참조하십시오.

### 2.9.1. Method Order

실제 단위 테스트는 일반적으로 실행 순서에 의존하지 않아야 하지만 특정 테스트 메소드 실행 순서를 적용해야 하는 경우가 있습니다. 예를 들어 통합 테스트나 기능 테스트를 작성할 때 테스트 순서가 특히 `@TestInstance(Lifecycle.PER_CLASS)`와 함께 사용하면 중요합니다.

테스트 method가 실행되는 순서를 제어하려면 [@TestMethodOrder](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestMethodOrder.html)로 테스트 클래스 또는 테스트 인터페이스에 annotation을 달고 원하는 [MethodOrderer](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.html) 구현을 지정하십시오. 사용자 정의 `MethodOrderer`를 구현하거나 다음 기본 제공 `MethodOrderer `구현 중 하나를 사용할 수 있습니다.

- [MethodOrderer.DisplayName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.DisplayName.html): display name에 따라 영숫자 순으로 테스트 메소드를 정렬합니다(`display name 생성 우선 순위 규칙(display name generation precedence)` 참조).
- [MethodOrderer.MethodName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.MethodName.html): 이름과 형식 매개변수 목록을 기반으로 테스트 메소드를 영숫자순으로 정렬합니다.
- [MethodOrderer.OrderAnnotation](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.OrderAnnotation.html): [@Order](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Order.html) annotation을 통해 지정된 값을 기반으로 테스트 메소드를 숫자로 정렬합니다.
- [MethodOrderer.Random](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.Random.html): 테스트 방법을 의사 무작위로 정렬하고 사용자 지정 시드 구성을 지원합니다.
- [MethodOrderer.Alphanumeric](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.Alphanumeric.html): 이름과 형식 매개변수 목록을 기반으로 테스트 방법을 영숫자순으로 정렬합니다. 6.0에서 제거될 [MethodOrderer.MethodName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.MethodName.html)을 위해 더 이상 사용되지 않음

>참조: `Wrapping Behavior of Callbacks(콜백의 래핑 동작)`

다음 예제는 `@Order` annotation을 통해 지정된 순서대로 테스트 메소드가 실행되도록 보장하는 방법을 보여줍니다.

```java
import org.junit.jupiter.api.MethodOrderer.OrderAnnotation;
import org.junit.jupiter.api.Order;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestMethodOrder;

@TestMethodOrder(OrderAnnotation.class)
class OrderedTestsDemo {

    @Test
    @Order(1)
    void nullValues() {
        // perform assertions against null values
    }

    @Test
    @Order(2)
    void emptyValues() {
        // perform assertions against empty values
    }

    @Test
    @Order(3)
    void validValues() {
        // perform assertions against valid values
    }

}
```

*Setting the Default Method Orderer(기본 메소드 순서 지정자 설정)*
`junit.jupiter.testmethod.order.default` `configuration parameter(구성 매개변수)`를 사용하여 기본적으로 사용하려는 [MethodOrderer](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.html)의 정규화된 클래스 이름을 지정할 수 있습니다. [@TestMethodOrder](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestMethodOrder.html) annotation을 통해 구성된 orderer와 마찬가지로 제공된 클래스는 `MethodOrderer` 인터페이스를 구현해야 합니다. `@TestMethodOrder` annotation이 둘러싸는 테스트 클래스 또는 테스트 인터페이스에 없으면 기본 순서 지정자가 모든 테스트에 사용됩니다.

예를 들어, [MethodOrderer.OrderAnnotation](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.OrderAnnotation.html) 메소드 orderer를 기본으로 사용하려면 구성 매개변수를 해당하는 fully qualified class name(정규화된 클래스 이름)으로 설정해야 합니다(예: `src/test/resources/junit-platform.properties`에서).

```xml
junit.jupiter.testmethod.order.default = \
    org.junit.jupiter.api.MethodOrderer$OrderAnnotation
```

마찬가지로 `MethodOrderer`를 구현하는 모든 사용자 지정 클래스의 정규화된 이름을 지정할 수 있습니다.

### 2.9.2. Class Order

테스트 클래스는 일반적으로 실행 순서에 의존하지 않아야 하지만 특정 테스트 클래스 실행 순서를 적용하는 것이 바람직한 경우가 있습니다. 테스트 클래스 간에 우발적인 종속성이 없는지 확인하기 위해 임의의 순서로 테스트 클래스를 실행하거나 다음 시나리오에 설명된 대로 빌드 시간을 최적화하기 위해 테스트 클래스를 주문할 수 있습니다.

- 이전에 실패한 테스트와 더 빠른 테스트를 먼저 실행: "빠른 실패" 모드
- 병렬 실행이 활성화된 상태에서 더 긴 테스트를 먼저 실행: "최단 테스트 계획 실행 기간" 모드
- 그 외 다양한 활용 사례

전체 test suite에 대해 테스트 클래스 실행 순서를 전역적으로 구성하려면 `junit.jupiter.testclass.order.default` `구성 매개변수`를 사용하여 사용하려는 [ClassOrderer](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.html)의 정규화된 클래스 이름을 지정합니다. 제공된 클래스는 `ClassOrderer` 인터페이스를 구현해야 합니다.

사용자 정의 `ClassOrderer`를 구현하거나 다음의 built-in `ClassOrderer` 구현 중 하나를 사용할 수 있습니다.

- [ClassOrderer.ClassName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.ClassName.html): 정규화된 클래스 이름을 기반으로 테스트 클래스를 영숫자 순으로 정렬합니다.
- [ClassOrderer.DisplayName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.DisplayName.html): display name에 따라 테스트 클래스를 영숫자 순으로 정렬합니다(`display name 생성 우선 순위 규칙(display name generation precedence rules)` 참조).
- [ClassOrderer.OrderAnnotation](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.OrderAnnotation.html): [@Order](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Order.html) annotation을 통해 지정된 값을 기반으로 테스트 클래스를 숫자로 정렬합니다.
- [ClassOrderer.Random](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.Random.html): 테스트 클래스를 의사 무작위로 정렬하고 사용자 지정 시드 구성을 지원합니다.

예를 들어 `@Order` annotation이 테스트 클래스에서 적용되려면 해당하는 정규화된 클래스 이름과 함께 구성 매개변수를 사용하여 [ClassOrderer.OrderAnnotation](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.OrderAnnotation.html) 클래스 순서 지정자를 구성해야 합니다(예: `src/test/resources/junit-platform.properties`에서 ):

```xml
junit.jupiter.testclass.order.default = \
    org.junit.jupiter.api.ClassOrderer$OrderAnnotation
```

구성된 `ClassOrderer`는 모든 최상위 테스트 클래스(`static` nested 테스트 클래스 포함) 및 `@Nested` 테스트 클래스에 적용됩니다.

>최상위 테스트 클래스는 서로 상대적으로 정렬됩니다. 반면 `@Nested` 테스트 클래스는 동일한 엔클로징 클래스를 공유하는 다른 `@Nested` 테스트 클래스와 관련하여 순서가 지정됩니다.

`@Nested` 테스트 클래스에 대해 로컬에서 테스트 클래스 실행 순서를 구성하려면 주문하려는 `@Nested` 테스트 클래스에 대한 엔클로징 클래스에 `@TestClassOrder` annotation을 선언하고 직접 사용하려는 ClassOrderer 구현에 대한 클래스 참조를 제공하십시오. `@TestClassOrder` annotation. 구성된 ClassOrderer는 `@Nested` 테스트 클래스와 `@Nested` 테스트 클래스에 재귀적으로 적용됩니다. 로컬 `@TestClassOrder` 선언은 항상 상속된 `@TestClassOrder` 선언 또는 `junit.jupiter.testclass.order.default` 구성 매개변수를 통해 전역적으로 구성된 ClassOrderer를 재정의합니다.

다음 예제는 `@Nested` 테스트 클래스가 `@Order` annotation을 통해 지정된 순서대로 실행되도록 보장하는 방법을 보여줍니다.

```java
import org.junit.jupiter.api.ClassOrderer;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Order;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestClassOrder;

@TestClassOrder(ClassOrderer.OrderAnnotation.class)
class OrderedNestedTestClassesDemo {

    @Nested
    @Order(1)
    class PrimaryTests {

        @Test
        void test1() {
        }
    }

    @Nested
    @Order(2)
    class SecondaryTests {

        @Test
        void test2() {
        }
    }
}
```

## 2.10. Test Instance Lifecycle

개별 테스트 메소드를 격리하여 실행할 수 있도록 하고 변경 가능한 테스트 인스턴스 상태로 인한 예기치 않은 부작용을 피하기 위해 JUnit은 각 테스트 메소드를 실행하기 전에 각 테스트 클래스의 새 인스턴스를 만듭니다(`테스트 클래스 및 메소드` 참조). 이 "메소드별" 테스트 인스턴스 수명 주기는 JUnit Jupiter의 기본 동작이며 모든 이전 버전의 JUnit과 유사합니다.

>"메소드별" 테스트 인스턴스 수명 주기 모드가 활성화된 경우에도 `조건`(예: `@Disabled`, `@DisabledOnOs` 등)을 통해 주어진 테스트 메소드가 비활성화되면 테스트 클래스가 계속 인스턴스화됩니다.

JUnit Jupiter가 동일한 테스트 인스턴스에서 모든 테스트 메소드를 실행하도록 하려면 테스트 클래스에 `@TestInstance(Lifecycle.PER_CLASS)` 주석을 추가하세요. 이 모드를 사용할 때 새 테스트 인스턴스는 테스트 클래스당 한 번 생성됩니다. 따라서 테스트 메소드가 인스턴스 변수에 저장된 상태에 의존하는 경우 `@BeforeEach` 또는 `@AfterEach` 메소드에서 해당 상태를 재설정해야 할 수 있습니다.

"클래스별" 모드는 기본 "메소드별" 모드에 비해 몇 가지 추가 이점이 있습니다. 특히 "클래스별" 모드를 사용하면 비static method와 인터페이스 기본 메소드에서 `@BeforeAll` 및 `@AfterAll`을 선언할 수 있습니다. 따라서 "클래스별" 모드를 사용하면 `@Nested` 테스트 클래스에서 `@BeforeAll` 및 `@AfterAll` 메소드를 사용할 수도 있습니다.

Kotlin 프로그래밍 언어를 사용하여 테스트를 작성하는 경우 "클래스별" 테스트 인스턴스 수명 주기 모드로 전환하여 `@BeforeAll` 및 `@AfterAll` 메소드를 더 쉽게 구현할 수도 있습니다.

### 2.10.1. Changing the Default Test Instance Lifecycle

테스트 클래스 또는 테스트 인터페이스에 `@TestInstance` annotation이 없으면 JUnit Jupiter는 기본 수명 주기 모드를 사용합니다. 표준 기본 모드는 `PER_METHOD`입니다. 그러나 전체 테스트 계획 실행에 대한 기본값을 변경할 수 있습니다. 기본 테스트 인스턴스 수명 주기 모드를 변경하려면 대소문자를 무시하고 `Junit.jupiter.testinstance.lifecycle.default` 구성 매개변수를 `TestInstance.Lifecycle`에 정의된 열거형 상수의 이름으로 설정합니다. 이는 JVM 시스템 속성, `Launcher`에 전달되는 `LauncherDiscoveryRequest`의 구성 매개변수 또는 JUnit 플랫폼 구성 파일을 통해 제공될 수 있습니다(자세한 내용은 `구성 매개변수` 참조).

예를 들어 기본 테스트 인스턴스 수명 주기 모드를 `Lifecycle.PER_CLASS`로 설정하려면 다음 시스템 속성으로 JVM을 시작할 수 있습니다.

`-Djunit.jupiter.testinstance.lifecycle.default=per_class`

그러나 JUnit 플랫폼 구성 파일을 통해 기본 테스트 인스턴스 수명 주기 모드를 설정하는 것이 더 강력한 솔루션입니다. 구성 파일을 프로젝트와 함께 버전 제어 시스템에 체크인할 수 있고 따라서 IDE 및 빌드 소프트웨어 내에서 사용할 수 있기 때문입니다. 

JUnit 플랫폼 구성 파일을 통해 기본 테스트 인스턴스 수명 주기 모드를 `Lifecycle.PER_CLASS`로 설정하려면 다음 내용으로 클래스 경로(예: `src/test/resources`)의 루트에 `junit-platform.properties`라는 파일을 생성합니다.

`junit.jupiter.testinstance.lifecycle.default = per_class`

>기본 테스트 인스턴스 수명 주기 모드를 변경하면 일관되게 적용하지 않으면 예측할 수 없는 결과와 취약한 빌드가 발생할 수 있습니다. 예를 들어 빌드가 "클래스별" 의미 체계를 기본값으로 구성하지만 IDE의 테스트가 "메소드별" 의미 체계를 사용하여 실행되는 경우 빌드 서버에서 발생하는 오류를 디버그하기 어려울 수 있습니다. 따라서 JVM 시스템 속성을 통하지 않고 JUnit 플랫폼 구성 파일에서 기본값을 변경하는 것이 좋습니다.

## 2.11. Nested Tests

`@Nested` 테스트는 테스트 작성자에게 여러 테스트 그룹 간의 관계를 표현할 수 있는 더 많은 기능을 제공합니다. 이러한 중첩 테스트는 Java의 중첩 클래스를 사용하고 테스트 구조에 대한 계층적 사고를 용이하게 합니다. 다음은 소스 코드와 IDE 내 실행의 스크린샷으로 된 정교한 예입니다.

*Nested test suite for testing a stack*
```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.junit.jupiter.api.Assertions.assertTrue;

import java.util.EmptyStackException;
import java.util.Stack;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;

@DisplayName("A stack")
class TestingAStackDemo {

    Stack<Object> stack;

    @Test
    @DisplayName("is instantiated with new Stack()")
    void isInstantiatedWithNew() {
        new Stack<>();
    }

    @Nested
    @DisplayName("when new")
    class WhenNew {

        @BeforeEach
        void createNewStack() {
            stack = new Stack<>();
        }

        @Test
        @DisplayName("is empty")
        void isEmpty() {
            assertTrue(stack.isEmpty());
        }

        @Test
        @DisplayName("throws EmptyStackException when popped")
        void throwsExceptionWhenPopped() {
            assertThrows(EmptyStackException.class, stack::pop);
        }

        @Test
        @DisplayName("throws EmptyStackException when peeked")
        void throwsExceptionWhenPeeked() {
            assertThrows(EmptyStackException.class, stack::peek);
        }

        @Nested
        @DisplayName("after pushing an element")
        class AfterPushing {

            String anElement = "an element";

            @BeforeEach
            void pushAnElement() {
                stack.push(anElement);
            }

            @Test
            @DisplayName("it is no longer empty")
            void isNotEmpty() {
                assertFalse(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when popped and is empty")
            void returnElementWhenPopped() {
                assertEquals(anElement, stack.pop());
                assertTrue(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when peeked but remains not empty")
            void returnElementWhenPeeked() {
                assertEquals(anElement, stack.peek());
                assertFalse(stack.isEmpty());
            }
        }
    }
}
```

IDE에서 이 예제를 실행할 때 GUI의 테스트 실행 트리는 다음 이미지와 유사하게 보일 것입니다.

![error window](..\images\junit5_guide_writing-tests_nested_test_ide.png)
_Executing a nested test in an IDE_

이 예에서 외부 테스트의 전제 조건은 설정 코드에 대한 계층적 수명 주기 메소드를 정의하여 내부 테스트에서 사용됩니다. 예를 들어 `createNewStack()`은 정의된 테스트 클래스와 정의된 클래스 아래의 중첩 트리의 모든 수준에서 사용되는 `@BeforeEach` 수명 주기 메소드입니다.

내부 테스트가 실행되기 전에 외부 테스트의 설정 코드가 실행된다는 사실은 모든 테스트를 독립적으로 실행할 수 있는 기능을 제공합니다. 외부 테스트의 설정 코드가 항상 실행되기 때문에 외부 테스트를 실행하지 않고 내부 테스트만 실행할 수도 있습니다.

>정적이 아닌 중첩 클래스(예: 내부 클래스)만 `@Nested` 테스트 클래스로 사용할 수 있습니다. 중첩은 임의로 깊을 수 있으며 이러한 내부 클래스는 `@BeforeAll` 및 `@AfterAll` 메소드가 기본적으로 작동하지 않는 한 가지 예외를 제외하고 전체 수명 주기 지원 대상입니다. 그 이유는 Java가 내부 클래스에 정적 멤버를 허용하지 않기 때문입니다. 그러나 이 제한은 `@Nested` 테스트 클래스에 `@TestInstance(Lifecycle.PER_CLASS`) annotation을 추가하여 우회할 수 있습니다(`테스트 인스턴스 수명 주기` 참조).

## 2.12. Dependency Injection for Constructors and Methods

모든 이전 JUnit 버전에서 테스트 생성자 또는 메소드에는 매개변수가 허용되지 않았습니다(적어도 표준 `Runner` 구현에서는 허용되지 않음). JUnit Jupiter의 주요 변경 사항 중 하나로 이제 테스트 생성자와 메소드 모두 매개변수를 가질 수 있습니다. 이것은 더 큰 유연성을 허용하고 생성자와 메소드에 대한 종속성 주입을 가능하게 합니다.

[ParameterResolver](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ParameterResolver.html)는 런타임에 매개변수를 동적으로 확인하려는 테스트 확장을 위한 API를 정의합니다. 테스트 클래스 생성자, 테스트 메소드 또는 수명 주기 메소드(`테스트 클래스 및 메소드` 참조)가 매개변수를 수락하는 경우 매개변수는 등록된 ParameterResolver에 의해 런타임에 확인되어야 합니다.

현재 자동으로 등록되는 3개의 built-in resolver가 있습니다.

- [TestInfoParameterResolver](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-engine/src/main/java/org/junit/jupiter/engine/extension/TestInfoParameterResolver.java): 생성자 또는 메소드 매개변수가 [TestInfo](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestInfo.html) 유형인 경우 `TestInfoParameterResolver`는 현재 컨테이너 또는 테스트에 해당하는 `TestInfo` 인스턴스를 매개변수 값으로 제공합니다. 그런 다음 `TestInfo`를 사용하여 display name, 테스트 클래스, 테스트 메소드 및 관련 태그와 같은 현재 컨테이너 또는 테스트에 대한 정보를 검색할 수 있습니다. display name은 테스트 클래스 또는 테스트 메소드의 이름과 같은 기술적인 이름이거나 `@DisplayName`을 통해 구성된 사용자 지정 이름입니다.
[TestInfo](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestInfo.html)는 JUnit 4의 TestName 규칙에 대한 drop-in 대체 역할을 합니다. 다음은     `TestInfo`를 테스트 생성자, `@BeforeEach` 메소드 및 `@Test` 메소드에 주입하는 방법을 보여줍니다.

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertTrue;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestInfo;

@DisplayName("TestInfo Demo")
class TestInfoDemo {

    TestInfoDemo(TestInfo testInfo) {
        assertEquals("TestInfo Demo", testInfo.getDisplayName());
    }

    @BeforeEach
    void init(TestInfo testInfo) {
        String displayName = testInfo.getDisplayName();
        assertTrue(displayName.equals("TEST 1") || displayName.equals("test2()"));
    }

    @Test
    @DisplayName("TEST 1")
    @Tag("my-tag")
    void test1(TestInfo testInfo) {
        assertEquals("TEST 1", testInfo.getDisplayName());
        assertTrue(testInfo.getTags().contains("my-tag"));
    }

    @Test
    void test2() {
    }

}
```

- [RepetitionInfoParameterResolver](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-engine/src/main/java/org/junit/jupiter/engine/extension/RepetitionInfoParameterResolver.java): `@RepeatedTest`, `@BeforeEach` 또는 `@AfterEach `메소드의 메소드 매개 변수가 [RepetitionInfo](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/RepetitionInfo.html) 유형인 경우 `RepetitionInfoParameterResolver`는 `RepetitionInfo`의 인스턴스를 제공합니다. 그런 다음 `RepetitionInfo`를 사용하여 현재 반복 및 해당 `@RepeatedTest`에 대한 총 반복 횟수에 대한 정보를 검색할 수 있습니다. 그러나 `RepetitionInfoParameterResolver`는 `@RepeatedTest` 컨텍스트 외부에 등록되지 않습니다. `Repeated test` 예를 참조하십시오.

- [TestReporterParameterResolver](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-engine/src/main/java/org/junit/jupiter/engine/extension/TestReporterParameterResolver.java): 생성자 또는 메소드 매개변수가 [TestReporter](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestReporter.html) 유형인 경우 `TestReporterParameterResolver`는 `TestReporter`의 인스턴스를 제공합니다. `TestReporter`는 현재 테스트 실행에 대한 추가 데이터를 게시하는 데 사용할 수 있습니다. 데이터는 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html)의 `ReportingEntryPublished()` 메소드를 통해 사용되어 IDE에서 보거나 보고서에 포함될 수 있습니다.
JUnit Jupiter에서는 JUnit 4에서 `stdout` 또는 `stderr`로 정보를 인쇄하는 데 사용한 `TestReporter`를 사용해야 합니다. `@RunWith(JUnitPlatform.class)`를 사용하면 보고된 모든 항목이 `stdout`으로 출력됩니다. 또한 일부 IDE는 보고서 항목을 `stdout`으로 인쇄하거나 테스트 결과를 위해 사용자 인터페이스에 표시합니다.

```java
class TestReporterDemo {

    @Test
    void reportSingleValue(TestReporter testReporter) {
        testReporter.publishEntry("a status message");
    }

    @Test
    void reportKeyValuePair(TestReporter testReporter) {
        testReporter.publishEntry("a key", "a value");
    }

    @Test
    void reportMultipleKeyValuePairs(TestReporter testReporter) {
        Map<String, String> values = new HashMap<>();
        values.put("user name", "dk38");
        values.put("award year", "1974");

        testReporter.publishEntry(values);
    }

}
```

>`@ExtendWith`를 통해 적절한 `extensions`을 등록하여 다른 매개변수 해석기를 명시적으로 활성화해야 합니다.

사용자 정의 [ParameterResolver](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ParameterResolver.html)의 예는 [RandomParametersExtension](https://github.com/junit-team/junit5-samples/blob/r5.8.2/junit5-jupiter-extensions/src/main/java/com/example/random/RandomParametersExtension.java)을 확인하십시오. production-ready가 되지는 않았지만 확장 모델과 매개변수 확인 프로세스의 단순성과 표현력을 보여줍니다. `MyRandomParametersTest`는 `@Test` 메소드에 임의의 값을 주입하는 방법을 보여줍니다.

```java
@ExtendWith(RandomParametersExtension.class)
class MyRandomParametersTest {

    @Test
    void injectsInteger(@Random int i, @Random int j) {
        assertNotEquals(i, j);
    }

    @Test
    void injectsDouble(@Random double d) {
        assertEquals(0.0, d, 1.0);
    }

}
```

실제 사용 사례의 경우 [MockitoExtension](https://github.com/mockito/mockito/blob/release/2.x/subprojects/junit-jupiter/src/main/java/org/mockito/junit/jupiter/MockitoExtension.java) 및 [SpringExtension](https://github.com/spring-projects/spring-framework/blob/172102d225c916e2ca24b87aa0f0a74d824815b2/spring-test/src/main/java/org/springframework/test/context/junit/jupiter/SpringExtension.java)의 소스 코드를 확인하십시오.

주입할 매개변수의 유형이 [ParameterResolver](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ParameterResolver.html)의 유일한 조건인 경우 일반 [TypeBasedParameterResolver](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-api/src/main/java/org/junit/jupiter/api/extension/support/TypeBasedParameterResolver.java) 기본 클래스를 사용할 수 있습니다. `supportParameters` 메소드는 배후에서 구현되며 매개변수화된 유형을 지원합니다.

## 2.13. Test Interfaces and Default Methods

JUnit Jupiter는 `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory`, `@TestTemplate`, `@BeforeEach` 및 `@AfterEach`가 인터페이스 `default` 메소드에 선언되도록 허용합니다. `@BeforeAll` 및 `@AfterAll`은 테스트 인터페이스의 `static` 메소드 또는 테스트 인터페이스 또는 테스트 클래스에 `@TestInstance(Lifecycle.PER_CLASS)` annotation이 달린 경우 인터페이스 `default` 메소드에서 선언할 수 있습니다(`테스트 인스턴스 수명 주기` 참조). 여기 몇 가지 예가 있어요.

```java
@TestInstance(Lifecycle.PER_CLASS)
interface TestLifecycleLogger {

    static final Logger logger = Logger.getLogger(TestLifecycleLogger.class.getName());

    @BeforeAll
    default void beforeAllTests() {
        logger.info("Before all tests");
    }

    @AfterAll
    default void afterAllTests() {
        logger.info("After all tests");
    }

    @BeforeEach
    default void beforeEachTest(TestInfo testInfo) {
        logger.info(() -> String.format("About to execute [%s]",
            testInfo.getDisplayName()));
    }

    @AfterEach
    default void afterEachTest(TestInfo testInfo) {
        logger.info(() -> String.format("Finished executing [%s]",
            testInfo.getDisplayName()));
    }

}
```
```java
interface TestInterfaceDynamicTestsDemo {

    @TestFactory
    default Stream<DynamicTest> dynamicTestsForPalindromes() {
        return Stream.of("racecar", "radar", "mom", "dad")
            .map(text -> dynamicTest(text, () -> assertTrue(isPalindrome(text))));
    }

}
```

`@ExtendWit`h 및 `@Tag`는 인터페이스를 구현하는 클래스가 태그와 확장을 자동으로 상속하도록 테스트 인터페이스에서 선언할 수 있습니다. `TimingExtension`의 소스 코드는 `테스트 실행 콜백 전후`를 참조하십시오.

```java
@Tag("timed")
@ExtendWith(TimingExtension.class)
interface TimeExecutionLogger {
}
```

In your test class you can then implement these test interfaces to have them applied.

```java
class TestInterfaceDemo implements TestLifecycleLogger,
        TimeExecutionLogger, TestInterfaceDynamicTestsDemo {

    @Test
    void isEqualValue() {
        assertEquals(1, "a".length(), "is always equal");
    }

}
```

TestInterfaceDemo를 실행하면 다음과 유사한 결과가 출력됩니다 :

```
INFO  example.TestLifecycleLogger - Before all tests
INFO  example.TestLifecycleLogger - About to execute [dynamicTestsForPalindromes()]
INFO  example.TimingExtension - Method [dynamicTestsForPalindromes] took 19 ms.
INFO  example.TestLifecycleLogger - Finished executing [dynamicTestsForPalindromes()]
INFO  example.TestLifecycleLogger - About to execute [isEqualValue()]
INFO  example.TimingExtension - Method [isEqualValue] took 1 ms.
INFO  example.TestLifecycleLogger - Finished executing [isEqualValue()]
INFO  example.TestLifecycleLogger - After all tests
```

이 기능의 또 다른 가능한 응용 프로그램은 인터페이스 계약에 대한 테스트를 작성하는 것입니다. 예를 들어 `Object.equals` 또는 `Comparable.compareTo`의 구현이 다음과 같이 동작해야 하는 방법에 대한 테스트를 작성할 수 있습니다.

```java
public interface Testable<T> {

    T createValue();

}
```

```java
public interface EqualsContract<T> extends Testable<T> {

    T createNotEqualValue();

    @Test
    default void valueEqualsItself() {
        T value = createValue();
        assertEquals(value, value);
    }

    @Test
    default void valueDoesNotEqualNull() {
        T value = createValue();
        assertFalse(value.equals(null));
    }

    @Test
    default void valueDoesNotEqualDifferentValue() {
        T value = createValue();
        T differentValue = createNotEqualValue();
        assertNotEquals(value, differentValue);
        assertNotEquals(differentValue, value);
    }

}
```

```java
public interface ComparableContract<T extends Comparable<T>> extends Testable<T> {

    T createSmallerValue();

    @Test
    default void returnsZeroWhenComparedToItself() {
        T value = createValue();
        assertEquals(0, value.compareTo(value));
    }

    @Test
    default void returnsPositiveNumberWhenComparedToSmallerValue() {
        T value = createValue();
        T smallerValue = createSmallerValue();
        assertTrue(value.compareTo(smallerValue) > 0);
    }

    @Test
    default void returnsNegativeNumberWhenComparedToLargerValue() {
        T value = createValue();
        T smallerValue = createSmallerValue();
        assertTrue(smallerValue.compareTo(value) < 0);
    }

}
```

그런 다음 테스트 클래스에서 두 계약 인터페이스를 모두 구현하여 해당 테스트를 상속할 수 있습니다. 물론 abstract method를 구현해야 합니다.

```java
class StringTests implements ComparableContract<String>, EqualsContract<String> {

    @Override
    public String createValue() {
        return "banana";
    }

    @Override
    public String createSmallerValue() {
        return "apple"; // 'a' < 'b' in "banana"
    }

    @Override
    public String createNotEqualValue() {
        return "cherry";
    }

}
```

>위의 테스트는 예시일 뿐이므로 완전하지 않습니다.

## 2.14. Repeated Tests

JUnit Jupiter는 `@RepeatedTest`로 메소드에 annotation을 달고 원하는 총 반복 횟수를 지정하여 지정된 횟수만큼 테스트를 반복할 수 있는 기능을 제공합니다. 반복되는 테스트의 각 호출은 동일한 수명 주기 콜백 및 확장을 완벽하게 지원하는 일반 `@Test` 메소드의 실행처럼 작동합니다.

다음 예제는 자동으로 10번 반복되는 `repeatTest()`라는 테스트를 선언하는 방법을 보여줍니다.

```java
@RepeatedTest(10)
void repeatedTest() {
    // ...
}
```

반복 횟수를 지정하는 것 외에도 `@RepeatedTest` annotation의 name 속성을 통해 각 반복에 대해 사용자 지정 display name을 구성할 수 있습니다. 또한 display name은 정적 텍스트와 동적 자리 표시자의 조합으로 구성된 패턴일 수 있습니다. 다음 자리 표시자가 현재 지원됩니다.

- `{displayName}`: `@RepeatedTest` 메소드의 display name
- `{currentRepetition}`: 현재 반복 횟수
- `{totalRepetitions}`: 총 반복 횟수

지정된 반복의 기본 display name은 `"repetition {currentRepetition} of {totalRepetitions}"` 패턴을 기반으로 생성됩니다. 따라서 이전 `repeatTest()` 예제의 개별 반복에 대한 display name은 `10번의 반복 1, 10의 반복 2` 등입니다. 각 반복의 이름에 `@RepeatedTest` 메소드의 display name을 포함하려면, 사용자 정의 패턴을 정의하거나 사전 정의된 `RepeatedTest.LONG_DISPLAY_NAME` 패턴을 사용할 수 있습니다. 후자는 `"{displayName} :: repeat {currentRepetition} of {totalRepetitions}"`와 같으며, 이는 `repeatTest() :: 반복 1/10, repeatTest() :: 반복 2/10 `등과 같은 개별 반복에 대한 display name을 생성합니다.

프로그래밍 방식으로 현재 반복 및 총 반복 횟수에 대한 정보를 검색하기 위해 개발자는 `RepetitionInfo` 인스턴스를 `@RepeatedTest`, `@BeforeEach` 또는 `@AfterEach` 메소드에 주입하도록 선택할 수 있습니다.

### 2.14.1. Repeated Test Examples

이 섹션의 끝에 있는 RepeatedTestsDemo 클래스는 `Repeated test`의 몇 가지 예를 보여줍니다.

`repeatTest()` method는 이전 섹션의 예제와 동일합니다. 반면, `repeatTestWithRepetitionInfo()`는 현재 반복되는 테스트의 총 반복 횟수에 액세스하기 위해 테스트에 `RepetitionInfo` 인스턴스를 주입하는 방법을 보여줍니다.

다음 두 method는 각 반복의 display name에 `@RepeatedTest` 메소드에 대한 사용자 지정 `@DisplayName`을 포함하는 방법을 보여줍니다. `customDisplayName()`은 사용자 지정 display name을 사용자 지정 패턴과 결합한 다음 `TestInfo`를 사용하여 생성된 display name의 형식을 확인합니다. `Repeat!`는 `@DisplayName` 선언에서 가져온 `{displayName}`이고 `1/1`은 `{currentRepetition}/{totalRepetitions}`에서 가져옵니다. 대조적으로 `customDisplayNameWithLongPattern()`은 앞서 정의된 `RepeatedTest.LONG_DISPLAY_NAME` 패턴을 사용합니다.

`repeatTestInGerman()`는 repeated test의 display name을 외국어로 번역하는 기능을 보여줍니다.  — 이 경우 독일어, 결과적으로 `Wiederholung 1 von 5, Wiederholung 2 von 5` 등과 같은 개별 반복에 대한 이름이 생성됩니다.

`beforeEach()` method는 `@BeforeEach` annotation이 달려 있으므로 각 repeated test가 반복되기 전에 실행됩니다. `TestInfo`와 `RepetitionInfo`를 메소드에 주입함으로써 현재 실행 중인 repeated test에 대한 정보를 얻을 수 있음을 알 수 있습니다. `INFO` 로그 수준이 활성화된 상태에서 `RepeatedTestsDemo`를 실행하면 다음 출력이 생성됩니다.

```
INFO: About to execute repetition 1 of 10 for repeatedTest
INFO: About to execute repetition 2 of 10 for repeatedTest
INFO: About to execute repetition 3 of 10 for repeatedTest
INFO: About to execute repetition 4 of 10 for repeatedTest
INFO: About to execute repetition 5 of 10 for repeatedTest
INFO: About to execute repetition 6 of 10 for repeatedTest
INFO: About to execute repetition 7 of 10 for repeatedTest
INFO: About to execute repetition 8 of 10 for repeatedTest
INFO: About to execute repetition 9 of 10 for repeatedTest
INFO: About to execute repetition 10 of 10 for repeatedTest
INFO: About to execute repetition 1 of 5 for repeatedTestWithRepetitionInfo
INFO: About to execute repetition 2 of 5 for repeatedTestWithRepetitionInfo
INFO: About to execute repetition 3 of 5 for repeatedTestWithRepetitionInfo
INFO: About to execute repetition 4 of 5 for repeatedTestWithRepetitionInfo
INFO: About to execute repetition 5 of 5 for repeatedTestWithRepetitionInfo
INFO: About to execute repetition 1 of 1 for customDisplayName
INFO: About to execute repetition 1 of 1 for customDisplayNameWithLongPattern
INFO: About to execute repetition 1 of 5 for repeatedTestInGerman
INFO: About to execute repetition 2 of 5 for repeatedTestInGerman
INFO: About to execute repetition 3 of 5 for repeatedTestInGerman
INFO: About to execute repetition 4 of 5 for repeatedTestInGerman
INFO: About to execute repetition 5 of 5 for repeatedTestInGerman
```

```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import java.util.logging.Logger;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.RepeatedTest;
import org.junit.jupiter.api.RepetitionInfo;
import org.junit.jupiter.api.TestInfo;

class RepeatedTestsDemo {

    private Logger logger = // ...

    @BeforeEach
    void beforeEach(TestInfo testInfo, RepetitionInfo repetitionInfo) {
        int currentRepetition = repetitionInfo.getCurrentRepetition();
        int totalRepetitions = repetitionInfo.getTotalRepetitions();
        String methodName = testInfo.getTestMethod().get().getName();
        logger.info(String.format("About to execute repetition %d of %d for %s", //
            currentRepetition, totalRepetitions, methodName));
    }

    @RepeatedTest(10)
    void repeatedTest() {
        // ...
    }

    @RepeatedTest(5)
    void repeatedTestWithRepetitionInfo(RepetitionInfo repetitionInfo) {
        assertEquals(5, repetitionInfo.getTotalRepetitions());
    }

    @RepeatedTest(value = 1, name = "{displayName} {currentRepetition}/{totalRepetitions}")
    @DisplayName("Repeat!")
    void customDisplayName(TestInfo testInfo) {
        assertEquals("Repeat! 1/1", testInfo.getDisplayName());
    }

    @RepeatedTest(value = 1, name = RepeatedTest.LONG_DISPLAY_NAME)
    @DisplayName("Details...")
    void customDisplayNameWithLongPattern(TestInfo testInfo) {
        assertEquals("Details... :: repetition 1 of 1", testInfo.getDisplayName());
    }

    @RepeatedTest(value = 5, name = "Wiederholung {currentRepetition} von {totalRepetitions}")
    void repeatedTestInGerman() {
        // ...
    }

}
```

유니코드 테마가 활성화된 `ConsoleLauncher`를 사용할 때 `RepeatedTestsDemo`를 실행하면 콘솔에 다음과 같은 출력이 표시됩니다.

```
├─ RepeatedTestsDemo ✔
│  ├─ repeatedTest() ✔
│  │  ├─ repetition 1 of 10 ✔
│  │  ├─ repetition 2 of 10 ✔
│  │  ├─ repetition 3 of 10 ✔
│  │  ├─ repetition 4 of 10 ✔
│  │  ├─ repetition 5 of 10 ✔
│  │  ├─ repetition 6 of 10 ✔
│  │  ├─ repetition 7 of 10 ✔
│  │  ├─ repetition 8 of 10 ✔
│  │  ├─ repetition 9 of 10 ✔
│  │  └─ repetition 10 of 10 ✔
│  ├─ repeatedTestWithRepetitionInfo(RepetitionInfo) ✔
│  │  ├─ repetition 1 of 5 ✔
│  │  ├─ repetition 2 of 5 ✔
│  │  ├─ repetition 3 of 5 ✔
│  │  ├─ repetition 4 of 5 ✔
│  │  └─ repetition 5 of 5 ✔
│  ├─ Repeat! ✔
│  │  └─ Repeat! 1/1 ✔
│  ├─ Details... ✔
│  │  └─ Details... :: repetition 1 of 1 ✔
│  └─ repeatedTestInGerman() ✔
│     ├─ Wiederholung 1 von 5 ✔
│     ├─ Wiederholung 2 von 5 ✔
│     ├─ Wiederholung 3 von 5 ✔
│     ├─ Wiederholung 4 von 5 ✔
│     └─ Wiederholung 5 von 5 ✔
```

## 2.15. Parameterized Tests

parameterized test를 사용하면 다른 인수로 테스트를 여러 번 실행할 수 있습니다. 그것들은 일반 `@Test` 메소드처럼 선언되지만 대신 [@ParameterizedTest](https://junit.org/junit5/docs/current/api/org.junit.jupiter.params/org/junit/jupiter/params/ParameterizedTest.html) annotation을 사용합니다. 또한 각 호출에 대한 인수를 제공한 다음 테스트 메소드에서 인수를 사용할 소스를 하나 이상 선언해야 합니다.

다음 예제는 `@ValueSource` annotation을 사용하여 `String` 배열을 인수의 소스로 지정하는 parameterized test를 보여줍니다.

```java
@ParameterizedTest
@ValueSource(strings = { "racecar", "radar", "able was I ere I saw elba" })
void palindromes(String candidate) {
    assertTrue(StringUtils.isPalindrome(candidate));
}
```

위의 parameterized test 방법을 실행할 때 각 호출은 별도로 보고됩니다. 예를 들어, `ConsoleLauncher`는 다음과 유사한 출력을 인쇄합니다.

```
palindromes(String) ✔
├─ [1] candidate=racecar ✔
├─ [2] candidate=radar ✔
└─ [3] candidate=able was I ere I saw elba ✔
```

### 2.15.1. Required Setup

parameterized test를 사용하려면 `junit-jupiter-params` artifact에 종속성을 추가해야 합니다. 자세한 내용은 `Dependency Metadata`를 참조하세요.

### 2.15.2. Consuming Arguments

`parameterized test` method는 일반적으로 인수 소스 인덱스와 메소드 매개변수 인덱스 간의 일대일 상관 관계에 따라 구성된 소스('Source of Arguments' 참조)에서 직접 인수를 사용합니다(`@CsvSource`의 예 참조). 그러나 `parameterized test` method는 소스의 인수를 메소드에 전달된 단일 개체로 집계하도록 선택할 수도 있습니다(`Argument Aggregation` 참조). `ParameterResolver`에서 추가 인수를 제공할 수도 있습니다(예: `TestInfo`, `TestReporter` 등의 인스턴스를 얻기 위해). 특히 `parameterized test` 방법은 다음 규칙에 따라 형식 매개변수를 선언해야 합니다.

- 0개 이상의 *indexed arguments 인덱싱된 인수*를 먼저 선언해야 합니다.
- 다음에 0개 이상의 *aggregators집계자*를 선언해야 합니다.
- `ParameterResolver`에서 제공하는 0개 이상의 인수는 마지막에 선언되어야 합니다.

이 컨텍스트에서 인덱싱된 인수는 메소드의 형식 매개 변수 목록에서 동일한 인덱스의 매개 변수화된 메소드에 인수로 전달되는 *ArgumentsProvider*에서 제공하는 *Arguments*의 지정된 인덱스에 대한 인수입니다. *aggregator집계자*는 *ArgumentsAccessor* 유형의 매개변수 또는 *@AggregateWith*로 annotation이 달린 매개변수입니다.

>*AutoCloseable arguments*
`java.lang.AutoCloseable`(또는 `java.lang.AutoCloseable`을 확장하는 `java.io.Closeabl`e)을 구현하는 인수는 `@AfterEach` 메소드 및 `AfterEachCallback` 확장이 현재 `parameterized test` 호출에 대해 호출된 후 자동으로 닫힙니다.

>이를 방지하려면 `@ParameterizedTest`의 `autoCloseArguments` 속성을 `false`로 설정하십시오. 특히 `AutoCloseable`을 구현하는 인수가 동일한 매개 변수화된 테스트 메소드의 여러 호출에 재사용되는 경우 호출 간에 인수가 닫히지 않도록 `@ParameterizedTest(autoCloseArguments = false)`로 메소드에 annotation을 달아야 합니다.

### 2.15.3. Sources of Arguments

JUnit Jupiter는 기본적으로 몇 가지 소스 annotation을 제공합니다. 다음 subsection은 각각에 대한 간략한 개요와 예를 제공합니다. 추가 정보는 [org.junit.jupiter.params.provider](https://junit.org/junit5/docs/current/api/org.junit.jupiter.params/org/junit/jupiter/params/provider/package-summary.html) 패키지의 Javadoc을 참조하십시오.

#### @ValueSource
`@ValueSource`는 가장 간단한 가능한 소스 중 하나입니다. 이를 통해 리터럴 값의 단일 배열을 지정할 수 있으며 매개 변수화된 테스트 호출당 단일 인수를 제공하는 데만 사용할 수 있습니다.

`@ValueSource`는 다음 유형의 리터럴 값을 지원합니다.

- short
- byte
- int
- long
- float
- double
- char
- boolean
- java.lang.String
- java.lang.Class

예를 들어 다음 `@ParameterizedTes`t method는 각각 1, 2, 3 값으로 세 번 호출됩니다.

```java
@ParameterizedTest
@ValueSource(ints = { 1, 2, 3 })
void testWithValueSource(int argument) {
    assertTrue(argument > 0 && argument < 4);
}
```

*Null and Empty Sources*

코너 케이스를 확인하고 잘못된 입력이 제공될 때 소프트웨어의 올바른 동작을 확인하려면 `parameterized test`에 null 및 빈 값을 제공하는 것이 유용할 수 있습니다. 다음 annotation은 단일 인수를 허용하는 `parameterized test`에 대한 null 및 빈 값의 소스 역할을 합니다.

>- [@NullSource](https://junit.org/junit5/docs/current/api/org.junit.jupiter.params/org/junit/jupiter/params/provider/NullSource.html): annotation이 달린 `@ParameterizedTest` 메소드에 단일 null 인수를 제공합니다.
    - `@NullSource`는 프리미티브 유형의 매개변수에 사용할 수 없습니다.
>- [@EmptySource](https://junit.org/junit5/docs/current/api/org.junit.jupiter.params/org/junit/jupiter/params/provider/EmptySource.html): `java.lang.String`, `java.util.List`, `java.util.Set`, `java.util.Map`, 기본 배열(예: , `int[]`, `char[][]` 등), 객체 배열(예: `String[]`, `Integer[][]` 등).
    - 지원되는 유형의 subtype은 지원되지 않습니다.
>- [@NullAndEmptySource](https://junit.org/junit5/docs/current/api/org.junit.jupiter.params/org/junit/jupiter/params/provider/NullAndEmptySource.html): `@NullSource`와 `@EmptySource`의 기능을 결합한 합성 annotation.

`parameterized test`에 다양한 유형의 빈 문자열을 제공해야 하는 경우 `@ValueSource` — 를 사용하여 이를 달성할 수 있습니다(예: `@ValueSource(strings = {" ", " ", "\t", "\n"}`) .

`@NullSource`, `@EmptySource` 및 `@ValueSource`를 결합하여 더 넓은 범위의 `null`, 공백 및 공백 입력을 테스트할 수도 있습니다. 다음 예제는 문자열에 대해 이를 달성하는 방법을 보여줍니다.

```java
@ParameterizedTest
@NullSource
@EmptySource
@ValueSource(strings = { " ", "   ", "\t", "\n" })
void nullEmptyAndBlankStrings(String text) {
    assertTrue(text == null || text.trim().isEmpty());
}
```

합성된 `@NullAndEmptySource` annotation을 사용하면 위의 내용이 다음과 같이 단순화됩니다.

```java
@ParameterizedTest
@NullAndEmptySource
@ValueSource(strings = { " ", "   ", "\t", "\n" })
void nullEmptyAndBlankStrings(String text) {
    assertTrue(text == null || text.trim().isEmpty());
}
```

>`nullEmptyAndBlankStrings(String)` 매개 변수화된 테스트 메소드의 두 변형은 모두 6개의 호출을 생성합니다. `null`의 경우 1, 빈 문자열의 경우 1, `@ValueSource`를 통해 제공된 명시적 빈 문자열의 경우 4입니다.

#### @EnumSource

`@EnumSource`는 Enum 상수를 사용하는 편리한 방법을 제공합니다.

```java
@ParameterizedTest
@EnumSource(ChronoUnit.class)
void testWithEnumSource(TemporalUnit unit) {
    assertNotNull(unit);
}
```

annotation의 `value` 속성은 선택 사항입니다. 생략하면 첫 번째 메소드 매개변수의 선언된 유형이 사용됩니다. 열거형을 참조하지 않으면 테스트가 실패합니다. 따라서 위의 예에서 `value` 속성은 메소드 매개변수가 `TemporalUnit`, 즉 열거형이 아닌 `ChronoUnit`에 의해 구현된 인터페이스로 선언되었기 때문에 필요합니다. 메소드 매개변수 유형을 `ChronoUnit`으로 변경하면 다음과 같이 annotation에서 명시적 열거형 유형을 생략할 수 있습니다.

```java
@ParameterizedTest
@EnumSource
void testWithEnumSourceWithAutoDetection(ChronoUnit unit) {
    assertNotNull(unit);
}
```

annotation은 다음 예제와 같이 사용할 상수를 지정할 수 있는 선택적 `name` attribute를 제공합니다. 생략하면 모든 상수가 사용됩니다.

```java
@ParameterizedTest
@EnumSource(names = { "DAYS", "HOURS" })
void testWithEnumSourceInclude(ChronoUnit unit) {
    assertTrue(EnumSet.of(ChronoUnit.DAYS, ChronoUnit.HOURS).contains(unit));
}
```

`@EnumSource` annotation은 또한 테스트 메소드에 전달되는 상수를 세밀하게 제어할 수 있는 선택적 `mode` attribute를 제공합니다. 예를 들어 열거형 상수 풀에서 이름을 제외하거나 다음 예와 같이 정규식을 지정할 수 있습니다.

```java
@ParameterizedTest
@EnumSource(mode = EXCLUDE, names = { "ERAS", "FOREVER" })
void testWithEnumSourceExclude(ChronoUnit unit) {
    assertFalse(EnumSet.of(ChronoUnit.ERAS, ChronoUnit.FOREVER).contains(unit));
}
@ParameterizedTest
@EnumSource(mode = MATCH_ALL, names = "^.*DAYS$")
void testWithEnumSourceRegex(ChronoUnit unit) {
    assertTrue(unit.name().endsWith("DAYS"));
}
```

#### @MethodSource

`@MethodSource`를 사용하면 테스트 클래스 또는 외부 클래스의 하나 이상의 `factory` 메소드를 참조할 수 있습니다.

테스트 클래스가 `@TestInstance(Lifecycle.PER_CLASS)`로 annotation 처리되지 않는 한 테스트 클래스 내의 factory method는 `static`이어야 합니다. 반면 외부 클래스의 factory method는 항상 `static`이어야 합니다. 또한 이러한 factory method는 인수를 허용하지 않아야 합니다.

각 factory method는 인수 stream을 생성해야 하며 stream 내의 각 인수 세트는 annotation이 달린 `@ParameterizedTest` 메소드의 개별 호출에 대한 물리적 인수로 제공됩니다. 일반적으로 이것은 `stream of arguments 인수 스트림`(예: `Stream<Arguments>`)으로 변환됩니다. 그러나 실제 구체적인 반환 유형은 다양한 형태를 취할 수 있습니다. 이 컨텍스트에서 "stream"은 `Stream`, `DoubleStream`, `LongStream`, `IntStream`, `Collection`, `Iterator`, `Iterable`, `객체 배열` 또는 `기본 요소 배열`과 같이 JUnit이 Stream으로 안정적으로 변환할 수 있는 모든 것입니다. stream 내의 "인수"는 `Arguments`의 인스턴스, 객체 배열(예: `Object[]`) 또는 `parameterized test` 메소드가 단일 인수를 허용하는 경우 단일 값으로 제공될 수 있습니다.

단일 매개변수만 필요한 경우 다음 예제와 같이 매개변수 유형 인스턴스의 `Stream`을 반환할 수 있습니다.

```java
@ParameterizedTest
@MethodSource("stringProvider")
void testWithExplicitLocalMethodSource(String argument) {
    assertNotNull(argument);
}

static Stream<String> stringProvider() {
    return Stream.of("apple", "banana");
}
```

`@MethodSource`를 통해 `factory` 메소드 이름을 명시적으로 제공하지 않으면 JUnit Jupiter는 규칙에 따라 현재 `@ParameterizedTest` 메소드와 동일한 이름을 갖는 factory method를 검색합니다. 이것은 다음 예에서 보여줍니다.

```java
@ParameterizedTest
@MethodSource
void testWithDefaultLocalMethodSource(String argument) {
    assertNotNull(argument);
}

static Stream<String> testWithDefaultLocalMethodSource() {
    return Stream.of("apple", "banana");
}
```

다음 예제와 같이 기본 유형(`DoubleStream`, `IntStream` 및 `LongStream`)에 대한 stream도 지원됩니다.

```java
@ParameterizedTest
@MethodSource("range")
void testWithRangeMethodSource(int argument) {
    assertNotEquals(9, argument);
}

static IntStream range() {
    return IntStream.range(0, 20).skip(10);
}
```

매개 변수화된 테스트 메소드가 여러 매개 변수를 선언하는 경우 아래와 같이 `Arguments` 인스턴스 또는 개체 배열의 collection, stream 또는 배열을 반환해야 합니다(지원되는 반환 유형에 대한 자세한 내용은 `@MethodSource`에 대한 Javadoc 참조). `arguments(Object…​)`는 Arguments 인터페이스에 정의된 정적 factory method입니다. 또한 `Arguments.of(Object…​)`는 `arguments(Object…​)`의 대안으로 사용될 수 있습니다.

```java
@ParameterizedTest
@MethodSource("stringIntAndListProvider")
void testWithMultiArgMethodSource(String str, int num, List<String> list) {
    assertEquals(5, str.length());
    assertTrue(num >=1 && num <=2);
    assertEquals(2, list.size());
}

static Stream<Arguments> stringIntAndListProvider() {
    return Stream.of(
        arguments("apple", 1, Arrays.asList("a", "b")),
        arguments("lemon", 2, Arrays.asList("x", "y"))
    );
}
```

다음 예제와 같이 *정규화된 메소드 이름*을 제공하여 외부의 `static` *factory* method를 참조할 수 있습니다.

```java
package example;

import java.util.stream.Stream;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.MethodSource;

class ExternalMethodSourceDemo {

    @ParameterizedTest
    @MethodSource("example.StringsProviders#tinyStrings")
    void testWithExternalMethodSource(String tinyString) {
        // test with tiny string
    }
}

class StringsProviders {

    static Stream<String> tinyStrings() {
        return Stream.of(".", "oo", "OOO");
    }
}
```

#### @CsvSource

`@CsvSource`를 사용하면 인수 목록을 쉼표로 구분된 값(즉, CSV 문자열 리터럴)으로 표현할 수 있습니다. `@CsvSource`의 값 속성을 통해 제공된 각 문자열은 CSV 레코드를 나타내며 `parameterized test`가 한 번 호출됩니다. 첫 번째 레코드는 선택적으로 CSV 헤더를 제공하는 데 사용할 수 있습니다(자세한 내용과 예제는 `useHeadersInDisplayName` 속성에 대한 Javadoc 참조).

```java
@ParameterizedTest
@CsvSource({
    "apple,         1",
    "banana,        2",
    "'lemon, lime', 0xF1",
    "strawberry,    700_000"
})
void testWithCsvSource(String fruit, int rank) {
    assertNotNull(fruit);
    assertNotEquals(0, rank);
}
```

기본 구분자는 쉼표(`,`)이지만 `delimiter` 속성을 설정하여 다른 문자를 사용할 수 있습니다. 또는 `delimiterString` 속성을 사용하면 단일 문자 대신 문자열 구분 기호를 사용할 수 있습니다. 그러나 두 구분 기호 속성을 동시에 설정할 수는 없습니다.

기본적으로 @CsvSource는 작은따옴표(`'`)를 인용 문자로 사용하지만 이는 `quoteCharacter` 속성을 통해 변경할 수 있습니다. 위의 예와 아래 표에서 `'레몬, 라임'` 값을 참조하십시오. 비어 있고 따옴표로 묶인 값('')은 `emptyValue` 속성이 설정되지 않은 한 빈 `String`이 됩니다. 반면 완전히 비어 있는 값은 `null` 참조로 해석됩니다. 하나 이상의 `nullValue를` 지정하여 사용자 지정 값을 null 참조로 해석할 수 있습니다(아래 표의 NIL 예 참조). null 참조의 대상 유형이 기본 유형인 경우 `ArgumentConversionException`이 발생합니다.

따옴표가 없는 빈 값은 `nullValues` 속성을 통해 구성된 사용자 지정 값에 관계없이 항상 null 참조로 변환됩니다.

따옴표로 묶인 문자열 내를 제외하고 CSV 열의 선행 및 후행 공백은 기본적으로 잘립니다. 이 동작은 `ignoreLeadingAndTrailingWhitespace` 속성을 `true`로 설정하여 변경할 수 있습니다.

|Example Input|Resulting Argument List|
|---|---|
|@CsvSource({ "apple, banana" })|`"apple"`, `"banana"`|
|@CsvSource({ "apple, 'lemon, lime'" })|`"apple"`, `"lemon`, `lime"`|
|@CsvSource({ "apple, ''" })|`"apple", ""`|
|@CsvSource({ "apple, " })|`"apple"`, `nul`l`|
|@CsvSource(value = { "apple, banana, NIL" }, nullValues = "NIL")|`"apple"`, `"banana"`, `null`|
|@CsvSource(value = { " apple , banana" }, ignoreLeadingAndTrailingWhitespace = false)|`" apple "`, `" banana"`|

사용 중인 프로그래밍 언어가 텍스트 블록 (예: Java SE 15 이상 )을 지원하는 경우 `@CsvSource`의 `textBlock` 속성을 대신 사용할 수 있습니다. 텍스트 블록 내의 각 레코드는 CSV 레코드를 나타내며 `parameterized test`가 한 번 호출됩니다. 첫 번째 레코드는 선택적으로 아래 예와 같이 `useHeadersInDisplayName` 속성을 `true`로 설정하여 CSV 헤더를 제공하는 데 사용할 수 있습니다.

텍스트 블록을 사용하여 이전 예제를 다음과 같이 구현할 수 있습니다.

```java
@ParameterizedTest(name = "[{index}] {arguments}")
@CsvSource(useHeadersInDisplayName = true, textBlock = """
    FRUIT,         RANK
    apple,         1
    banana,        2
    'lemon, lime', 0xF1
    strawberry,    700_000
    """)
void testWithCsvSource(String fruit, int rank) {
    // ...
}
```

이전 예에서 생성된 display name에는 CSV 헤더 이름이 포함됩니다.

```
[1] FRUIT = apple, RANK = 1
[2] FRUIT = banana, RANK = 2
[3] FRUIT = lemon, lime, RANK = 0xF1
[4] FRUIT = strawberry, RANK = 700_000
```

값 속성을 통해 제공되는 CSV 레코드와 달리 텍스트 블록에는 annotation이 포함될 수 있습니다. `#` 기호로 시작하는 줄은 annotation으로 처리되어 무시됩니다. 그러나 `#` 기호는 선행 공백 없이 행의 첫 번째 문자여야 합니다. 따라서 닫는 텍스트 블록 구분 기호(`"""`)를 입력의 마지막 줄이나 다음 줄에 배치하고 나머지 입력과 왼쪽 정렬하는 것이 좋습니다(아래 예에서 볼 수 있음). 표와 유사한 형식을 보여줍니다.

```java
@ParameterizedTest
@CsvSource(delimiter = '|', quoteCharacter = '"', textBlock = """
    #-----------------------------
    #    FRUIT     |     RANK
    #-----------------------------
         apple     |      1
    #-----------------------------
         banana    |      2
    #-----------------------------
      "lemon lime" |     0xF1
    #-----------------------------
       strawberry  |    700_000
    #-----------------------------
    """)
void testWithCsvSource(String fruit, int rank) {
    // ...
}
```

>Java의 [text block](https://docs.oracle.com/en/java/javase/15/text-blocks/index.html) 기능은 코드가 컴파일될 때 부수적인 공백을 자동으로 제거합니다. 그러나 Groovy 및 Kotlin과 같은 다른 JVM 언어는 그렇지 않습니다. 따라서 Java 이외의 프로그래밍 언어를 사용 중이고 텍스트 블록에 따옴표 붙은 문자열 내에 annotation이나 새 줄이 포함되어 있는 경우 텍스트 블록 내에 선행 공백이 없는지 확인해야 합니다.

#### @CsvFileSource

`@CsvFileSource`를 사용하면 클래스 경로 또는 로컬 파일 시스템에서 쉼표로 구분된 값(CSV) 파일을 사용할 수 있습니다. CSV 파일의 각 레코드는 `parameterized test`를 한 번 호출합니다. 첫 번째 레코드는 선택적으로 CSV 헤더를 제공하는 데 사용할 수 있습니다. `numLinesToSkip` 속성을 통해 헤더를 무시하도록 JUnit에 지시할 수 있습니다. display name에 헤더를 사용하려면 `useHeadersInDisplayName` 속성을 `true`로 설정하면 됩니다. 아래 예는 `numLinesToSkip` 및 `useHeadersInDisplayName` 사용을 보여줍니다.

기본 구분자는 쉼표(`,`)이지만 `delimiter` 속성을 설정하여 다른 문자를 사용할 수 있습니다. 또는 `delimiterString` 속성을 사용하면 단일 문자 대신 문자열 구분 기호를 사용할 수 있습니다. 그러나 두 구분 기호 속성을 동시에 설정할 수는 없습니다.

>*Comments in CSV files*
`#` 기호로 시작하는 줄은 annotation으로 해석되어 무시됩니다.

```java
@ParameterizedTest
@CsvFileSource(resources = "/two-column.csv", numLinesToSkip = 1)
void testWithCsvFileSourceFromClasspath(String country, int reference) {
    assertNotNull(country);
    assertNotEquals(0, reference);
}

@ParameterizedTest
@CsvFileSource(files = "src/test/resources/two-column.csv", numLinesToSkip = 1)
void testWithCsvFileSourceFromFile(String country, int reference) {
    assertNotNull(country);
    assertNotEquals(0, reference);
}

@ParameterizedTest(name = "[{index}] {arguments}")
@CsvFileSource(resources = "/two-column.csv", useHeadersInDisplayName = true)
void testWithCsvFileSourceAndHeaders(String country, int reference) {
    assertNotNull(country);
    assertNotEquals(0, reference);
}
```

*two-column.csv*
```
COUNTRY, REFERENCE
Sweden, 1
Poland, 2
"United States of America", 3
France, 700_000
```

다음 목록은 위의 처음 두 개의 `parameterized test` 방법에 대해 생성된 display name을 보여줍니다.

```
[1] country=Sweden, reference=1
[2] country=Poland, reference=2
[3] country=United States of America, reference=3
[4] country=France, reference=700_000
```

다음 목록은 CSV 헤더 이름을 사용하는 위의 마지막 `parameterized test` 방법에 대해 생성된 display name을 보여줍니다.

```
[1] COUNTRY = Sweden, REFERENCE = 1
[2] COUNTRY = Poland, REFERENCE = 2
[3] COUNTRY = United States of America, REFERENCE = 3
[4] COUNTRY = France, REFERENCE = 700_000
```

`@CsvSource`에서 사용되는 기본 구문과 달리 `@CsvFileSourc`e는 기본적으로 큰따옴표(")를 인용 문자로 사용하지만 이는 `quoteCharacter` 속성을 통해 변경할 수 있습니다. 위의 예에서 `"United States of America"` 값을 참조하십시오. . 따옴표로 묶인 비어 있는 값(`""`)은 `emptyValue` 속성이 설정되지 않은 한 빈 `String`이 됩니다. 반면에 완전히 비어 있는 값은 `null` 참조로 해석됩니다. 

>하나 이상의 `nullValue`를 지정하면 사용자 정의 값을 `null` 참조 `null` 참조의 대상 유형이 기본 유형인 경우 `ArgumentConversionException`이 발생합니다.

>따옴표가 없는 빈 값은 `nullValues` 속성을 통해 구성된 사용자 지정 값에 관계없이 항상 `null` 참조로 변환됩니다.
따옴표로 묶인 문자열 내를 제외하고 CSV 열의 선행 및 후행 공백은 기본적으로 잘립니다. 이 동작은 `ignoreLeadingAndTrailingWhitespace` 속성을 `true`로 설정하여 변경할 수 있습니다.

#### @ArgumentsSource

`@ArgumentsSource`는 재사용 가능한 맞춤형 `ArgumentsProvider`를 지정하는 데 사용할 수 있습니다. `ArgumentsProvider`의 구현은 최상위 클래스 또는 static nested 클래스로 선언되어야 합니다.

```java
@ParameterizedTest
@ArgumentsSource(MyArgumentsProvider.class)
void testWithArgumentsSource(String argument) {
    assertNotNull(argument);
}
```

```java
public class MyArgumentsProvider implements ArgumentsProvider {

    @Override
    public Stream<? extends Arguments> provideArguments(ExtensionContext context) {
        return Stream.of("apple", "banana").map(Arguments::of);
    }
}
```

### 2.15.4. Argument Conversion

#### Widening Conversion(확대 전환)

JUnit Jupiter는 `@ParameterizedTest`에 제공된 인수에 대해 [확대 기본 변환](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.1.2)을 지원합니다. 예를 들어` @ValueSource(ints = { 1, 2, 3 })` annotation이 달린 `parameterized test`는 `int` 유형의 인수뿐만 아니라 `long`, `float` 또는 `double` 유형의 인수도 허용하도록 선언할 수 있습니다.

#### Implicit Conversion(암시적 변환)

`@CsvSource`와 같은 사용 사례를 지원하기 위해 JUnit Jupiter는 여러 built-in 암시적 유형 변환기를 제공합니다. 변환 프로세스는 각 메소드 매개변수의 선언된 유형에 따라 다릅니다.

예를 들어 `@ParameterizedTest`가 `TimeUnit` 유형의 매개변수를 선언하고 선언된 소스에서 제공한 실제 유형이 `String`인 경우 문자열은 해당 `TimeUnit` 열거형 상수로 자동 변환됩니다.

```java
@ParameterizedTest
@ValueSource(strings = "SECONDS")
void testWithImplicitArgumentConversion(ChronoUnit argument) {
    assertNotNull(argument.name());
}
```

문자열 인스턴스는 암시적으로 다음 대상 유형으로 변환됩니다.

10진수, 16진수 및 8진수 문자열 리터럴은 `byte`, `short`, `int`, `long` 및 아래 표의 대응과 같은 정수 유형으로 변환됩니다.

|Target Type|Example|
|---|---|
|boolean/Boolean|`"true"` → `true`|
|byte/Byte|`"15"`, `"0xF"`, or `"017"` → `(byte) 15`|
|char/Character|`"o"` → '`o`'|
|short/Short|`"15"`, `"0xF"`, or `"017"` → `(short) 15`|
|int/Integer|`"15"`, `"0xF"`, or `"017"` → `15`|
|long/Long|`"15"`, `"0xF"`, or `"017"` → `15L`|
|float/Float|`"1.0"` → `1.0f`|
|double/Double|`"1.0"` → `1.0d`|
|Enum subclass|`"SECONDS"` → `TimeUnit.SECONDS`|
|java.io.File|`"/path/to/file"` → `new File("/path/to/file")`|
|java.lang.Class|"java.lang.Integer" → java.lang.Integer.class (use $ for nested classes, e.g. "java.lang.Thread$State")|
|java.lang.Class|"byte" → byte.class (primitive types are supported)|
|java.lang.Class|"char[]" → char[].class (array types are supported)|
|java.math.BigDecimal|"123.456e789" → new BigDecimal("123.456e789")|
|java.math.BigInteger|"1234567890123456789" → new BigInteger("1234567890123456789")|
|java.net.URI|"https://junit.org/" → URI.create("https://junit.org/")|
|java.net.URL|"https://junit.org/" → new URL("https://junit.org/")|
|java.nio.charset.Charset|"UTF-8" → Charset.forName("UTF-8")|
|java.nio.file.Path|"/path/to/file" → Paths.get("/path/to/file")|
|java.time.Duration|"PT3S" → Duration.ofSeconds(3)|
|java.time.Instant|"1970-01-01T00:00:00Z" → Instant.ofEpochMilli(0)|
|java.time.LocalDateTime|"2017-03-14T12:34:56.789" → LocalDateTime.of(2017, 3, 14, 12, 34, 56, 789_000_000)|
|java.time.LocalDate|"2017-03-14" → LocalDate.of(2017, 3, 14)|
|java.time.LocalTime|"12:34:56.789" → LocalTime.of(12, 34, 56, 789_000_000)|
|java.time.MonthDay|"--03-14" → MonthDay.of(3, 14)|
|java.time.OffsetDateTime|"2017-03-14T12:34:56.789Z" → OffsetDateTime.of(2017, 3, 14, 12, 34, 56, 789_000_000, ZoneOffset.UTC)|
|java.time.OffsetTime|"12:34:56.789Z" → OffsetTime.of(12, 34, 56, 789_000_000, ZoneOffset.UTC)|
|java.time.Period|"P2M6D" → Period.of(0, 2, 6)|
|java.time.YearMonth|"2017-03" → YearMonth.of(2017, 3)|
|java.time.Year|"2017" → Year.of(2017)|
|java.time.ZonedDateTime|"2017-03-14T12:34:56.789Z" → ZonedDateTime.of(2017, 3, 14, 12, 34, 56, 789_000_000, ZoneOffset.UTC)|
|java.time.ZoneId|"Europe/Berlin" → ZoneId.of("Europe/Berlin")|
|java.time.ZoneOffset|"+02:30" → ZoneOffset.ofHoursMinutes(2, 30)|
|java.util.Currency|"JPY" → Currency.getInstance("JPY")|
|java.util.Locale|"en" → new Locale("en")|
|java.util.UUID|"d043e930-7b3b-48e3-bdbe-5a3ccfb833db" → UUID.fromString("d043e930-7b3b-48e3-bdbe-5a3ccfb833db")|

#### Fallback String-to-Object Conversion(대체 문자열-객체 변환)

위 표에 나열된 대상 유형으로 `String`을 암시적으로 변환하는 것 외에도 JUnit Jupiter는 대상 유형이 정확히 하나의 적절한 *factory method* 또는 *factory constructor*를 아래에 정의되어 있습니다.

- *factory method*: 단일 `String` 인수를 받아들이고 대상 형식의 인스턴스를 반환하는 대상 형식에 선언된 private가 아닌 static method입니다. 메소드 이름은 임의적일 수 있으며 특정 규칙을 따를 필요는 없습니다.
- *factory constructor*: 단일 String 인수를 허용하는 대상 유형의 private가 아닌 생성자. 대상 유형은 최상위 클래스 또는 *static* nested 클래스로 선언되어야 합니다.

>여러 *factory method*가 발견되면 무시됩니다. *factory method*와 *factory constructor*가 발견되면 생성자 대신 *factory method*가 사용됩니다.
예를 들어 다음 `@ParameterizedTest` 메소드에서 Book 인수는 `Book.fromTitle(String)` factory method를 호출하고 책 제목으로 `"42 Cats"`를 전달하여 생성됩니다.

```java
@ParameterizedTest
@ValueSource(strings = "42 Cats")
void testWithImplicitFallbackArgumentConversion(Book book) {
    assertEquals("42 Cats", book.getTitle());
}
public class Book {

    private final String title;

    private Book(String title) {
        this.title = title;
    }

    public static Book fromTitle(String title) {
        return new Book(title);
    }

    public String getTitle() {
        return this.title;
    }
}
```

#### Explicit Conversion(암시적 변환)

암시적 인수 변환에 의존하는 대신 다음 예제와 같이 `@ConvertWith` annotation을 사용하여 특정 매개변수에 사용할 `ArgumentConverter`를 명시적으로 지정할 수 있습니다. `ArgumentConverter`의 구현은 최상위 클래스 또는 static nested 클래스로 선언되어야 합니다.

```java
@ParameterizedTest
@EnumSource(ChronoUnit.class)
void testWithExplicitArgumentConversion(
        @ConvertWith(ToStringArgumentConverter.class) String argument) {

    assertNotNull(ChronoUnit.valueOf(argument));
}
public class ToStringArgumentConverter extends SimpleArgumentConverter {

    @Override
    protected Object convert(Object source, Class<?> targetType) {
        assertEquals(String.class, targetType, "Can only convert to String");
        if (source instanceof Enum<?>) {
            return ((Enum<?>) source).name();
        }
        return String.valueOf(source);
    }
}
```

변환기가 한 유형을 다른 유형으로 변환하기 위한 것이라면 `TypedArgumentConverter`를 확장하여 상용구 유형 검사(boilerplate type check)를 피할 수 있습니다.

```java
public class ToLengthArgumentConverter extends TypedArgumentConverter<String, Integer> {

    protected ToLengthArgumentConverter() {
        super(String.class, Integer.class);
    }

    @Override
    protected Integer convert(String source) {
        return (source != null ? source.length() : 0);
    }

}
```

명시적 인수 변환기는 테스트 및 확장 작성자가 구현해야 합니다. 따라서 `junit-jupiter-params`는 참조 구현으로도 사용할 수 있는 단일 명시적 인수 변환기인 `JavaTimeArgumentConverter`만 제공합니다. 작성된 annotation `JavaTimeConversionPattern`을 통해 사용됩니다.

```java
@ParameterizedTest
@ValueSource(strings = { "01.01.2017", "31.12.2017" })
void testWithExplicitJavaTimeConverter(
        @JavaTimeConversionPattern("dd.MM.yyyy") LocalDate argument) {

    assertEquals(2017, argument.getYear());
}
```

### 2.15.5. Argument Aggregation

기본적으로 `@ParameterizedTest` 메소드에 제공된 각 인수는 단일 메소드 매개변수에 해당합니다. 결과적으로 많은 수의 인수를 제공할 것으로 예상되는 인수 소스는 큰 메소드 서명으로 이어질 수 있습니다.

이러한 경우 여러 매개변수 대신 `ArgumentsAccessor`를 사용할 수 있습니다. 이 API를 사용하면 테스트 메소드에 전달된 단일 인수를 통해 제공된 인수에 액세스할 수 있습니다. 또한 암시적 변환에서 설명한 대로 형식 변환이 지원됩니다.

```java
@ParameterizedTest
@CsvSource({
    "Jane, Doe, F, 1990-05-20",
    "John, Doe, M, 1990-10-22"
})
```

```java
void testWithArgumentsAccessor(ArgumentsAccessor arguments) {
    Person person = new Person(arguments.getString(0),
                               arguments.getString(1),
                               arguments.get(2, Gender.class),
                               arguments.get(3, LocalDate.class));

    if (person.getFirstName().equals("Jane")) {
        assertEquals(Gender.F, person.getGender());
    }
    else {
        assertEquals(Gender.M, person.getGender());
    }
    assertEquals("Doe", person.getLastName());
    assertEquals(1990, person.getDateOfBirth().getYear());
}
```

`ArgumentsAccessor`의 인스턴스는 `ArgumentsAccessor` 유형의 매개변수에 자동으로 삽입됩니다.

#### Custom Aggregators

`ArgumentsAccessor`를 사용하여 `@ParameterizedTest` 메소드의 인수에 직접 액세스하는 것 외에도 JUnit Jupiter는 재사용 가능한 사용자 지정 aggregator 사용을 지원합니다.

사용자 정의 수집기를 사용하려면 `ArgumentsAggregator` 인터페이스를 구현하고 `@ParameterizedTest` 메소드의 호환 가능한 매개변수에 `@AggregateWith` annotation을 통해 등록하십시오. aggregation 결과는 `parameterized test`가 호출될 때 해당 매개변수에 대한 인수로 제공됩니다. `ArgumentsAggregator`의 구현은 최상위 클래스 또는 static nested 클래스로 선언되어야 합니다.

```java
@ParameterizedTest
@CsvSource({
    "Jane, Doe, F, 1990-05-20",
    "John, Doe, M, 1990-10-22"
})
void testWithArgumentsAggregator(@AggregateWith(PersonAggregator.class) Person person) {
    // perform assertions against person
}
```

```java
public class PersonAggregator implements ArgumentsAggregator {
    @Override
    public Person aggregateArguments(ArgumentsAccessor arguments, ParameterContext context) {
        return new Person(arguments.getString(0),
                          arguments.getString(1),
                          arguments.get(2, Gender.class),
                          arguments.get(3, LocalDate.class));
    }
}
```

코드베이스에서 여러 `parameterized test` 메소드에 대해 `@AggregateWith(MyTypeAggregator.class)`를 반복적으로 선언하는 경우 `@AggregateWith(MyTypeAggregator.class`)로 메타 annotation이 달린` @CsvToMyType`과 같은 사용자 정의 합성 annotation을 생성할 수 있습니다. 다음 예제는 사용자 정의 `@CsvToPerson` annotation을 사용하여 이를 실제로 보여줍니다.

```java
@ParameterizedTest
@CsvSource({
    "Jane, Doe, F, 1990-05-20",
    "John, Doe, M, 1990-10-22"
})
void testWithCustomAggregatorAnnotation(@CsvToPerson Person person) {
    // perform assertions against person
}
```

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.PARAMETER)
@AggregateWith(PersonAggregator.class)
public @interface CsvToPerson {
}
```

### 2.15.6. Customizing Display Names

기본적으로 parameterized test 호출의 display name에는 호출 인덱스와 해당 특정 호출에 대한 모든 인수의 `String` 표현이 포함됩니다. 바이트코드에 있는 경우(Java의 경우 테스트 코드는 `-parameters` 컴파일러 플래그로 컴파일해야 함) 매개변수 이름이 각각 앞에 옵니다(인수가 `ArgumentsAccessor` 또는 `ArgumentAggregator`를 통해서만 사용 가능한 경우 제외).

그러나 다음 예제와 같이 `@ParameterizedTest` annotation의 name 속성을 통해 호출 display name을 사용자 정의할 수 있습니다.

```java
@DisplayName("Display name of container")
@ParameterizedTest(name = "{index} ==> the rank of ''{0}'' is {1}")
@CsvSource({ "apple, 1", "banana, 2", "'lemon, lime', 3" })
void testWithCustomDisplayNames(String fruit, int rank) {
}
```

`ConsoleLauncher`를 사용하여 위의 방법을 실행하면 다음과 유사한 출력이 표시됩니다.

```
Display name of container ✔
├─ 1 ==> the rank of 'apple' is 1 ✔
├─ 2 ==> the rank of 'banana' is 2 ✔
└─ 3 ==> the rank of 'lemon, lime' is 3 ✔
```

이름은 `MessageFormat` 패턴입니다. 따라서 작은따옴표(`'`)를 표시하려면 큰따옴표(`''`)로 표시해야 합니다.

다음 자리 표시자는 사용자 지정 display name 내에서 지원됩니다.

|Placeholder|Description|
|---|---|
|{displayName}|the display name of the method|
|{index}|the current invocation index (1-based)|
|{arguments}|the complete, comma-separated arguments list|
|{argumentsWithNames}|the complete, comma-separated arguments list with parameter names|
|{0}, {1}, …​|an individual argument|

>display name에 인수를 포함할 때 문자열 표현이 구성된 최대 길이를 초과하면 잘립니다. 제한은 `junit.jupiter.params.displayname.argument.maxlength` 구성 매개변수를 통해 구성할 수 있으며 기본값은 512자입니다.

`@MethodSource` 또는 `@ArgumentSource`를 사용할 때 인수에 이름을 지정할 수 있습니다. 이 이름은 아래 예와 같이 호출 display name에 인수가 포함된 경우 사용됩니다.

```java
@DisplayName("A parameterized test with named arguments")
@ParameterizedTest(name = "{index}: {0}")
@MethodSource("namedArguments")
void testWithNamedArguments(File file) {
}

static Stream<Arguments> namedArguments() {
    return Stream.of(arguments(Named.of("An important file", new File("path1"))),
        arguments(Named.of("Another file", new File("path2"))));
}
```

```
A parameterized test with named arguments ✔
├─ 1: An important file ✔
└─ 2: Another file ✔
```

프로젝트의 모든 parameterized test에 대한 기본 이름 패턴을 설정하려면 다음 구성을 `junit-platform.properties`에 추가할 수 있습니다.

`junit.jupiter.params.displayname.default = {index}`

매개 변수화된 메소드의 display name은 다음 우선 순위 규칙에 따라 결정됩니다.

1. `@ParameterizedTest`의 이름(이 있는 경우)
1. `junit.jupiter.params.displayname.default` 값(junit-platform.properties에서)(이 있는 경우)
1. `@ParameterizedTest`에 정의된 `DEFAULT_DISPLAY_NAME` 상수

### 2.15.7. Lifecycle and Interoperability

parameterized test의 각 호출은 일반 `@Test` 메소드와 동일한 수명 주기를 갖습니다. 예를 들어 `@BeforeEach` method는 각 호출 전에 실행됩니다. `dynamic test`와 유사하게 호출은 IDE의 테스트 트리에 하나씩 나타납니다. 동일한 테스트 클래스 내에서 일반 `@Test` 메소드와 `@ParameterizedTest `메소드를 마음대로 혼합할 수 있습니다.

`@ParameterizedTest` 메소드와 함께 `ParameterResolver` 확장을 사용할 수 있습니다. 그러나 인수 소스에 의해 확인되는 메소드 매개변수는 인수 목록에서 맨 처음에 와야 합니다. 테스트 클래스에는 일반 테스트와 다른 매개변수 목록이 있는 parameterized test가 포함될 수 있으므로 인수 소스의 값은 수명 주기 메소드(예: `@BeforeEach`) 및 테스트 클래스 생성자에 대해 확인되지 않습니다.

```java
@BeforeEach
void beforeEach(TestInfo testInfo) {
    // ...
}

@ParameterizedTest
@ValueSource(strings = "apple")
void testWithRegularParameterResolver(String argument, TestReporter testReporter) {
    testReporter.publishEntry("argument", argument);
}

@AfterEach
void afterEach(TestInfo testInfo) {
    // ...
}
```

## 2.16. Test Templates

[@TestTemplate](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestTemplate.html) 메소드는 일반 테스트 케이스가 아니라 테스트 케이스를 위한 템플릿입니다. 따라서 등록된 공급자가 반환하는 호출 컨텍스트의 수에 따라 여러 번 호출되도록 설계되었습니다. 따라서 등록된 [TestTemplateInvocationContextProvider](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestTemplateInvocationContextProvider.html) 확장과 함께 사용해야 합니다. 테스트 템플릿 메소드의 각 호출은 동일한 수명 주기 콜백 및 확장을 완벽하게 지원하는 일반 `@Test` 메소드 실행처럼 작동합니다. 사용 예는 `테스트 템플릿에 대한 호출 컨텍스트 제공`을 참조하십시오.

>`Repeated test` 및 `parameterized test`는 테스트 템플릿의 기본 제공 전문화입니다.

## 2.17. Dynamic Tests

`Annotation`에 설명된 JUnit Jupiter의 표준  `@Test`  annotation은 JUnit 4의  `@Test`  annotation과 매우 유사합니다. 둘 다 테스트 케이스를 구현하는 메소드를 설명합니다. 이러한 테스트 케이스는 컴파일 시간에 완전히 지정된다는 점에서 정적이며 런타임에 발생하는 어떤 것으로도 그 동작을 변경할 수 없습니다. *assumption은 기본적인 형태의 동적 행동을 제공하지만 의도적으로 표현이 다소 제한됩니다.*

이러한 표준 테스트 외에도 완전히 새로운 종류의 테스트 프로그래밍 모델이 JUnit Jupiter에 도입되었습니다. 이 새로운 종류의 테스트는 `@TestFactory`로 annotation이 달린 factory method에 의해 런타임에 생성되는 dynamic test입니다.

`@Tes`t 메소드와 달리 `@TestFactory` 메소드는 그 자체가 테스트 케이스가 아니라 테스트 케이스를 위한 팩토리입니다. 따라서 `dynamic test`는 factory의 산물입니다. 기술적으로 말하면 `@TestFactory `method는 단일 `DynamicNode` 또는 `Stream`, `Collection`, `Iterable`, `Iterator` 또는 `DynamicNode` 인스턴스의 배열을 반환해야 합니다. `DynamicNode`의 인스턴스화 가능한 subclass는 `DynamicContainer` 및 `DynamicTest`입니다. `DynamicContainer` 인스턴스는 display name과 동적 자식 노드 목록으로 구성되어 동적 노드의 임의 중첩 계층을 생성할 수 있습니다. DynamicTest 인스턴스는 느리게 실행되어 테스트 사례의 동적 및 비결정적 생성을 가능하게 합니다.

`@TestFactory`에서 반환된 모든 `stream`은 `stream.close()`를 호출하여 적절하게 닫히므로 `Files.lines()`와 같은 리소스를 안전하게 사용할 수 있습니다.

`@Test` 메소드와 마찬가지로 `@TestFactory` method는 *private*이거나 *static*이어서는 안 되며 선택적으로 `ParameterResolvers`에서 확인할 매개변수를 선언할 수 있습니다.

`DynamicTest`는 런타임에 생성되는 테스트 케이스입니다. display name과 `실행` 파일로 구성됩니다. `실행` 파일은 `@FunctionalInterface`입니다. 즉, dynamic test의 구현이 *lambda expressions* 또는 *메소드 참조*로 제공될 수 있습니다.

>*Dynamic Test Lifecycle*
`dynamic test`의 실행 수명 주기는 표준 `@Test` 경우와 상당히 다릅니다. 특히 개별 `dynamic test`에는 수명 주기 콜백이 없습니다. 즉, `@BeforeEach` 및 `@AfterEach` 메소드와 해당 확장 콜백은 `@TestFactory` 메소드에 대해 실행되지만 각 `dynamic test`에 대해서는 실행되지 않습니다. 즉, *dynamic test*를 위한 lambda expressions 내 테스트 인스턴스의 필드에 액세스하는 경우 해당 필드는 동일한 `@TestFactory` 메소드에 의해 생성된 개별 *dynamic test* 실행 사이의 확장 또는 콜백 메소드에 의해 재설정되지 않습니다.

JUnit Jupiter 5.8.2부터 *dynamic test*는 항상 factory method로 생성해야 합니다. 그러나 이는 이후 릴리스의 등록 기능으로 보완될 수 있습니다.

### 2.17.1. Dynamic Test Examples

다음 `DynamicTestsDemo` 클래스는 테스트 팩토리 및 dynamic test의 몇 가지 예를 보여줍니다.

첫 번째 method는 잘못된 반환 유형을 반환합니다. 유효하지 않은 반환 유형은 컴파일 시간에 감지할 수 없으므로 런타임에 감지되면 `JUnitException`이 발생합니다.

다음 6가지 방법은 `Collection`, `Iterable`, `Iterator`, `array` 또는 DynamicTest 인스턴스의 `Stream` 생성을 보여주는 매우 간단한 예입니다. 이러한 예제의 대부분은 실제로 동적 동작을 나타내지 않고 원칙적으로 지원되는 반환 유형을 보여줍니다. 그러나 `dynamicTestsFromStream()` 및 `dynamicTestsFromIntStream()`은 주어진 문자열 세트 또는 입력 숫자 범위에 대한 dynamic test를 생성하는 것이 얼마나 쉬운지를 보여줍니다.

다음 방법은 본질적으로 동적입니다. `generateRandomNumberOfTests()`는 난수를 생성하는 `Iterator`, display name 생성기 및 테스트 실행기를 구현한 다음 세 가지 모두를 `DynamicTest.stream()`에 제공합니다. 물론 `generateRandomNumberOfTests()`의 비결정적 동작은 테스트 반복성과 충돌하므로 주의해서 사용해야 하지만 dynamic test의 표현력과 능력을 입증하는 역할을 합니다.

다음 방법은 유연성 측면에서 `generateRandomNumberOfTests()`와 유사합니다. 그러나 `dynamicTestsFromStreamFactoryMethod()`는 `DynamicTest.stream()` factory method를 통해 기존 stream에서 dynamic test stream을 생성합니다.

데모를 위해 `dynamicNodeSingleTest()` method는 stream 대신 단일 DynamicTest를 생성하고 `dynamicNodeSingleContainer()` method는 DynamicContainer를 활용하는 dynamic test의 중첩 계층을 생성합니다.

```java
import static example.util.StringUtils.isPalindrome;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertTrue;
import static org.junit.jupiter.api.DynamicContainer.dynamicContainer;
import static org.junit.jupiter.api.DynamicTest.dynamicTest;
import static org.junit.jupiter.api.Named.named;

import java.util.Arrays;
import java.util.Collection;
import java.util.Iterator;
import java.util.List;
import java.util.Random;
import java.util.function.Function;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import example.util.Calculator;

import org.junit.jupiter.api.DynamicNode;
import org.junit.jupiter.api.DynamicTest;
import org.junit.jupiter.api.Named;
import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.TestFactory;
import org.junit.jupiter.api.function.ThrowingConsumer;

class DynamicTestsDemo {

    private final Calculator calculator = new Calculator();

    // This will result in a JUnitException!
    @TestFactory
    List<String> dynamicTestsWithInvalidReturnType() {
        return Arrays.asList("Hello");
    }

    @TestFactory
    Collection<DynamicTest> dynamicTestsFromCollection() {
        return Arrays.asList(
            dynamicTest("1st dynamic test", () -> assertTrue(isPalindrome("madam"))),
            dynamicTest("2nd dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        );
    }

    @TestFactory
    Iterable<DynamicTest> dynamicTestsFromIterable() {
        return Arrays.asList(
            dynamicTest("3rd dynamic test", () -> assertTrue(isPalindrome("madam"))),
            dynamicTest("4th dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        );
    }

    @TestFactory
    Iterator<DynamicTest> dynamicTestsFromIterator() {
        return Arrays.asList(
            dynamicTest("5th dynamic test", () -> assertTrue(isPalindrome("madam"))),
            dynamicTest("6th dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        ).iterator();
    }

    @TestFactory
    DynamicTest[] dynamicTestsFromArray() {
        return new DynamicTest[] {
            dynamicTest("7th dynamic test", () -> assertTrue(isPalindrome("madam"))),
            dynamicTest("8th dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        };
    }

    @TestFactory
    Stream<DynamicTest> dynamicTestsFromStream() {
        return Stream.of("racecar", "radar", "mom", "dad")
            .map(text -> dynamicTest(text, () -> assertTrue(isPalindrome(text))));
    }

    @TestFactory
    Stream<DynamicTest> dynamicTestsFromIntStream() {
        // Generates tests for the first 10 even integers.
        return IntStream.iterate(0, n -> n + 2).limit(10)
            .mapToObj(n -> dynamicTest("test" + n, () -> assertTrue(n % 2 == 0)));
    }

    @TestFactory
    Stream<DynamicTest> generateRandomNumberOfTestsFromIterator() {

        // Generates random positive integers between 0 and 100 until
        // a number evenly divisible by 7 is encountered.
        Iterator<Integer> inputGenerator = new Iterator<Integer>() {

            Random random = new Random();
            int current;

            @Override
            public boolean hasNext() {
                current = random.nextInt(100);
                return current % 7 != 0;
            }

            @Override
            public Integer next() {
                return current;
            }
        };

        // Generates display names like: input:5, input:37, input:85, etc.
        Function<Integer, String> displayNameGenerator = (input) -> "input:" + input;

        // Executes tests based on the current input value.
        ThrowingConsumer<Integer> testExecutor = (input) -> assertTrue(input % 7 != 0);

        // Returns a stream of dynamic tests.
        return DynamicTest.stream(inputGenerator, displayNameGenerator, testExecutor);
    }

    @TestFactory
    Stream<DynamicTest> dynamicTestsFromStreamFactoryMethod() {
        // Stream of palindromes to check
        Stream<String> inputStream = Stream.of("racecar", "radar", "mom", "dad");

        // Generates display names like: racecar is a palindrome
        Function<String, String> displayNameGenerator = text -> text + " is a palindrome";

        // Executes tests based on the current input value.
        ThrowingConsumer<String> testExecutor = text -> assertTrue(isPalindrome(text));

        // Returns a stream of dynamic tests.
        return DynamicTest.stream(inputStream, displayNameGenerator, testExecutor);
    }

    @TestFactory
    Stream<DynamicTest> dynamicTestsFromStreamFactoryMethodWithNames() {
        // Stream of palindromes to check
        Stream<Named<String>> inputStream = Stream.of(
                named("racecar is a palindrome", "racecar"),
                named("radar is also a palindrome", "radar"),
                named("mom also seems to be a palindrome", "mom"),
                named("dad is yet another palindrome", "dad")
            );

        // Returns a stream of dynamic tests.
        return DynamicTest.stream(inputStream,
            text -> assertTrue(isPalindrome(text)));
    }

    @TestFactory
    Stream<DynamicNode> dynamicTestsWithContainers() {
        return Stream.of("A", "B", "C")
            .map(input -> dynamicContainer("Container " + input, Stream.of(
                dynamicTest("not null", () -> assertNotNull(input)),
                dynamicContainer("properties", Stream.of(
                    dynamicTest("length > 0", () -> assertTrue(input.length() > 0)),
                    dynamicTest("not empty", () -> assertFalse(input.isEmpty()))
                ))
            )));
    }

    @TestFactory
    DynamicNode dynamicNodeSingleTest() {
        return dynamicTest("'pop' is a palindrome", () -> assertTrue(isPalindrome("pop")));
    }

    @TestFactory
    DynamicNode dynamicNodeSingleContainer() {
        return dynamicContainer("palindromes",
            Stream.of("racecar", "radar", "mom", "dad")
                .map(text -> dynamicTest(text, () -> assertTrue(isPalindrome(text)))
        ));
    }

}
```

### 2.17.2. URI Test Sources for Dynamic Tests

JUnit 플랫폼은 IDE 및 빌드 도구에서 해당 위치로 이동하는 데 사용되는 테스트 또는 컨테이너의 소스를 나타내는 `TestSource`를 제공합니다.

dynamic test 또는 동적 컨테이너에 대한 `TestSource`는 각각` DynamicTest.dynamicTest(String, URI, Executable)` 또는 `DynamicContainer.dynamicContainer(String, URI, Stream)` factory method를 통해 제공될 수 있는 `java.net.URI`에서 구성될 수 있습니다. `URI`는 다음 `TestSource` 구현 중 하나로 변환됩니다.

`ClasspathResourceSource`
&nbsp;&nbsp;&nbsp;`URI`에 classpath 체계 — 가 포함된 경우(예: `classpath:/test/foo.xml?line=20,column=2`).

`DirectorySource`
&nbsp;&nbsp;&nbsp;`URI`가 파일 시스템에 있는 디렉토리를 나타내는 경우.

`FileSource`
&nbsp;&nbsp;&nbsp;`URI`가 파일 시스템에 있는 파일을 나타내는 경우.

`MethodSource`
&nbsp;&nbsp;&nbsp;`URI`에 메소드 체계와 FQMN(정규화된 메소드 이름) (예: `method:org.junit.Foo#bar(java.lang.String, java.lang.String[])`)이 포함된 경우. FQMN에 지원되는 형식은 `DiscoverySelectors.selectMethod(String)`에 대한 Javadoc을 참조하십시오.

`ClassSource`
&nbsp;&nbsp;&nbsp;`URI`에 `클래스` 체계와 정규화된 클래스 이름이 포함된 경우(예: `class:org.junit.Foo?line=42`).

`UriSource`
&nbsp;&nbsp;&nbsp;위의 `TestSource` 구현이 적용되지 않는 경우.

## 2.18. Timeouts

`@Timeout` annotation을 사용하면 실행 시간이 주어진 기간을 초과하는 경우 테스트, 테스트 팩토리, 테스트 템플릿 또는 수명 주기 메소드가 실패해야 한다고 선언할 수 있습니다. 지속 시간의 시간 단위는 기본적으로 초로 설정되지만 구성할 수 있습니다.

다음 예제는 `@Timeout`이 수명 주기 및 테스트 메소드에 어떻게 적용되는지 보여줍니다.

```java
class TimeoutDemo {

    @BeforeEach
    @Timeout(5)
    void setUp() {
        // fails if execution time exceeds 5 seconds
    }

    @Test
    @Timeout(value = 100, unit = TimeUnit.MILLISECONDS)
    void failsIfExecutionTimeExceeds100Milliseconds() {
        // fails if execution time exceeds 100 milliseconds
    }

}
```

`assertTimeoutPreemptively()` assertion과 달리 annotation이 달린 메소드의 실행은 테스트의 메인 스레드에서 진행됩니다. 타임아웃이 초과되면 메인 쓰레드는 다른 쓰레드에서 인터럽트된다. 이는 현재 실행 중인 스레드(예: `ThreadLocal` 트랜잭션 관리)에 민감한 메커니즘을 사용하는 Spring과 같은 프레임워크와의 상호 운용성을 보장하기 위해 수행됩니다.

테스트 클래스 내의 모든 테스트 메소드와 모든 `@Nested` 클래스에 동일한 시간 초과를 적용하려면 클래스 수준에서 `@Timeout` annotation을 선언할 수 있습니다. 그런 다음 특정 메소드 또는 `@Nested` 클래스에 대한 `@Timeout` annotation으로 재정의되지 않는 한 해당 클래스 및 해당 `@Nested` 클래스 내의 모든 테스트, 테스트 팩토리 및 테스트 템플릿 메소드에 적용됩니다. 클래스 수준에서 선언된 `@Timeout` annotation은 수명 주기 메소드에 적용되지 않습니다.

`@TestFactory` 메소드에서 `@Timeout`을 선언하면 factory method가 지정된 기간 내에 반환되는지 확인하지만 팩토리에서 생성된 각 개별 DynamicTest의 실행 시간은 확인하지 않습니다. 그 목적을 위해 `assertTimeout()` 또는 `assertTimeoutPreemptively()`를 사용하십시오.

`@Timeout`이 `@TestTemplate` 메소드(예: `@RepeatedTest` 또는 `@ParameterizedTest`)에 있는 경우 각 호출에 지정된 시간 제한이 적용됩니다.

다음 `구성 매개변수(configuration parameters)`를 사용하여 특정 범주의 모든 메소드 또는 둘러싸는 테스트 클래스에 `@Timeout` annotation이 지정되지 않는 한 전역 시간 초과를 지정할 수 있습니다.

`junit.jupiter.execution.timeout.default`
&nbsp;&nbsp;&nbsp; Default timeout for all testable and lifecycle methods

`junit.jupiter.execution.timeout.testable.method.default`
&nbsp;&nbsp;&nbsp; Default timeout for all testable methods

`junit.jupiter.execution.timeout.test.method.default`
&nbsp;&nbsp;&nbsp; Default timeout for `@Test methods

`junit.jupiter.execution.timeout.testtemplate.method.default`
&nbsp;&nbsp;&nbsp; Default timeout for `@TestTemplate` methods

`junit.jupiter.execution.timeout.testfactory.method.default`
&nbsp;&nbsp;&nbsp; Default timeout for `@TestFactory` methods

`junit.jupiter.execution.timeout.lifecycle.method.default`
&nbsp;&nbsp;&nbsp; Default timeout for all lifecycle methods

`junit.jupiter.execution.timeout.beforeall.method.default`
&nbsp;&nbsp;&nbsp; Default timeout for `@BeforeAll methods

`junit.jupiter.execution.timeout.beforeeach.method.default`
&nbsp;&nbsp;&nbsp; Default timeout for `@BeforeEach` methods

`junit.jupiter.execution.timeout.aftereach.method.default`
&nbsp;&nbsp;&nbsp; Default timeout for `@AfterEach` methods

`junit.jupiter.execution.timeout.afterall.method.default`
&nbsp;&nbsp;&nbsp; Default timeout for `@AfterAll` methods

보다 구체적인 구성 매개변수는 덜 구체적인 구성 매개변수보다 우선합니다. 예를 들어, `junit.jupiter.execution.timeout.test.method.default`는 `junit.jupiter.execution.timeout.default`를 재정의하는 `junit.jupiter.execution.timeout.testable.method.default`를 재정의합니다.

이러한 구성 매개변수의 값은 대소문자를 구분하지 않는 형식이어야 합니다. `<number> [ns|μs|ms|s|m|h|d]`. 숫자와 단위 사이의 공백은 생략할 수 있습니다. 단위를 지정하지 않는 것은 초를 사용하는 것과 같습니다.

*Table 1. Example timeout configuration parameter values*

|Parameter value|Equivalent annotation|
|---|---|
|42|@Timeout(42)
|42 ns|@Timeout(value = 42, unit = NANOSECONDS)
|42 μs|@Timeout(value = 42, unit = MICROSECONDS)
|42 ms|@Timeout(value = 42, unit = MILLISECONDS)
|42 s|@Timeout(value = 42, unit = SECONDS)
|42 m|@Timeout(value = 42, unit = MINUTES)
|42 h|@Timeout(value = 42, unit = HOURS)
|42 d|@Timeout(value = 42, unit = DAYS)

### 2.18.1. Using @Timeout for Polling Tests

비동기 코드를 처리할 때 assertion을 수행하기 전에 어떤 일이 일어나기를 기다리는 동안 폴링하는 테스트를 작성하는 것이 일반적입니다. 어떤 경우에는 `CountDownLatch` 또는 다른 동기화 메커니즘을 사용하도록 논리를 다시 작성할 수 있지만 때로는 불가능합니다. 예를 들어 테스트 대상이 외부 메시지 브로커의 채널에 메시지를 보내고 assertion은 메시지가 채널을 통해 성공적으로 전송되었습니다. 이와 같은 비동기식 테스트는 비동기식 메시지가 성공적으로 전달되지 않는 경우와 같이 무기한 실행하여 test suite를 중단하지 않도록 하기 위해 일종의 시간 초과가 필요합니다.

폴링하는 비동기 테스트에 대한 시간 초과를 구성하여 테스트가 무기한 실행되지 않도록 할 수 있습니다. 다음 예제는 JUnit Jupiter의 `@Timeout` annotation으로 이를 달성하는 방법을 보여줍니다. 이 기술은 "poll until" 논리를 매우 쉽게 구현하는 데 사용할 수 있습니다.

```java
@Test
@Timeout(5) // Poll at most 5 seconds
void pollUntil() throws InterruptedException {
    while (asynchronousResultNotAvailable()) {
        Thread.sleep(250); // custom poll interval
    }
    // Obtain the asynchronous result and perform assertions
}
```

>폴링 간격을 더 많이 제어하고 비동기 테스트를 통해 더 큰 유연성이 필요한 경우 `Awaitility`와 같은 전용 라이브러리를 사용하는 것이 좋습니다.

### 2.18.2. Disable @Timeout Globally

디버그 세션에서 코드를 단계별로 실행할 때 고정된 시간 제한이 테스트 결과에 영향을 미칠 수 있습니다. 모든 주장이 충족되었지만 테스트를 실패로 표시합니다.

JUnit Jupiter는 제한 시간이 적용될 때 구성하기 위해 `junit.jupiter.execution.timeout.mode `구성 매개변수를 지원합니다. `enabled`, `disabled` 및 `disabled_on_debug`의 세 가지 모드가 있습니다. 기본 모드는 `enabled`입니다. VM 런타임은 입력 매개변수 중 하나가 `-agentlib:jdwp`로 시작하는 경우 debug mode에서 실행되는 것으로 간주됩니다. 이 휴리스틱은 `disabled_on_debug` 모드에서 쿼리됩니다.

## 2.19. Parallel Execution

>*병렬 테스트 실행은 실험적인 기능입니다.*
JUnit 팀이 이 기능을 개선하고 궁극적으로 `홍보`할 수 있도록 시도해 보고 피드백을 제공하도록 초대받았습니다.

기본적으로 JUnit Jupiter 테스트는 단일 스레드에서 순차적으로 실행됩니다.   병렬 테스트 실행 —예: 실행 속도 향상 — 은 버전 5.3부터 옵트인 기능으로 사용할 수 있습니다. 병렬 실행을 활성화하려면 `junit.jupiter.execution.parallel.enabled` 구성 매개변수를 `true` — 로 설정합니다(예: junit-platform.properties에서 구성 매개변수 참조)

이 속성을 활성화하는 것은 테스트를 병렬로 실행하는 데 필요한 첫 번째 단계일 뿐입니다. 활성화된 경우 테스트 클래스 및 method는 기본적으로 계속 순차적으로 실행됩니다. 테스트 트리의 노드가 동시에 실행되는지 여부는 실행 모드에 의해 제어됩니다. 다음 두 가지 모드를 사용할 수 있습니다.

`SAME_THREAD`
&nbsp;&nbsp;&nbsp;부모가 사용하는 동일한 스레드에서 강제 실행합니다. 예를 들어, 테스트 메소드에서 사용될 때 테스트 메소드는 포함하는 테스트 클래스의 `@BeforeAll` 또는 `@AfterAll` 메소드와 동일한 스레드에서 실행됩니다.

`CONCURRENT`
&nbsp;&nbsp;&nbsp;리소스 잠금이 동일한 스레드에서 강제로 실행되지 않는 한 동시에 실행합니다.

기본적으로 테스트 트리의 노드는 `SAME_THREAD` 실행 모드를 사용합니다. `junit.jupiter.execution.parallel.mode.default` 구성 매개변수를 설정하여 기본값을 변경할 수 있습니다. 또는 `@Execution` annotation을 사용하여 annotation이 달린 요소와 해당 subelements(가 있는 경우)의 실행 모드를 변경하여 개별 테스트 클래스에 대해 하나씩 병렬 실행을 활성화할 수 있습니다.

*Configuration parameters to execute all tests in parallel*
```
junit.jupiter.execution.parallel.enabled = true
junit.jupiter.execution.parallel.mode.default = concurrent
```

기본 실행 모드는 몇 가지 주목할 만한 예외, 즉 `Lifecycle.PER_CLASS` 모드 또는 [MethodOrderer](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.html) ([MethodOrderer.Random](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.Random.html) 제외) 사용하는 테스트 클래스를 제외하고 테스트 트리의 모든 노드에 적용됩니다. 전자의 경우 테스트 작성자는 테스트 클래스가 스레드로부터 안전한지 확인해야 합니다. 후자의 경우 동시 실행이 구성된 실행 순서와 충돌할 수 있습니다. 따라서 두 경우 모두 이러한 테스트 클래스의 테스트 method는 `@Execution(CONCURRENT)` annotation이 테스트 클래스 또는 메소드에 있는 경우에만 동시에 실행됩니다.

`CONCURRENT` 실행 모드로 구성된 테스트 트리의 모든 노드는 선언적 `동기화(synchronization)` 메커니즘을 관찰하면서 제공된 구성에 따라 완전히 병렬로 실행됩니다. `표준 출력/오류 캡처(Capturing Standard Output/Error)`는 별도로 활성화해야 합니다.

한 `junit.jupiter.execution.parallel.mode.classes.default` 구성 매개변수를 설정하여 최상위 클래스에 대한 기본 실행 모드를 구성할 수 있습니다. 두 구성 매개변수를 결합하여 클래스가 병렬로 실행되도록 구성할 수 있지만 해당 method는 동일한 스레드에서 실행됩니다.

*Configuration parameters to execute top-level classes in parallel but methods in same thread*
```
junit.jupiter.execution.parallel.enabled = true
junit.jupiter.execution.parallel.mode.default = same_thread
junit.jupiter.execution.parallel.mode.classes.default = concurrent
```

반대 조합은 한 클래스 내의 모든 메소드를 병렬로 실행하지만 최상위 클래스는 순차적으로 실행됩니다.

*Configuration parameters to execute top-level classes sequentially but their methods in parallel*
```
junit.jupiter.execution.parallel.enabled = true
junit.jupiter.execution.parallel.mode.default = concurrent
junit.jupiter.execution.parallel.mode.classes.default = same_thread
```

다음 다이어그램은 클래스당 두 개의 테스트 메소드가 있는 두 개의 최상위 테스트 클래스 A와 B의 실행이 `junit.jupiter.execution.parallel.mode.default` 및 `junit.jupiter.execution.parallel.mode.classes.default`의 네 가지 조합 모두에 대해 어떻게 작동하는지 보여줍니다. (첫 번째 열의 레이블 참조).

![junit5_guide_writing-tests_execution_mode](../images/junit5_guide_writing-tests_execution_mode.svg)

*기본 실행 모드 구성 조합*

`junit.jupiter.execution.parallel.mode.classes.default` 구성 매개변수가 명시적으로 설정되지 않은 경우 `junit.jupiter.execution.parallel.mode.default` 값이 대신 사용됩니다.

### 2.19.1. Configuration

`ParallelExecutionConfigurationStrategy`를 사용하여 원하는 병렬 처리 및 최대 풀 크기와 같은 속성을 구성할 수 있습니다. JUnit 플랫폼은 기본적으로 동적 및 고정의 두 가지 구현을 제공합니다. 또는 사용자 지정 전략을 구현할 수 있습니다.

전략을 선택하려면 `junit.jupiter.execution.parallel.config.strategy` 구성 매개변수를 다음 옵션 중 하나로 설정합니다.

`dynamic`
&nbsp;&nbsp;&nbsp;사용 가능한 프로세서/코어 수에 `junit.jupiter.execution.parallel.config.dynamic.factor` 구성 매개변수를 곱하여 원하는 병렬도를 계산합니다(기본값은 `1`).
`fixed`
&nbsp;&nbsp;&nbsp;필수 `junit.jupiter.execution.parallel.config.fixed.parallelism` 구성 매개변수를 원하는 병렬 처리로 사용합니다.
`custom`
&nbsp;&nbsp;&nbsp;필수 `junit.jupiter.execution.parallel.config.custom.class` 구성 매개변수를 통해 사용자 지정 `ParallelExecutionConfigurationStrategy` 구현을 지정하여 원하는 구성을 결정할 수 있습니다.

구성 전략이 설정되지 않은 경우 JUnit Jupiter는 인수 `1`로 동적 구성 전략을 사용합니다. 결과적으로 원하는 병렬 처리는 사용 가능한 프로세서/코어 수와 동일합니다.

>*병렬 처리는 최대 동시 스레드 수를 의미하지 않습니다.*
>JUnit Jupiter는 동시에 실행되는 테스트 수가 구성된 병렬 처리를 초과하지 않을 것이라고 보장하지 않습니다. 예를 들어, 다음 섹션에서 설명하는 동기화 메커니즘 중 하나를 사용할 때 장면 뒤에서 사용되는 `ForkJoinPool`은 실행이 충분한 병렬 처리로 계속되도록 하기 위해 추가 스레드를 생성할 수 있습니다. 따라서 테스트 클래스에서 이러한 보장이 필요한 경우 동시성을 제어하는 자체 수단을 사용하십시오.

### 2.19.2. Synchronization

`@Execution` annotation을 사용하여 실행 모드를 제어하는 것 외에도 JUnit Jupiter는 또 다른 annotation 기반 선언적 동기화 메커니즘을 제공합니다. `@ResourceLock` annotation을 사용하면 테스트 클래스 또는 메소드가 안정적인 테스트 실행을 보장하기 위해 동기화된 액세스가 필요한 특정 공유 리소스를 사용한다고 선언할 수 있습니다. 공유 리소스는 문자열인 고유한 이름으로 식별됩니다. 이름은 사용자 정의이거나 [리소스](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/parallel/Resources.html)에 있는 사전 정의된 상수 중 하나일 수 있습니다. `SYSTEM_PROPERTIES`, `SYSTEM_OUT`, `SYSTEM_ERR`, `LOCALE` 또는 `TIME_ZONE`.

다음 예제의 테스트가 `@ResourceLock`을 사용하지 않고 병렬로 실행되면 불안정합니다. 때로는 통과하고 다른 때는 동일한 JVM 시스템 속성을 쓰고 읽는 고유한 경쟁 조건으로 인해 실패합니다.

`@ResourceLock` annotation을 사용하여 공유 리소스에 대한 액세스가 선언되면 JUnit Jupiter 엔진은 이 정보를 사용하여 충돌하는 테스트가 병렬로 실행되지 않도록 합니다.

>*격리된 테스트 실행*
>대부분의 테스트 클래스를 동기화 없이 병렬로 실행할 수 있지만 일부 테스트 클래스를 격리하여 실행해야 하는 경우 후자를 `@Isolated` annotation으로 표시할 수 있습니다. 이러한 클래스의 테스트는 동시에 실행되는 다른 테스트 없이 순차적으로 실행됩니다.

공유 리소스를 고유하게 식별하는 문자열 외에도 액세스 모드를 지정할 수 있습니다. 공유 리소스에 대한 `READ` 액세스가 필요한 두 테스트는 서로 병렬로 실행될 수 있지만 동일한 공유 리소스에 대한 `READ_WRITE` 액세스가 필요한 다른 테스트가 실행되는 동안에는 실행되지 않습니다.

```java
@Execution(CONCURRENT)
class SharedResourcesDemo {

    private Properties backup;

    @BeforeEach
    void backup() {
        backup = new Properties();
        backup.putAll(System.getProperties());
    }

    @AfterEach
    void restore() {
        System.setProperties(backup);
    }

    @Test
    @ResourceLock(value = SYSTEM_PROPERTIES, mode = READ)
    void customPropertyIsNotSetByDefault() {
        assertNull(System.getProperty("my.prop"));
    }

    @Test
    @ResourceLock(value = SYSTEM_PROPERTIES, mode = READ_WRITE)
    void canSetCustomPropertyToApple() {
        System.setProperty("my.prop", "apple");
        assertEquals("apple", System.getProperty("my.prop"));
    }

    @Test
    @ResourceLock(value = SYSTEM_PROPERTIES, mode = READ_WRITE)
    void canSetCustomPropertyToBanana() {
        System.setProperty("my.prop", "banana");
        assertEquals("banana", System.getProperty("my.prop"));
    }

}
```

## 2.20. Built-in Extensions

JUnit 팀은 재사용 가능한 확장을 별도의 라이브러리에 패키징하고 유지하도록 권장하지만 JUnit Jupiter API artifact는 사용자가 다른 종속성을 추가할 필요가 없어야 하는 일반적으로 유용한 것으로 간주되는 몇 가지 사용자 대면 확장 구현을 포함합니다.

### 2.20.1. The TempDirectory Extension

>`@TempDir`*은 실험적인 기능입니다.*
JUnit 팀이 이 기능을 개선하고 궁극적으로 *홍보*할 수 있도록 시도해 보고 피드백을 제공하도록 초대받았습니다.

기본 제공 `TempDirectory` 확장은 개별 테스트 또는 테스트 클래스의 모든 테스트를 위한 임시 디렉터리를 만들고 정리하는 데 사용됩니다. 기본적으로 등록되어 있습니다. 그것을 사용하려면 `@TempDir`로 `java.nio.file.Path` 또는 `java.io.File` 유형의 필드에 annotation을 추가하거나 `@TempDir`로 annotation이 달린 `java.nio.file.Path` 또는 `java.io.File` 유형의 매개변수를 추가하십시오. 수명 주기 방법 또는 테스트 방법.

예를 들어 다음 테스트는 단일 테스트 메소드에 대해 `@TempDir` annotation이 달린 매개변수를 선언하고 임시 디렉토리에 파일을 생성 및 기록하고 그 내용을 확인합니다.

*A test method that requires a temporary directory*
```java
@Test
void writeItemsToFile(@TempDir Path tempDir) throws IOException {
    Path file = tempDir.resolve("test.txt");

    new ListWriter(file).write("a", "b", "c");

    assertEquals(singletonList("a,b,c"), Files.readAllLines(file));
}
```

annotation이 달린 여러 매개변수를 지정하여 여러 임시 디렉토리를 삽입할 수 있습니다.

*A test method that requires multiple temporary directories*
```java
@Test
void copyFileFromSourceToTarget(@TempDir Path source, @TempDir Path target) throws IOException {
    Path sourceFile = source.resolve("test.txt");
    new ListWriter(sourceFile).write("a", "b", "c");

    Path targetFile = Files.copy(sourceFile, target.resolve("test.txt"));

    assertNotEquals(sourceFile, targetFile);
    assertEquals(singletonList("a,b,c"), Files.readAllLines(targetFile));
}
```

>전체 테스트 클래스 또는 메소드에 대해 단일 임시 디렉터리를 사용하는 이전 동작으로 되돌리려면(어노테이션이 사용된 수준에 따라 다름) `junit.jupiter.tempdir.scope` 구성 매개변수를 `per_context`로 설정할 수 있습니다. 그러나 이 옵션은 더 이상 사용되지 않으며 향후 릴리스에서 제거될 예정입니다.

`@TempDir`은 생성자 매개변수에서 지원되지 않습니다. 수명 주기 메소드와 현재 테스트 메소드에서 임시 디렉터리에 대한 단일 참조를 유지하려면 인스턴스 필드에 `@TempDir` annotation을 추가하여 필드 주입을 사용하세요.

다음 예는 공유 임시 디렉토리를 정적 필드에 저장합니다. 이를 통해 테스트 클래스의 모든 수명 주기 메소드 및 테스트 메소드에서 동일한 `sharedTempDir`을 사용할 수 있습니다. 더 나은 격리를 위해 각 테스트 메소드가 별도의 디렉터리를 사용하도록 인스턴스 필드를 사용해야 합니다.

*A test class that shares a temporary directory across test methods*
```java
class SharedTempDirectoryDemo {

    @TempDir
    static Path sharedTempDir;

    @Test
    void writeItemsToFile() throws IOException {
        Path file = sharedTempDir.resolve("test.txt");

        new ListWriter(file).write("a", "b", "c");

        assertEquals(singletonList("a,b,c"), Files.readAllLines(file));
    }

    @Test
    void anotherTestThatUsesTheSameTempDir() {
        // use sharedTempDir
    }

}
```

# 3. Migrating from JUnit 4

JUnit Jupiter 프로그래밍 모델 및 확장 모델은 기본적으로 규칙 및 Runner와 같은 JUnit 4 기능을 지원하지 않지만 소스 코드 유지 관리자는 migration을 위해 기존 테스트, 테스트 확장 및 사용자 정의 빌드 테스트 인프라를 모두 업데이트해야 할 것으로 예상되지 않습니다. JUnit 목성에.

대신 JUnit은 JUnit 3 및 JUnit 4를 기반으로 하는 기존 테스트가 JUnit 플랫폼 인프라를 사용하여 실행될 수 있도록 하는 JUnit Vintage 테스트 엔진을 통해 부드러운 migration 경로를 제공합니다. JUnit Jupiter와 관련된 모든 클래스와 annotation은 새로운 `org.junit.jupiter` 기본 패키지 아래에 있으므로 클래스 경로에 JUnit 4와 JUnit Jupiter가 모두 있으면 충돌이 발생하지 않습니다. 따라서 JUnit Jupiter 테스트와 함께 기존 JUnit 4 테스트를 유지하는 것이 안전합니다. 또한 JUnit 팀이 JUnit 4.x 기준선에 대한 유지 관리 및 버그 수정 릴리스를 계속 제공할 것이기 때문에 개발자는 자신의 일정에 따라 JUnit Jupiter로 migration할 충분한 시간이 있습니다.

## 3.1. Running JUnit 4 Tests on the JUnit Platform

`junit-vintage-engine` artifact가 테스트 런타임 경로에 있는지 확인하십시오. 이 경우 JUnit 3 및 JUnit 4 테스트는 JUnit 플랫폼 런처에 의해 자동으로 선택됩니다.

junit5-samples](https://github.com/junit-team/junit5-samples) repository의 예제 프로젝트를 참조하여 Gradle 및 Maven으로 이 작업을 수행하는 방법을 알아보세요.

### 3.1.1. Categories Support

`@Category`로 annotation이 달린 테스트 클래스 또는 메소드의 경우 JUnit Vintage 테스트 엔진은 카테고리의 정규화된 클래스 이름을 해당 테스트 식별자의 `태그`로 노출합니다. 예를 들어 테스트 메소드에 `@Category(Example.class)` annotation이 달린 경우 `"com.acme.Example`"로 태그가 지정됩니다. JUnit 4의 카테고리 runner와 유사하게 이 정보는 검색된 테스트를 실행하기 전에 필터링하는 데 사용할 수 있습니다(자세한 내용은 `테스트 실행(Runnig Test)` 참조).

## 3.2. Migration Tips

다음은 기존 JUnit 4 테스트를 JUnit Jupiter로 migration할 때 알고 있어야 하는 주제입니다.

- Annotation은 `org.junit.jupiter.api` 패키지에 있습니다.
- Assertion은 `org.junit.jupiter.api.Assertions`에 있습니다.
   - `org.junit.Assert` 또는 [AssertJ](https://joel-costigliola.github.io/assertj/), [Hamcrest](https://hamcrest.org/JavaHamcrest/), [Truth](https://truth.dev/) 등과 같은 다른 주장 라이브러리의 주장 방법을 계속 사용할 수 있습니다.
- Assumption은 `org.junit.jupiter.api.Assumptions`에 있습니다.
   - JUnit Jupiter 5.4 이상 버전은 가정을 위해 JUnit 4의 `org.junit.Assume` 클래스에서 메소드를 지원합니다. 특히 JUnit Jupiter는 JUnit 4의 `AssumptionViolatedException`을 지원하여 테스트가 실패로 표시되는 대신 중단되어야 함을 알립니다.
- `@Before` 및 `@After`는 더 이상 존재하지 않습니다. 대신 `@BeforeEac`h 및 `@AfterEach`를 사용하십시오.
- `@BeforeClass` 및 `@AfterClass`는 더 이상 존재하지 않습니다. 대신 `@BeforeAll` 및 `@AfterAll`을 사용하십시오.
- `@Ignore`가 더 이상 존재하지 않음: 대신 `@Disable`d 또는 다른 built-in 실행 조건 중 하나를 사용하십시오.
   - `JUnit 4 @Ignore 지원(JUnit 4 @Ignore Support)`도 참조하십시오.
- `@Category`는 더 이상 존재하지 않습니다. 대신 `@Tag`를 사용하십시오.
- `@RunWith`는 더 이상 존재하지 않습니다. `@ExtendWith`로 대체되었습니다.
- `@Rule` 및 `@ClassRule`은 더 이상 존재하지 않습니다. `@ExtendWith` 및 `@RegisterExtension`으로 대체됨 
   - `Limited JUnit 4 Rule Support` 참조하세요.

## 3.3. Limited JUnit 4 Rule Support

위에서 언급했듯이 JUnit Jupiter는 기본적으로 JUnit 4 규칙을 지원하지 않으며 지원하지도 않습니다. 그러나 JUnit 팀은 많은 조직, 특히 대규모 조직이 사용자 지정 규칙을 사용하는 대규모 JUnit 4 코드 기반을 가질 가능성이 높다는 것을 알고 있습니다. 이러한 조직에 서비스를 제공하고 점진적인 migration 경로를 활성화하기 위해 JUnit 팀은 JUnit Jupiter 내에서 JUnit 4 규칙을 그대로 지원하기로 결정했습니다. 이 지원은 어댑터를 기반으로 하며 JUnit Jupiter 확장 모델과 의미적으로 호환되는 규칙, 즉 테스트의 전체 실행 흐름을 완전히 변경하지 않는 규칙으로 제한됩니다.

JUnit Jupiter의 `junit-jupiter-migrationsupport` 모듈은 현재 이러한 유형의 subclasses를 포함하여 다음 세 가지 규칙 유형을 지원합니다.

- `org.junit.rules.ExternalResource` (including `org.junit.rules.TemporaryFolder`)
- `org.junit.rules.Verifier` (including `org.junit.rules.ErrorCollector`)
- `org.junit.rules.ExpectedException`

JUnit 4에서와 같이 규칙 annotation 필드와 메소드가 지원됩니다. 테스트 클래스에서 이러한 클래스 수준 확장을 사용하면 JUnit 4 규칙 가져오기 문을 포함하여 레거시 코드 기반의 `규칙` 구현을 변경하지 않은 상태로 둘 수 있습니다.

이 제한된 형태의 규칙 지원은 클래스 수준 annotation [@EnableRuleMigrationSupport](https://junit.org/junit5/docs/current/api/org.junit.jupiter.migrationsupport/org/junit/jupiter/migrationsupport/rules/EnableRuleMigrationSupport.html)에 의해 켜질 수 있습니다. 이 annotation은 모든 규칙 migration 지원 확장인 `VerifierSupport`, `ExternalResourceSupport` 및 `ExpectedExceptionSupport`를 활성화하는 구성된 annotation입니다. 또는 규칙 및 JUnit 4의 `@Ignore` annotation에 대한 migration 지원을 등록하는 `@EnableJUnit4MigrationSupport`로 테스트 클래스에 annotation을 달도록 선택할 수 있습니다(`JUnit 4 @Ignore 지원` 참조).

그러나 JUnit 5에 대한 새 확장을 개발하려는 경우 JUnit 4의 규칙 기반 모델 대신 JUnit Jupiter의 새 확장 모델을 사용하십시오.

## 3.4. JUnit 4 @Ignore Support

JUnit 4에서 JUnit Jupiter로의 원활한 migration 경로를 제공하기 위해 `junit-jupiter-migrationsupport` 모듈은 Jupiter의 [@Disabled](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Disabled.html) annotation과 유사한 JUnit 4의 `@Ignore` annotation에 대한 지원을 제공합니다.

JUnit Jupiter 기반 테스트와 함께 `@Ignore`를 사용하려면 빌드에서 junit-jupiter-migrationsupport 모듈에 대한 테스트 종속성을 구성한 다음 테스트 클래스에 `@ExtendWith(IgnoreCondition.class)` 또는 `@EnableJUnit4MigrationSupport`(이는 `IgnoreCondition`을 다음과 함께 자동으로 등록합니다. `제한된 JUnit 4 규칙 지원(Limited JUnit 4 Rule Support)`). `IgnoreCondition`은 `@Ignore` annotation이 달린 테스트 클래스 또는 테스트 메소드를 비활성화하는 []ExecutionCondition](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ExecutionCondition.html)입니다.

```java
import org.junit.Ignore;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.migrationsupport.EnableJUnit4MigrationSupport;

// @ExtendWith(IgnoreCondition.class)
@EnableJUnit4MigrationSupport
class IgnoredTestsDemo {

    @Ignore
    @Test
    void testWillBeIgnored() {
    }

    @Test
    void testWillBeExecuted() {
    }
}
```

# 4. Running Tests

## 4.1. IDE Support

### 4.1.1. IntelliJ IDEA

IntelliJ IDEA는 버전 2016.2부터 JUnit 플랫폼에서 테스트 실행을 지원합니다. 자세한 내용은 [IntelliJ IDEA 블로그의 게시물](https://blog.jetbrains.com/idea/2016/08/using-junit-5-in-intellij-idea/)을 참조하세요. 그러나 이러한 최신 버전의 IDEA는 프로젝트에 사용된 API 버전을 기반으로 다음 JAR을 자동으로 다운로드하므로 IDEA 2017.3 이상을 사용하는 것이 좋습니다. `junit-platform-launcher`, `junit-jupiter-engine` 및 `junit-vintage-engine`.

>IDEA 2017.3 이전 IntelliJ IDEA 릴리스는 JUnit 5의 특정 버전을 번들합니다. 따라서 JUnit Jupiter의 최신 버전을 사용하려는 경우 버전 충돌로 인해 IDE 내에서 테스트 실행이 실패할 수 있습니다. 이러한 경우 IntelliJ IDEA와 함께 번들로 제공되는 JUnit 5보다 최신 버전의 JUnit 5를 사용하려면 아래 지침을 따르십시오.

다른 JUnit 5 버전(예: 5.8.2)을 사용하려면 해당 버전의 `junit-platform-launcher`, `junit-jupiter-engine` 및 `junit-vintage-engine` JAR을 클래스 경로에 포함해야 할 수 있습니다.

*Additional Gradle Dependencies*
```groovy
testImplementation(platform("org.junit:junit-bom:5.8.2"))
testRuntimeOnly("org.junit.platform:junit-platform-launcher") {
  because("Only needed to run tests in a version of IntelliJ IDEA that bundles older versions")
}
testRuntimeOnly("org.junit.jupiter:junit-jupiter-engine")
testRuntimeOnly("org.junit.vintage:junit-vintage-engine")
```

*Additional Maven Dependencies*
```xml
<!-- ... -->
<dependencies>
    <!-- Only needed to run tests in a version of IntelliJ IDEA that bundles older versions -->
    <dependency>
        <groupId>org.junit.platform</groupId>
        <artifactId>junit-platform-launcher</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.vintage</groupId>
        <artifactId>junit-vintage-engine</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.junit</groupId>
            <artifactId>junit-bom</artifactId>
            <version>5.8.2</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 4.1.2. Eclipse

Eclipse IDE는 Eclipse Oxygen.1a(4.7.1a) 릴리스부터 JUnit 플랫폼에 대한 지원을 제공합니다.

Eclipse에서 JUnit 5를 사용하는 방법에 대한 자세한 내용은 [Eclipse Project Oxygen.1a(4.7.1a) - 새롭고 주목할만한(New and Noteworthy)](https://www.eclipse.org/eclipse/news/4.7.1a/#junit-5-support)문서의 JUnit 5에 대한 공식 Eclipse 지원 섹션을 참조하세요.

### 4.1.3. NetBeans

NetBeans는 [Apache NetBeans 10.0 릴리스](https://netbeans.apache.org/download/nb100/nb100.html) 이후 JUnit Jupiter 및 JUnit 플랫폼에 대한 지원을 제공합니다.

자세한 내용은 [Apache NetBeans 10.0 릴리스 노트](https://netbeans.apache.org/download/nb100/index.html#_junit_5)의 JUnit 5 섹션을 참조하십시오.

### 4.1.4. Visual Studio Code

[Visual Studio Code](https://code.visualstudio.com/)는 기본적으로 [Java Extension Pack](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)의 일부로 설치되는 [Java Test Runner](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-test) 확장을 통해 JUnit Jupiter 및 JUnit 플랫폼을 지원합니다.

자세한 내용은 [Visual Studio Code 설명서에서 Java의 테스트 섹션](https://code.visualstudio.com/docs/languages/java#_testing)을 참조하세요.

### 4.1.5. Other IDEs

이전 섹션에 나열된 것 이외의 편집기 또는 IDE를 사용하는 경우 JUnit 팀은 JUnit 5 사용을 지원하는 두 가지 대체 솔루션을 제공합니다. `콘솔 시작 관리자`를 수동으로 사용할 수 있습니다. 또는 IDE에 JUnit 4 지원 기능이 내장된 경우 `JUnit 4 기반 Runner`로 테스트를 실행하십시오.

## 4.2. Build Support

### 4.2.1. Gradle

>*JUnit 플랫폼 Gradle 플러그인이 중단되었습니다.*
JUnit 팀에서 개발한 `junit-platform-gradle-plugin`은 JUnit 플랫폼 1.2에서 더 이상 사용되지 않으며 1.3에서 중단되었습니다. Gradle의 표준 *테스트* 작업으로 전환하십시오.

[버전 4.6](https://docs.gradle.org/4.6/release-notes.html)부터 Gradle은 JUnit 플랫폼에서 테스트를 실행하기 위한 [기본 지원(native support)](https://docs.gradle.org/current/userguide/java_testing.html#using_junit5)을 제공합니다. 활성화하려면 `build.gradle`의 `test` 작업 선언 내에서 `useJUnitPlatform()`을 지정하기만 하면 됩니다.

```groovy
test {
    useJUnitPlatform()
}
```

`tags`, `tag expresskons` 또는 엔진별 필터링도 지원됩니다.

```groovy
test {
    useJUnitPlatform {
        includeTags("fast", "smoke & feature-a")
        // excludeTags("slow", "ci")
        includeEngines("junit-jupiter")
        // excludeEngines("junit-vintage")
    }
}
```

전체 옵션 목록은 [공식 Gradle 문서](https://docs.gradle.org/current/userguide/java_plugin.html#sec:java_test)를 참조하세요.

#### Configuration Parameters
표준 Gradle 테스트 작업은 현재 테스트 검색 및 실행에 영향을 미치도록 JUnit 플랫폼 `구성 매개변수`를 설정하는 전용 DSL을 제공하지 않습니다. 그러나 시스템 속성(아래 참조) 또는 `junit-platform.properties` 파일을 통해 빌드 스크립트 내에서 구성 매개변수를 제공할 수 있습니다.

```groovy
test {
    // ...
    systemProperty("junit.jupiter.conditions.deactivate", "*")
    systemProperty("junit.jupiter.extensions.autodetection.enabled", true)
    systemProperty("junit.jupiter.testinstance.lifecycle.default", "per_class")
    // ...
}
```

#### Configuring Test Engines
테스트를 전혀 실행하려면 `TestEngine` 구현이 클래스 경로에 있어야 합니다.

JUnit Jupiter 기반 테스트에 대한 지원을 구성하려면 다음과 유사한 종속성 집계 JUnit Jupiter artifact에 대한 `testImplementation` 종속성을 구성하십시오.

```groovy
dependencies {
    testImplementation("org.junit.jupiter:junit-jupiter:5.8.2")
}
```

JUnit 플랫폼은 JUnit 4에 대한 `testImplementation` 종속성과 다음과 유사한 JUnit Vintage `TestEngine` 구현에 대한 `testRuntimeOnly` 종속성을 구성하는 한 JUnit 4 기반 테스트를 실행할 수 있습니다.

```groovy
dependencies {
    testImplementation("junit:junit:4.13.2")
    testRuntimeOnly("org.junit.vintage:junit-vintage-engine:5.8.2")
}
```

#### Configuring Logging (optional)
JUnit은 경고 및 디버그 정보를 생성하기 위해 `java.util.loggin`g 패키지(JUL)의 Java 로깅 API를 사용합니다. 구성 옵션은 [LogManager](https://docs.oracle.com/javase/8/docs/api/java/util/logging/LogManager.html)의 공식 문서를 참조하십시오.

또는 [Log4j](https://logging.apache.org/log4j/2.x/) 또는 [Logback](https://logback.qos.ch/)과 같은 다른 로깅 프레임워크로 로그 메시지를 리디렉션할 수 있습니다. [LogManager](https://docs.oracle.com/javase/8/docs/api/java/util/logging/LogManager.html)의 사용자 정의 구현을 제공하는 로깅 프레임워크를 사용하려면 `java.util.logging.manager` 시스템 속성을 사용할 [LogManager](https://docs.oracle.com/javase/8/docs/api/java/util/logging/LogManager.html) 구현의 정규화된 클래스 이름으로 설정하십시오. 아래 예는 Log4j 2.x를 구성하는 방법을 보여줍니다(자세한 내용은 [Log4j JDK 로깅 어댑터(Log4j JDK Logging Adapter)](https://logging.apache.org/log4j/2.x/log4j-jul/index.html) 참조).

```groovy
test {
    systemProperty("java.util.logging.manager", "org.apache.logging.log4j.jul.LogManager")
}
```

다른 로깅 프레임워크는 `java.util.logging`을 사용하여 로깅된 메시지를 리디렉션하는 다른 방법을 제공합니다. 예를 들어, [Logback](https://logback.qos.ch/)의 경우 런타임 클래스 경로에 추가 종속성을 추가하여 [JUL에서 SLF4J로 브리지](https://www.slf4j.org/legacy.html#jul-to-slf4j)를 사용할 수 있습니다.

#### 4.2.2. Maven

>*JUnit 플랫폼 Maven Surefire 공급자가 중단되었습니다.*
원래 JUnit 팀에서 개발한 `junit-platform-surefire-provider`는 JUnit 플랫폼 1.3에서 더 이상 사용되지 않으며 1.4에서 중단되었습니다. 대신 Maven Surefire의 기본 지원을 사용하세요.

[버전 2.22.0](https://issues.apache.org/jira/browse/SUREFIRE-1330)부터 Maven Surefire 및 Maven Failsafe는 JUnit 플랫폼에서 테스트를 실행하기 위한 [기본 지원(native support)](https://maven.apache.org/surefire/maven-surefire-plugin/examples/junit-platform.html)을 제공합니다. [junit5-jupiter-starter-maven](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-maven) 프로젝트의 `pom.xml` 파일은 Maven Surefire 플러그인을 사용하는 방법을 보여주며 Maven 빌드 구성을 위한 시작점 역할을 할 수 있습니다.

#### Configuring Test Engines
Maven Surefire 또는 Maven Failsafe가 모든 테스트를 실행하도록 하려면 테스트 클래스 경로에 하나 이상의 `TestEngine` 구현을 추가해야 합니다.

JUnit Jupiter 기반 테스트에 대한 지원을 구성하려면 다음과 유사한 JUnit Jupiter API 및 JUnit Jupiter `TestEngine` 구현에 대한 테스트 범위 종속성을 구성하십시오.

```xml
<!-- ... -->
<dependencies>
    <!-- ... -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.8.2</version>
        <scope>test</scope>
    </dependency>
    <!-- ... -->
</dependencies>
<build>
    <plugins>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.22.2</version>
        </plugin>
        <plugin>
            <artifactId>maven-failsafe-plugin</artifactId>
            <version>2.22.2</version>
        </plugin>
    </plugins>
</build>
<!-- ... -->
```

Maven Surefire 및 Maven Failsafe는 JUnit 4 및 다음과 유사한 JUnit Vintage `TestEngine` 구현에 대한 `테스트` 범위 종속성을 구성하는 한 Jupiter 테스트와 함께 JUnit 4 기반 테스트를 실행할 수 있습니다.

```xml
<!-- ... -->
<dependencies>
    <!-- ... -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.vintage</groupId>
        <artifactId>junit-vintage-engine</artifactId>
        <version>5.8.2</version>
        <scope>test</scope>
    </dependency>
    <!-- ... -->
</dependencies>
<!-- ... -->
<build>
    <plugins>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.22.2</version>
        </plugin>
        <plugin>
            <artifactId>maven-failsafe-plugin</artifactId>
            <version>2.22.2</version>
        </plugin>
    </plugins>
</build>
<!-- ... -->
```

#### Filtering by Test Class Names
Maven Surefire 플러그인은 정규화된 이름이 다음 패턴과 일치하는 테스트 클래스를 검색합니다.

- `**/Test*.java`
- `**/*Test.java`
- `**/*Tests.java`
- `**/*TestCase.java`

또한 기본적으로 모든 nested 클래스(static member classes 포함)를 제외합니다.

그러나 `pom.xml` 파일에서 명시적 `include` 및 `exclude` 규칙을 구성하여 이 기본 동작을 재정의할 수 있습니다. 예를 들어 Maven Surefire가 static member classes를 제외하지 않도록 하려면 다음과 같이 제외 규칙을 재정의할 수 있습니다.

*Overriding exclude rules of Maven Surefire*
```xml
<!-- ... -->
<build>
    <plugins>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.22.2</version>
            <configuration>
                <excludes>
                    <exclude/>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
<!-- ... -->
```

자세한 내용은 Maven Surefire에 대한 [테스트 포함 및 제외 문서(Inclusions and Exclusions of Tests)](https://maven.apache.org/surefire/maven-surefire-plugin/examples/inclusion-exclusion.html)를 참조하세요.

#### Filtering by Tags
다음 구성 속성을 사용하여 `tags` 또는 `tag expresskons`으로 테스트를 필터링할 수 있습니다.

- to include tags or tag expressions, use `groups`.
- to exclude tags or tag expressions, use `excludedGroups`.

```xml
<!-- ... -->
<build>
    <plugins>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.22.2</version>
            <configuration>
                <groups>acceptance | !feature-a</groups>
                <excludedGroups>integration, regression</excludedGroups>
            </configuration>
        </plugin>
    </plugins>
</build>
<!-- ... -->
```

#### Configuration Parameters
구성 `매개변수 속성`을 선언하고 Java `속성(Properties)` 파일 구문(아래 참조)을 사용하거나 `junit-platform.properties` 파일을 통해 키-값 쌍을 제공하여 JUnit 플랫폼 구성 매개변수를 설정하여 테스트 검색 및 실행에 영향을 줄 수 있습니다.

```xml
<!-- ... -->
<build>
    <plugins>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.22.2</version>
            <configuration>
                <properties>
                    <configurationParameters>
                        junit.jupiter.conditions.deactivate = *
                        junit.jupiter.extensions.autodetection.enabled = true
                        junit.jupiter.testinstance.lifecycle.default = per_class
                    </configurationParameters>
                </properties>
            </configuration>
        </plugin>
    </plugins>
</build>
<!-- ... -->
```

### 4.2.3. Ant

[Ant](https://ant.apache.org/) 버전 `1.10.3`부터 JUnit 플랫폼에서 테스트를 시작하기 위한 기본 지원을 제공하기 위해 새로운 `junitlauncher` 작업이 도입되었습니다. `junitlauncher` 작업은 JUnit 플랫폼을 시작하고 선택한 테스트 모음을 전달하는 단독 책임입니다. 그런 다음 JUnit 플랫폼은 등록된 테스트 엔진에 위임하여 테스트를 검색하고 실행합니다.

`junitlauncher` 작업은 사용자가 테스트 엔진에서 실행하려는 테스트를 선택할 수 있도록 [리소스 컬렉션(resource collections)](https://ant.apache.org/manual/Types/resources.html#collection)과 같은 기본 Ant 구성과 최대한 가깝게 정렬하려고 시도합니다. 이것은 다른 많은 핵심 Ant 태스크와 비교할 때 태스크에 일관되고 자연스러운 느낌을 줍니다.

Ant 버전 `1.10.6`부터 `junitlauncher` 태스크는 별도의 [JVM에서 테스트 분기](https://ant.apache.org/manual/Tasks/junitlauncher.html#fork)를 지원합니다.

[junit5-jupiter-starter-ant](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-ant) 프로젝트의 `build.xml` 파일은 작업 사용 방법을 보여주며 시작점 역할을 할 수 있습니다.

#### Basic Usage
다음 예제는 단일 테스트 클래스(예: `org.myapp.test.MyFirstJUnit5Test`)를 선택하도록 `junitlauncher` 작업을 구성하는 방법을 보여줍니다.

```xml
<path id="test.classpath">
    <!-- The location where you have your compiled classes -->
    <pathelement location="${build.classes.dir}" />
</path>

<!-- ... -->

<junitlauncher>
    <classpath refid="test.classpath" />
    <test name="org.myapp.test.MyFirstJUnit5Test" />
</junitlauncher>
```

`테스트` 요소를 사용하면 선택 및 실행하려는 단일 테스트 클래스를 지정할 수 있습니다. `classpath` 요소를 사용하면 JUnit 플랫폼을 시작하는 데 사용할 클래스 경로를 지정할 수 있습니다. 이 클래스 경로는 실행의 일부인 테스트 클래스를 찾는 데에도 사용됩니다.

다음 예는 여러 위치에서 테스트 클래스를 선택하도록 `junitlauncher` 태스크를 구성하는 방법을 보여줍니다.

```xml
<path id="test.classpath">
    <!-- The location where you have your compiled classes -->
    <pathelement location="${build.classes.dir}" />
</path>
<!-- ... -->
<junitlauncher>
    <classpath refid="test.classpath" />
    <testclasses outputdir="${output.dir}">
        <fileset dir="${build.classes.dir}">
            <include name="org/example/**/demo/**/" />
        </fileset>
        <fileset dir="${some.other.dir}">
            <include name="org/myapp/**/" />
        </fileset>
    </testclasses>
</junitlauncher>
```

위의 예에서 `testclasses` 요소를 사용하면 다른 위치에 있는 여러 테스트 클래스를 선택할 수 있습니다.

사용법 및 구성 옵션에 대한 자세한 내용은 `junitlauncher` [task](https://ant.apache.org/manual/Tasks/junitlauncher.html)에 대한 공식 Ant 문서를 참조하십시오.

## 4.3. Console Launcher

[ConsoleLauncher](https://junit.org/junit5/docs/current/api/org.junit.platform.console/org/junit/platform/console/ConsoleLauncher.html)는 콘솔에서 JUnit 플랫폼을 시작할 수 있는 명령줄 Java 응용 프로그램입니다. 예를 들어 JUnit Vintage 및 JUnit Jupiter 테스트를 실행하고 테스트 실행 결과를 콘솔에 인쇄하는 데 사용할 수 있습니다.

모든 종속성이 포함된 실행 가능한 `junit-platform-console-standalone-1.8.2.jar`은 [junit-platform-console-standalone](https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/) 디렉토리 아래 [Maven Central](https://search.maven.org/) repository에 게시됩니다. 아래와 같이 독립형 ConsoleLauncher를 [실행](https://docs.oracle.com/javase/tutorial/deployment/jar/run.html)할 수 있습니다.

`java -jar junit-platform-console-standalone-1.8.2.jar` `<Options>`

Here’s an example of its output:

```
├─ JUnit Vintage
│  └─ example.JUnit4Tests
│     └─ standardJUnit4Test ✔
└─ JUnit Jupiter
   ├─ StandardTests
   │  ├─ succeedingTest() ✔
   │  └─ skippedTest() ↷ for demonstration purposes
   └─ A special test case
      ├─ Custom test name containing spaces ✔
      ├─ ╯°□°)╯ ✔
      └─ 😱 ✔

Test run finished after 64 ms
[         5 containers found      ]
[         0 containers skipped    ]
[         5 containers started    ]
[         0 containers aborted    ]
[         5 containers successful ]
[         0 containers failed     ]
[         6 tests found           ]
[         1 tests skipped         ]
[         5 tests started         ]
[         0 tests aborted         ]
[         5 tests successful      ]
[         0 tests failed          ]
```

>*Exit Code*
컨테이너 또는 테스트가 실패한 경우 `ConsoleLauncher`가 상태 코드 `1`과 함께 종료됩니다. 테스트가 검색되지 않고 `--fail-if-no-tests` 명령줄 옵션이 제공되면 `ConsoleLauncher`가 상태 코드 `2`와 함께 종료됩니다. 그렇지 않으면 종료 코드는 `0`입니다.

### 4.3.1. Options

```
Usage: ConsoleLauncher [-h] [--disable-ansi-colors] [--disable-banner]
                       [--fail-if-no-tests] [--scan-modules] [--scan-classpath[=PATH[;|:
                       PATH...]]]... [--details=MODE] [--details-theme=THEME]
                       [--reports-dir=DIR] [-c=CLASS]... [--config=KEY=VALUE]... [-cp=PATH
                       [;|:PATH...]]... [-d=DIR]... [-e=ID]... [-E=ID]...
                       [--exclude-package=PKG]... [-f=FILE]... [--include-package=PKG]...
                       [-m=NAME]... [-n=PATTERN]... [-N=PATTERN]... [-o=NAME]...
                       [-p=PKG]... [-r=RESOURCE]... [-t=TAG]... [-T=TAG]... [-u=URI]...
Launches the JUnit Platform from the console.
  -h, --help                 Display help information.
      --disable-ansi-colors  Disable ANSI colors in output (not supported by all
                               terminals).
      --disable-banner       Disable print out of the welcome message.
      --details=MODE         Select an output details mode for when tests are executed.
                               Use one of: none, summary, flat, tree, verbose. If 'none'
                               is selected, then only the summary and test failures are
                               shown. Default: tree.
      --details-theme=THEME  Select an output details tree theme for when tests are
                               executed. Use one of: ascii, unicode. Default: unicode.
      -cp, --classpath, --class-path=PATH[;|:PATH...]
                             Provide additional classpath entries -- for example, for
                               adding engines and their dependencies. This option can be
                               repeated.
      --fail-if-no-tests     Fail and return exit status code 2 if no tests are found.
      --reports-dir=DIR      Enable report output into a specified local directory (will
                               be created if it does not exist).
      --scan-modules         EXPERIMENTAL: Scan all resolved modules for test discovery.
  -o, --select-module=NAME   EXPERIMENTAL: Select single module for test discovery. This
                               option can be repeated.
      --scan-classpath, --scan-class-path[=PATH[;|:PATH...]]
                             Scan all directories on the classpath or explicit classpath
                               roots. Without arguments, only directories on the system
                               classpath as well as additional classpath entries supplied
                               via -cp (directories and JAR files) are scanned. Explicit
                               classpath roots that are not on the classpath will be
                               silently ignored. This option can be repeated.
  -u, --select-uri=URI       Select a URI for test discovery. This option can be repeated.
  -f, --select-file=FILE     Select a file for test discovery. This option can be
                               repeated.
  -d, --select-directory=DIR Select a directory for test discovery. This option can be
                               repeated.
  -p, --select-package=PKG   Select a package for test discovery. This option can be
                               repeated.
  -c, --select-class=CLASS   Select a class for test discovery. This option can be
                               repeated.
  -m, --select-method=NAME   Select a method for test discovery. This option can be
                               repeated.
  -r, --select-resource=RESOURCE
                             Select a classpath resource for test discovery. This option
                               can be repeated.
  -n, --include-classname=PATTERN
                             Provide a regular expression to include only classes whose
                               fully qualified names match. To avoid loading classes
                               unnecessarily, the default pattern only includes class
                               names that begin with "Test" or end with "Test" or
                               "Tests". When this option is repeated, all patterns will
                               be combined using OR semantics. Default: [^(Test.*|.+[.$]
                               Test.*|.*Tests?)$]
  -N, --exclude-classname=PATTERN
                             Provide a regular expression to exclude those classes whose
                               fully qualified names match. When this option is repeated,
                               all patterns will be combined using OR semantics.
      --include-package=PKG  Provide a package to be included in the test run. This
                               option can be repeated.
      --exclude-package=PKG  Provide a package to be excluded from the test run. This
                               option can be repeated.
  -t, --include-tag=TAG      Provide a tag or tag expression to include only tests whose
                               tags match. When this option is repeated, all patterns
                               will be combined using OR semantics.
  -T, --exclude-tag=TAG      Provide a tag or tag expression to exclude those tests whose
                               tags match. When this option is repeated, all patterns
                               will be combined using OR semantics.
  -e, --include-engine=ID   테스트 실행에 포함될 엔진의 ID를 제공하십시오.
                                이 옵션은 반복할 수 있습니다.
  -E, --exclude-engine=ID    테스트 실행에서 제외할 엔진의 ID를 제공하십시오. 
                                이 옵션은 반복할 수 있습니다.
      --config=KEY=VALUE     테스트 검색 및 실행을 위한 구성 매개변수를 설정합니다. 
                                이 옵션은 반복할 수 있습니다.
```

### 4.3.2. Argument Files (@-files)

일부 플랫폼에서는 옵션이 많거나 긴 인수가 있는 명령줄을 만들 때 명령줄 길이에 대한 시스템 제한이 발생할 수 있습니다.

버전 1.3부터 `ConsoleLauncher`는 @-files라고도 하는 인수 파일을 지원합니다. 인수 파일은 명령에 전달할 인수를 자체적으로 포함하는 파일입니다. 기본 [picocli](https://github.com/remkop/picocli) 명령줄 파서는 `@` 문자로 시작하는 인수를 만나면 해당 파일의 내용을 인수 목록으로 확장합니다.

파일 내의 인수는 공백이나 줄 바꿈으로 구분할 수 있습니다. 인수에 공백이 포함되어 있으면 전체 인수를 큰따옴표나 작은따옴표 로 묶어야 합니다(예: `"-f=My Files/Stuff.java"`).

인수 파일이 존재하지 않거나 읽을 수 없는 경우 인수는 문자 그대로 처리되고 제거되지 않습니다. 이로 인해 "일치하지 않는 인수" 오류 메시지가 나타날 수 있습니다. `picocli.trac`e 시스템 속성을 `DEBUG`로 설정하여 명령을 실행하여 이러한 오류를 해결할 수 있습니다.

명령줄에 여러 @-파일을 지정할 수 있습니다. 지정된 경로는 현재 디렉토리에 상대적이거나 절대 경로일 수 있습니다.

추가 `@` 기호로 이스케이프하여 초기 `@` 문자가 있는 실제 매개변수를 전달할 수 있습니다. 예를 들어 `@@somearg`는 `@somearg`가 되며 확장 대상이 아닙니다.

## 4.4. Using JUnit 4 to run the JUnit Platform

>*`JUnitPlatform` runner는 더 이상 사용되지 않습니다.*
`JUnitPlatform` runner는 JUnit 4 환경의 JUnit 플랫폼에서 test suite 및 테스트를 실행하기 위한 임시 솔루션으로 JUnit 팀에 의해 개발되었습니다.
>최근 몇 년 동안 모든 주류 빌드 도구와 IDE는 JUnit 플랫폼에서 직접 테스트를 실행하기 위한 내장 지원을 제공합니다.
>또한 `junit-platform-suite-engine` 모듈에서 제공하는 `@Suite` 지원의 도입으로 JUnitPlatform runner가 더 이상 사용되지 않습니다. 자세한 내용은 `JUnit 플랫폼 제품군 엔진(JUnit Platform Suite Engine)`을 참조하세요.
>따라서 `JUnitPlatform` runner 및 `@UseTechnicalNames` annotation은 JUnit 플랫폼 1.8에서 더 이상 사용되지 않으며 JUnit 플랫폼 2.0에서 제거됩니다.
>`JUnitPlatform` runner를 사용하는 경우 `@Suite` 지원으로 migration하십시오.

`JUnitPlatform` runner는 JUnit 4 환경 (예: JUnit Jupiter 테스트 클래스)의 JUnit 플랫폼에서 프로그래밍 모델이 지원되는 모든 테스트를 실행할 수 있는 JUnit 4 기반 `Runner`입니다.

클래스에 `@RunWith(JUnitPlatform.class)` annotation을 추가하면 JUnit 4를 지원하지만 아직 JUnit 플랫폼을 직접 지원하지 않는 IDE 및 빌드 시스템으로 클래스를 실행할 수 있습니다.

>JUnit 플랫폼에는 JUnit 4에 없는 기능이 있으므로 실행자는 특히 보고와 관련하여 JUnit 플랫폼 기능의 하위 집합만 지원할 수 있습니다(`Display names 대 Technical Names(Display Names vs. Technical Names)` 참조).

### 4.4.1. Setup

다음 artifact와 클래스 경로에 대한 종속성이 필요합니다. 그룹 ID, artifact ID 및 버전에 대한 자세한 내용은 `종속성 메타데이터(Dependency Metadata)`를 참조하세요.

#### Explicit Dependencies
- `junit-platform-runner` in test scope: location of the `JUnitPlatform` runner
- `junit-4.13.2.jar` in test scope: to run tests using JUnit 4
- `junit-jupiter-api` in test scope: API for writing tests using JUnit Jupiter, including `@Test`, etc.
- `junit-jupiter-engine` in test runtime scope: implementation of the `TestEngine` API for JUnit Jupiter

#### Transitive Dependencies
- `junit-platform-suite-api` in test scope
- `junit-platform-suite-commons` in test scope
- `junit-platform-launcher` in test scope
- `junit-platform-engine` in test scope
- `junit-platform-commons` in test scope
- `opentest4j` in test scope

### 4.4.2. Display Names vs. Technical Names

`@RunWith(JUnitPlatform.class)`를 통해 실행되는 클래스의 사용자 정의 display name을 정의하려면 `@SuiteDisplayName`으로 클래스에 annotation을 달고 사용자 정의 값을 제공하십시오.

기본적으로 display name은 테스트 artifact에 사용됩니다. 그러나 `JUnitPlatform` runner가 Gradle 또는 Maven과 같은 빌드 도구로 테스트를 실행하는 데 사용되는 경우 생성된 테스트 보고서에는 다음과 같은 짧은 display name 대신 테스트 artifact의 *technical name*(예: 정규화된 클래스 이름)이 포함되어야 하는 경우가 많습니다. 테스트 클래스의 단순 이름 또는 특수 문자가 포함된 사용자 지정 display name입니다. 보고 목적으로 기술 이름을 활성화하려면 `@RunWith(JUnitPlatform.class)`와 함께 `@UseTechnicalNames` annotation을 선언하십시오.

`@UseTechnicalNames`가 있으면 `@SuiteDisplayName`을 통해 구성된 모든 사용자 지정 display name을 재정의합니다.

### 4.4.3. Single Test Class

`JUnitPlatform` runner를 사용하는 한 가지 방법은 테스트 클래스에 `@RunWith(JUnitPlatform.class)` 직접 annotation을 추가하는 것입니다. 다음 예제의 테스트 method는 `org.junit.Test(JUnit 4)`가 아닌 `org.junit.jupiter.api.Test(JUnit Jupiter)`로 annotation 처리되어 있음을 유의하십시오. 더욱이 이 경우 테스트 클래스는 공개되어야 합니다. 그렇지 않으면 일부 IDE 및 빌드 도구에서 JUnit 4 테스트 클래스로 인식하지 못할 수 있습니다.

```java
import static org.junit.jupiter.api.Assertions.fail;

import org.junit.jupiter.api.Test;
import org.junit.runner.RunWith;

@RunWith(org.junit.platform.runner.JUnitPlatform.class)
public class JUnitPlatformClassDemo {

    @Test
    void succeedingTest() {
        /* no-op */
    }

    @Test
    void failingTest() {
        fail("Failing for failing's sake.");
    }

}
```

### 4.4.4. Test Suite

여러 테스트 클래스가 있는 경우 다음 예제에서 볼 수 있는 것처럼 test suite을 만들 수 있습니다.

```java
import org.junit.platform.suite.api.SelectPackages;
import org.junit.platform.suite.api.SuiteDisplayName;
import org.junit.runner.RunWith;

@RunWith(org.junit.platform.runner.JUnitPlatform.class)
@SuiteDisplayName("JUnit Platform Suite Demo")
@SelectPackages("example")
public class JUnitPlatformSuiteDemo {
}
```

`JUnitPlatformSuiteDemo`는 `예제` 패키지와 해당 하위 패키지의 모든 테스트를 검색하고 실행합니다. 기본적으로 이름이 `Test`로 시작하거나 `Test` 또는 `Tests`로 끝나는 테스트 클래스만 포함됩니다.

>*Additional Configuration Options*
>`@SelectPackages` 외에도 테스트 검색 및 필터링을 위한 구성 옵션이 더 있습니다. 자세한 내용은 `org.junit.platform.suite.api` 패키지의 Javadoc을 참조하십시오.
>`@RunWith(JUnitPlatform.class)` annotation이 달린 테스트 클래스 및 스위트는 JUnit 플랫폼에서 직접 실행할 수 없습니다(또는 일부 IDE에 문서화된 "JUnit 5" 테스트로). 이러한 클래스와 제품군은 JUnit 4 인프라를 통해서만 실행할 수 있습니다.

## 4.5. Configuration Parameters

포함할 테스트 클래스 및 테스트 엔진, 스캔할 패키지 등을 플랫폼에 지시하는 것 외에도 특정 테스트 엔진, 수신기 또는 등록된 확장과 관련된 추가 사용자 지정 구성 매개변수를 제공해야 하는 경우가 있습니다. 예를 들어 JUnit Jupiter `TestEngine`은 다음 사용 사례에 대한 구성 매개변수를 지원합니다.

- 기본 테스트 인스턴스 수명 주기 변경
- 자동 확장 감지 활성화
- 비활성화 조건
- 기본 display name 생성기 설정

구성 매개변수는 다음 메커니즘 중 하나를 통해 JUnit 플랫폼에서 실행되는 테스트 엔진에 제공할 수 있는 텍스트 기반 키-값 쌍(key-value pairs)입니다.

1. `Launcher API`에 제공되는 요청을 빌드하는 데 사용되는 `LauncherDiscoveryRequestBuilder`의 `configurationParameter()` 및 `configurationParameters()` 메소드. JUnit 플랫폼에서 제공하는 도구 중 하나를 통해 테스트를 실행할 때 다음과 같이 구성 매개변수를 지정할 수 있습니다.
   - `Console Launcher`: use the `--config command-line` option.
   - `Gradle`: use the `systemProperty` or `systemProperties` DSL.
   - `Maven Surefire provider`: use the `configurationParameters` property.
1. JVM system properties :  a file named junit-platform.properties in the root of the class path that follows the syntax rules for a Java Properties file.
1. JUnit 플랫폼 구성 파일: Java 속성 파일에 대한 구문 규칙을 따르는 클래스 경로의 루트에 있는 `junit-platform.properties`라는 파일입니다.

>구성 매개변수는 위에 정의된 정확한 순서로 조회됩니다. 결과적으로 `Launcher`에 직접 제공된 구성 매개변수는 시스템 속성 및 구성 파일을 통해 제공된 매개변수보다 우선합니다. 마찬가지로 시스템 속성을 통해 제공되는 구성 매개변수는 구성 파일을 통해 제공되는 매개변수보다 우선합니다.

### 4.5.1. Pattern Matching Syntax

이 섹션에서는 다음 기능에 사용되는 구성 매개변수에 적용되는 패턴 일치 구문에 대해 설명합니다.

- ` Deactivating Conditions`
- `Deactivating a TestExecutionListener`

지정된 구성 매개변수의 값이 별표(`*`)로만 구성된 경우 패턴은 모든 후보 클래스와 일치합니다. 그렇지 않으면 값은 각 패턴이 각 후보 클래스의 FQCN(정규화된 클래스 이름)과 일치하는 쉼표로 구분된 패턴 목록으로 처리됩니다. 패턴의 모든 점(`.`)은 FQCN의 점(`.`) 또는 달러 기호(`$`)와 일치합니다. 별표(`*`)는 FQCN에서 하나 이상의 문자와 일치합니다. 패턴의 다른 모든 문자는 FQCN에 대해 일대일로 일치됩니다.

Examples:
- `*`: 모든 후보 클래스와 일치합니다.
- `org.junit.*`: `org.junit` 기본 패키지 및 해당 하위 패키지 아래의 모든 후보 클래스와 일치합니다.
- `*.MyCustomImpl`: 단순 클래스 이름이 정확히 `MyCustomImpl`인 모든 후보 클래스와 일치합니다.
- `*System*`: FQCN에 `System`이 포함된 모든 후보 클래스와 일치합니다.
- `*System*+, +*Unit*`: FQCN에 `system` 또는 단위가 포함된 모든 후보 클래스와 일치합니다.
- `org.example.MyCustomImpl`: FQCN이 정확히 `org.example.MyCustomImpl`인 후보 클래스와 일치합니다.
- `org.example.MyCustomImpl, org.example.TheirCustomImpl`: FQCN이 정확히 `org.example.MyCustomImpl` 또는 `org.example.TheirCustomImpl`인 후보 클래스와 일치합니다.

## 4.6. Tags

태그는 테스트를 표시하고 필터링하기 위한 JUnit 플랫폼 개념입니다. 컨테이너 및 테스트에 태그를 추가하기 위한 프로그래밍 모델은 테스트 프레임워크에 의해 정의됩니다. 예를 들어 JUnit Jupiter 기반 테스트에서는 `@Tag` annotation(`태그 지정 및 필터링` 참조)을 사용해야 합니다. JUnit 4 기반 테스트의 경우 빈티지 엔진은 `@Category` annotation을 태그에 매핑합니다(`카테고리 지원` 참조). 다른 테스트 프레임워크는 사용자가 태그를 지정하기 위한 고유한 annotation 또는 기타 수단을 정의할 수 있습니다.

### 4.6.1. Syntax Rules for Tags

태그 지정 방법에 관계없이 JUnit 플랫폼은 다음 규칙을 적용합니다.

- 태그는 `null`이거나 공백일 수 없습니다.
- 잘린 태그는 공백을 포함할 수 없습니다.
- 트리밍된 태그에는 ISO 제어 문자가 포함되어서는 안 됩니다.
- 트리밍된 태그에는 다음 예약 문자가 포함되어서는 안 됩니다.
    - `,`: comma
    - `(`: left parenthesis
    - `)`: right parenthesis
    - `&`: ampersand
    - `|`: vertical bar
    - `!`: exclamation point

>위 컨텍스트에서 "trimmed"는 선행 및 후행 공백 문자가 제거되었음을 의미합니다.

### 4.6.2. Tag Expressions

태그 표현식은 `!`, `&` 및 `|` 연산자가 있는 부울 표현식입니다. 또한 `(` 및 `)` 연산자를 사용하여 연산자 우선 순위를 조정할 수 있습니다.

태그가 있는 모든 테스트와 태그가 없는 모든 테스트를 각각 선택하는 `any()` 및 `none()`의 두 가지 특수 표현식이 지원됩니다. 이러한 특수 표현식은 일반 태그와 마찬가지로 다른 표현식과 결합될 수 있습니다.

Table 2. Operators (in descending order of precedence)
|Operator|Meaning|Associativity|
|---|---|---|
|!|not|right|
|&|and|left|
|\||or|left|

여러 차원에 걸쳐 테스트에 태그를 지정하는 경우 태그 표현식은 실행할 테스트를 선택하는 데 도움이 됩니다. 테스트 유형(예: 마이크로, 통합, 종단 간) 및 기능(예: **제품, 카탈로그, 배송**)으로 태그를 지정할 때 다음 태그 표현식이 유용할 수 있습니다.

|Tag Expression|Selection|
|---|---|
|product|all tests for **product**|
|catalog \| shipping|all tests for catalog plus all tests for **shipping**|
|catalog & shipping|all tests for the intersection between **catalog** and **shipping**|
|product & !end-to-end|all tests for **product**, but not the end-to-end tests|
|(micro \| integration) & (product \| shipping)|all micro or integration tests for **product** or **shipping**|

## 4.7. Capturing Standard Output/Error

버전 1.3부터 JUnit 플랫폼은 `System.out` 및 `System.err`에 인쇄된 출력을 캡처하기 위한 옵트인 지원을 제공합니다. 활성화하려면 `junit.platform.output.capture.stdout` 및/또는 `junit.platform.output.capture.stderr` 구성 `매개변수`를 `true`로 설정하십시오. 또한 `junit.platform.output.capture.maxBuffer`를 사용하여 실행된 테스트 또는 컨테이너당 사용할 최대 버퍼링된 바이트 수를 구성할 수 있습니다.

활성화된 경우 JUnit 플랫폼은 해당 출력을 캡처하고 테스트 또는 컨테이너를 완료된 것으로 보고하기 직전에 등록된 모든 `TestExecutionListener` 인스턴스에 `stdout` 또는 `stderr` 키를 사용하여 보고서 항목으로 게시합니다.

캡처된 출력에는 컨테이너 또는 테스트를 실행하는 데 사용된 스레드에서 내보낸 출력만 포함됩니다. 특히 `병렬로 테스트를 실행(executing tests in parallel)`할 때 특정 테스트나 컨테이너에 속성을 부여하는 것이 불가능하기 때문에 다른 스레드의 출력은 생략됩니다.

## 4.8. Using Listeners

JUnit 플랫폼은 JUnit, 타사 및 사용자 정의 사용자 코드가 `TestPlan`의 검색 및 실행 중 다양한 지점에서 발생한 이벤트에 반응할 수 있도록 하는 다음 리스너 API를 제공합니다.

- [LauncherSessionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSessionListener.html): [LauncherSession](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSession.html)이 열리고 닫힐 때 이벤트를 수신합니다.
- [LauncherDiscoveryListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryListener.html): 테스트 검색 중에 발생하는 이벤트를 수신합니다.
- [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html): 테스트 실행 중 발생하는 이벤트를 수신합니다.

`LauncherSessionListener` API는 일반적으로 빌드 도구 또는 IDE에 의해 구현되며 빌드 도구 또는 IDE의 일부 기능을 지원하기 위해 자동으로 등록됩니다.

`LauncherDiscoveryListener` 및 `TestExecutionListener` API는 보고서 형식을 생성하거나 IDE에서 테스트 계획의 그래픽 표현을 표시하기 위해 종종 구현됩니다. 이러한 리스너는 빌드 도구 또는 IDE에 의해 구현되고 자동으로 등록되거나 third-party 라이브러리에 포함될 수 있습니다(잠재적으로 자동으로 등록될 수 있음). 자체 리스너를 구현하고 등록할 수도 있습니다.

리스너 등록 및 구성에 대한 자세한 내용은 이 가이드의 다음 섹션을 참조하십시오.

- `Registering a LauncherSessionListener`
- `Registering a LauncherDiscoveryListener`
- `Registering a TestExecutionListener`
- `Configuring a TestExecutionListener`
- `Deactivating a TestExecutionListener`

JUnit 플랫폼은 test suite와 함께 사용할 수 있는 다음 리스너를 제공합니다.

**`Flight Recorder Support`**
&nbsp;&nbsp;&nbsp;테스트 검색 및 실행 중에 Java Flight Recorder 이벤트를 생성하는 `FlightRecordingExecutionListener` 및 `FlightRecordingDiscoveryListener`.

[LegacyXmlReportGeneratingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.reporting/org/junit/platform/reporting/legacy/xml/LegacyXmlReportGeneratingListener.html)
&nbsp;&nbsp;&nbsp;JUnit 4 기반 테스트 보고서의 사실상 표준과 호환되는 XML 보고서를 생성하는 `TestExecutionListener`. 자세한 내용은 `JUnit 플랫폼 보고(JUnit Platform Reporting)`를 참조하세요.

[LoggingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/LoggingListener.html)
&nbsp;&nbsp;&nbsp;`Throwable` 및 `Supplier<String>`을 사용하는 `BiConsumer`를 통해 모든 이벤트에 대한 정보 메시지를 로깅하기 위한 `TestExecutionListener`.

[SummaryGeneratingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/SummaryGeneratingListener.html)
&nbsp;&nbsp;&nbsp;`PrintWriter`를 통해 인쇄할 수 있는 테스트 실행 요약을 생성하는 `TestExecutionListener`.

[UniqueIdTrackingListener()](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/UniqueIdTrackingListener.html)
&nbsp;&nbsp;&nbsp;`TestPlan` 실행 중 건너뛰거나 실행된 모든 테스트의 고유 ID를 추적하고 `TestPlan` 실행이 완료되면 고유 ID를 포함하는 파일을 생성하는 `TestExecutionListener`.

### 4.8.1. Flight Recorder Support

버전 1.7부터 JUnit 플랫폼은 Flight Recorder 이벤트 생성을 위한 옵트인 지원을 제공합니다. [JEP 328](https://openjdk.java.net/jeps/328)은 JFR(Java Flight Recorder)을 다음과 같이 설명합니다.

>Flight Recorder는 애플리케이션, JVM 및 OS에서 발생하는 이벤트를 기록합니다. 이벤트는 버그 보고서에 첨부할 수 있고 지원 엔지니어가 검사할 수 있는 단일 파일에 저장되므로 문제가 발생하기까지의 기간 동안 문제에 대한 사후 분석이 가능합니다.

테스트를 실행하는 동안 생성된 Flight Recorder 이벤트를 기록하려면 다음을 수행해야 합니다.

1. Java 8 업데이트 262 이상 또는 Java 11 이상을 사용하고 있는지 확인합니다.
1. 테스트 런타임 시 클래스 경로 또는 모듈 경로에 `org.junit.platform.jfr` 모듈(`junit-platform-jfr-1.8.2.jar`)을 제공합니다.
1. 테스트 실행을 시작할 때 비행 기록을 시작합니다. Flight Recorder는 Java 명령줄 옵션을 통해 시작할 수 있습니다.

`-XX:StartFlightRecording:filename=...`

적절한 명령에 대해서는 빌드 도구의 매뉴얼을 참조하십시오.

기록된 이벤트를 분석하려면 최근 JDK와 함께 제공된 [jfr](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jfr.html) 명령줄 도구를 사용하거나 [JDK Mission Control](https://jdk.java.net/jmc/)로 기록 파일을 엽니다.

>Flight Recorder 지원은 현재 실험적인 기능입니다. JUnit 팀이 이 기능을 개선하고 궁극적으로 `홍보`할 수 있도록 시도해 보고 피드백을 제공하도록 초대받았습니다.

# 5. Extension Model

## 5.1. Overview

JUnit 4의 경쟁적인 `Runner`, `TestRule` 및 `MethodRule` 확장 지점과 달리 JUnit Jupiter 확장 모델은 `Extension` API라는 일관된 단일 개념으로 구성됩니다. 그러나 `Extension` 자체는 마커 인터페이스일 뿐입니다.

## 5.2. Registering Extensions

확장은 `@ExtendWith`를 통해 선언적으로 등록하거나 `@RegisterExtension`을 통해 프로그래밍 방식으로 또는 Java의 `ServiceLoader` 메커니즘을 통해 자동으로 등록할 수 있습니다.

### 5.2.1. Declarative Extension Registration

개발자는 테스트 인터페이스, 테스트 클래스, 테스트 메소드 또는 사용자 정의 `구성 annotation(composed annotation)`에 `@ExtendWith(…​)` annotation을 달고 등록할 확장에 대한 클래스 참조를 제공하여 하나 이상의 확장을 선언적으로 등록할 수 있습니다. JUnit Jupiter 5.8부터` @ExtendWith`는 테스트 클래스 생성자의 필드 또는 매개변수, 테스트 메소드 및 `@BeforeAll`, `@AfterAll`, `@BeforeEach` 및 `@AfterEach` 수명 주기 메소드에서도 선언될 수 있습니다.

예를 들어, 특정 테스트 방법에 대해 `WebServerExtension`을 등록하려면 다음과 같이 테스트 방법에 annotation을 답니다. `WebServerExtension`이 로컬 웹 서버를 시작하고 서버의 URL을 `@WebServerUrl`로 annotation이 달린 매개변수에 주입한다고 가정합니다.

```java
@Test
@ExtendWith(WebServerExtension.class)
void getProductList(@WebServerUrl String serverUrl) {
    WebClient webClient = new WebClient();
    // Use WebClient to connect to web server using serverUrl and verify response
    assertEquals(200, webClient.get(serverUrl + "/products").getResponseStatus());
}
```

특정 클래스 및 해당 subclasses의 모든 테스트에 대해 `WebServerExtension`을 등록하려면 다음과 같이 테스트 클래스에 annotation을 추가합니다.

```java
@ExtendWith(WebServerExtension.class)
class MyTests {
    // ...
}
```

다음과 같이 여러 확장을 함께 등록할 수 있습니다.

```java
@ExtendWith({ DatabaseExtension.class, WebServerExtension.class })
class MyFirstTests {
    // ...
}
```

대안으로 다음과 같이 여러 확장을 별도로 등록할 수 있습니다.

```java
@ExtendWith(DatabaseExtension.class)
@ExtendWith(WebServerExtension.class)
class MySecondTests {
    // ...
}
```

>*Extension Registration Order*
클래스 수준, 메소드 수준 또는 매개 변수 수준에서 `@ExtendWith`를 통해 선언적으로 등록된 확장은 소스 코드에서 선언된 순서대로 실행됩니다. 예를 들어 `MyFirstTests`와 `MySecondTests` 모두에서 테스트 실행은 `DatabaseExtension`과 `WebServerExtension`에 의해 **정확히 그 순서대로** 확장됩니다.

재사용 가능한 방식으로 여러 확장을 결합하려면 다음 코드 목록과 같이 사용자 정의 구성된 annotation을 정의하고 `@ExtendWith`를 meta-annotation으로 사용할 수 있습니다. 그런 다음 `@ExtendWith({ DatabaseExtension.class, WebServerExtension.class })` 대신 `@DatabaseAndWebServerExtension`을 사용할 수 있습니다.

```java
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@ExtendWith({ DatabaseExtension.class, WebServerExtension.class })
public @interface DatabaseAndWebServerExtension {
}
```

위의 예는 `@ExtendWith`가 클래스 수준이나 메소드 수준에서 어떻게 적용될 수 있는지 보여줍니다. 그러나 특정 사용 사례의 경우 확장을 필드 또는 매개변수 수준에서 선언적으로 등록하는 것이 좋습니다. 필드에 삽입하거나 생성자, 테스트 메소드 또는 수명 주기 메소드의 매개 변수를 통해 삽입할 수 있는 난수를 생성하는 `RandomNumberExtension`을 고려하십시오. 확장이 `@ExtendWith(RandomNumberExtension.class)`로 메타 annotation이 달린 `@Random` annotation을 제공하는 경우(아래 목록 참조) 확장은 다음 `RandomNumberDemo` 예제와 같이 투명하게 사용할 수 있습니다.

```java
@Target({ ElementType.FIELD, ElementType.PARAMETER })
@Retention(RetentionPolicy.RUNTIME)
@ExtendWith(RandomNumberExtension.class)
public @interface Random {
}
```
```java
class RandomNumberDemo {

    // use random number field in test methods and @BeforeEach
    // or @AfterEach lifecycle methods
    @Random
    private int randomNumber1;

    RandomNumberDemo(@Random int randomNumber2) {
        // use random number in constructor
    }

    @BeforeEach
    void beforeEach(@Random int randomNumber3) {
        // use random number in @BeforeEach method
    }

    @Test
    void test(@Random int randomNumber4) {
        // use random number in test method
    }

}
```
>*필드의 `@ExtendWith`에 대한 확장 등록 순서*
필드에서 `@ExtendWith`를 통해 선언적으로 등록된 확장은 결정적이지만 의도적으로 명확하지 않은 알고리즘을 사용하여 `@RegisterExtension` 필드 및 기타 `@ExtendWith` 필드를 기준으로 정렬됩니다. 그러나 `@ExtendWith` 필드는 `@Order` annotation을 사용하여 정렬할 수 있습니다. 자세한 내용은 `@RegisterExtension` 필드에 대한 `확장 등록 주문` 팁을 참조하십시오.

>`@ExtendWith` 필드는 `정적`이거나 비정적일 수 있습니다. `@RegisterExtension` 필드의 `정적 필드(Static Fields)` 및 `인스턴스 필드(Instance Fields)`에 대한 문서는 `@ExtendWith` 필드에도 적용됩니다.

### 5.2.2. Programmatic Extension Registration

개발자는 [@RegisterExtension](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/RegisterExtension.html)으로 테스트 클래스의 필드에 annotation을 추가하여 프로그래밍 방식으로 확장을 등록할 수 있습니다.

확장이 `@ExtendWith`를 통해 선언적으로 등록되면 일반적으로 annotation을 통해서만 구성할 수 있습니다. 대조적으로, 확장이 `@RegisterExtension`을 통해 등록되면 확장의 생성자, 정적 팩토리 메소드 또는 빌더 API에 인수를 전달하기 위해 프로그래밍 방식으로 구성할 수 있습니다.

#### Extension Registration Order
기본적으로 `@RegisterExtension`을 통해 프로그래밍 방식으로 등록되거나 필드에서 `@ExtendWith`를 통해 선언적으로 등록된 확장은 결정적이지만 의도적으로 명확하지 않은 알고리즘을 사용하여 정렬됩니다. 이것은 test suite의 후속 실행이 동일한 순서로 확장을 실행하도록 하여 반복 가능한 빌드를 허용합니다. 그러나 확장을 명시적인 순서로 등록해야 하는 경우가 있습니다. 이를 달성하려면 `@RegisterExtension` 필드 또는` @ExtendWith `필드에 [@Order](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Order.html)를 추가하십시오.

`@Order`로 annotation이 지정되지 않은 `@RegisterExtension` 필드 또는 `@ExtendWith` 필드는 값이 `Integer.MAX_VALUE` / 2인 기본 순서를 사용하여 정렬됩니다. 이를 통해 `@Order` annotation이 있는 확장 필드를 annotation이 없는 확장 필드 앞이나 뒤에 명시적으로 정렬할 수 있습니다. 기본 순서 값보다 작은 명시적 순서 값을 가진 확장은 annotation이 없는 확장보다 먼저 등록됩니다. 마찬가지로 기본 순서 값보다 큰 명시적 순서 값을 가진 확장은 annotation이 없는 확장 후에 등록됩니다. 예를 들어, 기본 순서 값보다 큰 명시적 순서 값을 확장에 할당하면 프로그래밍 방식으로 등록된 다른 확장에 비해 콜백 확장 이전이 마지막으로 등록되고 콜백 확장 이후에 먼저 등록됩니다.

`@RegisterExtension` 필드는 `null`이 아니어야 하지만(평가 시) `정적`이거나 비정적일 수 있습니다.

#### Static Fields
`@RegisterExtension` 필드가 정적이면 `@ExtendWith`를 통해 클래스 수준에서 등록된 확장 이후에 확장이 등록됩니다. 이러한 정적 확장은 구현할 수 있는 확장 API에 제한이 없습니다. 따라서 정적 필드를 통해 등록된 확장은 `BeforeAllCallback`, `AfterAllCallback`, `TestInstancePostProcessor` 및 `TestInstancePreDestroyCallback`과 같은 클래스 수준 및 인스턴스 수준 확장 API와 `BeforeEachCallback` 등과 같은 메소드 수준 확장 API를 구현할 수 있습니다.

다음 예제에서 테스트 클래스의 서버 필드는 `WebServerExtension`에서 지원하는 빌더 패턴을 사용하여 프로그래밍 방식으로 초기화됩니다. 구성된 `WebServerExtension`은 클래스 수준 에서 확장으로 자동 등록됩니다. 또한 `@BeforeAll` 또는 `@AfterAll` annotation이 달린 정적 라이프사이클 메소드와 `@BeforeEach`, `@AfterEach`, `@Test` method는 필요한 경우 서버 필드를 통해 확장의 인스턴스에 액세스할 수 있습니다.

*Registering an extension via a static field in Java(Java에서 정적 필드를 통해 확장 등록
)*
```java
class WebServerDemo {

    @RegisterExtension
    static WebServerExtension server = WebServerExtension.builder()
        .enableSecurity(false)
        .build();

    @Test
    void getProductList() {
        WebClient webClient = new WebClient();
        String serverUrl = server.getServerUrl();
        // Use WebClient to connect to web server using serverUrl and verify response
        assertEquals(200, webClient.get(serverUrl + "/products").getResponseStatus());
    }

}
```

#### Static Fields in Kotlin
Kotlin 프로그래밍 언어에는 `static` 필드의 개념이 없습니다. 그러나 컴파일러는 Kotlin의 `@JvmStatic` annotation을 사용하여 `private static` 필드를 생성하도록 지시할 수 있습니다. Kotlin 컴파일러가 공용 정적 필드를 생성하도록 하려면 `@JvmField` annotation을 대신 사용할 수 있습니다.

다음 예제는 Kotlin으로 이식된 이전 섹션의 `WebServerDemo` 버전입니다.

*Registering an extension via a static field in Kotlin(Kotlin에서 정적 필드를 통해 확장 등록
)*
```java
class KotlinWebServerDemo {

    companion object {
        @JvmStatic
        @RegisterExtension
        val server = WebServerExtension.builder()
                .enableSecurity(false)
                .build()
    }

    @Test
    fun getProductList() {
        // Use WebClient to connect to web server using serverUrl and verify response
        val webClient = WebClient()
        val serverUrl = server.serverUrl
        assertEquals(200, webClient.get("$serverUrl/products").responseStatus)
    }
}
```

#### Instance Fields
`@RegisterExtension` 필드가 비정적(즉, 인스턴스 필드)인 경우 확장은 테스트 클래스가 인스턴스화되고 등록된 각 `TestInstancePostProcessor`에 테스트 인스턴스를 사후 처리할 기회가 주어진 후에 등록됩니다(잠재적으로 annotation 필드에 사용할 확장의 인스턴스). 따라서 이러한 *인스턴스 확장*이 `BeforeAllCallback`, `AfterAllCallback` 또는 `TestInstancePostProcessor`와 같은 클래스 수준 또는 인스턴스 수준 확장 API를 구현하는 경우 해당 API는 적용되지 않습니다. 기본적으로 인스턴스 확장은 `@ExtendWith`를 통해 메소드 수준에서 등록된 확장 이후에 등록됩니다. 그러나 테스트 클래스가 `@TestInstance(Lifecycle.PER_CLASS)` 의미 체계로 구성된 경우 인스턴스 확장은 `@ExtendWith`를 통해 메소드 수준에서 등록된 확장보다 먼저 등록됩니다.

다음 예제에서 테스트 클래스의 문서 필드는 사용자 정의 `lookUpDocsDir()` 메소드를 호출하고 `DocumentationExtension`의 정적 `forPath()` 팩토리 메소드에 결과를 제공하여 프로그래밍 방식으로 초기화됩니다. 구성된 `DocumentationExtension`은 메소드 수준에서 자동으로 확장으로 등록됩니다. 또한 `@BeforeEach`, `@AfterEach`, `@Test` 메소드는 필요한 경우 `docs` 필드를 통해 확장의 인스턴스에 액세스할 수 있습니다.

*An extension registered via an instance field(인스턴스 필드를 통해 등록된 확장자)*
```java
class DocumentationDemo {

    static Path lookUpDocsDir() {
        // return path to docs dir
    }

    @RegisterExtension
    DocumentationExtension docs = DocumentationExtension.forPath(lookUpDocsDir());

    @Test
    void generateDocumentation() {
        // use this.docs ...
    }
}
```

### 5.2.3. Automatic Extension Registration

annotation을 사용한 `선언적 확장 등록(declarative extension registration)` 및 `프로그래밍 방식 확장 등록( programmatic extension registration)` 지원 외에도 JUnit Jupiter는 Java의 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 전역 확장 등록을 지원하므로 클래스 경로에서 사용 가능한 항목을 기반으로 타사 확장을 자동 감지하고 자동으로 등록할 수 있습니다.

특히, 사용자 정의 확장은 포함하는 JAR 파일의 `/META-INF/services` 폴더 내 `org.junit.jupiter.api.extension.Extension`이라는 파일에 정규화된 클래스 이름을 제공하여 등록할 수 있습니다.

#### Enabling Automatic Extension Detection
자동 감지는 고급 기능이므로 기본적으로 활성화되어 있지 않습니다. 활성화하려면 `junit.jupiter.extensions.autodetection.enabled` 구성 매개변수를 `true`로 설정하십시오. 이는 JVM 시스템 속성, `Launcher`에 전달되는 `LauncherDiscoveryRequest`의 구성 매개변수 또는 JUnit 플랫폼 구성 파일을 통해 제공될 수 있습니다(자세한 내용은 `구성 매개변수` 참조).

예를 들어 확장 자동 감지를 활성화하려면 다음 시스템 속성으로 JVM을 시작할 수 있습니다.

`-Djunit.jupiter.extensions.autodetection.enabled=true`

자동 감지가 활성화되면 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 발견된 확장은 JUnit Jupiter의 전역 확장(예: `TestInfo`, `TestReporter` 등에 대한 지원) 이후 확장 레지스트리에 추가됩니다.

### 5.2.4. Extension Inheritance

등록된 확장은 하향식 의미 체계를 사용하여 테스트 클래스 계층 내에서 상속됩니다. 마찬가지로 클래스 수준에서 등록된 확장은 메소드 수준에서 상속됩니다. 또한 특정 확장 구현은 주어진 확장 컨텍스트 및 해당 상위 컨텍스트에 대해 한 번만 등록할 수 있습니다. 결과적으로 중복 확장 구현을 등록하려는 모든 시도는 무시됩니다.

## 5.3. Conditional Test Execution

[ExecutionCondition](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ExecutionCondition.html)은 프로그래밍 방식의 *조건부 테스트 실행*을 위한 `확장` API를 정의합니다.

`ExecutionCondition`은 포함된 모든 테스트가 제공된 `ExtensionContext`를 기반으로 실행되어야 하는지 결정하기 위해 각 컨테이너(예: 테스트 클래스)에 대해 평가됩니다. 마찬가지로 `ExecutionCondition`은 제공된 `ExtensionContext`를 기반으로 주어진 테스트 메소드를 실행해야 하는지 여부를 결정하기 위해 각 테스트에 대해 평가됩니다.

여러 `ExecutionCondition` 확장이 등록되면 조건 중 하나가 *disabled*됨을 반환하는 즉시 컨테이너 또는 테스트가 비활성화됩니다. 따라서 다른 확장으로 인해 이미 컨테이너 또는 테스트가 비활성화되었을 수 있으므로 조건이 평가된다는 보장이 없습니다. 즉, 평가는 단락 부울 OR 연산자처럼 작동합니다.

구체적인 예는 [DisabledCondition](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-engine/src/main/java/org/junit/jupiter/engine/extension/DisabledCondition.java) 및 [@Disabled](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Disabled.html)의 소스 코드를 참조하십시오.

### 5.3.1. Deactivating Conditions

때로는 특정 조건이 활성화되지 않은 상태에서 test suite를 실행하는 것이 유용할 수 있습니다. 예를 들어, `@Disabled` annotation이 달린 경우에도 테스트를 실행하여 여전히 손상되었는지 확인할 수 있습니다. 이를 수행하려면 `junit.jupiter.conditions.deactivate` 구성 매개변수에 패턴을 제공하여 현재 테스트 실행에 대해 비활성화(즉, 평가되지 않음)해야 하는 조건을 지정합니다. 패턴은 JVM 시스템 속성, `Launcher`에 전달되는 `LauncherDiscoveryRequest`의 구성 매개변수 또는 JUnit 플랫폼 구성 파일을 통해 제공될 수 있습니다(자세한 내용은 `구성 매개변수` 참조).

예를 들어, JUnit의 `@Disabled` 조건을 비활성화하려면 다음 시스템 속성으로 JVM을 시작할 수 있습니다.

`-Djunit.jupiter.conditions.deactivate=org.junit.*DisabledCondition`

#### Pattern Matching Syntax
Refer to `Pattern Matching Syntax` for details.

## 5.4. Test Instance Factories

`TestInstanceFactory`는 테스트 클래스 인스턴스를 생성하려는 `Extension`용 API를 정의합니다.

일반적인 사용 사례에는 종속성 주입 프레임워크에서 테스트 인스턴스를 획득하거나 테스트 클래스 인스턴스를 생성하기 위해 정적 팩토리 메소드를 호출하는 것이 포함됩니다.

`TestInstanceFactory`가 등록되지 않은 경우 프레임워크는 테스트 클래스에 대한 유일한 생성자를 호출하여 인스턴스화하고 등록된 `ParameterResolver` 확장을 통해 잠재적으로 생성자 인수를 해결할 수 있습니다.

`TestInstanceFactory`를 구현하는 확장은 테스트 인터페이스, 최상위 테스트 클래스 또는 `@Nested` 테스트 클래스에 등록할 수 있습니다.

>단일 클래스에 대해 `TestInstanceFactory`를 구현하는 여러 확장을 등록하면 해당 클래스, subclasses 및 nested 클래스의 모든 테스트에 대해 예외가 throw됩니다. 수퍼클래스 또는 *인클로징* 클래스(예: @Nested 테스트 클래스의 경우)에 등록된 모든 `TestInstanceFactory`는 *상속됩니다*. 특정 테스트 클래스에 대해 하나의 `TestInstanceFactory`만 등록되도록 하는 것은 사용자의 책임입니다.

## 5.5. Test Instance Post-processing

`TestInstancePostProcessor`는 테스트 인스턴스를 사후 처리하려는 `Extension`용 API를 정의합니다.

일반적인 사용 사례에는 테스트 인스턴스에 종속성 주입, 테스트 인스턴스에서 사용자 지정 초기화 메소드 호출 등이 포함됩니다.

구체적인 예는 [MockitoExtension](https://github.com/mockito/mockito/blob/release/2.x/subprojects/junit-jupiter/src/main/java/org/mockito/junit/jupiter/MockitoExtension.java) 및 [SpringExtension](https://github.com/spring-projects/spring-framework/blob/305055d6b1a42c7795891b7b389936ed80270505/spring-test/src/main/java/org/springframework/test/context/junit/jupiter/SpringExtension.java)에 대한 소스 코드를 참조하십시오.

## 5.6. Test Instance Pre-destroy Callback

`TestInstancePreDestroyCallback`은 테스트에서 사용된 *후* 소멸되기 *전*에 테스트 인스턴스를 처리하려는 확장용 API를 정의합니다.

일반적인 사용 사례에는 테스트 인스턴스에 주입된 종속성 정리, 테스트 인스턴스에서 사용자 지정 초기화 해제 메소드 호출 등이 있습니다.

## 5.7. Parameter Resolution

ParameterResolver는 런타임에 매개변수를 동적으로 해석하기 위한 확장 API를 정의합니다.

*테스트 클래스* 생성자, 테스트 *메소드* 또는 *수명 주기* 메소드(`테스트 클래스 및 메소드(Test Classes and Methods)` 참조)가 매개변수를 선언하는 경우 매개변수는 `ParameterResolver`에 의해 런타임에 확인되어야 합니다. `ParameterResolver`는 built-in되거나(`TestInfoParameterResolver` 참조) `사용자가 등록(registered by the user)`할 수 있습니다. 일반적으로 매개변수는 이름, 유형, annotation 또는 이들의 조합으로 확인할 수 있습니다.

매개변수 유형에만 기반하여 매개변수를 확인하는 사용자 지정 `ParameterResolver`를 구현하려는 경우 이러한 사용 사례에 대한 일반 어댑터 역할을 하는 `TypeBasedParameterResolver`를 확장하는 것이 편리할 수 있습니다.

구체적인 예는 `CustomTypeParameterResolver`, `CustomAnnotationParameterResolver` 및 `MapOfListsTypeBasedParameterResolver`에 대한 소스 코드를 참조하십시오.

>JDK 9 이전 버전의 JDK에서 javac에 의해 생성된 바이트 코드의 버그로 인해 핵심 `java.lang.reflect.Parameter` API를 통해 직접 매개변수에 대한 annotation을 찾는 것은 내부 클래스 생성자(예: `@nested` 테스트 클래스).

>따라서 `ParameterResolver` 구현에 제공되는 [ParameterContext](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ParameterContext.html) API에는 매개변수에 대한 annotation을 올바르게 조회하기 위한 다음과 같은 편리한 메소드가 포함됩니다. 확장 작성자는 JDK에서 이 버그를 방지하기 위해 `java.lang.reflect.Parameter`에 제공된 메소드 대신 이러한 메소드를 사용하는 것이 좋습니다.

>- `boolean isAnnotated(Class<? extends Annotation> annotationType)`
>- `Optional<A> findAnnotation(Class<A> annotationType)`
>- `List<A> findRepeatableAnnotations(Class<A> annotationType)`

## 5.8. Test Result Processing

[TestWatcher](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestWatcher.html)는 테스트 메소드 실행 결과를 처리하려는 확장에 대한 API를 정의합니다. 특히 `TestWatcher`는 다음 이벤트에 대한 컨텍스트 정보와 함께 호출됩니다.

- `testDisabled`: 비활성화된 테스트 메소드를 건너뛴 후 호출됩니다.
- `testSuccessful`: 테스트 메소드가 성공적으로 완료된 후 호출됨
- `testAborted`: 테스트 메소드가 중단된 후 호출됨
- `testFailed`: 테스트 메소드가 실패한 후 호출됨

>`테스트 클래스 및 메서드`에 제시된 "테스트 메서드"의 정의와 달리 이 컨텍스트에서 테스트 메서드는 `@Test` 메서드 또는 @`TestTemplate` 메서드(예: @`RepeatedTest` 또는 @`ParameterizedTest`)를 나타냅니다.

이 인터페이스를 구현하는 확장은 메소드 수준 또는 클래스 수준에서 등록할 수 있습니다. 후자의 경우 `@Nested` 클래스를 포함하여 포함된 모든 *테스트 메소드*에 대해 호출됩니다.

>제공된 [ExtensionContext](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ExtensionContext.html)의 저장소에 저장된 `ExtensionContext.Store.CloseableResource`의 모든 인스턴스는 이 API의 메소드가 호출되기 *전*에 닫힙니다(`확장에서 상태 유지(Keeping State in Extensions)` 참조). 부모 컨텍스트의 `Store`를 사용하여 이러한 리소스로 작업할 수 있습니다.

## 5.9. Test Lifecycle Callbacks

다음 인터페이스는 테스트 실행 수명 주기의 다양한 지점에서 테스트를 확장하기 위한 API를 정의합니다. 예제는 다음 섹션을 참조하고 자세한 내용은 [org.junit.jupiter.api.extension](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/package-summary.html) 패키지의 이러한 각 인터페이스에 대한 Javadoc을 참조하십시오.

- [BeforeAllCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/BeforeAllCallback.html)
    - [BeforeEachCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/BeforeEachCallback.html)
        - [BeforeTestExecutionCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/BeforeTestExecutionCallback.html)
        - [AfterTestExecutionCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/AfterTestExecutionCallback.html)
    - [AfterEachCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/AfterEachCallback.html)
- [AfterAllCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/AfterAllCallback.html)

>*Implementing Multiple Extension APIs*
>확장 개발자는 단일 확장 내에서 이러한 인터페이스를 원하는 수만큼 구현하도록 선택할 수 있습니다. 구체적인 예는 [SpringExtension](https://github.com/spring-projects/spring-framework/blob/305055d6b1a42c7795891b7b389936ed80270505/spring-test/src/main/java/org/springframework/test/context/junit/jupiter/SpringExtension.java)의 소스 코드를 참조하십시오.

### 5.9.1. Before and After Test Execution Callbacks

[BeforeTestExecutionCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/BeforeTestExecutionCallback.html)과 [AfterTestExecutionCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/AfterTestExecutionCallback.html) 은 각각 테스트 메소드가 실행되기 직전과 직후에 실행될 동작을 추가하고자 하는 Extension용 API를 정의합니다. 따라서 이러한 콜백은 타이밍, 추적 및 유사한 사용 사례에 매우 적합합니다. `@BeforeEach` 및 `@AfterEach` 메소드 주변에서 호출되는 콜백을 구현해야 하는 경우 대신 `BeforeEachCallback` 및 `AfterEachCallback`을 구현합니다.

다음 예제에서는 이러한 콜백을 사용하여 테스트 메소드의 실행 시간을 계산하고 기록하는 방법을 보여줍니다. `TimingExtension`은 테스트 실행의 시간을 측정하고 기록하기 위해 `BeforeTestExecutionCallback`과 `AfterTestExecutionCallback`을 모두 구현합니다.

*An extension that times and logs the execution of test methods(테스트 메소드의 실행 시간을 측정하고 기록하는 확장)*
```java
import java.lang.reflect.Method;
import java.util.logging.Logger;

import org.junit.jupiter.api.extension.AfterTestExecutionCallback;
import org.junit.jupiter.api.extension.BeforeTestExecutionCallback;
import org.junit.jupiter.api.extension.ExtensionContext;
import org.junit.jupiter.api.extension.ExtensionContext.Namespace;
import org.junit.jupiter.api.extension.ExtensionContext.Store;

public class TimingExtension implements BeforeTestExecutionCallback, AfterTestExecutionCallback {

    private static final Logger logger = Logger.getLogger(TimingExtension.class.getName());

    private static final String START_TIME = "start time";

    @Override
    public void beforeTestExecution(ExtensionContext context) throws Exception {
        getStore(context).put(START_TIME, System.currentTimeMillis());
    }

    @Override
    public void afterTestExecution(ExtensionContext context) throws Exception {
        Method testMethod = context.getRequiredTestMethod();
        long startTime = getStore(context).remove(START_TIME, long.class);
        long duration = System.currentTimeMillis() - startTime;

        logger.info(() ->
            String.format("Method [%s] took %s ms.", testMethod.getName(), duration));
    }

    private Store getStore(ExtensionContext context) {
        return context.getStore(Namespace.create(getClass(), context.getRequiredTestMethod()));
    }

}
```

`TimingExtensionTests` 클래스는 `@ExtendWith`를 통해 `TimingExtension`을 등록하기 때문에 테스트가 실행될 때 이 타이밍이 적용됩니다.

*A test class that uses the example TimingExtension(TimingExtension 예제를 사용하는 테스트 클래스)*
```java
@ExtendWith(TimingExtension.class)
class TimingExtensionTests {

    @Test
    void sleep20ms() throws Exception {
        Thread.sleep(20);
    }

    @Test
    void sleep50ms() throws Exception {
        Thread.sleep(50);
    }

}
```

다음은 `TimingExtensionTests` 실행 시 생성되는 로깅의 예이다.

```
INFO: Method [sleep20ms] took 24 ms.
INFO: Method [sleep50ms] took 53 ms.
```

## 5.10. Exception Handling

테스트 실행 중에 발생한 예외는 추가로 전파되기 전에 가로채서 처리될 수 있으므로 오류 로깅 또는 리소스 해제와 같은 특정 작업이 특수 확장에 정의될 수 있습니다. JUnit Jupiter는 [TestExecutionExceptionHandler](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestExecutionExceptionHandler.html)를 통해 `@Test` 메소드 동안 throw된 예외와 [LifecycleMethodExecutionExceptionHandler](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/LifecycleMethodExecutionExceptionHandler.html)를 통해 테스트 수명 주기 메소드(`@BeforeAll`, `@BeforeEach`, `@AfterEach` 및 `@AfterAll`) 중 하나에서 throw된 예외를 처리하려는 `Extension`용 API를 제공합니다.

다음 예제는 `IOException`의 모든 인스턴스를 삼키지만 다른 유형의 예외를 다시 발생시키는 확장을 보여줍니다.

*An exception handling extension that filters IOExceptions in test execution(테스트 실행에서 IOException을 필터링하는 예외 처리 확장)*
```java
public class IgnoreIOExceptionExtension implements TestExecutionExceptionHandler {

    @Override
    public void handleTestExecutionException(ExtensionContext context, Throwable throwable)
            throws Throwable {

        if (throwable instanceof IOException) {
            return;
        }
        throw throwable;
    }
}
```

또 다른 예는 설정 및 정리 중에 예기치 않은 예외가 발생하는 지점에서 테스트 중인 응용 프로그램의 상태를 정확히 기록하는 방법을 보여줍니다. 테스트 상태에 따라 실행되거나 실행되지 않을 수 있는 수명 주기 콜백에 의존하는 것과 달리 이 솔루션은 `@BeforeAll`, `@BeforeEach`, `@AfterEach` 또는 `@AfterAll` 실패 직후 실행을 보장합니다.

*An exception handling extension that records application state on error(오류 시 애플리케이션 상태를 기록하는 예외 처리 확장)*
```java
class RecordStateOnErrorExtension implements LifecycleMethodExecutionExceptionHandler {

    @Override
    public void handleBeforeAllMethodExecutionException(ExtensionContext context, Throwable ex)
            throws Throwable {
        memoryDumpForFurtherInvestigation("Failure recorded during class setup");
        throw ex;
    }

    @Override
    public void handleBeforeEachMethodExecutionException(ExtensionContext context, Throwable ex)
            throws Throwable {
        memoryDumpForFurtherInvestigation("Failure recorded during test setup");
        throw ex;
    }

    @Override
    public void handleAfterEachMethodExecutionException(ExtensionContext context, Throwable ex)
            throws Throwable {
        memoryDumpForFurtherInvestigation("Failure recorded during test cleanup");
        throw ex;
    }

    @Override
    public void handleAfterAllMethodExecutionException(ExtensionContext context, Throwable ex)
            throws Throwable {
        memoryDumpForFurtherInvestigation("Failure recorded during class cleanup");
        throw ex;
    }
}
```

동일한 수명 주기 메소드에 대해 선언 순서대로 여러 실행 예외 처리기를 호출할 수 있습니다. 처리기 중 하나가 처리된 예외를 삼키면 후속 처리가 실행되지 않고 예외가 발생하지 않은 것처럼 JUnit 엔진에 오류가 전파되지 않습니다. 핸들러는 또한 예외를 다시 throw하거나 다른 예외를 throw하여 잠재적으로 원본을 래핑하도록 선택할 수 있습니다.

`@BeforeAll` 또는 `@AfterAll` 중에 발생한 예외를 처리하려는 [LifecycleMethodExecutionExceptionHandler](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/LifecycleMethodExecutionExceptionHandler.html)를 구현하는 확장은 클래스 수준에서 등록해야 하는 반면, `BeforeEach` 및 `AfterEach`에 대한 처리기는 개별 테스트 메소드에 대해 등록할 수도 있습니다.

*Registering multiple exception handling extensions(여러 예외 처리 확장 등록)*
```java
// Register handlers for @Test, @BeforeEach, @AfterEach as well as @BeforeAll and @AfterAll
@ExtendWith(ThirdExecutedHandler.class)
class MultipleHandlersTestCase {

    // Register handlers for @Test, @BeforeEach, @AfterEach only
    @ExtendWith(SecondExecutedHandler.class)
    @ExtendWith(FirstExecutedHandler.class)
    @Test
    void testMethod() {
    }

}
```

## 5.11. Intercepting Invocations

[InvocationInterceptor](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/InvocationInterceptor.html) 테스트 코드에 대한 호출을 가로채려는 `확장`을 위한 API를 정의합니다.

다음 예제는 Swing의 Event Dispatch Thread에서 모든 테스트 메소드를 실행하는 확장을 보여줍니다.

*An extension that executes tests in a user-defined thread(사용자 정의 스레드에서 테스트를 실행하는 확장)*
```java
public class SwingEdtInterceptor implements InvocationInterceptor {

    @Override
    public void interceptTestMethod(Invocation<Void> invocation,
            ReflectiveInvocationContext<Method> invocationContext,
            ExtensionContext extensionContext) throws Throwable {

        AtomicReference<Throwable> throwable = new AtomicReference<>();

        SwingUtilities.invokeAndWait(() -> {
            try {
                invocation.proceed();
            }
            catch (Throwable t) {
                throwable.set(t);
            }
        });
        Throwable t = throwable.get();
        if (t != null) {
            throw t;
        }
    }
}
```

## 5.12. Providing Invocation Contexts for Test Templates

[@TestTemplate](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestTemplate.html) method는 하나 이상의 [TestTemplateInvocationContextProvider](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestTemplateInvocationContextProvider.html)가 등록된 경우에만 실행할 수 있습니다. 이러한 각 공급자는 [TestTemplateInvocationContext](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestTemplateInvocationContext.html) 인스턴스의 스트림을 제공할 책임이 있습니다. 각 컨텍스트는 사용자 지정 display name과 [@TestTemplate](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestTemplate.html) 메소드의 다음 호출에만 사용할 추가 확장 목록을 지정할 수 있습니다.

다음 예제에서는 테스트 템플릿을 작성하는 방법과 [TestTemplateInvocationContextProvider](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestTemplateInvocationContextProvider.html)를 등록하고 구현하는 방법을 보여줍니다.

*A test template with accompanying extension(확장이 수반되는 테스트 템플릿)*
```java
final List<String> fruits = Arrays.asList("apple", "banana", "lemon");

@TestTemplate
@ExtendWith(MyTestTemplateInvocationContextProvider.class)
void testTemplate(String fruit) {
    assertTrue(fruits.contains(fruit));
}

public class MyTestTemplateInvocationContextProvider
        implements TestTemplateInvocationContextProvider {

    @Override
    public boolean supportsTestTemplate(ExtensionContext context) {
        return true;
    }

    @Override
    public Stream<TestTemplateInvocationContext> provideTestTemplateInvocationContexts(
            ExtensionContext context) {

        return Stream.of(invocationContext("apple"), invocationContext("banana"));
    }

    private TestTemplateInvocationContext invocationContext(String parameter) {
        return new TestTemplateInvocationContext() {
            @Override
            public String getDisplayName(int invocationIndex) {
                return parameter;
            }

            @Override
            public List<Extension> getAdditionalExtensions() {
                return Collections.singletonList(new ParameterResolver() {
                    @Override
                    public boolean supportsParameter(ParameterContext parameterContext,
                            ExtensionContext extensionContext) {
                        return parameterContext.getParameter().getType().equals(String.class);
                    }

                    @Override
                    public Object resolveParameter(ParameterContext parameterContext,
                            ExtensionContext extensionContext) {
                        return parameter;
                    }
                });
            }
        };
    }
}
```

이 예에서 테스트 템플릿은 두 번 호출됩니다. 호출의 display name은 호출 컨텍스트에 지정된 대로 `사과` 및 `바나나`입니다. 각 호출은 메소드 매개 변수를 확인하는 데 사용되는 사용자 지정 [ParameterResolver](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ParameterResolver.html)를 등록합니다. ConsoleLauncher 사용 시 출력은 다음과 같습니다.

```
└─ testTemplate(String) ✔
   ├─ apple ✔
   └─ banana ✔
```

[TestTemplateInvocationContextProvider](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestTemplateInvocationContextProvider.html) 확장 API는 기본적으로 다른 컨텍스트(예: 다른 매개변수를 사용하여 테스트 클래스 인스턴스를 다르게 준비하거나 수정하지 않고 여러 번)에서라도 테스트와 유사한 메소드의 반복적인 호출에 의존하는 다양한 종류의 테스트를 구현하기 위한 것입니다. 문맥. 기능을 제공하기 위해 이 확장점을 사용하는 `repeated test` 또는 `parameterized test`의 구현을 참조하십시오.

## 5.13. Keeping State in Extensions

일반적으로 확장은 한 번만 인스턴스화됩니다. 따라서 질문은 관련이 있습니다. 한 번의 확장 호출에서 다음 호출로 상태를 유지하는 방법은 무엇입니까? `ExtensionContext` API는 정확히 이 목적을 위해 `Store`를 제공합니다. 확장은 나중에 검색하기 위해 값을 저장소에 저장할 수 있습니다. 메소드 수준 범위와 함께 저장소를 사용하는 예는 `TimingExtension`을 참조하세요. 테스트 실행 중에 `ExtensionContext`에 저장된 값은 주변 `ExtensionContext`에서 사용할 수 없다는 것을 기억하는 것이 중요합니다. `ExtensionContext`는 nested될 수 있으므로 내부 컨텍스트의 범위도 제한될 수 있습니다. `Store`를 통해 값을 저장하고 검색하는 데 사용할 수 있는 방법에 대한 자세한 내용은 해당 Javadoc을 참조하세요.

>`ExtensionContext.Store.CloseableResource`
>확장 컨텍스트 저장소는 확장 컨텍스트 수명 주기에 바인딩됩니다. 확장 컨텍스트 수명 주기가 끝나면 연결된 저장소가 닫힙니다. `CloseableResource`의 인스턴스인 모든 저장된 값은 추가된 역순으로 해당 `close()` 메소드를 호출하여 알림을 받습니다.

## 5.14. Supported Utilities in Extensions

`junit-platform-commons` artifact는 annotation, 클래스, 리플렉션 및 클래스 경로 스캔 작업 작업을 위한 *유지 관리* 유틸리티 메소드가 포함된 `org.junit.platform.commons.support`라는 패키지를 노출합니다. `TestEngine` 및 `Extension` 작성자는 JUnit 플랫폼의 동작에 맞추기 위해 이러한 지원 방법을 사용하는 것이 좋습니다.

### 5.14.1. Annotation Support

`AnnotationSupport`는 annotation이 달린 요소(예: 패키지, annotation, 클래스, 인터페이스, 생성자, 메소드 및 필드)에서 작동하는 정적 유틸리티 메소드를 제공합니다. 여기에는 요소가 특정 annotation으로 annotation 처리되었는지 또는 meta-annotation 처리되었는지 확인하고, 특정 annotation을 검색하고, 클래스 또는 인터페이스에서 annotation이 달린 메소드 및 필드를 찾는 메소드가 포함됩니다. 이러한 메소드 중 일부는 구현된 인터페이스와 클래스 계층 내에서 검색하여 annotation을 찾습니다. 자세한 내용은 [AnnotationSupport](https://junit.org/junit5/docs/current/api/org.junit.platform.commons/org/junit/platform/commons/support/AnnotationSupport.html)에 대한 Javadoc을 참조하십시오.

### 5.14.2. Class Support

`ClassSupport`는 클래스(즉, `java.lang.Class`의 인스턴스) 작업을 위한 정적 유틸리티 메소드를 제공합니다. 자세한 내용은 [ClassSupport](https://junit.org/junit5/docs/current/api/org.junit.platform.commons/org/junit/platform/commons/support/ClassSupport.html)에 대한 Javadoc을 참조하십시오.

### 5.14.3. Reflection Support

`ReflectionSupport`는 표준 JDK 리플렉션 및 클래스 로딩 메커니즘을 보강하는 정적 유틸리티 메소드를 제공합니다. 여기에는 지정된 술어와 일치하는 클래스를 검색하여 클래스 경로를 스캔하고, 클래스의 새 인스턴스를 로드 및 생성하고, 메소드를 찾고 호출하는 메소드가 포함됩니다. 이러한 메소드 중 일부는 클래스 계층을 탐색하여 일치하는 메소드를 찾습니다. 자세한 내용은 [ReflectionSupport](https://junit.org/junit5/docs/current/api/org.junit.platform.commons/org/junit/platform/commons/support/ReflectionSupport.html)에 대한 Javadoc을 참조하십시오.

### 5.14.4. Modifier Support

`ModifierSupport`는 멤버 및 클래스 수정자  작업을 위한 정적 유틸리티 메소드를 제공합니다. 예를 들어 멤버가 `public`, `private`, `abstract`, `static` 등으로 선언되었는지 확인합니다. 자세한 내용은 [ModifierSupport](https://junit.org/junit5/docs/current/api/org.junit.platform.commons/org/junit/platform/commons/support/ModifierSupport.html)에 대한 Javadoc을 참조하세요.

## 5.15. Relative Execution Order of User Code and Extensions

하나 이상의 테스트 메소드가 포함된 테스트 클래스를 실행할 때 사용자 제공 테스트 및 수명 주기 메소드 외에도 여러 확장 콜백이 호출됩니다.

>See also: `Test Execution Order`

### 5.15.1. User and Extension Code

다음 다이어그램은 사용자 제공 코드와 확장 코드의 상대적 순서를 보여줍니다. 사용자 제공 테스트 및 수명 주기 method는 주황색으로 표시되고 콜백 코드는 파란색으로 표시된 확장으로 구현됩니다. 회색 상자는 단일 테스트 메소드의 실행을 나타내며 테스트 클래스의 모든 테스트 메소드에 대해 반복됩니다.

extensions lifecycle
![image](../images/junit5_guide_extensions_lifecycle.png)
*User code and extension code*

다음 표에서는 `사용자 코드 및 확장 코드 다이어그램`의 16단계에 대해 자세히 설명합니다.
|단계|인터페이스/annotation|설명|
|---|---|---|
|1|인터페이스 org.junit.jupiter.api.extension.BeforeAllCallback|컨테이너의 모든 테스트가 실행되기 전에 실행되는 확장 코드|
|2|annotation org.junit.jupiter.api.BeforeAll|컨테이너의 모든 테스트가 실행되기 전에 실행되는 사용자 코드|
|3|인터페이스 org.junit.jupiter.api.extension.LifecycleMethodExecutionExceptionHandler #handleBeforeAllMethodExecutionException|@BeforeAll 메소드에서 throw된 예외를 처리하기 위한 확장 코드|
|4|인터페이스 org.junit.jupiter.api.extension.BeforeEachCallback|각 테스트가 실행되기 전에 실행되는 확장 코드|
|5|annotation org.junit.jupiter.api.BeforeEach|각 테스트가 실행되기 전에 실행된 사용자 코드|
|6|인터페이스 org.junit.jupiter.api.extension.LifecycleMethodExecutionExceptionHandler #handleBeforeEachMethodExecutionException|@BeforeEach 메소드에서 throw된 예외를 처리하기 위한 확장 코드|
|7|인터페이스 org.junit.jupiter.api.extension.BeforeTestExecutionCallback|테스트 실행 직전에 실행되는 확장 코드|
|8|annotation org.junit.jupiter.api.Test|실제 테스트 방법의 사용자 코드|
|9|인터페이스 org.junit.jupiter.api.extension.TestExecutionExceptionHandler|테스트 중에 발생한 예외를 처리하기 위한 확장 코드|
|10|인터페이스 org.junit.jupiter.api.extension.AfterTestExecutionCallback|테스트 실행 직후 실행되는 확장 코드 및 해당 예외 핸들러|
|11|annotation org.junit.jupiter.api.AfterEach|각 테스트 실행 후 실행되는 사용자 코드|
|12|인터페이스 org.junit.jupiter.api.extension.LifecycleMethodExecutionExceptionHandler #handleAfterEachMethodExecutionException|@AfterEach 메소드에서 throw된 예외를 처리하기 위한 확장 코드|
|13|인터페이스 org.junit.jupiter.api.extension.AfterEachCallback|각 테스트 실행 후 실행되는 확장 코드|
|14|annotation org.junit.jupiter.api.AfterAll|컨테이너의 모든 테스트가 실행된 후 실행되는 사용자 코드|
|15|인터페이스 org.junit.jupiter.api.extension.LifecycleMethodExecutionExceptionHandler #handleAfterAllMethodExecutionException|@AfterAll 메소드에서 throw된 예외를 처리하기 위한 확장 코드|
|16|인터페이스 org.junit.jupiter.api.extension.AfterAllCallback|컨테이너의 모든 테스트가 실행된 후 실행되는 확장 코드|

가장 단순한 경우에는 실제 테스트 방법만 실행됩니다(8단계). 다른 모든 단계는 해당 수명 주기 콜백에 대한 사용자 코드 또는 확장 지원의 존재 여부에 따라 선택 사항입니다. 다양한 수명 주기 콜백에 대한 자세한 내용은 각 annotation 및 확장에 대한 해당 Javadoc을 참조하십시오.

위 표의 모든 사용자 코드 메소드 호출은 `InvocationInterceptor`를 구현하여 추가로 가로챌 수 있습니다.

### 5.15.2. Wrapping Behavior of Callbacks

JUnit Jupiter는 `BeforeAllCallback`, `AfterAllCallback`, `BeforeEachCallback`, `AfterEachCallback`, `BeforeTestExecutionCallback` 및 `AfterTestExecutionCallback`과 같은 수명 주기 콜백을 구현하는 여러 등록된 확장에 대한 래핑 동작을 항상 보장합니다.

즉, `Extension1`이 `Extension2`보다 먼저 등록된 `Extension1`과 `Extension2`가 두 개 있는 경우 `Extension1`에 의해 구현된 모든 "이전" 콜백은 `Extension2`에 의해 구현된 "이전" 콜백보다 **먼저** 실행되도록 보장됩니다. 유사하게, 동일한 순서로 등록된 두 개의 동일한 확장이 주어지면 `Extension1`에 의해 구현된 "이후" 콜백은 `Extension2`에 의해 구현된 모든 "이후" 콜백 **이후**에 실행되도록 보장됩니다. 따라서 `Extension1`은 `Extension2`를 래핑한다고 합니다.

Unit Jupiter는 또한 사용자 제공 *수명 주기* 메소드에 대한 클래스 및 인터페이스 계층 내 *래핑* 동작을 보장합니다(`테스트 클래스 및 메소드` 참조).

- `@BeforeAll` method는 *숨겨지거나 재정의되지 않는 한* 슈퍼클래스에서 *상속*됩니다. 또한 슈퍼클래스의 `@BeforeAll` method는 서브클래스의 `@BeforeAll` 메소드보다 **먼저** 실행됩니다.
    - 마찬가지로 인터페이스에 선언된 `@BeforeAll` method는 *숨겨지거나 재정의되지 않는 한* *상속*되며, 인터페이스의 `@BeforeAll` method는 인터페이스를 구현하는 클래스의 `@BeforeAl`l 메소드보다 **먼저** 실행됩니다.
- `@AfterAll` method는 *숨겨지거나 재정의되지 않는 한* 슈퍼클래스에서 *상속*됩니다. 또한 슈퍼클래스의 `@AfterAll` method는 서브클래스의 `@AfterAll` 메소드 **다음**에 실행됩니다.
    - 마찬가지로 인터페이스에 선언된 `@AfterAll` 메소드는 *숨겨지거나 재정의되지 않는 한* *상속*되며, 인터페이스의 `@AfterAll` 메소드는 인터페이스를 구현하는 클래스의 `@AfterAl`l 메소드 **다음**에 실행됩니다.
- `@BeforeEach` method는 *재정의되지 않는 한* 슈퍼클래스에서 *상속*됩니다. 또한 슈퍼클래스의 `@BeforeEach` method는 서브클래스의 `@BeforeEach` 메소드보다 **먼저** 실행됩니다.
    - 마찬가지로 인터페이스 기본 메소드로 선언된 `@BeforeEach` method는 *재정의되지 않는 한* *상속*되며, `@BeforeEach` 기본 method는 인터페이스를 구현하는 클래스의 `@BeforeEach` 메소드보다 **먼저** 실행됩니다.
- `@AfterEach` method는 *재정의되지 않는 한* 슈퍼클래스에서 *상속*됩니다. 또한 슈퍼클래스의 `@AfterEach` method는 서브클래스의 `@AfterEach` 메소드 **다음**에 실행됩니다.
    - 마찬가지로, 인터페이스 기본 메소드로 선언된 `@AfterEach` 메소드는 *재정의되지 않는 한* *상속*되며, `@AfterEach` 기본 메소드는 인터페이스를 구현한 클래스의 `@AfterEach` 메소드 **이후**에 실행됩니다.

다음 예제에서는 이 동작을 보여줍니다. 예제는 실제로 현실적이지 않다는 점에 유의하십시오. 대신 데이터베이스와의 상호 작용을 테스트하기 위한 일반적인 시나리오를 모방합니다. `Logger` 클래스에서 정적으로 가져온 모든 method는 확장에서 사용자 제공 콜백 메소드 및 콜백 메소드의 실행 순서를 더 잘 이해할 수 있도록 컨텍스트 정보를 기록합니다.

*Extension1*
```java
import static example.callbacks.Logger.afterEachCallback;
import static example.callbacks.Logger.beforeEachCallback;

import org.junit.jupiter.api.extension.AfterEachCallback;
import org.junit.jupiter.api.extension.BeforeEachCallback;
import org.junit.jupiter.api.extension.ExtensionContext;

public class Extension1 implements BeforeEachCallback, AfterEachCallback {

    @Override
    public void beforeEach(ExtensionContext context) {
        beforeEachCallback(this);
    }

    @Override
    public void afterEach(ExtensionContext context) {
        afterEachCallback(this);
    }

}
```

*Extension2*
```java
import static example.callbacks.Logger.afterEachCallback;
import static example.callbacks.Logger.beforeEachCallback;

import org.junit.jupiter.api.extension.AfterEachCallback;
import org.junit.jupiter.api.extension.BeforeEachCallback;
import org.junit.jupiter.api.extension.ExtensionContext;

public class Extension2 implements BeforeEachCallback, AfterEachCallback {

    @Override
    public void beforeEach(ExtensionContext context) {
        beforeEachCallback(this);
    }

    @Override
    public void afterEach(ExtensionContext context) {
        afterEachCallback(this);
    }

}
```

*AbstractDatabaseTests*
```java
import static example.callbacks.Logger.afterAllMethod;
import static example.callbacks.Logger.afterEachMethod;
import static example.callbacks.Logger.beforeAllMethod;
import static example.callbacks.Logger.beforeEachMethod;

import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;

/**
 * Abstract base class for tests that use the database.
 */
abstract class AbstractDatabaseTests {

    @BeforeAll
    static void createDatabase() {
        beforeAllMethod(AbstractDatabaseTests.class.getSimpleName() + ".createDatabase()");
    }

    @BeforeEach
    void connectToDatabase() {
        beforeEachMethod(AbstractDatabaseTests.class.getSimpleName() + ".connectToDatabase()");
    }

    @AfterEach
    void disconnectFromDatabase() {
        afterEachMethod(AbstractDatabaseTests.class.getSimpleName() + ".disconnectFromDatabase()");
    }

    @AfterAll
    static void destroyDatabase() {
        afterAllMethod(AbstractDatabaseTests.class.getSimpleName() + ".destroyDatabase()");
    }

}
```

*DatabaseTestsDemo*
```java
import static example.callbacks.Logger.afterEachMethod;
import static example.callbacks.Logger.beforeAllMethod;
import static example.callbacks.Logger.beforeEachMethod;
import static example.callbacks.Logger.testMethod;

import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;

/**
 * Extension of {@link AbstractDatabaseTests} that inserts test data
 * into the database (after the database connection has been opened)
 * and deletes test data (before the database connection is closed).
 */
@ExtendWith({ Extension1.class, Extension2.class })
class DatabaseTestsDemo extends AbstractDatabaseTests {

    @BeforeAll
    static void beforeAll() {
        beforeAllMethod(DatabaseTestsDemo.class.getSimpleName() + ".beforeAll()");
    }

    @BeforeEach
    void insertTestDataIntoDatabase() {
        beforeEachMethod(getClass().getSimpleName() + ".insertTestDataIntoDatabase()");
    }

    @Test
    void testDatabaseFunctionality() {
        testMethod(getClass().getSimpleName() + ".testDatabaseFunctionality()");
    }

    @AfterEach
    void deleteTestDataFromDatabase() {
        afterEachMethod(getClass().getSimpleName() + ".deleteTestDataFromDatabase()");
    }

    @AfterAll
    static void afterAll() {
        beforeAllMethod(DatabaseTestsDemo.class.getSimpleName() + ".afterAll()");
    }

}
```

`DatabaseTestsDemo` 테스트 클래스를 실행하면 다음과 같이 기록된다.

```
@BeforeAll AbstractDatabaseTests.createDatabase()
@BeforeAll DatabaseTestsDemo.beforeAll()
  Extension1.beforeEach()
  Extension2.beforeEach()
    @BeforeEach AbstractDatabaseTests.connectToDatabase()
    @BeforeEach DatabaseTestsDemo.insertTestDataIntoDatabase()
      @Test DatabaseTestsDemo.testDatabaseFunctionality()
    @AfterEach DatabaseTestsDemo.deleteTestDataFromDatabase()
    @AfterEach AbstractDatabaseTests.disconnectFromDatabase()
  Extension2.afterEach()
  Extension1.afterEach()
@BeforeAll DatabaseTestsDemo.afterAll()
@AfterAll AbstractDatabaseTests.destroyDatabase()
```

다음 시퀀스 다이어그램은 `DatabaseTestsDemo` 테스트 클래스가 실행될 때 `JupiterTestEngine` 내에서 실제로 무슨 일이 일어나는지 더 자세히 설명하는 데 도움이 됩니다.

![image](../images/juni5_guide_extensions_DatabaseTestsDemo.png)
*DatabaseTestsDemo*

JUnit Jupiter는 단일 테스트 클래스 또는 테스트 인터페이스 내에서 선언된 여러 수명 주기 메소드의 실행 순서를 **보장하지 않습니다**. JUnit Jupiter가 알파벳 순서로 이러한 메소드를 호출하는 것처럼 보일 수 있습니다. 그러나 그것은 정확히 사실이 아닙니다. 순서는 단일 테스트 클래스 내에서 `@Test` 메소드의 순서와 유사합니다.

단일 테스트 클래스 또는 테스트 인터페이스 내에서 선언된 수명 주기 method는 결정적이지만 의도적으로 명확하지 않은 알고리즘을 사용하여 정렬됩니다. 이렇게 하면 test suite의 후속 실행이 동일한 순서로 수명 주기 메소드를 실행하므로 반복 가능한 빌드가 가능합니다.

또한 JUnit Jupiter는 단일 테스트 클래스 또는 테스트 인터페이스 내에서 선언된 여러 수명 주기 메소드에 대한 래핑 동작을 **지원하지 않습니다**.

다음 예제에서는 이 동작을 보여줍니다. 특히, 로컬로 선언된 수명 주기 메소드가 실행되는 순서로 인해 수명 주기 메소드 구성이 손상됩니다.

데이터베이스 연결이 열리기 *전에* 테스트 데이터가 삽입되어 데이터베이스 연결에 실패합니다.

테스트 데이터를 삭제하기 *전에* 데이터베이스 연결이 닫히므로 데이터베이스 연결에 실패합니다.

*BrokenLifecycleMethodConfigDemo*
```java
import static example.callbacks.Logger.afterEachMethod;
import static example.callbacks.Logger.beforeEachMethod;
import static example.callbacks.Logger.testMethod;

import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;

/**
 * Example of "broken" lifecycle method configuration.
 *
 * <p>Test data is inserted before the database connection has been opened.
 *
 * <p>Database connection is closed before deleting test data.
 */
@ExtendWith({ Extension1.class, Extension2.class })
class BrokenLifecycleMethodConfigDemo {

    @BeforeEach
    void connectToDatabase() {
        beforeEachMethod(getClass().getSimpleName() + ".connectToDatabase()");
    }

    @BeforeEach
    void insertTestDataIntoDatabase() {
        beforeEachMethod(getClass().getSimpleName() + ".insertTestDataIntoDatabase()");
    }

    @Test
    void testDatabaseFunctionality() {
        testMethod(getClass().getSimpleName() + ".testDatabaseFunctionality()");
    }

    @AfterEach
    void deleteTestDataFromDatabase() {
        afterEachMethod(getClass().getSimpleName() + ".deleteTestDataFromDatabase()");
    }

    @AfterEach
    void disconnectFromDatabase() {
        afterEachMethod(getClass().getSimpleName() + ".disconnectFromDatabase()");
    }

}
```

`BrokenLifecycleMethodConfigDemo` 테스트 클래스를 실행하면 다음과 같이 기록된다.

```
Extension1.beforeEach()
Extension2.beforeEach()
  @BeforeEach BrokenLifecycleMethodConfigDemo.insertTestDataIntoDatabase()
  @BeforeEach BrokenLifecycleMethodConfigDemo.connectToDatabase()
    @Test BrokenLifecycleMethodConfigDemo.testDatabaseFunctionality()
  @AfterEach BrokenLifecycleMethodConfigDemo.disconnectFromDatabase()
  @AfterEach BrokenLifecycleMethodConfigDemo.deleteTestDataFromDatabase()
Extension2.afterEach()
Extension1.afterEach()
```

다음 시퀀스 다이어그램은 `BrokenLifecycleMethodConfigDemo` 테스트 클래스가 실행될 때 `JupiterTestEngine` 내에서 실제로 무슨 일이 일어나는지 더 자세히 설명하는 데 도움이 됩니다.

![image](../images/junit5_guide_extensions_BrokenLifecycleMethodConfigDemo.png)
*BrokenLifecycleMethodConfigDemo*

>앞서 언급한 동작으로 인해 JUnit 팀은 개발자가 이러한 수명 주기 메소드 간에 종속성이 없는 경우를 제외하고 테스트 클래스 또는 테스트 인터페이스별로 각 유형의 *수명 주기 메소드*(`테스트 클래스 및 메소드` 참조)를 최대 하나씩 선언할 것을 권장합니다.

# 6. Advanced Topics

## 6.1. JUnit Platform Launcher API

JUnit 5의 주요 목표 중 하나는 JUnit과 프로그래밍 방식 클라이언트(빌드 도구 및 IDE) 간의 인터페이스를 보다 강력하고 안정적으로 만드는 것입니다. 목적은 외부에서 필요한 모든 필터링 및 구성에서 테스트를 검색하고 실행하는 내부를 분리하는 것입니다.

JUnit 5는 테스트를 검색, 필터링 및 실행하는 데 사용할 수 있는 `Launcher`의 개념을 도입했습니다. 또한 Spock, Cucumber 및 FitNesse와 같은 타사 테스트 라이브러리는 사용자 지정 [TestEngine](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/TestEngine.html)을 제공하여 JUnit 플랫폼의 출시 인프라에 연결할 수 있습니다.

launcher API는 [junit-platform-launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/package-summary.html) 모듈에 있습니다

launcher API의 예시 소비자는 [junit-platform-console](https://junit.org/junit5/docs/current/api/org.junit.platform.console/org/junit/platform/console/package-summary.html) 프로젝트의 [ConsoleLauncher](https://junit.org/junit5/docs/current/api/org.junit.platform.console/org/junit/platform/console/ConsoleLauncher.html)입니다.

### 6.1.1. Discovering Tests

플랫폼 자체의 전용 기능으로 *test discovery*을 사용하면 이전 버전의 JUnit에서 테스트 클래스와 테스트 방법을 식별하기 위해 겪었던 대부분의 어려움에서 IDE 및 빌드 도구를 자유롭게 할 수 있습니다.

Usage Example:
```java
import static org.junit.platform.engine.discovery.ClassNameFilter.includeClassNamePatterns;
import static org.junit.platform.engine.discovery.DiscoverySelectors.selectClass;
import static org.junit.platform.engine.discovery.DiscoverySelectors.selectPackage;

import java.io.PrintWriter;
import java.nio.file.Path;
import java.nio.file.Paths;

import org.junit.platform.engine.FilterResult;
import org.junit.platform.engine.TestDescriptor;
import org.junit.platform.launcher.Launcher;
import org.junit.platform.launcher.LauncherDiscoveryListener;
import org.junit.platform.launcher.LauncherDiscoveryRequest;
import org.junit.platform.launcher.LauncherSession;
import org.junit.platform.launcher.LauncherSessionListener;
import org.junit.platform.launcher.PostDiscoveryFilter;
import org.junit.platform.launcher.TestExecutionListener;
import org.junit.platform.launcher.TestPlan;
import org.junit.platform.launcher.core.LauncherConfig;
import org.junit.platform.launcher.core.LauncherDiscoveryRequestBuilder;
import org.junit.platform.launcher.core.LauncherFactory;
import org.junit.platform.launcher.listeners.SummaryGeneratingListener;
import org.junit.platform.launcher.listeners.TestExecutionSummary;
import org.junit.platform.reporting.legacy.xml.LegacyXmlReportGeneratingListener;
```

```java
LauncherDiscoveryRequest request = LauncherDiscoveryRequestBuilder.request()
    .selectors(
        selectPackage("com.example.mytests"),
        selectClass(MyTestClass.class)
    )
    .filters(
        includeClassNamePatterns(".*Tests")
    )
    .build();

try (LauncherSession session = LauncherFactory.openSession()) {
    TestPlan testPlan = session.getLauncher().discover(request);

    // ... discover additional test plans or execute tests
}
```

패키지의 클래스, 메소드 및 모든 클래스를 선택하거나 클래스 경로 또는 모듈 경로에서 모든 테스트를 검색할 수도 있습니다. 검색은 참여하는 모든 테스트 엔진에서 발생합니다.

결과 `TestPlan`은 `LauncherDiscoveryRequest`에 맞는 모든 엔진, 클래스 및 테스트 방법에 대한 계층적(읽기 전용) 설명입니다. 클라이언트는 트리를 탐색하고 노드에 대한 세부 정보를 검색하고 원본 소스(클래스, 메소드 또는 파일 위치와 같은)에 대한 링크를 얻을 수 있습니다. 테스트 계획의 모든 노드에는 특정 테스트 또는 테스트 그룹을 호출하는 데 사용할 수 있는 *unique ID*가 있습니다.

클라이언트는 [LauncherDiscoveryRequestBuilder](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherDiscoveryRequestBuilder.html)를 통해 하나 이상의 [LauncherDiscoveryListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryListener.html) 구현을 등록하여 테스트 검색 중에 발생하는 이벤트에 대한 통찰력을 얻을 수 있습니다. 기본적으로 빌더는 첫 번째 검색 실패가 발생한 후 테스트 검색을 중단하는 "실패 시 중단" 수신기를 등록합니다. 기본 `LauncherDiscoveryListener`는 `junit.platform.discovery.listener.default` `구성 매개변수`를 통해 변경할 수 있습니다.

### 6.1.2. Executing Tests

테스트를 실행하기 위해 클라이언트는 검색 단계에서와 동일한 `LauncherDiscoveryRequest`를 사용하거나 새 요청을 생성할 수 있습니다. 다음 예제와 같이 런처에 하나 이상의 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html) 구현을 등록하여 테스트 진행 및 보고를 수행할 수 있습니다.

```java
LauncherDiscoveryRequest request = LauncherDiscoveryRequestBuilder.request()
    .selectors(
        selectPackage("com.example.mytests"),
        selectClass(MyTestClass.class)
    )
    .filters(
        includeClassNamePatterns(".*Tests")
    )
    .build();

SummaryGeneratingListener listener = new SummaryGeneratingListener();

try (LauncherSession session = LauncherFactory.openSession()) {
    Launcher launcher = session.getLauncher();
    // Register a listener of your choice
    launcher.registerTestExecutionListeners(listener);
    // Discover tests and build a test plan
    TestPlan testPlan = launcher.discover(request);
    // Execute test plan
    launcher.execute(testPlan);
    // Alternatively, execute the request directly
    launcher.execute(request);
}

TestExecutionSummary summary = listener.getSummary();
// Do something with the summary...
```

`execute()` 메소드에 대한 반환 값은 없지만 `TestExecutionListener`를 사용하여 결과를 집계할 수 있습니다. 예제는 [SummaryGeneratingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/SummaryGeneratingListener.html), [LegacyXmlReportGeneratingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.reporting/org/junit/platform/reporting/legacy/xml/LegacyXmlReportGeneratingListener.html) 및 [UniqueIdTrackingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/UniqueIdTrackingListener.html)를 참조하십시오.

### 6.1.3. Registering a TestEngine

JUnit provides three [TestEngine](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/TestEngine.html) implementations.

- [junit-jupiter-engine](https://junit.org/junit5/docs/current/api/org.junit.jupiter.engine/org/junit/jupiter/engine/package-summary.html): The core of JUnit Jupiter.
- [junit-vintage-engine](https://junit.org/junit5/docs/current/api/org.junit.vintage.engine/org/junit/vintage/engine/package-summary.html): A thin layer on top of JUnit 4 to allow running vintage tests with the launcher infrastructure.
- [junit-platform-suite-engine](https://junit.org/junit5/docs/current/api/org.junit.platform.suite.engine/org/junit/platform/suite/engine/package-summary.html): To execute declarative suites of tests with the launcher infrastructure.

Third-party는 [junit-platform-engine](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/package-summary.html) 모듈에서 인터페이스를 구현하고 엔진을 등록하여 자체 `TestEngine`을 제공할 수도 있습니다. 엔진 등록은 Java의 `ServiceLoader` 메커니즘을 통해 지원됩니다. 예를 들어, `junit-jupiter-engine` 모듈은 `junit-jupiter-engine`의 `/META-INF/services` 폴더에 있는 `org.junit.platform.engine.TestEngine`이라는 파일에 `org.junit.jupiter.engine.JupiterTestEngine` JAR를 등록합니다.

>[HierarchicalTestEngine](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/support/hierarchical/HierarchicalTestEngine.html)은 ([junit-jupiter-engine](https://junit.org/junit5/docs/current/api/org.junit.jupiter.engine/org/junit/jupiter/engine/package-summary.html)에서 사용하는) 편리한 추상 기본 구현으로, 구현자가 테스트 검색을 위한 논리를 제공하기만 하면 됩니다. 병렬 실행 지원을 포함하여 `node` 인터페이스를 구현하는 `TestDescriptors`의 실행을 구현합니다.

>*`junit-` 접두사는 JUnit 팀의 TestEngines용으로 예약되어 있습니다.*
JUnit 플랫폼 `launcher`는 JUnit 팀에서 게시한 `TestEngine` 구현만 `TestEngine` ID에 대해 `junit-` 접두사를 사용할 수 있도록 합니다.

>- 타사 `TestEngine`이 `junit-jupiter` 또는 `junit-vintage`라고 주장하는 경우 예외가 발생하여 JUnit 플랫폼의 실행이 즉시 중단됩니다.
>- 타사 `TestEngine`이 ID로 `junit-` 접두사를 사용하는 경우 경고 메시지가 기록됩니다. JUnit 플랫폼의 이후 릴리스에서는 이러한 위반에 대해 예외가 발생합니다.

### 6.1.4. Registering a PostDiscoveryFilter

[Launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/Launcher.html) API에 전달된 [LauncherDiscoveryRequest](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryRequest.html)의 일부로 사후 검색 필터를 지정하는 것 외에도 [PostDiscoveryFilter](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/PostDiscoveryFilter.html) 구현은 런타임에 Java의 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 검색되고 요청의 일부인 것 외에 `Launcher`에 의해 자동으로 적용됩니다.

예를 들어 `PostDiscoveryFilter`를 구현하고 `/META-INF/services/org.junit.platform.launcher.PostDiscoveryFilter` 파일 내에 선언된 `example.CustomTagFilter` 클래스가 자동으로 로드되어 적용됩니다.

### 6.1.5. Registering a LauncherSessionListener

[LauncherSessionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSessionListener.html)의 등록된 구현은 [LauncherSession](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSession.html)이 열릴 때([Launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/Launcher.html)가 테스트를 처음 발견하고 실행하기 전) 닫힐 때(더 이상 테스트가 발견되거나 실행되지 않을 때) 알림을 받습니다. [LauncherFactory](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherFactory.html)에 전달되는 [LauncherConfig](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherConfig.html)를 통해 프로그래밍 방식으로 등록하거나 Java의 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 런타임에 검색하고 [LauncherSession](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSession.html)에 자동으로 등록할 수 있습니다(자동 등록이 비활성화되지 않은 경우).

[LauncherSessionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSessionListener.html)는 각각 런처 세션의 첫 번째 테스트 이전과 마지막 테스트 이후에 호출되기 때문에 JVM당 한 번 설정/해제 동작을 구현하는 데 적합합니다. 런처 세션의 범위는 사용된 IDE 또는 빌드 도구에 따라 다르지만 일반적으로 테스트 JVM의 수명 주기에 해당합니다. 첫 번째 테스트를 실행하기 전에 HTTP 서버를 시작하고 마지막 테스트가 실행된 후에 중지하는 사용자 지정 수신기는 다음과 같습니다.

*src/test/java/example/session/GlobalSetupTeardownListener.java*
```java
package example.session;

import static java.net.InetAddress.getLoopbackAddress;

import java.io.IOException;
import java.io.UncheckedIOException;
import java.net.InetSocketAddress;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

import com.sun.net.httpserver.HttpServer;

import org.junit.platform.launcher.LauncherSession;
import org.junit.platform.launcher.LauncherSessionListener;
import org.junit.platform.launcher.TestExecutionListener;
import org.junit.platform.launcher.TestPlan;

public class GlobalSetupTeardownListener implements LauncherSessionListener {

    private Fixture fixture;

    @Override
    public void launcherSessionOpened(LauncherSession session) {
        // Avoid setup for test discovery by delaying it until tests are about to be executed
        session.getLauncher().registerTestExecutionListeners(new TestExecutionListener() {
            @Override
            public void testPlanExecutionStarted(TestPlan testPlan) {
                if (fixture == null) {
                    fixture = new Fixture();
                    fixture.setUp();
                }
            }
        });
    }

    @Override
    public void launcherSessionClosed(LauncherSession session) {
        if (fixture != null) {
            fixture.tearDown();
            fixture = null;
        }
    }

    static class Fixture {

        private HttpServer server;
        private ExecutorService executorService;

        void setUp() {
            try {
                server = HttpServer.create(new InetSocketAddress(getLoopbackAddress(), 0), 0);
            }
            catch (IOException e) {
                throw new UncheckedIOException("Failed to start HTTP server", e);
            }
            server.createContext("/test", exchange -> {
                exchange.sendResponseHeaders(204, -1);
                exchange.close();
            });
            executorService = Executors.newCachedThreadPool();
            server.setExecutor(executorService);
            server.start(); // Step1 
            int port = server.getAddress().getPort();
            System.setProperty("http.server.host", getLoopbackAddress().getHostAddress()); // Step2 
            System.setProperty("http.server.port", String.valueOf(port)); // Step3
        }

        void tearDown() {
            server.stop(0); // Step4
            executorService.shutdownNow();
        }
    }

}

Step1. Start the HTTP server
Step2. Export its host address as a system property for consumption by tests
Step3. Export its port as a system property for consumption by tests
Step4. Stop the HTTP server
```

이 샘플은 JDK와 함께 제공되지만 다른 서버 또는 리소스와 유사하게 작동하는 jdk.httpserver 모듈의 HTTP 서버 구현을 사용합니다. JUnit 플랫폼에서 리스너를 선택하려면 테스트 런타임 클래스 경로에 다음 이름과 내용의 리소스 파일을 추가하여 서비스로 등록해야 합니다(예:` src/test/resources`에 파일 추가). :

*src/test/resources/META-INF/services/org.junit.platform.launcher.LauncherSessionListener
*
`example.session.GlobalSetupTeardownListener`

You can now use the resource from your test:

*src/test/java/example/session/HttpTests.java*
```java
package example.session;

import static org.junit.jupiter.api.Assertions.assertEquals;

import java.net.HttpURLConnection;
import java.net.URL;

import org.junit.jupiter.api.Test;

class HttpTests {

    @Test
    void respondsWith204() throws Exception {
        String host = System.getProperty("http.server.host"); // step1
        String port = System.getProperty("http.server.port"); // step2
        URL url = new URL("http://" + host + ":" + port + "/test");

        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        connection.setRequestMethod("GET");
        int responseCode = connection.getResponseCode(); // step3

        assertEquals(204, responseCode); // step4
    }
}

step1. Read the host address of the server from the system property set by the listener
step2. Read the port of the server from the system property set by the listener
step3. Send a request to the server
step4. Check the status code of the response
```

### 6.1.6. Registering a LauncherDiscoveryListener

[LauncherDiscoveryRequest](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryRequest.html)의 일부로 검색 수신기를 지정하거나 [Launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/Launcher.html) API를 통해 프로그래밍 방식으로 등록하는 것 외에도 사용자 지정 `LauncherDiscoveryListener` 구현은 Java의 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 런타임에 검색되고 [LauncherFactory](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherFactory.html)를 통해 생성된 `Launcher`에 자동으로 등록될 수 있습니다.

예를 들어 `LauncherDiscoveryListener`를 구현하고 `/META-INF/services/org.junit.platform.launcher.LauncherDiscoveryListener` 파일 내에 선언된 `example.CustomLauncherDiscoveryListener` 클래스가 자동으로 로드되어 등록됩니다.

### 6.1.7. Registering a TestExecutionListener

프로그래밍 방식으로 테스트 실행 리스너를 등록하기 위한 공개 `Launcher` API 방법 외에도 사용자 지정 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html) 구현은 런타임 시 Java의 `ServiceLoader` 메커니즘을 통해 검색되고 `LauncherFactory`를 통해 생성된 Launcher에 자동으로 등록됩니다.

예를 들어 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html)를 구현하고 /`META-INF/services/org.junit.platform.launcher.TestExecutionListener` 파일 내에 선언된 `example.CustomTestExecutionListener` 클래스가 자동으로 로드되어 등록됩니다.

### 6.1.8. Configuring a TestExecutionListener

[TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html)가 Launcher API를 통해 프로그래밍 방식으로 등록되면 리스너는 생성자, 설정자 메소드 등을 통해 구성할 프로그래밍 방식을 제공할 수 있습니다. 그러나 `TestExecutionListener`가 Java의 ServiceLoader 메커니즘(`Registering a TestExecutionListener` 참조), 사용자가 리스너를 직접 구성할 수 있는 방법이 없습니다. 이러한 경우 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html)의 작성자는 `구성 매개변수`를 통해 리스너를 구성할 수 있도록 선택할 수 있습니다. 그러면 리스너는 `testPlanExecutionStarted(TestPlan)` 및 `testPlanExecutionFinished(TestPlan)` 콜백 메소드에 제공된 TestPlan을 통해 구성 매개변수에 액세스할 수 있습니다. 예는 [UniqueIdTrackingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/UniqueIdTrackingListener.html)를 참조하십시오.

### 6.1.9. Deactivating a TestExecutionListener

때로는 특정 실행 수신기가 활성화되지 않은 상태에서 test suite를 실행하는 것이 유용할 수 있습니다. 예를 들어, 보고 목적으로 테스트 결과를 외부 시스템으로 보내는 사용자 정의 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html)가 있을 수 있으며 디버깅하는 동안 이러한 디버그 결과가 보고되지 않기를 원할 수 있습니다. 이를 수행하려면 `junit.platform.execution.listeners.deactivate` *구성 매개변수*에 패턴을 제공하여 현재 테스트 실행에 대해 비활성화(즉, 등록되지 않음)해야 하는 실행 리스너를 지정합니다.

`/META-INF/services/org.junit.platform.launcher.TestExecutionListener` 파일 내에서 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 등록된 리스너만 비활성화할 수 있습니다. 즉, [LauncherDiscoveryRequest](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryRequest.html)를 통해 명시적으로 등록된 `TestExecutionListener`는 `junit.platform.execution.listeners.deactivate` 구성 매개변수를 통해 비활성화할 수 없습니다.

또한 실행 리스너는 테스트 실행이 시작되기 전에 등록되기 때문에 `junit.platform.execution.listeners.deactivate` 구성 매개변수는 JVM 시스템 속성으로 또는 JUnit 플랫폼 구성 파일을 통해서만 제공될 수 있습니다(자세한 내용은 `구성 매개변수` 참조). 이 구성 매개변수는 [Launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/Launcher.html)에 전달되는 [LauncherDiscoveryRequest](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryRequest.html)에 제공할 수 없습니다.

#### Pattern Matching Syntax
Refer to `Pattern Matching Syntax` for details.

### 6.1.10. Configuring the Launcher

테스트 엔진 및 리스너의 자동 감지 및 등록에 대한 세분화된 제어가 필요한 경우 [LauncherConfig](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherConfig.html) 인스턴스를 생성하고 이를 [LauncherFactory](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherFactory.html)에 제공할 수 있습니다. 일반적으로 `LauncherConfig`의 인스턴스는 다음 예제와 같이 내장된 Fluent *Builder* API를 통해 생성됩니다.

```java
LauncherConfig launcherConfig = LauncherConfig.builder()
    .enableTestEngineAutoRegistration(false)
    .enableLauncherSessionListenerAutoRegistration(false)
    .enableLauncherDiscoveryListenerAutoRegistration(false)
    .enablePostDiscoveryFilterAutoRegistration(false)
    .enableTestExecutionListenerAutoRegistration(false)
    .addTestEngines(new CustomTestEngine())
    .addLauncherSessionListeners(new CustomLauncherSessionListener())
    .addLauncherDiscoveryListeners(new CustomLauncherDiscoveryListener())
    .addPostDiscoveryFilters(new CustomPostDiscoveryFilter())
    .addTestExecutionListeners(new LegacyXmlReportGeneratingListener(reportsDir, out))
    .addTestExecutionListeners(new CustomTestExecutionListener())
    .build();

LauncherDiscoveryRequest request = LauncherDiscoveryRequestBuilder.request()
    .selectors(selectPackage("com.example.mytests"))
    .build();

try (LauncherSession session = LauncherFactory.openSession(launcherConfig)) {
    session.getLauncher().execute(request);
}
```

## 6.2. JUnit Platform Reporting

`junit-platform-reporting` artifact는 테스트 보고서를 생성하는 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html) 구현을 포함합니다. 이러한 수신기는 일반적으로 IDE 및 빌드 도구에서 사용됩니다. `org.junit.platform.reporting.legacy.xml` 패키지에는 현재 다음 구현이 포함되어 있습니다.

[LegacyXmlReportGeneratingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.reporting/org/junit/platform/reporting/legacy/xml/LegacyXmlReportGeneratingListener.html)는 [TestPlan](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestPlan.html)의 각 루트에 대해 별도의 XML 보고서를 생성합니다. 생성된 XML 형식은 Ant 빌드 시스템에서 널리 보급된 JUnit 4 기반 테스트 보고서의 사실상 표준과 호환됩니다. `LegacyXmlReportGeneratingListener`는 `Console Launcher`에서도 사용됩니다.

[junit-platform-launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/package-summary.html) 모듈에는 보고 목적으로 사용할 수 있는 `TestExecutionListener `구현도 포함되어 있습니다. 자세한 내용은 `리스너 사용(Using Listener)`을 참조하십시오.

## 6.3. JUnit Platform Suite Engine

JUnit 플랫폼은 JUnit 플랫폼을 사용하는 모든 테스트 엔진에서 test suite의 선언적 정의 및 실행을 지원합니다.

### 6.3.1. Setup

`junit-platform-suite-api` 및 `junit-platform-suite-engine` artifact 외에도 적어도 하나의 다른 테스트 엔진과 클래스 경로에 대한 종속성이 필요합니다. 그룹 ID, artifact ID 및 버전에 대한 자세한 내용은 `종속성 메타데이터(Dependency Metadata)`를 참조하세요.

#### Required Dependencies
- 테스트 범위의 `junit-platform-suite-api`: test suite를 구성하는 데 필요한 annotation을 포함하는 artifact
- 테스트 런타임 범위의 `junit-platform-suite-engine`: 선언적 test suite를 위한 `TestEngine` API 구현

>두 필수 종속성은 `junit-platform-suite-api` 및 `junit-platform-suite-engine`에 대한 명시적 종속성을 선언하는 대신 테스트 범위에서 선언할 수 있는 `junit-platform-suite` artifact에 집계됩니다.

#### Transitive Dependencies
- `junit-platform-suite-commons` in test scope
- `junit-platform-launcher` in test scope
- `junit-platform-engine` in test scope
- `junit-platform-commons` in test scope
- `opentest4j` in test scope

### 6.3.2. @Suite Example

`@Suite`로 클래스에 annotation을 추가하면 JUnit 플랫폼에서 test suite로 표시됩니다. 다음 예에서 볼 수 있듯이 선택기 및 필터 annotation을 사용하여 제품군의 내용을 제어할 수 있습니다.

```java
import org.junit.platform.suite.api.IncludeClassNamePatterns;
import org.junit.platform.suite.api.SelectPackages;
import org.junit.platform.suite.api.Suite;
import org.junit.platform.suite.api.SuiteDisplayName;

@Suite
@SuiteDisplayName("JUnit Platform Suite Demo")
@SelectPackages("example")
@IncludeClassNamePatterns(".*Tests")
class SuiteDemo {
}
```

*Additional Configuration Options*
test suite에서 테스트를 검색하고 필터링하기 위한 다양한 구성 옵션이 있습니다. 지원되는 annotation의 전체 목록과 추가 세부사항은 [org.junit.platform.suite.api](https://junit.org/junit5/docs/current/api/org.junit.platform.suite.api/org/junit/platform/suite/api/package-summary.html) 패키지의 Javadoc을 참조하십시오.

## 6.4. JUnit Platform Test Kit

`junit-platform-testkit` artifact는 JUnit 플랫폼에서 테스트 계획을 실행한 다음 예상 결과를 확인하기 위한 지원을 제공합니다. JUnit 플랫폼 1.4부터 이 지원은 단일 `TestEngine`의 실행으로 제한됩니다(`엔진 테스트 키트(Engine Test Kit)` 참조).

### 6.4.1. Engine Test Kit

[org.junit.platform.testkit.engine](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/package-summary.html) 패키지는 JUnit 플랫폼에서 실행되는 주어진 [TestEngine](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/TestEngine.html)에 대한 [TestPlan](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestPlan.html)을 실행한 다음 예상 결과를 확인하기 위해 유창한 API를 통해 결과에 액세스하기 위한 지원을 제공합니다. 이 API의 주요 진입점은 `engine()` 및 `execute()`라는 정적 팩토리 메소드를 제공하는 [EngineTestKit](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/EngineTestKit.html)입니다. `LauncherDiscoveryRequest`를 빌드하기 위한 유창한 API의 이점을 얻으려면 `engine()` 변형 중 하나를 선택하는 것이 좋습니다.

`Launcher` API에서 `LauncherDiscoveryRequestBuilder`를 사용하여 `LauncherDiscoveryRequest`를 빌드하려면 `EngineTestKit`에서 `execute()` 변형 중 하나를 사용해야 합니다.
JUnit Jupiter를 사용하여 작성된 다음 테스트 클래스는 후속 예제에서 사용됩니다.

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assumptions.assumeTrue;

import example.util.Calculator;

import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.MethodOrderer.OrderAnnotation;
import org.junit.jupiter.api.Order;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestMethodOrder;

@TestMethodOrder(OrderAnnotation.class)
public class ExampleTestCase {

    private final Calculator calculator = new Calculator();

    @Test
    @Disabled("for demonstration purposes")
    @Order(1)
    void skippedTest() {
        // skipped ...
    }

    @Test
    @Order(2)
    void succeedingTest() {
        assertEquals(42, calculator.multiply(6, 7));
    }

    @Test
    @Order(3)
    void abortedTest() {
        assumeTrue("abc".contains("Z"), "abc does not contain Z");
        // aborted ...
    }

    @Test
    @Order(4)
    void failingTest() {
        // The following throws an ArithmeticException: "/ by zero"
        calculator.divide(1, 0);
    }

}
```

간결함을 위해 다음 섹션에서는 고유한 엔진 ID가 `"junit-jupiter"`인 JUnit의 자체 `JupiterTestEngine`을 테스트하는 방법을 보여줍니다. 고유한 `TestEngine` 구현을 테스트하려면 고유한 엔진 ID를 사용해야 합니다. 또는 `EngineTestKit.engine(TestEngine)` 정적 팩토리 메소드에 인스턴스를 제공하여 고유한 `TestEngine`을 테스트할 수 있습니다.

### 6.4.2. Asserting Statistics

Test Kit의 가장 일반적인 기능 중 하나는 `TestPlan` 실행 중에 발생한 이벤트에 대한 통계를 주장하는 기능입니다. 다음 테스트는 JUnit Jupiter `TestEngine`에서 컨테이너 및 테스트에 대한 통계를 assertion하는 방법을 보여줍니다. 사용 가능한 통계에 대한 자세한 내용은 [EventStatistics](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/EventStatistics.html)에 대한 Javadoc을 참조하십시오.

```java
import static org.junit.platform.engine.discovery.DiscoverySelectors.selectClass;

import example.ExampleTestCase;

import org.junit.jupiter.api.Test;
import org.junit.platform.testkit.engine.EngineTestKit;

class EngineTestKitStatisticsDemo {

    @Test
    void verifyJupiterContainerStats() {
        EngineTestKit
            .engine("junit-jupiter") 
            .selectors(selectClass(ExampleTestCase.class)) 
            .execute() 
            .containerEvents() 
            .assertStatistics(stats -> stats.started(2).succeeded(2)); 
    }

    @Test
    void verifyJupiterTestStats() {
        EngineTestKit
            .engine("junit-jupiter") 
            .selectors(selectClass(ExampleTestCase.class)) 
            .execute() 
            .testEvents() 
            .assertStatistics(stats ->
                stats.skipped(1).started(3).succeeded(1).aborted(1).failed(1)); 
    }

}
Select the JUnit Jupiter TestEngine.
Select the ExampleTestCase test class.
Execute the TestPlan.
Filter by container events.
Assert statistics for container events.
Filter by test events.
Assert statistics for test events.
```

`verifyJupiterContainerStats()` 테스트 메소드에서 `JupiterTestEngine` 및 `ExampleTestCase` 클래스는 모두 컨테이너로 간주되기 때문에 시작 및 성공한 통계의 개수는 `2`입니다.

### 6.4.3. Asserting Events

`assertion 통계(asserting statistics)`만으로는 테스트 실행의 예상 동작을 확인하는 데 충분하지 않은 경우 기록된 [이벤트](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/Event.html) 요소로 직접 작업하고 이에 대해 assertion을 수행할 수 있습니다.

예를 들어, `ExampleTestCase`의 `skippedTest()` 메소드가 생략된 이유를 확인하려면 다음과 같이 하면 됩니다.

다음 예제의 `assertThatEvents()` method는 [AssertJ](https://joel-costigliola.github.io/assertj/) assertion 라이브러리의 `org.assertj.core.api.Assertions.assertThat(events.list())`에 대한 바로 가기입니다.

이벤트에 대한 AssertJ assertion과 함께 사용할 수 있는 조건에 대한 자세한 내용은 [EventConditions](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/EventConditions.html)에 대한 Javadoc을 참조하십시오.

```java
import static org.junit.platform.engine.discovery.DiscoverySelectors.selectMethod;
import static org.junit.platform.testkit.engine.EventConditions.event;
import static org.junit.platform.testkit.engine.EventConditions.skippedWithReason;
import static org.junit.platform.testkit.engine.EventConditions.test;

import example.ExampleTestCase;

import org.junit.jupiter.api.Test;
import org.junit.platform.testkit.engine.EngineTestKit;
import org.junit.platform.testkit.engine.Events;

class EngineTestKitSkippedMethodDemo {

    @Test
    void verifyJupiterMethodWasSkipped() {
        String methodName = "skippedTest";

        Events testEvents = EngineTestKit //step 5
            .engine("junit-jupiter") //step 1
            .selectors(selectMethod(ExampleTestCase.class, methodName)) //step 2
            .execute() //step 3
            .testEvents(); //step 4

        testEvents.assertStatistics(stats -> stats.skipped(1)); //step 6

        testEvents.assertThatEvents() //step 7
            .haveExactly(1, event(test(methodName),
                skippedWithReason("for demonstration purposes")));
    }

}
step 1. Select the JUnit Jupiter TestEngine.
step 2. Select the skippedTest() method in the ExampleTestCase test class.
step 3. Execute the TestPlan.
step 4. Filter by test events.
step 5. Save the test Events to a local variable.
step 6. Optionally assert the expected statistics.
step 7. Assert that the recorded test events contain exactly one skipped test named skippedTest with "for demonstration purposes" as the reason.
```

`ExampleTestCase`의 `failingTest()` 메소드에서 throw된 예외 유형을 확인하려면 다음과 같이 하면 됩니다.

>이벤트 및 실행 결과에 대한 AssertJ assertion과 함께 사용할 수 있는 조건에 대한 자세한 내용은 각각 [EventConditions](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/EventConditions.html) 및 [TestExecutionResultConditions](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/TestExecutionResultConditions.html)에 대한 Javadoc을 참조하십시오.

```java
import static org.junit.platform.engine.discovery.DiscoverySelectors.selectClass;
import static org.junit.platform.testkit.engine.EventConditions.event;
import static org.junit.platform.testkit.engine.EventConditions.finishedWithFailure;
import static org.junit.platform.testkit.engine.EventConditions.test;
import static org.junit.platform.testkit.engine.TestExecutionResultConditions.instanceOf;
import static org.junit.platform.testkit.engine.TestExecutionResultConditions.message;

import example.ExampleTestCase;

import org.junit.jupiter.api.Test;
import org.junit.platform.testkit.engine.EngineTestKit;

class EngineTestKitFailedMethodDemo {

    @Test
    void verifyJupiterMethodFailed() {
        EngineTestKit.engine("junit-jupiter") // step 1. 
            .selectors(selectClass(ExampleTestCase.class)) // step 2. 
            .execute() // step 3. 
            .testEvents() // step 4. 
            .assertThatEvents().haveExactly(1, // step 5.  
                event(test("failingTest"),
                    finishedWithFailure(
                        instanceOf(ArithmeticException.class), message("/ by zero"))));
    }

}
step 1. Select the JUnit Jupiter TestEngine.
step 2. Select the ExampleTestCase test class.
step 3. Execute the TestPlan.
step 4. Filter by test events.
step 5. Assert that the recorded test events contain exactly one failing test named failingTest with an exception of type ArithmeticException and an error message equal to "/ by zero".
```

일반적으로 불필요하지만 `TestPlan` 실행 중에 발생한 **모든** 이벤트를 확인해야 하는 경우가 있습니다. 다음 테스트는 `EngineTestKit` API의 `assertEventsMatchExactly()` 메소드를 통해 이를 달성하는 방법을 보여줍니다.

>`assertEventsMatchExactly()`는 이벤트가 발생한 순서대로 조건을 정확히 일치시키기 때문에 `ExampleTestCase`는 `@TestMethodOrder(OrderAnnotation.class)`로 annotation 처리되었고 각 테스트 메소드는 `@Order(…​)`로 annotation 처리되었습니다. 이것은 우리가 테스트 메소드가 실행되는 순서를 강제할 수 있게 하여 우리의 `verifyAllJupiterEvents()` 테스트가 신뢰할 수 있게 해줍니다.

순서 지정 요구 사항이 *있거나 없는* *부분* 일치를 수행하려는 경우 각각 `assertEventsMatchLooselyInOrder()` 및 `assertEventsMatchLoosely()` 메소드를 사용할 수 있습니다.

```java
import static org.junit.jupiter.api.condition.JRE.JAVA_18;
import static org.junit.platform.engine.discovery.DiscoverySelectors.selectClass;
import static org.junit.platform.testkit.engine.EventConditions.abortedWithReason;
import static org.junit.platform.testkit.engine.EventConditions.container;
import static org.junit.platform.testkit.engine.EventConditions.engine;
import static org.junit.platform.testkit.engine.EventConditions.event;
import static org.junit.platform.testkit.engine.EventConditions.finishedSuccessfully;
import static org.junit.platform.testkit.engine.EventConditions.finishedWithFailure;
import static org.junit.platform.testkit.engine.EventConditions.skippedWithReason;
import static org.junit.platform.testkit.engine.EventConditions.started;
import static org.junit.platform.testkit.engine.EventConditions.test;
import static org.junit.platform.testkit.engine.TestExecutionResultConditions.instanceOf;
import static org.junit.platform.testkit.engine.TestExecutionResultConditions.message;

import java.io.StringWriter;
import java.io.Writer;

import example.ExampleTestCase;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.condition.DisabledOnJre;
import org.junit.platform.testkit.engine.EngineTestKit;
import org.opentest4j.TestAbortedException;

class EngineTestKitAllEventsDemo {

    @Test
    @DisabledOnJre(JAVA_18) // https://github.com/assertj/assertj-core/issues/2340
    void verifyAllJupiterEvents() {
        Writer writer = // create a java.io.Writer for debug output

        EngineTestKit.engine("junit-jupiter") //step 1.
            .selectors(selectClass(ExampleTestCase.class)) //step 2.
            .execute() //step 3.
            .allEvents() //step 4.
            .debug(writer) //step 5.
            .assertEventsMatchExactly( //step 6. 
                event(engine(), started()),
                event(container(ExampleTestCase.class), started()),
                event(test("skippedTest"), skippedWithReason("for demonstration purposes")),
                event(test("succeedingTest"), started()),
                event(test("succeedingTest"), finishedSuccessfully()),
                event(test("abortedTest"), started()),
                event(test("abortedTest"),
                    abortedWithReason(instanceOf(TestAbortedException.class),
                        message(m -> m.contains("abc does not contain Z")))),
                event(test("failingTest"), started()),
                event(test("failingTest"), finishedWithFailure(
                    instanceOf(ArithmeticException.class), message("/ by zero"))),
                event(container(ExampleTestCase.class), finishedSuccessfully()),
                event(engine(), finishedSuccessfully()));
    }

}
step 1. Select the JUnit Jupiter TestEngine.
step 2. Select the ExampleTestCase test class.
step 3. Execute the TestPlan.
step 4. Filter by all events.
step 5. Print all events to the supplied writer for debugging purposes. Debug information can also be written to an OutputStream such as System.out or System.err.
step 6. Assert all events in exactly the order in which they were fired by the test engine.
```

앞의 예제에서 `debug()`를 호출하면 다음과 유사한 출력이 나타납니다.

```
All Events:
    Event [type = STARTED, testDescriptor = JupiterEngineDescriptor: [engine:junit-jupiter], timestamp = 2018-12-14T12:45:14.082280Z, payload = null]
    Event [type = STARTED, testDescriptor = ClassTestDescriptor: [engine:junit-jupiter]/[class:example.ExampleTestCase], timestamp = 2018-12-14T12:45:14.089339Z, payload = null]
    Event [type = SKIPPED, testDescriptor = TestMethodTestDescriptor: [engine:junit-jupiter]/[class:example.ExampleTestCase]/[method:skippedTest()], timestamp = 2018-12-14T12:45:14.094314Z, payload = 'for demonstration purposes']
    Event [type = STARTED, testDescriptor = TestMethodTestDescriptor: [engine:junit-jupiter]/[class:example.ExampleTestCase]/[method:succeedingTest()], timestamp = 2018-12-14T12:45:14.095182Z, payload = null]
    Event [type = FINISHED, testDescriptor = TestMethodTestDescriptor: [engine:junit-jupiter]/[class:example.ExampleTestCase]/[method:succeedingTest()], timestamp = 2018-12-14T12:45:14.104922Z, payload = TestExecutionResult [status = SUCCESSFUL, throwable = null]]
    Event [type = STARTED, testDescriptor = TestMethodTestDescriptor: [engine:junit-jupiter]/[class:example.ExampleTestCase]/[method:abortedTest()], timestamp = 2018-12-14T12:45:14.106121Z, payload = null]
    Event [type = FINISHED, testDescriptor = TestMethodTestDescriptor: [engine:junit-jupiter]/[class:example.ExampleTestCase]/[method:abortedTest()], timestamp = 2018-12-14T12:45:14.109956Z, payload = TestExecutionResult [status = ABORTED, throwable = org.opentest4j.TestAbortedException: Assumption failed: abc does not contain Z]]
    Event [type = STARTED, testDescriptor = TestMethodTestDescriptor: [engine:junit-jupiter]/[class:example.ExampleTestCase]/[method:failingTest()], timestamp = 2018-12-14T12:45:14.110680Z, payload = null]
    Event [type = FINISHED, testDescriptor = TestMethodTestDescriptor: [engine:junit-jupiter]/[class:example.ExampleTestCase]/[method:failingTest()], timestamp = 2018-12-14T12:45:14.111217Z, payload = TestExecutionResult [status = FAILED, throwable = java.lang.ArithmeticException: / by zero]]
    Event [type = FINISHED, testDescriptor = ClassTestDescriptor: [engine:junit-jupiter]/[class:example.ExampleTestCase], timestamp = 2018-12-14T12:45:14.113731Z, payload = TestExecutionResult [status = SUCCESSFUL, throwable = null]]
    Event [type = FINISHED, testDescriptor = JupiterEngineDescriptor: [engine:junit-jupiter], timestamp = 2018-12-14T12:45:14.113806Z, payload = TestExecutionResult [status = SUCCESSFUL, throwable = null]]
```

# 7. API Evolution

JUnit 5의 주요 목표 중 하나는 JUnit이 많은 프로젝트에서 사용되고 있음에도 불구하고 JUnit을 발전시킬 수 있도록 유지 관리자의 능력을 향상시키는 것입니다. JUnit 4에서는 원래 내부 구성으로 추가되었던 많은 것들이 외부 확장 작성자와 도구 빌더에서만 사용되었습니다. 이로 인해 JUnit 4를 변경하는 것이 특히 어렵고 때로는 불가능했습니다.

이것이 JUnit 5가 공개적으로 사용 가능한 모든 인터페이스, 클래스 및 메소드에 대해 정의된 수명 주기를 도입한 이유입니다.

## 7.1. API Version and Status

게시된 모든 artifact에는 버전 번호 `<major>`.`<minor>`.`<patch>`가 있으며 공개적으로 사용 가능한 모든 인터페이스, 클래스 및 method는 [@API Guardian](https://github.com/apiguardian-team/apiguardian) 프로젝트의 [@API](https://apiguardian-team.github.io/apiguardian/docs/current/api/org/apiguardian/api/package-summary.html)로 annotation 처리됩니다. annotation의 `상태` 속성은 다음 값 중 하나를 할당할 수 있습니다.

|Status|Description|
|---|---|
|INTERNAL|JUnit 자체 이외의 다른 코드에서 사용하면 안 됩니다. 사전 통보 없이 삭제될 수 있습니다.|
|DEPRECATED|더 이상 사용되지 않아야 합니다. 다음 마이너 릴리스에서 사라질 수 있습니다.|
|EXPERIMENTAL|피드백을 찾고 있는 새롭고 실험적인 기능을 위한 것입니다. 이 요소는 주의해서 사용하십시오. 향후 `MAINTAINED` 또는 `STABLE`로 승격될 수 있지만 패치에서도 사전 통지 없이 제거될 수도 있습니다.|
|MAINTAINED|최소한 현재 주 버전의 다음 부 릴리스에 대해 이전 버전과 호환되지 않는 방식으로 변경되지 않는 기능을 위한 것입니다. 제거 예정인 경우 먼저 `DEPRECATED`로 강등됩니다.|
|STABLE|현재 주요 버전(5.*)에서 이전 버전과 호환되지 않는 방식으로 변경되지 않는 기능을 위한 것입니다.|

`@API` annotation이 유형에 있는 경우 해당 유형의 모든 공개 멤버에도 적용 가능한 것으로 간주됩니다. 구성원은 낮은 안정성의 다른 `상태` 값을 선언할 수 있습니다.

## 7.2. Experimental APIs

다음 표는 현재 `@API(status = EXPERIMENTAL)`를 통해 실험용으로 지정된 API를 나열합니다. 이러한 API에 의존할 때는 주의를 기울여야 합니다.

|Package Name|Type Name|Since|
|---|---|---|
|org.junit.jupiter.api|`ClassDescriptor` *(interface)*|5.8|
|org.junit.jupiter.api|`ClassOrderer` *(interface)*|5.8|
|org.junit.jupiter.api|`ClassOrdererContext` *(interface)*|5.8|
|org.junit.jupiter.api|`DisplayNameGenerator.IndicativeSentences` *(class)*|5.7|
|org.junit.jupiter.api|`IndicativeSentencesGeneration` *(annotation)*|5.7|
|org.junit.jupiter.api|`MethodOrderer.DisplayName` *(class)*|5.7|
|org.junit.jupiter.api|`MethodOrderer.MethodName` *(class)*|5.7|
|org.junit.jupiter.api|`Order` *(annotation)*|5.4|
|org.junit.jupiter.api|`TestClassOrder` *(annotation)*|5.8|
|org.junit.jupiter.api.extension|`DynamicTestInvocationContext` *(interface)*|5.8|
|org.junit.jupiter.api.extension|`InvocationInterceptor` *(interface)*|5.5|
|org.junit.jupiter.api.extension|`InvocationInterceptor.Invocation` *(interface)*|5.5|
|org.junit.jupiter.api.extension|`LifecycleMethodExecutionExceptionHandler` *(interface)*|5.5|
|org.junit.jupiter.api.extension|`ReflectiveInvocationContext` *(interface)*|5.5|
|org.junit.jupiter.api.extension|`TestInstantiationException` *(class)*|5.3|
|org.junit.jupiter.api.extension.support|`TypeBasedParameterResolver` *(class)*|5.6|
|org.junit.jupiter.api.io|`TempDir` *(annotation)*|5.4|
|org.junit.jupiter.api.parallel|`Execution` *(annotation)*|5.3|
|org.junit.jupiter.api.parallel|`ExecutionMode` *(enum)*|5.3|
|org.junit.jupiter.api.parallel|`Isolated` *(annotation)*|5.7|
|org.junit.jupiter.api.parallel|`ResourceAccessMode` *(enum)*|5.3|
|org.junit.jupiter.api.parallel|`ResourceLock` *(annotation)*|5.3|
|org.junit.jupiter.api.parallel|`ResourceLocks` *(annotation)*|5.3|
|org.junit.jupiter.api.parallel|`Resources` *(class)*|5.3|
|org.junit.jupiter.params.converter|`TypedArgumentConverter` *(class)*|5.7|
|org.junit.platform.commons.support|`SearchOption` *(enum)*|1.8|
|org.junit.platform.console|`ConsoleLauncherToolProvider` *(class)*|1.6|
|org.junit.platform.engine|`EngineDiscoveryListener` *(interface)*|1.6|
|org.junit.platform.engine|`SelectorResolutionResult` *(class)*|1.6|
|org.junit.platform.engine.support.config|`PrefixedConfigurationParameters` *(class)*|1.3|
|org.junit.platform.engine.support.discovery|`EngineDiscoveryRequestResolver` *(class)*|1.5|
|org.junit.platform.engine.support.discovery|`EngineDiscoveryRequestResolver.Builder` *(class)*|1.5|
|org.junit.platform.engine.support.discovery|`EngineDiscoveryRequestResolver.InitializationContext` *(interface)*|1.5|
|org.junit.platform.engine.support.discovery|`SelectorResolver` *(interface)*|1.5|
|org.junit.platform.engine.support.discovery|`SelectorResolver.Context` *(interface)*|1.5|
|org.junit.platform.engine.support.discovery|`SelectorResolver.Match` *(class)*|1.5|
|org.junit.platform.engine.support.discovery|`SelectorResolver.Resolution` *(class)*|1.5|
|org.junit.platform.engine.support.hierarchical|`DefaultParallelExecutionConfigurationStrategy` *(enum)*|1.3|
|org.junit.platform.engine.support.hierarchical|`ExclusiveResource` *(class)*|1.3|
|org.junit.platform.engine.support.hierarchical|`ForkJoinPoolHierarchicalTestExecutorService` *(class)*|1.3|
|org.junit.platform.engine.support.hierarchical|`HierarchicalTestExecutorService` *(interface)*|1.3|
|org.junit.platform.engine.support.hierarchical|`Node.ExecutionMode` *(enum)*|1.3|
|org.junit.platform.engine.support.hierarchical|`Node.Invocation` *(interface)*|1.4|
|org.junit.platform.engine.support.hierarchical|`ParallelExecutionConfiguration` *(interface)*|1.3|
|org.junit.platform.engine.support.hierarchical|`ParallelExecutionConfigurationStrategy` *(interface)*|1.3|
|org.junit.platform.engine.support.hierarchical|`ResourceLock` *(interface)*|1.3|
|org.junit.platform.engine.support.hierarchical|`SameThreadHierarchicalTestExecutorService` *(class)*|1.3|
|org.junit.platform.jfr|`FlightRecordingDiscoveryListener` *(class)*|1.8|
|org.junit.platform.jfr|`FlightRecordingExecutionListener` *(class)*|1.8|
|org.junit.platform.launcher|`EngineDiscoveryResult` *(class)*|1.6|
|org.junit.platform.launcher|`LauncherDiscoveryListener` *(interface)*|1.6|
|org.junit.platform.launcher|`LauncherSession` *(interface)*|1.8|
|org.junit.platform.launcher|`LauncherSessionListener` *(interface)*|1.8|
|org.junit.platform.launcher.listeners|`UniqueIdTrackingListener` *(class)*|1.8|
|org.junit.platform.launcher.listeners.discovery|`LauncherDiscoveryListeners` *(class)*|1.6|
|org.junit.platform.suite.api|`ConfigurationParameter` *(annotation)*|1.8|
|org.junit.platform.suite.api|`ConfigurationParameters` *(annotation)*|1.8|
|org.junit.platform.suite.api|`DisableParentConfigurationParameters` *(annotation)*|1.8|
|org.junit.platform.suite.api|`SelectClasspathResource` *(annotation)*|1.8|
|org.junit.platform.suite.api|`SelectClasspathResources` *(annotation)*|1.8|
|org.junit.platform.suite.api|`SelectDirectories` *(annotation)*|1.8|
|org.junit.platform.suite.api|`SelectFile` *(annotation)*|1.8|
|org.junit.platform.suite.api|`SelectFiles` *(annotation)*|1.8|
|org.junit.platform.suite.api|`SelectModules` *(annotation)*|1.8|
|org.junit.platform.suite.api|`SelectUris` *(annotation)*|1.8|
|org.junit.platform.suite.api|`Suite` *(annotation)*||1.8|

## 7.3. Deprecated APIs

The following table lists which APIs are currently designated as deprecated via `@API(status = DEPRECATED)`. You should avoid using deprecated APIs whenever possible, since such APIs will likely be removed in an upcoming release.

|Package Name|Type Name|Since|
|---|---|---|
|org.junit.jupiter.api|`MethodOrderer.Alphanumeric` *(class)*|5.7|
|org.junit.platform.commons.util|`BlacklistedExceptions` *(class)*|1.7|
|org.junit.platform.commons.util|`PreconditionViolationException` *(class)*|1.5|
|org.junit.platform.engine.support.filter|`ClasspathScanningSupport` *(class)*|1.5|
|org.junit.platform.engine.support.hierarchical|`SingleTestExecutor` *(class)*|1.2|
|org.junit.platform.launcher.listeners|`LegacyReportingUtils` *(class)*|1.6|
|org.junit.platform.runner|JUnitPla`tform *(class)*|1.8|
|org.junit.platform.suite.api|`UseTechnicalNames` *(annotation)*|1.8|

## 7.4. @API Tooling Support

[@API Guardian](https://github.com/apiguardian-team/apiguardian) 프로젝트는 [@API](https://apiguardian-team.github.io/apiguardian/docs/current/api/org/apiguardian/api/package-summary.html) annotation이 달린 API의 게시자와 소비자를 위한 도구 지원을 제공할 계획입니다. 예를 들어, 도구 지원은 JUnit API가 `@API` annotation 선언에 따라 사용되고 있는지 확인하는 수단을 제공할 것입니다.

# 8. Contributors
Browse the [current list of contributors](https://github.com/junit-team/junit5/graphs/contributors) directly on GitHub.

# 9. Release Notes
The release notes are available [here](https://junit.org/junit5/docs/current/release-notes/index.html#release-notes).

# 10. Appendix

## 10.1. Reproducible Builds

버전 5.7부터 JUnit 5는 non-javadoc JAR을 [재생산](https://reproducible-builds.org/)하는 것을 목표로 합니다.

Java 버전과 같은 동일한 빌드 조건에서 반복되는 빌드는 동일한 출력을 바이트 단위로 제공해야 합니다.

이는 누구나 Maven Central/Sonatype에서 artifact의 빌드 조건을 재현하고 동일한 출력 artifact를 로컬에서 생성할 수 있음을 의미하며, 저장소의 artifact가 실제로 이 소스 코드에서 생성되었음을 확인합니다.

## 10.2. Dependency Metadata
Artifacts for final releases and milestones are deployed to Maven Central, and snapshot artifacts are deployed to Sonatype’s snapshots repository under /org/junit.
>최종 릴리스 및 마일스톤에 대한 artifact는 [Maven Central](https://search.maven.org/)에 배포되고 스냅샷 artifact는 [/org/junit](https://oss.sonatype.org/content/repositories/snapshots/org/junit/) 아래의 Sonatype의 [snapshots repository](https://oss.sonatype.org/content/repositories/snapshots/)에 배포됩니다.

10.2.1. JUnit Platform

- Group ID: org.junit.platform
- Version: 1.8.2
- Artifact IDs:

&nbsp;&nbsp;&nbsp;`junit-platform-commons`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼용 공통 API 및 지원 유틸리티. `@API(status = INTERNAL)` annotation이 달린 모든 API는 JUnit 프레임워크 자체 내에서만 사용하기 위한 것입니다. 외부 당사자의 내부 API 사용은 지원되지 않습니다!

&nbsp;&nbsp;&nbsp;`junit-platform-console`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;콘솔에서 JUnit 플랫폼에 대한 테스트 검색 및 실행을 지원합니다. 자세한 내용은 `콘솔 런처(Console Launcher)`를 참조하십시오.

&nbsp;&nbsp;&nbsp;`junit-platform-console-standalone`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;모든 종속성이 포함된 실행 가능한 JAR은 Maven Central의 [junit-platform-console-standalone](https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/) 디렉토리 아래에 제공됩니다. 자세한 내용은 `콘솔 런처(Console Launcher)`를 참조하십시오.

&nbsp;&nbsp;&nbsp;`junit-platform-engine`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;테스트 엔진용 공개 API. 자세한 내용은 `TestEngine 등록`을 참조하십시오.

&nbsp;&nbsp;&nbsp;`junit-platform-jfr`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼의 Java Flight Recorder 이벤트에 대해 `LauncherDiscoveryListener` 및 `TestExecutionListener를` 제공합니다. 자세한 내용은 `비행 레코더 지원(Flight Recorder Support)`을 참조하십시오.

&nbsp;&nbsp;&nbsp;`junit-platform-launcher`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;테스트 계획 구성 및 실행을 위한 공개 API — 일반적으로 IDE 및 빌드 도구에서 사용합니다. 자세한 내용은 `JUnit 플랫폼 런처 API(JUnit Platform Launcher API)`를 참조하세요.

&nbsp;&nbsp;&nbsp;`junit-platform-reporting`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;테스트 보고서  를 생성하는 `TestExecutionListener` 구현 -  일반적으로 IDE 및 빌드 도구에서 사용합니다. 자세한 내용은 `JUnit 플랫폼 보고(JUnit Platform Reporting)`를 참조하세요.

&nbsp;&nbsp;&nbsp;`junit-platform-runner`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 4 환경의 JUnit 플랫폼에서 테스트 및 test suite를 실행하기 위한 runner. 자세한 내용은 `JUnit 4를 사용하여 JUnit 플랫폼 실행(Using JUnit 4 to run the JUnit Platform)`을 참조하십시오.

&nbsp;&nbsp;&nbsp;`junit-platform-suite`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Gradle 및 Maven과 같은 빌드 도구에서 단순화된 종속성 관리를 위해 `junit-platform-suite-api` 및 `junit-platform-suite-engine`에 대한 종속성을 전이적으로 끌어오는 JUnit 플랫폼 제품군 artifact.

&nbsp;&nbsp;&nbsp;`junit-platform-suite-api`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼에서 test suite를 구성하기 위한 annotation. `JUnit Platform Suite Engine` 및 `JUnitPlatform runner`에서 지원합니다.

&nbsp;&nbsp;&nbsp;`junit-platform-suite-commons`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼에서 test suite를 실행하기 위한 공통 지원 유틸리티.

&nbsp;&nbsp;&nbsp;`junit-platform-suite-engine`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼에서 test suite를 실행하는 엔진. 런타임에만 필요합니다. 자세한 내용은` JUnit 플랫폼 제품군 엔진(JUnit Platform Suite Engine)`을 참조하세요.

&nbsp;&nbsp;&nbsp;`junit-platform-testkit`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;주어진 `TestEngine`에 대한 테스트 계획을 실행한 다음 유창한 API를 통해 결과에 액세스하여 예상 결과를 확인하기 위한 지원을 제공합니다.

### 10.2.2. JUnit Jupiter

- Group ID: `org.junit.jupiter`
- Version: `5.8.2`
- Artifact IDs:

&nbsp;&nbsp;&nbsp;`junit-jupiter`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Gradle 및 Maven과 같은 빌드 도구에서 단순화된 종속성 관리를 위해 `junit-jupiter-api`, `junit-jupiter-params` 및 `junit-jupiter-engine`에 대한 종속성을 전이적으로 끌어오는 JUnit Jupiter 수집기 artifact.

&nbsp;&nbsp;&nbsp;`junit-jupiter-api`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`테스트 작성(writing tests)` 및 `확장 작성(extensions)`하기 위한 JUnit Jupiter API.

&nbsp;&nbsp;&nbsp;`junit-jupiter-engine`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit Jupiter 테스트 엔진 구현; 런타임에만 필요합니다.

&nbsp;&nbsp;&nbsp;`junit-jupiter-params`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit Jupiter에서 `parameterized test`를 지원합니다.

&nbsp;&nbsp;&nbsp;`junit-jupiter-migrationsupport`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 4에서 JUnit Jupiter로의 migration 지원; JUnit 4의 @Ignore annotation을 지원하고 선택한 JUnit 4 규칙을 실행하는 데만 필요합니다.

### 10.2.3. JUnit Vintage

- Group ID: `org.junit.vintage`
- Version: `5.8.2`
- Artifact ID:

&nbsp;&nbsp;&nbsp;`junit-vintage-engine`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼에서 빈티지 JUnit 테스트를 실행할 수 있게 해주는 JUnit 빈티지 테스트 엔진 구현. 빈티지 테스트에는 JUnit 3 또는 JUnit 4 API를 사용하여 작성된 테스트 또는 해당 API에 구축된 테스트 프레임워크를 사용하여 작성된 테스트가 포함됩니다.

### 10.2.4. Bill of Materials (BOM)

다음 Maven 좌표에서 제공되는 BOM POM은 [Maven](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Importing_Dependencies) 또는 [Gradle](https://docs.gradle.org/current/userguide/managing_transitive_dependencies.html#sec:bom_import)을 사용하여 위의 여러 artifact를 참조할 때 종속성 관리를 용이하게 하는 데 사용할 수 있습니다.

- Group ID: `org.junit`
- Artifact ID: `junit-bom`
- Version: `5.8.2`

### 10.2.5. Dependencies

위의 artifact 대부분은 다음 @API Guardian JAR에 게시된 Maven POM에 종속성이 있습니다.

- Group ID: `org.apiguardian`
- Artifact ID: `apiguardian-api`
- Version: `1.1.2`

또한 위의 대부분의 artifact에는 다음 OpenTest4J JAR에 대한 직접적 또는 전이적 종속성이 있습니다.

- Group ID: `org.opentest4j`
- Artifact ID: `opentest4j`
- Version: `1.2.0`

### 10.3. Dependency Diagram
component diagram
![image](../images/juni5_guide_component-diagram.svg)
