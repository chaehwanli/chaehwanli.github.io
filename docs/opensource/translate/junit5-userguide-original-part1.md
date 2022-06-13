---
layout : default
title : translate junit5 user guide doc
parent: Translate
---

이 페이지는 [Junit5](https://junit.org/junit5/docs/current/user-guide/) 에 정리되어 있는 document를 한국어로 번역해 보기 위해서 만든 페이지이다.
전문 번역가가 아닌 관계로 번역 용어에 대한 다름이 있을수 있음.

아래와 같은 단계로 이루어질 예정임.
1. google 번역
2. 부자연스러운 번역 수정
3. 전문 번역 용어 검색
4. Review

|Chapter|Paragraph|Progress(stage)|
|----|----|----|
|Overview||3|
|Writing Tests||3|
|Migrating from JUnit 4||XX|
|Running Tests||XX|
|Extension Model||XX|
|Advanced Topics||XX|
|API Evolution||XX|
|Contributors||XX|
|OverRelease Notesview||XX|
|Appendix||XX|

1. Overview
>이 문서의 목표는 테스트를 작성하는 프로그래머, 확장 작성자 및 엔진 작성자는 물론 빌드 도구 및 IDE 공급업체를 위한 포괄적인 참조 문서를 제공하는 것입니다.

>이 문서는 [PDF](https://junit.org/junit5/docs/current/user-guide/junit-user-guide-5.8.2.pdf) 다운로드로도 제공됩니다.

1.1. What is JUnit 5?

>이전 버전의 JUnit과 달리 JUnit 5는 세 가지 다른 subprojects의 여러 모듈로 구성됩니다.

>*JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage*

>JUnit Platform은 JVM에서 `testing framework를 시작`하기 위한 기반 역할을 합니다. 또한 플랫폼에서 실행되는 testing framework를 개발하기 위한 [TestEngine API](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/TestEngine.html)를 정의합니다. 또한 플랫폼은 명령줄에서 플랫폼을 시작하는 `console launcher`와 플랫폼에서 하나 이상의 테스트 엔진을 사용하여 사용자 지정 테스트 제품군을 실행하기 위한 `JUnit Platform Suite Engine`을 제공합니다. JUnit 플랫폼에 대한 일급 지원은 널리 사용되는 IDE(`IntelliJ IDEA`, `Eclipse`, `NetBeans` 및 `Visual Studio Code` 참조) 및 빌드 도구(`Gradle`, `Maven` 및 `Ant` 참조)에도 존재합니다.

>JUnit Jupiter는 JUnit 5에서 테스트 및 확장을 작성하기 위한 새로운 `프로그래밍 모델`과 `확장 모델`의 조합입니다. Jupiter 하위 프로젝트는 플랫폼에서 Jupiter 기반 테스트를 실행하기 위한 TestEngine을 제공합니다.

>JUnit Vintage는 플랫폼에서 JUnit 3 및 JUnit 4 기반 테스트를 실행하기 위한 TestEngine을 제공합니다. 클래스 경로 또는 모듈 경로에 JUnit 4.12 이상이 있어야 합니다.

1.2. Supported Java Versions
>JUnit 5는 런타임에 Java 8(또는 그 이상)이 필요합니다. 그러나 이전 버전의 JDK로 컴파일된 코드는 계속 테스트할 수 있습니다.

1.3. Getting Help
>[Stack Overflow](https://stackoverflow.com/questions/tagged/junit5)에서 JUnit 5 관련 질문을 하거나 [Gitter](https://gitter.im/junit-team/junit5)에서 커뮤니티와 이야기하세요.

1.4. Getting Started

1.4.1. Downloading JUnit Artifacts

To find out what artifacts are available for download and inclusion in your project, refer to Dependency Metadata. To set up dependency management for your build, refer to Build Support and the Example Projects.
>다운로드하여 프로젝트에 포함할 수 있는 artifact를 알아보려면 `Dependency Metadata`를 참조하세요. 빌드에 대한 종속성 관리를 설정하려면 `빌드 지원` 및 `예제 프로젝트`를 참조하십시오.

1.4.2. JUnit 5 Features

To find out what features are available in JUnit 5 and how to use them, read the corresponding sections of this User Guide, organized by topic.
>JUnit 5에서 사용할 수 있는 기능과 사용 방법을 알아보려면 주제별로 구성된 이 사용자 가이드의 해당 섹션을 읽으십시오.

- `Writing Tests in JUnit Jupiter`
- `Migrating from JUnit 4 to JUnit Jupiter`
- `Running Tests`
- `Extension Model for JUnit Jupiter`
- Advanced Topics
    - `JUnit Platform Launcher API`
    - `JUnit Platform Test Kit`

1.4.3. Example Projects

To see complete, working examples of projects that you can copy and experiment with, the junit5-samples repository is a good place to start. The junit5-samples repository hosts a collection of sample projects based on JUnit Jupiter, JUnit Vintage, and other testing frameworks. You’ll find appropriate build scripts (e.g., build.gradle, pom.xml, etc.) in the example projects. The links below highlight some of the combinations you can choose from.
>복사하고 실험할 수 있는 완전한 프로젝트 예제를 보려면 [junit5-samples](https://github.com/junit-team/junit5-samples) repository를 시작하는 것이 좋습니다. [junit5-samples](https://github.com/junit-team/junit5-samples) 저장소는 JUnit Jupiter, JUnit Vintage 및 기타 testing framework를 기반으로 하는 샘플 프로젝트 모음을 호스팅합니다. 예제 프로젝트에서 적절한 빌드 스크립트(예: `build.gradle`, `pom.xml` 등)를 찾을 수 있습니다. 아래 링크는 선택할 수 있는 몇 가지 조합을 강조 표시합니다.

- For Gradle and Java, check out the [junit5-jupiter-starter-gradle](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-gradle) project.
- For Gradle and Kotlin, check out the [junit5-jupiter-starter-gradle-kotlin](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-gradle-kotlin) project.
- For Gradle and Groovy, check out the [junit5-jupiter-starter-gradle-groovy](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-gradle-groovy) project.
- For Maven, check out the [junit5-jupiter-starter-maven](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-maven) project.
- For Ant, check out the [junit5-jupiter-starter-ant](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-ant) project.

2. Writing Tests

The following example provides a glimpse at the minimum requirements for writing a test in JUnit Jupiter. Subsequent sections of this chapter will provide further details on all available features.
>다음 예제는 JUnit Jupiter에서 테스트를 작성하기 위한 최소 요구 사항을 보여줍니다. 이 장의 다음 섹션에서는 사용 가능한 모든 기능에 대해 자세히 설명합니다.

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

JUnit Jupiter supports the following annotations for configuring tests and extending the framework.
Unless otherwise stated, all core annotations are located in the org.junit.jupiter.api package in the junit-jupiter-api module.
>JUnit Jupiter는 테스트 구성 및 framework 확장을 위해 다음 annotation들을 지원합니다.
달리 명시되지 않는 한 모든 core annotation들은 `junit-jupiter-api` 모듈의 [org.junit.jupiter.api](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/package-summary.html) 패키지에 있습니다.

|Annotation|Description|
|---|---|
|@Test|Denotes that a method is a test method. Unlike JUnit 4’s @Test annotation, this annotation does not declare any attributes, since test extensions in JUnit Jupiter operate based on their own dedicated annotations. Such methods are inherited unless they are overridden.|
|@ParameterizedTest|Denotes that a method is a parameterized test. Such methods are inherited unless they are overridden.|
|@RepeatedTest|Denotes that a method is a test template for a repeated test. Such methods are inherited unless they are overridden.|
|@TestFactory|Denotes that a method is a test factory for dynamic tests. Such methods are inherited unless they are overridden.|
|@TestTemplate|Denotes that a method is a template for test cases designed to be invoked multiple times depending on the number of invocation contexts returned by the registered providers. Such methods are inherited unless they are overridden.|
|@TestClassOrder|Used to configure the test class execution order for @Nested test classes in the annotated test class. Such annotations are inherited.|
|@TestMethodOrder|Used to configure the test method execution order for the annotated test class; similar to JUnit 4’s @FixMethodOrder. Such annotations are inherited.|
|@TestInstance|Used to configure the test instance lifecycle for the annotated test class. Such annotations are inherited.|
|@DisplayName|Declares a custom display name for the test class or test method. Such annotations are not inherited.|
|@DisplayNameGeneration|Declares a custom display name generator for the test class. Such annotations are inherited.|
|@BeforeEach|Denotes that the annotated method should be executed before each @Test, @RepeatedTest, @ParameterizedTest, or @TestFactory method in the current class; analogous to JUnit 4’s @Before. Such methods are inherited unless they are overridden.|
|@AfterEach|Denotes that the annotated method should be executed after each @Test, @RepeatedTest, @ParameterizedTest, or @TestFactory method in the current class; analogous to JUnit 4’s @After. Such methods are inherited unless they are overridden.|
|@BeforeAll|Denotes that the annotated method should be executed before all @Test, @RepeatedTest, @ParameterizedTest, and @TestFactory methods in the current class; analogous to JUnit 4’s @BeforeClass. Such methods are inherited (unless they are hidden or overridden) and must be static (unless the "per-class" test instance lifecycle is used).|
|@AfterAll|Denotes that the annotated method should be executed after all @Test, @RepeatedTest, @ParameterizedTest, and @TestFactory methods in the current class; analogous to JUnit 4’s @AfterClass. Such methods are inherited (unless they are hidden or overridden) and must be static (unless the "per-class" test instance lifecycle is used).|
|@Nested|Denotes that the annotated class is a non-static nested test class. @BeforeAll and @AfterAll methods cannot be used directly in a @Nested test class unless the "per-class" test instance lifecycle is used. Such annotations are not inherited.|
|@Tag|Used to declare tags for filtering tests, either at the class or method level; analogous to test groups in TestNG or Categories in JUnit 4. Such annotations are inherited at the class level but not at the method level.|
|@Disabled|Used to disable a test class or test method; analogous to JUnit 4’s @Ignore. Such annotations are not inherited.|
|@Timeout|Used to fail a test, test factory, test template, or lifecycle method if its execution exceeds a given duration. Such annotations are inherited.|
|@ExtendWith|Used to register extensions declaratively. Such annotations are inherited.|
|@RegisterExtension|Used to register extensions programmatically via fields. Such fields are inherited unless they are shadowed.|
|@TempDir|Used to supply a temporary directory via field injection or parameter injection in a lifecycle method or test method; located in the org.junit.jupiter.api.io package.|

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

Some annotations may currently be experimental. Consult the table in Experimental APIs for details.
>일부 annotation들은 현재 실험적일 수 있습니다. 자세한 내용은 `Experimental API`의 표를 참조하세요.

2.1.1. Meta-Annotations and Composed Annotations

JUnit Jupiter annotations can be used as meta-annotations. That means that you can define your own composed annotation that will automatically inherit the semantics of its meta-annotations.
>JUnit Jupiter annotation은 meta-annotation으로 사용할 수 있습니다. 즉, meta-annotation의 의미를 자동으로 상속하는 자신만의 합성 annotation을 정의할 수 있습니다.

For example, instead of copying and pasting @Tag("fast") throughout your code base (see Tagging and Filtering), you can create a custom composed annotation named @Fast as follows. @Fast can then be used as a drop-in replacement for @Tag("fast").
>예를 들어, 코드 기반 전체에서 `@Tag("fast")`를 복사하여 붙여넣는 대신(`태그 지정 및 필터링` 참조) 다음과 같이 @Fast라는 사용자 지정 합성 annotation을 만들 수 있습니다. `@Fast`는 `@Tag("fast")`에 대한 drop-in 교체로 사용할 수 있습니다.

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

The following @Test method demonstrates usage of the @Fast annotation.
>다음 `@Test` 메소드는 `@Fast` annotation의 사용법을 보여줍니다.

```java
@Fast
@Test
void myFastTest() {
    // ...
}
```

You can even take that one step further by introducing a custom @FastTest annotation that can be used as a drop-in replacement for @Tag("fast") and @Test.
>`@Tag("fast")` 및 `@Test`에 대한 drop-in 교체로 사용할 수 있는 사용자 정의 `@FastTest` annotation을 도입하여 한 단계 더 나아갈 수도 있습니다.

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
/
```

JUnit automatically recognizes the following as a @Test method that is tagged with "fast".
>JUnit은 다음을 "fast"로 태그가 지정된 `@Test` 메소드로 자동 인식합니다.

```java
@FastTest
void myFastTest() {
    // ...
}
```

2.2. Test Classes and Methods

Test Class: any top-level class, static member class, or @Nested class that contains at least one test method.
>테스트 클래스: 모든 최상위 클래스, 정적 멤버 클래스 또는 하나 이상의 테스트 메소드를 포함하는 `@Nested` 클래스.

Test classes must not be abstract and must have a single constructor.
>테스트 클래스는 `추상적`이지 아니어야 하며 단일 생성자가 있어야 합니다.

Test Method: any instance method that is directly annotated or meta-annotated with @Test, @RepeatedTest, @ParameterizedTest, @TestFactory, or @TestTemplate.
>테스트 방법: `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory` 또는 `@TestTemplate`으로 직접 annotation 처리되거나 meta-annotation 처리된 모든 인스턴스 메소드.

Lifecycle Method: any method that is directly annotated or meta-annotated with @BeforeAll, @AfterAll, @BeforeEach, or @AfterEach.
>수명 주기 방법: `@BeforeAll`, `@AfterAll`, `@BeforeEach` 또는 `@AfterEach`로 직접 annotation 처리되거나 meta-annotation 처리된 모든 방법.

Test methods and lifecycle methods may be declared locally within the current test class, inherited from superclasses, or inherited from interfaces (see Test Interfaces and Default Methods). In addition, test methods and lifecycle methods must not be abstract and must not return a value (except @TestFactory methods which are required to return a value).
>테스트 메소드 및 수명 주기 method는 현재 테스트 클래스 내에서 로컬로 선언되거나 슈퍼클래스에서 상속되거나 인터페이스에서 상속될 수 있습니다(`Test Interfaces and Default Methods(테스트 인터페이스 및 기본 메소드)` 참조). 또한 테스트 메소드 및 수명 주기 method는 `추상적`이지 아니어야 하며 값을 반환하지 않아야 합니다(값을 반환하는 데 필요한 `@TestFactory` 메소드 제외).


>>Class and method visibility
Test classes, test methods, and lifecycle methods are not required to be public, but they must not be private.
테스트 클래스, 테스트 메소드 및 수명 주기 method는 `public`일 필요는 없지만 `private`일 수는 없습니다.

>>It is generally recommended to omit the public modifier for test classes, test methods, and lifecycle methods unless there is a technical reason for doing so – for example, when a test class is extended by a test class in another package. Another technical reason for making classes and methods public is to simplify testing on the module path when using the Java Module System.
테스트 클래스가 다른 패키지의 테스트 클래스에 의해 확장되는 경우와 같이 기술적인 이유가 없는 한 일반적으로 테스트 클래스, 테스트 메소드 및 수명 주기 메소드에 대해 `public` 한정자를 생략하는 것이 좋습니다. 클래스와 메소드를 공개하는 또 다른 기술적 이유는 Java Module System을 사용할 때 모듈 경로에 대한 테스트를 단순화하기 위함입니다.

The following test class demonstrates the use of @Test methods and all supported lifecycle methods. For further information on runtime semantics, see Test Execution Order and Wrapping Behavior of Callbacks.
>다음 테스트 클래스는 `@Test` 메소드와 지원되는 모든 수명 주기 메소드의 사용을 보여줍니다. 런타임 시맨틱에 대한 자세한 내용은 `Test Execution Order(테스트 실행 순서)` 및 `Wrapping Behavior of Callbacks(콜백의 래핑 동작)`을 참조하세요.

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

2.3. Display Names

Test classes and test methods can declare custom display names via @DisplayName — with spaces, special characters, and even emojis — that will be displayed in test reports and by test runners and IDEs.
>테스트 클래스와 테스트 method는 `@DisplayName` (공백, 특수 문자, 이모티콘 포함  포함)을 통해 사용자 지정 display name을 선언할 수 있으며, 이 이름은 테스트 보고서와 테스트 실행기 및 IDE에 표시됩니다.

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

2.3.1. Display Name Generators

JUnit Jupiter supports custom display name generators that can be configured via the @DisplayNameGeneration annotation. Values provided via @DisplayName annotations always take precedence over display names generated by a DisplayNameGenerator.
>JUnit Jupiter는 `@DisplayNameGeneration` annotation을 통해 구성할 수 있는 사용자 지정 display name 생성기를 지원합니다. `@DisplayName` annotation을 통해 제공된 값은 항상 `DisplayNameGenerator`에 의해 생성된 display name보다 우선합니다.

Generators can be created by implementing DisplayNameGenerator. Here are some default ones available in Jupiter:
>`DisplayNameGenerator`를 구현하여 생성기를 만들 수 있습니다. 다음은 Jupiter에서 사용할 수 있는 몇 가지 기본 항목입니다.

|DisplayNameGenerator|Behavior|
|---|---|
|Standard|Matches the standard display name generation behavior in place since JUnit Jupiter 5.0 was released.|
|Simple|Removes trailing parentheses for methods with no parameters.|
|ReplaceUnderscores|Replaces underscores with spaces.|
|IndicativeSentences|Generates complete sentences by concatenating the names of the test and the enclosing classes.|

|DisplayNameGenerator|Behavior|
|---|---|
|Standard|JUnit Jupiter 5.0이 릴리스된 이후의 표준 display name 생성 동작과 일치합니다.|
|Simple|매개변수가 없는 메소드의 후행 괄호를 제거합니다.|
|ReplaceUnderscores|밑줄을 공백으로 바꿉니다.|
|IndicativeSentences|테스트 이름과 포함하는 클래스를 연결하여 완전한 문장을 생성합니다.|

Note that for IndicativeSentences, you can customize the separator and the underlying generator by using @IndicativeSentencesGeneration as shown in the following example.
>`IndicativeSentences`의 경우 다음 예제와 같이 `@IndicativeSentencesGeneration`을 사용하여 구분 기호와 기본 생성기를 사용자 지정할 수 있습니다.

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

2.3.2. Setting the Default Display Name Generator
You can use the junit.jupiter.displayname.generator.default configuration parameter to specify the fully qualified class name of the DisplayNameGenerator you would like to use by default. Just like for display name generators configured via the @DisplayNameGeneration annotation, the supplied class has to implement the DisplayNameGenerator interface. The default display name generator will be used for all tests unless the @DisplayNameGeneration annotation is present on an enclosing test class or test interface. Values provided via @DisplayName annotations always take precedence over display names generated by a DisplayNameGenerator.
>`junit.jupiter.displayname.generator.default` 구성 `매개변수`를 사용하여 기본적으로 사용하려는 `DisplayNameGenerator`의 정규화된 클래스 이름을 지정할 수 있습니다. `@DisplayNameGeneration annotation`을 통해 구성된 display name 생성기와 마찬가지로 제공된 클래스는 `DisplayNameGenerator` 인터페이스를 구현해야 합니다. `@DisplayNameGeneration` annotation이 둘러싸는 테스트 클래스 또는 테스트 인터페이스에 없는 경우 기본 display name 생성기가 모든 테스트에 사용됩니다. `@DisplayName` annotation을 통해 제공된 값은 항상 `DisplayNameGenerator`에 의해 생성된 display name보다 우선합니다.

For example, to use the ReplaceUnderscores display name generator by default, you should set the configuration parameter to the corresponding fully qualified class name (e.g., in src/test/resources/junit-platform.properties):
>예를 들어, 기본적으로 `ReplaceUnderscores` display name 생성기를 사용하려면 구성 매개변수를 해당하는 정규화된 클래스 이름으로 설정해야 합니다(예: `src/test/resources/junit-platform.properties`에서).

```
junit.jupiter.displayname.generator.default = \
    org.junit.jupiter.api.DisplayNameGenerator$ReplaceUnderscores
```

Similarly, you can specify the fully qualified name of any custom class that implements DisplayNameGenerator.
>마찬가지로 `DisplayNameGenerator`를 구현하는 모든 사용자 지정 클래스의 정규화된 이름을 지정할 수 있습니다.

In summary, the display name for a test class or method is determined according to the following precedence rules:
>요약하면 테스트 클래스 또는 메소드의 display name은 다음 우선 순위 규칙에 따라 결정됩니다.

value of the @DisplayName annotation, if present
>`@DisplayName` annotation의 값(이 있는 경우)

by calling the DisplayNameGenerator specified in the @DisplayNameGeneration annotation, if present
>`@DisplayNameGeneration` annotation에 지정된 `DisplayNameGenerator`를 호출하여(있는 경우)

by calling the default DisplayNameGenerator configured via the configuration parameter, if present
>존재하는 경우 구성 매개변수를 통해 구성된 기본 `DisplayNameGenerator`를 호출하여

by calling org.junit.jupiter.api.DisplayNameGenerator.Standard
>`org.junit.jupiter.api.DisplayNameGenerator.Standard`를 호출하여

2.4. Assertions

JUnit Jupiter comes with many of the assertion methods that JUnit 4 has and adds a few that lend themselves well to being used with Java 8 lambdas. All JUnit Jupiter assertions are static methods in the org.junit.jupiter.api.Assertions class.
>JUnit Jupiter는 JUnit 4에 있는 많은 assertion method와 함께 제공되며 Java 8 lambds와 함께 사용하기에 적합한 몇 가지를 추가합니다. 모든 JUnit Jupiter assertion은 `org.junit.jupiter.api.Assertions` 클래스의 `static` 메소드입니다.

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

>>Preemptive Timeouts with assertTimeoutPreemptively()
Contrary to declarative timeouts, the various assertTimeoutPreemptively() methods in the Assertions class execute the provided executable or supplier in a different thread than that of the calling code. This behavior can lead to undesirable side effects if the code that is executed within the executable or supplier relies on java.lang.ThreadLocal storage.
*Preemptive Timeouts with* `assertTimeoutPreemptively()`(`assertTimeoutPreemptively()`를 사용한 Timeout 선점)
`선언적 시간 제한`과 달리 `Assertions` 클래스의 다양한 `assertTimeoutPreemptively()` method는 호출 코드와 다른 스레드에서 제공된 `실행 파일`이나 `공급자`를 실행합니다. `실행 파일` 또는 `공급자` 내에서 실행되는 코드가 `java.lang.ThreadLocal` 저장소에 의존하는 경우 이 동작으로 인해 바람직하지 않은 부작용이 발생할 수 있습니다.

>>One common example of this is the transactional testing support in the Spring Framework. Specifically, Spring’s testing support binds transaction state to the current thread (via a ThreadLocal) before a test method is invoked. Consequently, if an executable or supplier provided to assertTimeoutPreemptively() invokes Spring-managed components that participate in transactions, any actions taken by those components will not be rolled back with the test-managed transaction. On the contrary, such actions will be committed to the persistent store (e.g., relational database) even though the test-managed transaction is rolled back.
이에 대한 한 가지 일반적인 예는 Spring Framework의 트랜잭션 테스트 지원입니다. 특히, Spring의 테스트 지원은 테스트 method가 호출되기 전에 트랜잭션 상태를 현재 스레드(`ThreadLocal`을 통해)에 바인딩합니다. 결과적으로 `assertTimeoutPreemptively()`에 제공된 `실행 파일`이나 `공급자`가 트랜잭션에 참여하는 Spring 관리 구성 요소를 호출하는 경우 해당 구성 요소에서 수행한 모든 작업은 테스트 관리 트랜잭션과 함께 롤백되지 않습니다. 반대로 테스트 관리 트랜잭션이 롤백되더라도 이러한 작업은 영구 저장소(예: 관계형 데이터베이스)에 커밋됩니다.

>>Similar side effects may be encountered with other frameworks that rely on ThreadLocal storage.
`ThreadLocal` 저장소에 의존하는 다른 프레임워크에서도 유사한 부작용이 발생할 수 있습니다.

2.4.1. Kotlin Assertion Support

JUnit Jupiter also comes with a few assertion methods that lend themselves well to being used in Kotlin. All JUnit Jupiter Kotlin assertions are top-level functions in the org.junit.jupiter.api package.
>JUnit Jupiter는 또한 [Kotlin](https://kotlinlang.org/)에서 사용하기에 적합한 몇 가지 assertion method와 함께 제공됩니다. 모든 JUnit Jupiter Kotlin assertion은 `org.junit.jupiter.api` 패키지의 최상위 기능입니다.

```java
import example.domain.Person
import example.util.Calculator
import java.time.Duration
import org.junit.jupiter.api.Assertions.assertEquals
import org.junit.jupiter.api.Assertions.assertTrue
import org.junit.jupiter.api.Test
import org.junit.jupiter.api.assertAll
import org.junit.jupiter.api.assertDoesNotThrow
import org.junit.jupiter.api.assertThrows
import org.junit.jupiter.api.assertTimeout
import org.junit.jupiter.api.assertTimeoutPreemptively

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

2.4.2. Third-party Assertion Libraries

Even though the assertion facilities provided by JUnit Jupiter are sufficient for many testing scenarios, there are times when more power and additional functionality such as matchers are desired or required. In such cases, the JUnit team recommends the use of third-party assertion libraries such as AssertJ, Hamcrest, Truth, etc. Developers are therefore free to use the assertion library of their choice.
>JUnit Jupiter가 제공하는 assertion 기능은 많은 테스트 시나리오에 충분하지만 더 많은 성능과 matcher와 같은 추가 기능이 필요하거나 필요한 경우가 있습니다. 이러한 경우 JUnit 팀은 [AssertJ](https://joel-costigliola.github.io/assertj/), [Hamcrest](https://hamcrest.org/JavaHamcrest/), [Truth](https://truth.dev/) 등과 같은 third-party assertion 라이브러리의 사용을 권장합니다. 따라서 개발자는 선택한 assertion 라이브러리를 자유롭게 사용할 수 있습니다.

For example, the combination of matchers and a fluent API can be used to make assertions more descriptive and readable. However, JUnit Jupiter’s org.junit.jupiter.api.Assertions class does not provide an assertThat() method like the one found in JUnit 4’s org.junit.Assert class which accepts a Hamcrest Matcher. Instead, developers are encouraged to use the built-in support for matchers provided by third-party assertion libraries.
>예를 들어, matcher와 유창한 API의 조합을 사용하여 assertion을 보다 설명적이고 읽기 쉽게 만들 수 있습니다. 그러나 JUnit Jupiter의 `org.junit.jupiter.api.Assertions` 클래스는 Hamcrest [Matcher](https://junit.org/junit4/javadoc/latest/org/hamcrest/Matcher.html)를 허용하는 JUnit 4의 org.junit.Assert 클래스에서 볼 수 있는 것과 같은 `assertThat()` 메소드를 제공하지 않습니다. 대신 개발자는 third-party assertion 라이브러리에서 제공하는 matcher에 대한 기본 제공 지원을 사용하는 것이 좋습니다.

The following example demonstrates how to use the assertThat() support from Hamcrest in a JUnit Jupiter test. As long as the Hamcrest library has been added to the classpath, you can statically import methods such as assertThat(), is(), and equalTo() and then use them in tests like in the assertWithHamcrestMatcher() method below.
>다음 예제는 JUnit Jupiter 테스트에서 Hamcrest의 `assertThat()` 지원을 사용하는 방법을 보여줍니다. Hamcrest 라이브러리가 클래스 경로에 추가되어 있으면 `assertThat()`, `is()` 및 `equalTo()`와 같은 메소드를 정적으로 가져온 다음 아래의 `assertWithHamcrestMatcher()` 메소드와 같은 테스트에서 사용할 수 있습니다.

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

Naturally, legacy tests based on the JUnit 4 programming model can continue using org.junit.Assert#assertThat.
>당연히 JUnit 4 프로그래밍 모델을 기반으로 하는 레거시 테스트는 `org.junit.Assert#assertThat`을 계속 사용할 수 있습니다.

2.5. Assumptions

JUnit Jupiter comes with a subset of the assumption methods that JUnit 4 provides and adds a few that lend themselves well to being used with Java 8 lambda expressions and method references. All JUnit Jupiter assumptions are static methods in the org.junit.jupiter.api.Assumptions class.
>JUnit Jupiter는 JUnit 4가 제공하는 assumpton method의 subset과 함께 제공되며 Java 8 lambda expressions 및 메소드 참조와 함께 사용하기에 적합한 몇 가지를 추가합니다. 모든 JUnit Jupiter assumption은 `org.junit.jupiter.api.Assumptions` 클래스의 static method입니다.

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

>>As of JUnit Jupiter 5.4, it is also possible to use methods from JUnit 4’s org.junit.Assume class for assumptions. Specifically, JUnit Jupiter supports JUnit 4’s AssumptionViolatedException to signal that a test should be aborted instead of marked as a failure.
JUnit Jupiter 5.4부터는 assumption을 위해 JUnit 4의 `org.junit.Assume` 클래스에서 메소드를 사용할 수도 있습니다. 특히 JUnit Jupiter는 JUnit 4의 `AssumptionViolatedException`을 지원하여 테스트가 실패로 표시되는 대신 중단되어야 한다는 신호를 보냅니다.

2.6. Disabling Tests

Entire test classes or individual test methods may be disabled via the @Disabled annotation, via one of the annotations discussed in Conditional Test Execution, or via a custom ExecutionCondition.
>전체 테스트 클래스 또는 개별 테스트 방법은 `@Disabled` annotation, `조건부 테스트 실행`에서 논의된 annotation 중 하나 또는 사용자 정의 `ExecutionCondition`을 통해 *비활성화*될 수 있습니다.

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

>>@Disabled may be declared without providing a reason; however, the JUnit team recommends that developers provide a short explanation for why a test class or test method has been disabled. Consequently, the above examples both show the use of a reason — for example, @Disabled("Disabled until bug #42 has been resolved"). Some development teams even require the presence of issue tracking numbers in the reason for automated traceability, etc.
`@Disabled`는 이유를 제공하지 않고 선언될 수 있습니다. 그러나 JUnit 팀은 개발자가 테스트 클래스 또는 테스트 메소드가 비활성화된 이유에 대한 간단한 설명을 제공할 것을 권장합니다. 결과적으로 위의 예에서는 `@Disabled`(`"bug #42가 해결될 때까지 비활성화됨"`)과 같이 이유 — 의 사용을 보여줍니다. 일부 개발 팀은 automated traceability 등을 위해  issue tracking number의 존재를 요구하기도 합니다.

2.7. Conditional Test Execution

The ExecutionCondition extension API in JUnit Jupiter allows developers to either enable or disable a container or test based on certain conditions programmatically. The simplest example of such a condition is the built-in DisabledCondition which supports the @Disabled annotation (see Disabling Tests). In addition to @Disabled, JUnit Jupiter also supports several other annotation-based conditions in the org.junit.jupiter.api.condition package that allow developers to enable or disable containers and tests declaratively. When multiple ExecutionCondition extensions are registered, a container or test is disabled as soon as one of the conditions returns disabled. If you wish to provide details about why they might be disabled, every annotation associated with these built-in conditions has a disabledReason attribute available for that purpose.
>JUnit Jupiter의 `ExecutionCondition 확장 API`를 사용하면 개발자가 컨테이너를 활성화 또는 비활성화하거나 특정 조건에 따라 프로그래밍 방식으로 테스트할 수 있습니다. 이러한 조건의 가장 간단한 예는 `@Disabled` annotation을 지원하는 built-in [DisabledCondition](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-engine/src/main/java/org/junit/jupiter/engine/extension/DisabledCondition.java)입니다(테스트 비활성화 참조). `@Disabled` 외에도 JUnit Jupiter는 `org.junit.jupiter.api.condition` 패키지에서 여러 다른 annotation 기반 조건을 지원하여 개발자가 선언적으로 컨테이너 및 테스트를 활성화 또는 비활성화할 수 있도록 합니다. 여러 ExecutionCondition 확장이 등록되면 조건 중 하나가 비활성화됨을 반환하는 즉시 컨테이너 또는 테스트가 비활성화됩니다. 비활성화될 수 있는 이유에 대한 세부 정보를 제공하려는 경우 이러한 기본 제공 조건과 관련된 모든 annotation에는 해당 용도로 사용할 수 있는 `disabledReason` 속성이 있습니다.

See ExecutionCondition and the following sections for details.
>자세한 내용은 `ExecutionCondition` 및 다음 섹션을 참조하세요.

>>Composed Annotations
Note that any of the conditional annotations listed in the following sections may also be used as a meta-annotation in order to create a custom composed annotation. For example, the @TestOnMac annotation in the @EnabledOnOs demo shows how you can combine @Test and @EnabledOnOs in a single, reusable annotation.
>Composed Annotations
다음 섹션에 나열된 조건부 annotation은 사용자 정의 작성된 annotation을 생성하기 위해 meta-annotation으로 사용될 수도 있습니다. 예를 들어 `@EnabledOnOs` 데모의 `@TestOnMac` annotation은 `@Test`와 `@EnabledOnO`를 재사용 가능한 단일 annotation으로 결합하는 방법을 보여줍니다.

>>Unless otherwise stated, each of the conditional annotations listed in the following sections can only be declared once on a given test interface, test class, or test method. If a conditional annotation is directly present, indirectly present, or meta-present multiple times on a given element, only the first such annotation discovered by JUnit will be used; any additional declarations will be silently ignored. Note, however, that each conditional annotation may be used in conjunction with other conditional annotations in the org.junit.jupiter.api.condition package.
달리 명시되지 않는 한 다음 섹션에 나열된 각 조건부 annotation은 지정된 테스트 인터페이스, 테스트 클래스 또는 테스트 메소드에서 한 번만 선언될 수 있습니다. 조건부 annotation이 주어진 요소에 직접 존재하거나 간접적으로 존재하거나 여러 번 meta-present하는 경우 JUnit에서 발견한 첫 번째 annotation만 사용됩니다. 추가 선언은 자동으로 무시됩니다. 그러나 각 조건부 annotation은 `org.junit.jupiter.api.condition` 패키지의 다른 조건부 annotation과 함께 사용될 수 있습니다.

2.7.1. Operating System Conditions

A container or test may be enabled or disabled on a particular operating system via the @EnabledOnOs and @DisabledOnOs annotations.
>컨테이너 또는 테스트는 `@EnabledOnOs` 및 `@DisabledOnOs` annotation을 통해 특정 운영 체제에서 활성화 또는 비활성화될 수 있습니다.

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

2.7.2. Java Runtime Environment Conditions

A container or test may be enabled or disabled on particular versions of the Java Runtime Environment (JRE) via the @EnabledOnJre and @DisabledOnJre annotations or on a particular range of versions of the JRE via the @EnabledForJreRange and @DisabledForJreRange annotations. The range defaults to JRE.JAVA_8 as the lower border (min) and JRE.OTHER as the higher border (max), which allows usage of half open ranges.
>`@EnabledOnJre` 및 `@DisabledOnJre` annotation을 통해 JRE(Java Runtime Environment)의 특정 버전에서 또는 @EnabledForJreRange 및 @DisabledForJreRange annotation을 통해 [JRE](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/condition/JRE.html)의 특정 버전 범위에서 컨테이너 또는 테스트를 활성화하거나 비활성화할 수 있습니다. 범위는 기본적으로 [JRE](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/condition/JRE.html).JAVA_8이 아래쪽 경계(`min`)이고 [JRE](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/condition/JRE.html).OTHER가 위쪽 경계(`max`)로, 절반의 열린 범위를 사용할 수 있습니다.

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

2.7.3. System Property Conditions

A container or test may be enabled or disabled based on the value of the named JVM system property via the @EnabledIfSystemProperty and @DisabledIfSystemProperty annotations. The value supplied via the matches attribute will be interpreted as a regular expression.
>컨테이너 또는 테스트는 `@EnabledIfSystemProperty` 및 `@DisabledIfSystemProperty` annotation을 통해 `named` JVM 시스템 속성 값을 기반으로 활성화 또는 비활성화될 수 있습니다. `matches` attribute을 통해 제공된 값은 정규식으로 해석됩니다.

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

>>As of JUnit Jupiter 5.6, @EnabledIfSystemProperty and @DisabledIfSystemProperty are repeatable annotations. Consequently, these annotations may be declared multiple times on a test interface, test class, or test method. Specifically, these annotations will be found if they are directly present, indirectly present, or meta-present on a given element.
JUnit Jupiter 5.6부터 `@EnabledIfSystemProperty` 및 `@DisabledIfSystemProperty`는 *repeatable annotations*입니다. 결과적으로 이러한 annotation은 테스트 인터페이스, 테스트 클래스 또는 테스트 메소드에서 여러 번 선언될 수 있습니다. 특히 이러한 annotation은 주어진 요소에 직접 존재하거나 간접적으로 존재하거나 meta-present하는 경우 발견됩니다.

2.7.4. Environment Variable Conditions

A container or test may be enabled or disabled based on the value of the named environment variable from the underlying operating system via the @EnabledIfEnvironmentVariable and @DisabledIfEnvironmentVariable annotations. The value supplied via the matches attribute will be interpreted as a regular expression.
>컨테이너 또는 테스트는 `@EnabledIfEnvironmentVariable` 및 `@DisabledIfEnvironmentVariable` annotation을 통해 기본 운영 체제의 `named` 환경 변수 값을 기반으로 활성화 또는 비활성화할 수 있습니다. `matches` attribute을 통해 제공된 값은 정규식으로 해석됩니다.

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

>>As of JUnit Jupiter 5.6, @EnabledIfEnvironmentVariable and @DisabledIfEnvironmentVariable are repeatable annotations. Consequently, these annotations may be declared multiple times on a test interface, test class, or test method. Specifically, these annotations will be found if they are directly present, indirectly present, or meta-present on a given element.
>JUnit Jupiter 5.6부터 `@EnabledIfEnvironmentVariable` 및 `@DisabledIfEnvironmentVariable`은 *repeatable annotations*입니다. 결과적으로 이러한 annotation은 테스트 인터페이스, 테스트 클래스 또는 테스트 메소드에서 여러 번 선언될 수 있습니다. 특히 이러한 annotation은 주어진 요소에 직접 존재하거나 간접적으로 존재하거나 meta-present하는 경우 발견됩니다.

2.7.5. Custom Conditions

A container or test may be enabled or disabled based on the boolean return of a method via the @EnabledIf and @DisabledIf annotations. The method is provided to the annotation via its name. If needed, the condition method can take a single parameter of type ExtensionContext.
>`@EnabledIf` 및 `@DisabledIf` annotation을 통해 메소드의 boolean 반환을 기반으로 컨테이너 또는 테스트를 활성화하거나 비활성화할 수 있습니다. method는 이름을 통해 annotation에 제공됩니다. 필요한 경우 조건 method는 `ExtensionContext` 유형의 단일 매개 변수를 사용할 수 있습니다.

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

Alternatively, the condition method can be located outside the test class. In this case, it has to be referenced by its fully qualified name as demonstrated in the following example.
>또는 조건 메소드가 테스트 클래스 외부에 있을 수 있습니다. 이 경우 다음 예와 같이 *fully qualified name(완전히 정규화된 이름)*으로 참조해야 합니다.

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

>>When @EnabledIf or @DisabledIf is used at class level, the condition method must always be static. Condition methods located in external classes must also be static. In any other case, you can use both static or instance methods.
>@EnabledIf 또는 @DisabledIf가 클래스 수준에서 사용되는 경우 조건 method는 항상 `static`이어야 합니다. 외부 클래스에 있는 조건 메소드도 `static`이어야 합니다. 다른 경우에는 정적 또는 인스턴스 방법을 모두 사용할 수 있습니다.

2.8. Tagging and Filtering

Test classes and methods can be tagged via the @Tag annotation. Those tags can later be used to filter test discovery and execution. Please refer to the Tags section for more information about tag support in the JUnit Platform.
>`@Tag` annotation을 통해 테스트 클래스와 메소드에 태그를 지정할 수 있습니다. 이러한 태그는 나중에 `테스트 검색 및 실행을 필터링`하는 데 사용할 수 있습니다. JUnit 플랫폼의 태그 지원에 대한 자세한 내용은 `Tags` section을 참조하십시오.

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

>>See Meta-Annotations and Composed Annotations for examples demonstrating how to create custom annotations for tags.
>태그에 대한 사용자 정의 annotation을 만드는 방법을 보여주는 예제는 `Meta-Annotation 및 Composed annotation`을 참조하십시오.

2.9. Test Execution Order

By default, test classes and methods will be ordered using an algorithm that is deterministic but intentionally nonobvious. This ensures that subsequent runs of a test suite execute test classes and test methods in the same order, thereby allowing for repeatable builds.
>기본적으로 테스트 클래스와 method는 결정적이지만 의도적으로 명확하지 않은 알고리즘을 사용하여 정렬됩니다. 이것은 test suite의 후속 실행이 동일한 순서로 테스트 클래스와 테스트 메소드를 실행하도록 하여 반복 가능한 빌드를 허용합니다.

>>See Test Classes and Methods for a definition of test method and test class.
>`테스트 메소드 및 테스트 클래스`의 정의는 테스트 클래스 및 메소드를 참조하십시오.

2.9.1. Method Order

Although true unit tests typically should not rely on the order in which they are executed, there are times when it is necessary to enforce a specific test method execution order — for example, when writing integration tests or functional tests where the sequence of the tests is important, especially in conjunction with @TestInstance(Lifecycle.PER_CLASS).
>실제 단위 테스트는 일반적으로 실행 순서에 의존하지 않아야 하지만 특정 테스트 메소드 실행 순서를 적용해야 하는 경우가 있습니다. 예를 들어 통합 테스트나 기능 테스트를 작성할 때 테스트 순서가 특히 `@TestInstance(Lifecycle.PER_CLASS)`와 함께 사용하면 중요합니다.

To control the order in which test methods are executed, annotate your test class or test interface with @TestMethodOrder and specify the desired MethodOrderer implementation. You can implement your own custom MethodOrderer or use one of the following built-in MethodOrderer implementations.
>테스트 method가 실행되는 순서를 제어하려면 [@TestMethodOrder](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestMethodOrder.html)로 테스트 클래스 또는 테스트 인터페이스에 annotation을 달고 원하는 [MethodOrderer](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.html) 구현을 지정하십시오. 사용자 정의 `MethodOrderer`를 구현하거나 다음 기본 제공 `MethodOrderer `구현 중 하나를 사용할 수 있습니다.

- [MethodOrderer.DisplayName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.DisplayName.html): sorts test methods alphanumerically based on their display names (see `display name generation precedence` rules)

- [MethodOrderer.MethodName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.MethodName.html): sorts test methods alphanumerically based on their names and formal parameter lists
- [MethodOrderer.OrderAnnotation](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.OrderAnnotation.html): sorts test methods numerically based on values specified via the [@Order](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Order.html) annotation
- [MethodOrderer.Random](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.Random.html): orders test methods pseudo-randomly and supports configuration of a custom seed
- [MethodOrderer.Alphanumeric](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.Alphanumeric.html): sorts test methods alphanumerically based on their names and formal parameter lists; deprecated in favor of MethodOrderer.MethodName, to be removed in 6.0

>- [MethodOrderer.DisplayName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.DisplayName.html): display name에 따라 영숫자 순으로 테스트 메소드를 정렬합니다(`display name 생성 우선 순위 규칙` 참조).
>- [MethodOrderer.MethodName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.MethodName.html): 이름과 형식 매개변수 목록을 기반으로 테스트 메소드를 영숫자순으로 정렬합니다.
>- [MethodOrderer.OrderAnnotation](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.OrderAnnotation.html): [@Order](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Order.html) annotation을 통해 지정된 값을 기반으로 테스트 메소드를 숫자로 정렬합니다.
>- [MethodOrderer.Random](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.Random.html): 테스트 방법을 의사 무작위로 정렬하고 사용자 지정 시드 구성을 지원합니다.
>- [MethodOrderer.Alphanumeric](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.Alphanumeric.html): 이름과 형식 매개변수 목록을 기반으로 테스트 방법을 영숫자순으로 정렬합니다. 6.0에서 제거될 [MethodOrderer.MethodName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.MethodName.html)을 위해 더 이상 사용되지 않음

>>See also: Wrapping Behavior of Callbacks
>참조: `Wrapping Behavior of Callbacks(콜백의 래핑 동작)`

The following example demonstrates how to guarantee that test methods are executed in the order specified via the @Order annotation.
>다음 예제는 `@Order` annotation을 통해 지정된 순서대로 테스트 메소드가 실행되도록 보장하는 방법을 보여줍니다.


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

Setting the Default Method Orderer
You can use the junit.jupiter.testmethod.order.default configuration parameter to specify the fully qualified class name of the MethodOrderer you would like to use by default. Just like for the orderer configured via the @TestMethodOrder annotation, the supplied class has to implement the MethodOrderer interface. The default orderer will be used for all tests unless the @TestMethodOrder annotation is present on an enclosing test class or test interface.
>*Setting the Default Method Orderer(기본 메소드 순서 지정자 설정)*
`junit.jupiter.testmethod.order.default` `configuration parameter(구성 매개변수)`를 사용하여 기본적으로 사용하려는 [MethodOrderer](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.html)의 정규화된 클래스 이름을 지정할 수 있습니다. [@TestMethodOrder](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestMethodOrder.html) annotation을 통해 구성된 orderer와 마찬가지로 제공된 클래스는 `MethodOrderer` 인터페이스를 구현해야 합니다. `@TestMethodOrder` annotation이 둘러싸는 테스트 클래스 또는 테스트 인터페이스에 없으면 기본 순서 지정자가 모든 테스트에 사용됩니다.

For example, to use the MethodOrderer.OrderAnnotation method orderer by default, you should set the configuration parameter to the corresponding fully qualified class name (e.g., in src/test/resources/junit-platform.properties):
>예를 들어, [MethodOrderer.OrderAnnotation](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.OrderAnnotation.html) 메소드 orderer를 기본으로 사용하려면 구성 매개변수를 해당하는 fully qualified class name(정규화된 클래스 이름)으로 설정해야 합니다(예: `src/test/resources/junit-platform.properties`에서).

```
junit.jupiter.testmethod.order.default = \
    org.junit.jupiter.api.MethodOrderer$OrderAnnotation
```

Similarly, you can specify the fully qualified name of any custom class that implements MethodOrderer.
>마찬가지로 `MethodOrderer`를 구현하는 모든 사용자 지정 클래스의 정규화된 이름을 지정할 수 있습니다.

2.9.2. Class Order

Although test classes typically should not rely on the order in which they are executed, there are times when it is desirable to enforce a specific test class execution order. You may wish to execute test classes in a random order to ensure there are no accidental dependencies between test classes, or you may wish to order test classes to optimize build time as outlined in the following scenarios.
>테스트 클래스는 일반적으로 실행 순서에 의존하지 않아야 하지만 특정 테스트 클래스 실행 순서를 적용하는 것이 바람직한 경우가 있습니다. 테스트 클래스 간에 우발적인 종속성이 없는지 확인하기 위해 임의의 순서로 테스트 클래스를 실행하거나 다음 시나리오에 설명된 대로 빌드 시간을 최적화하기 위해 테스트 클래스를 주문할 수 있습니다.

- Run previously failing tests and faster tests first: "fail fast" mode
- With parallel execution enabled, run longer tests first: "shortest test plan execution duration" mode
- Various other use cases

>- 이전에 실패한 테스트와 더 빠른 테스트를 먼저 실행: "빠른 실패" 모드
>- 병렬 실행이 활성화된 상태에서 더 긴 테스트를 먼저 실행: "최단 테스트 계획 실행 기간" 모드
>- 그 외 다양한 활용 사례

To configure test class execution order globally for the entire test suite, use the junit.jupiter.testclass.order.default configuration parameter to specify the fully qualified class name of the ClassOrderer you would like to use. The supplied class must implement the ClassOrderer interface.
>전체 test suite에 대해 테스트 클래스 실행 순서를 전역적으로 구성하려면 `junit.jupiter.testclass.order.default` `구성 매개변수`를 사용하여 사용하려는 [ClassOrderer](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.html)의 정규화된 클래스 이름을 지정합니다. 제공된 클래스는 `ClassOrderer` 인터페이스를 구현해야 합니다.

You can implement your own custom ClassOrderer or use one of the following built-in ClassOrderer implementations.
>사용자 정의 `ClassOrderer`를 구현하거나 다음의 built-in `ClassOrderer` 구현 중 하나를 사용할 수 있습니다.

- ClassOrderer.ClassName: sorts test classes alphanumerically based on their fully qualified class names
- ClassOrderer.DisplayName: sorts test classes alphanumerically based on their display names (see display name generation precedence rules)
- ClassOrderer.OrderAnnotation: sorts test classes numerically based on values specified via the @Order annotation
- ClassOrderer.Random: orders test classes pseudo-randomly and supports configuration of a custom seed

>- [ClassOrderer.ClassName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.ClassName.html): 정규화된 클래스 이름을 기반으로 테스트 클래스를 영숫자 순으로 정렬합니다.
>- [ClassOrderer.DisplayName](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.DisplayName.html): display name에 따라 테스트 클래스를 영숫자 순으로 정렬합니다(`display name 생성 우선 순위 규칙` 참조).
>- [ClassOrderer.OrderAnnotation](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.OrderAnnotation.html): [@Order](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Order.html) annotation을 통해 지정된 값을 기반으로 테스트 클래스를 숫자로 정렬합니다.
>- [ClassOrderer.Random](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.Random.html): 테스트 클래스를 의사 무작위로 정렬하고 사용자 지정 시드 구성을 지원합니다.

For example, for the @Order annotation to be honored on test classes, you should configure the ClassOrderer.OrderAnnotation class orderer using the configuration parameter with the corresponding fully qualified class name (e.g., in src/test/resources/junit-platform.properties):
>예를 들어 `@Order` annotation이 테스트 클래스에서 적용되려면 해당하는 정규화된 클래스 이름과 함께 구성 매개변수를 사용하여 [ClassOrderer.OrderAnnotation](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/ClassOrderer.OrderAnnotation.html) 클래스 순서 지정자를 구성해야 합니다(예: `src/test/resources/junit-platform.properties`에서 ):

```
junit.jupiter.testclass.order.default = \
    org.junit.jupiter.api.ClassOrderer$OrderAnnotation
```

The configured ClassOrderer will be applied to all top-level test classes (including static nested test classes) and @Nested test classes.
>구성된 `ClassOrderer`는 모든 최상위 테스트 클래스(`static` nested 테스트 클래스 포함) 및 `@Nested` 테스트 클래스에 적용됩니다.

>>Top-level test classes will be ordered relative to each other; whereas, @Nested test classes will be ordered relative to other @Nested test classes sharing the same enclosing class.
최상위 테스트 클래스는 서로 상대적으로 정렬됩니다. 반면 `@Nested` 테스트 클래스는 동일한 엔클로징 클래스를 공유하는 다른 `@Nested` 테스트 클래스와 관련하여 순서가 지정됩니다.

To configure test class execution order locally for @Nested test classes, declare the @TestClassOrder annotation on the enclosing class for the @Nested test classes you want to order, and supply a class reference to the ClassOrderer implementation you would like to use directly in the @TestClassOrder annotation. The configured ClassOrderer will be applied recursively to @Nested test classes and their @Nested test classes. Note that a local @TestClassOrder declaration always overrides an inherited @TestClassOrder declaration or a ClassOrderer configured globally via the junit.jupiter.testclass.order.default configuration parameter.
>`@Nested` 테스트 클래스에 대해 로컬에서 테스트 클래스 실행 순서를 구성하려면 주문하려는 `@Nested` 테스트 클래스에 대한 엔클로징 클래스에 `@TestClassOrder` annotation을 선언하고 직접 사용하려는 ClassOrderer 구현에 대한 클래스 참조를 제공하십시오. `@TestClassOrder` annotation. 구성된 ClassOrderer는 `@Nested` 테스트 클래스와 `@Nested` 테스트 클래스에 재귀적으로 적용됩니다. 로컬 `@TestClassOrder` 선언은 항상 상속된 `@TestClassOrder` 선언 또는 `junit.jupiter.testclass.order.default` 구성 매개변수를 통해 전역적으로 구성된 ClassOrderer를 재정의합니다.

The following example demonstrates how to guarantee that @Nested test classes are executed in the order specified via the @Order annotation.
>다음 예제는 `@Nested` 테스트 클래스가 `@Order` annotation을 통해 지정된 순서대로 실행되도록 보장하는 방법을 보여줍니다.

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

2.10. Test Instance Lifecycle

In order to allow individual test methods to be executed in isolation and to avoid unexpected side effects due to mutable test instance state, JUnit creates a new instance of each test class before executing each test method (see Test Classes and Methods). This "per-method" test instance lifecycle is the default behavior in JUnit Jupiter and is analogous to all previous versions of JUnit.
>개별 테스트 메소드를 격리하여 실행할 수 있도록 하고 변경 가능한 테스트 인스턴스 상태로 인한 예기치 않은 부작용을 피하기 위해 JUnit은 각 테스트 메소드를 실행하기 전에 각 테스트 클래스의 새 인스턴스를 만듭니다(`테스트 클래스 및 메소드` 참조). 이 "메소드별" 테스트 인스턴스 수명 주기는 JUnit Jupiter의 기본 동작이며 모든 이전 버전의 JUnit과 유사합니다.

>>Please note that the test class will still be instantiated if a given test method is disabled via a condition (e.g., @Disabled, @DisabledOnOs, etc.) even when the "per-method" test instance lifecycle mode is active.
>"메소드별" 테스트 인스턴스 수명 주기 모드가 활성화된 경우에도 `조건`(예: `@Disabled`, `@DisabledOnOs` 등)을 통해 주어진 테스트 메소드가 비활성화되면 테스트 클래스가 계속 인스턴스화됩니다.

If you would prefer that JUnit Jupiter execute all test methods on the same test instance, annotate your test class with @TestInstance(Lifecycle.PER_CLASS). When using this mode, a new test instance will be created once per test class. Thus, if your test methods rely on state stored in instance variables, you may need to reset that state in @BeforeEach or @AfterEach methods.
>JUnit Jupiter가 동일한 테스트 인스턴스에서 모든 테스트 메소드를 실행하도록 하려면 테스트 클래스에 `@TestInstance(Lifecycle.PER_CLASS)` 주석을 추가하세요. 이 모드를 사용할 때 새 테스트 인스턴스는 테스트 클래스당 한 번 생성됩니다. 따라서 테스트 메소드가 인스턴스 변수에 저장된 상태에 의존하는 경우 `@BeforeEach` 또는 `@AfterEach` 메소드에서 해당 상태를 재설정해야 할 수 있습니다.

The "per-class" mode has some additional benefits over the default "per-method" mode. Specifically, with the "per-class" mode it becomes possible to declare @BeforeAll and @AfterAll on non-static methods as well as on interface default methods. The "per-class" mode therefore also makes it possible to use @BeforeAll and @AfterAll methods in @Nested test classes.
>"클래스별" 모드는 기본 "메소드별" 모드에 비해 몇 가지 추가 이점이 있습니다. 특히 "클래스별" 모드를 사용하면 비static method와 인터페이스 기본 메소드에서 `@BeforeAll` 및 `@AfterAll`을 선언할 수 있습니다. 따라서 "클래스별" 모드를 사용하면 `@Nested` 테스트 클래스에서 `@BeforeAll` 및 `@AfterAll` 메소드를 사용할 수도 있습니다.

If you are authoring tests using the Kotlin programming language, you may also find it easier to implement @BeforeAll and @AfterAll methods by switching to the "per-class" test instance lifecycle mode.
>Kotlin 프로그래밍 언어를 사용하여 테스트를 작성하는 경우 "클래스별" 테스트 인스턴스 수명 주기 모드로 전환하여 `@BeforeAll` 및 `@AfterAll` 메소드를 더 쉽게 구현할 수도 있습니다.

2.10.1. Changing the Default Test Instance Lifecycle

If a test class or test interface is not annotated with @TestInstance, JUnit Jupiter will use a default lifecycle mode. The standard default mode is PER_METHOD; however, it is possible to change the default for the execution of an entire test plan. To change the default test instance lifecycle mode, set the junit.jupiter.testinstance.lifecycle.default configuration parameter to the name of an enum constant defined in TestInstance.Lifecycle, ignoring case. This can be supplied as a JVM system property, as a configuration parameter in the LauncherDiscoveryRequest that is passed to the Launcher, or via the JUnit Platform configuration file (see Configuration Parameters for details).
>테스트 클래스 또는 테스트 인터페이스에 `@TestInstance` annotation이 없으면 JUnit Jupiter는 기본 수명 주기 모드를 사용합니다. 표준 기본 모드는 `PER_METHOD`입니다. 그러나 전체 테스트 계획 실행에 대한 기본값을 변경할 수 있습니다. 기본 테스트 인스턴스 수명 주기 모드를 변경하려면 대소문자를 무시하고 `Junit.jupiter.testinstance.lifecycle.default` 구성 매개변수를 `TestInstance.Lifecycle`에 정의된 열거형 상수의 이름으로 설정합니다. 이는 JVM 시스템 속성, `Launcher`에 전달되는 `LauncherDiscoveryRequest`의 구성 매개변수 또는 JUnit 플랫폼 구성 파일을 통해 제공될 수 있습니다(자세한 내용은 `구성 매개변수` 참조).

For example, to set the default test instance lifecycle mode to Lifecycle.PER_CLASS, you can start your JVM with the following system property.
>예를 들어 기본 테스트 인스턴스 수명 주기 모드를 `Lifecycle.PER_CLASS`로 설정하려면 다음 시스템 속성으로 JVM을 시작할 수 있습니다.

```
-Djunit.jupiter.testinstance.lifecycle.default=per_class
```

Note, however, that setting the default test instance lifecycle mode via the JUnit Platform configuration file is a more robust solution since the configuration file can be checked into a version control system along with your project and can therefore be used within IDEs and your build software.
>그러나 JUnit 플랫폼 구성 파일을 통해 기본 테스트 인스턴스 수명 주기 모드를 설정하는 것이 더 강력한 솔루션입니다. 구성 파일을 프로젝트와 함께 버전 제어 시스템에 체크인할 수 있고 따라서 IDE 및 빌드 소프트웨어 내에서 사용할 수 있기 때문입니다. 

To set the default test instance lifecycle mode to Lifecycle.PER_CLASS via the JUnit Platform configuration file, create a file named junit-platform.properties in the root of the class path (e.g., src/test/resources) with the following content.
>JUnit 플랫폼 구성 파일을 통해 기본 테스트 인스턴스 수명 주기 모드를 `Lifecycle.PER_CLASS`로 설정하려면 다음 내용으로 클래스 경로(예: `src/test/resources`)의 루트에 `junit-platform.properties`라는 파일을 생성합니다.

```
junit.jupiter.testinstance.lifecycle.default = per_class
```

>>Changing the default test instance lifecycle mode can lead to unpredictable results and fragile builds if not applied consistently. For example, if the build configures "per-class" semantics as the default but tests in the IDE are executed using "per-method" semantics, that can make it difficult to debug errors that occur on the build server. It is therefore recommended to change the default in the JUnit Platform configuration file instead of via a JVM system property.
>기본 테스트 인스턴스 수명 주기 모드를 변경하면 일관되게 적용하지 않으면 예측할 수 없는 결과와 취약한 빌드가 발생할 수 있습니다. 예를 들어 빌드가 "클래스별" 의미 체계를 기본값으로 구성하지만 IDE의 테스트가 "메소드별" 의미 체계를 사용하여 실행되는 경우 빌드 서버에서 발생하는 오류를 디버그하기 어려울 수 있습니다. 따라서 JVM 시스템 속성을 통하지 않고 JUnit 플랫폼 구성 파일에서 기본값을 변경하는 것이 좋습니다.

2.11. Nested Tests

@Nested tests give the test writer more capabilities to express the relationship among several groups of tests. Such nested tests make use of Java’s nested classes and facilitate hierarchical thinking about the test structure. Here’s an elaborate example, both as source code and as a screenshot of the execution within an IDE.
>`@Nested` 테스트는 테스트 작성자에게 여러 테스트 그룹 간의 관계를 표현할 수 있는 더 많은 기능을 제공합니다. 이러한 중첩 테스트는 Java의 중첩 클래스를 사용하고 테스트 구조에 대한 계층적 사고를 용이하게 합니다. 다음은 소스 코드와 IDE 내 실행의 스크린샷으로 된 정교한 예입니다.

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

When executing this example in an IDE, the test execution tree in the GUI will look similar to the following image.
>IDE에서 이 예제를 실행할 때 GUI의 테스트 실행 트리는 다음 이미지와 유사하게 보일 것입니다.

![error window](..\images\junit5_guide_writing-tests_nested_test_ide.png)
_Executing a nested test in an IDE_

In this example, preconditions from outer tests are used in inner tests by defining hierarchical lifecycle methods for the setup code. For example, createNewStack() is a @BeforeEach lifecycle method that is used in the test class in which it is defined and in all levels in the nesting tree below the class in which it is defined.
>이 예에서 외부 테스트의 전제 조건은 설정 코드에 대한 계층적 수명 주기 메소드를 정의하여 내부 테스트에서 사용됩니다. 예를 들어 `createNewStack()`은 정의된 테스트 클래스와 정의된 클래스 아래의 중첩 트리의 모든 수준에서 사용되는 `@BeforeEach` 수명 주기 메소드입니다.

The fact that setup code from outer tests is run before inner tests are executed gives you the ability to run all tests independently. You can even run inner tests alone without running the outer tests, because the setup code from the outer tests is always executed.
>내부 테스트가 실행되기 전에 외부 테스트의 설정 코드가 실행된다는 사실은 모든 테스트를 독립적으로 실행할 수 있는 기능을 제공합니다. 외부 테스트의 설정 코드가 항상 실행되기 때문에 외부 테스트를 실행하지 않고 내부 테스트만 실행할 수도 있습니다.

>>Only non-static nested classes (i.e. inner classes) can serve as @Nested test classes. Nesting can be arbitrarily deep, and those inner classes are subject to full lifecycle support with one exception: @BeforeAll and @AfterAll methods do not work by default. The reason is that Java does not allow static members in inner classes. However, this restriction can be circumvented by annotating a @Nested test class with @TestInstance(Lifecycle.PER_CLASS) (see Test Instance Lifecycle).
>정적이 아닌 중첩 클래스(예: 내부 클래스)만 `@Nested` 테스트 클래스로 사용할 수 있습니다. 중첩은 임의로 깊을 수 있으며 이러한 내부 클래스는 `@BeforeAll` 및 `@AfterAll` 메소드가 기본적으로 작동하지 않는 한 가지 예외를 제외하고 전체 수명 주기 지원 대상입니다. 그 이유는 Java가 내부 클래스에 정적 멤버를 허용하지 않기 때문입니다. 그러나 이 제한은 `@Nested` 테스트 클래스에 `@TestInstance(Lifecycle.PER_CLASS`) annotation을 추가하여 우회할 수 있습니다(`테스트 인스턴스 수명 주기` 참조).

2.12. Dependency Injection for Constructors and Methods

In all prior JUnit versions, test constructors or methods were not allowed to have parameters (at least not with the standard Runner implementations). As one of the major changes in JUnit Jupiter, both test constructors and methods are now permitted to have parameters. This allows for greater flexibility and enables Dependency Injection for constructors and methods.
>모든 이전 JUnit 버전에서 테스트 생성자 또는 메소드에는 매개변수가 허용되지 않았습니다(적어도 표준 `Runner` 구현에서는 허용되지 않음). JUnit Jupiter의 주요 변경 사항 중 하나로 이제 테스트 생성자와 메소드 모두 매개변수를 가질 수 있습니다. 이것은 더 큰 유연성을 허용하고 생성자와 메소드에 대한 종속성 주입을 가능하게 합니다.

ParameterResolver defines the API for test extensions that wish to dynamically resolve parameters at runtime. If a test class constructor, a test method, or a lifecycle method (see Test Classes and Methods) accepts a parameter, the parameter must be resolved at runtime by a registered ParameterResolver.
>[ParameterResolver](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ParameterResolver.html)는 런타임에 매개변수를 동적으로 확인하려는 테스트 확장을 위한 API를 정의합니다. 테스트 클래스 생성자, 테스트 메소드 또는 수명 주기 메소드(`테스트 클래스 및 메소드` 참조)가 매개변수를 수락하는 경우 매개변수는 등록된 ParameterResolver에 의해 런타임에 확인되어야 합니다.

There are currently three built-in resolvers that are registered automatically.
>현재 자동으로 등록되는 3개의 built-in resolver가 있습니다.

- TestInfoParameterResolver(): if a constructor or method parameter is of type TestInfo, the TestInfoParameterResolver will supply an instance of TestInfo corresponding to the current container or test as the value for the parameter. The TestInfo can then be used to retrieve information about the current container or test such as the display name, the test class, the test method, and associated tags. The display name is either a technical name, such as the name of the test class or test method, or a custom name configured via @DisplayName.
TestInfo acts as a drop-in replacement for the TestName rule from JUnit 4. The following demonstrates how to have TestInfo injected into a test constructor, @BeforeEach method, and @Test method.
>- [TestInfoParameterResolver](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-engine/src/main/java/org/junit/jupiter/engine/extension/TestInfoParameterResolver.java): 생성자 또는 메소드 매개변수가 [TestInfo](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestInfo.html) 유형인 경우 `TestInfoParameterResolver`는 현재 컨테이너 또는 테스트에 해당하는 `TestInfo` 인스턴스를 매개변수 값으로 제공합니다. 그런 다음 `TestInfo`를 사용하여 display name, 테스트 클래스, 테스트 메소드 및 관련 태그와 같은 현재 컨테이너 또는 테스트에 대한 정보를 검색할 수 있습니다. display name은 테스트 클래스 또는 테스트 메소드의 이름과 같은 기술적인 이름이거나 `@DisplayName`을 통해 구성된 사용자 지정 이름입니다.
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

- RepetitionInfoParameterResolver: if a method parameter in a @RepeatedTest, @BeforeEach, or @AfterEach method is of type RepetitionInfo, the RepetitionInfoParameterResolver will supply an instance of RepetitionInfo. RepetitionInfo can then be used to retrieve information about the current repetition and the total number of repetitions for the corresponding @RepeatedTest. Note, however, that RepetitionInfoParameterResolver is not registered outside the context of a @RepeatedTest. See Repeated Test Examples.
>- [RepetitionInfoParameterResolver](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-engine/src/main/java/org/junit/jupiter/engine/extension/RepetitionInfoParameterResolver.java): `@RepeatedTest`, `@BeforeEach` 또는 `@AfterEach `메소드의 메소드 매개 변수가 [RepetitionInfo](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/RepetitionInfo.html) 유형인 경우 `RepetitionInfoParameterResolver`는 `RepetitionInfo`의 인스턴스를 제공합니다. 그런 다음 `RepetitionInfo`를 사용하여 현재 반복 및 해당 `@RepeatedTest`에 대한 총 반복 횟수에 대한 정보를 검색할 수 있습니다. 그러나 `RepetitionInfoParameterResolver`는 `@RepeatedTest` 컨텍스트 외부에 등록되지 않습니다. `Repeated test` 예를 참조하십시오.

- TestReporterParameterResolver: if a constructor or method parameter is of type TestReporter, the TestReporterParameterResolver will supply an instance of TestReporter. The TestReporter can be used to publish additional data about the current test run. The data can be consumed via the reportingEntryPublished() method in a TestExecutionListener, allowing it to be viewed in IDEs or included in reports.
In JUnit Jupiter you should use TestReporter where you used to print information to stdout or stderr in JUnit 4. Using @RunWith(JUnitPlatform.class) will output all reported entries to stdout. In addition, some IDEs print report entries to stdout or display them in the user interface for test results.
>- [TestReporterParameterResolver](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-engine/src/main/java/org/junit/jupiter/engine/extension/TestReporterParameterResolver.java): 생성자 또는 메소드 매개변수가 [TestReporter](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestReporter.html) 유형인 경우 `TestReporterParameterResolver`는 `TestReporter`의 인스턴스를 제공합니다. `TestReporter`는 현재 테스트 실행에 대한 추가 데이터를 게시하는 데 사용할 수 있습니다. 데이터는 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html)의 `ReportingEntryPublished()` 메소드를 통해 사용되어 IDE에서 보거나 보고서에 포함될 수 있습니다.
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

>>Other parameter resolvers must be explicitly enabled by registering appropriate extensions via @ExtendWith.
>`@ExtendWith`를 통해 적절한 `extensions`을 등록하여 다른 매개변수 해석기를 명시적으로 활성화해야 합니다.

Check out the RandomParametersExtension for an example of a custom ParameterResolver. While not intended to be production-ready, it demonstrates the simplicity and expressiveness of both the extension model and the parameter resolution process. MyRandomParametersTest demonstrates how to inject random values into @Test methods.
>사용자 정의 [ParameterResolver](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ParameterResolver.html)의 예는 [RandomParametersExtension](https://github.com/junit-team/junit5-samples/blob/r5.8.2/junit5-jupiter-extensions/src/main/java/com/example/random/RandomParametersExtension.java)을 확인하십시오. production-ready가 되지는 않았지만 확장 모델과 매개변수 확인 프로세스의 단순성과 표현력을 보여줍니다. `MyRandomParametersTest`는 `@Test` 메소드에 임의의 값을 주입하는 방법을 보여줍니다.

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

For real-world use cases, check out the source code for the MockitoExtension and the SpringExtension.
>실제 사용 사례의 경우 [MockitoExtension](https://github.com/mockito/mockito/blob/release/2.x/subprojects/junit-jupiter/src/main/java/org/mockito/junit/jupiter/MockitoExtension.java) 및 [SpringExtension](https://github.com/spring-projects/spring-framework/blob/172102d225c916e2ca24b87aa0f0a74d824815b2/spring-test/src/main/java/org/springframework/test/context/junit/jupiter/SpringExtension.java)의 소스 코드를 확인하십시오.

When the type of the parameter to inject is the only condition for your ParameterResolver, you can use the generic TypeBasedParameterResolver base class. The supportsParameters method is implemented behind the scenes and supports parameterized types.
>주입할 매개변수의 유형이 [ParameterResolver](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ParameterResolver.html)의 유일한 조건인 경우 일반 [TypeBasedParameterResolver](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-api/src/main/java/org/junit/jupiter/api/extension/support/TypeBasedParameterResolver.java) 기본 클래스를 사용할 수 있습니다. `supportParameters` 메소드는 배후에서 구현되며 매개변수화된 유형을 지원합니다.

2.13. Test Interfaces and Default Methods

JUnit Jupiter allows @Test, @RepeatedTest, @ParameterizedTest, @TestFactory, @TestTemplate, @BeforeEach, and @AfterEach to be declared on interface default methods. @BeforeAll and @AfterAll can either be declared on static methods in a test interface or on interface default methods if the test interface or test class is annotated with @TestInstance(Lifecycle.PER_CLASS) (see Test Instance Lifecycle). Here are some examples.
>JUnit Jupiter는 `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory`, `@TestTemplate`, `@BeforeEach` 및 `@AfterEach`가 인터페이스 `default` 메소드에 선언되도록 허용합니다. `@BeforeAll` 및 `@AfterAll`은 테스트 인터페이스의 `static` 메소드 또는 테스트 인터페이스 또는 테스트 클래스에 `@TestInstance(Lifecycle.PER_CLASS)` annotation이 달린 경우 인터페이스 `default` 메소드에서 선언할 수 있습니다(`테스트 인스턴스 수명 주기` 참조). 여기 몇 가지 예가 있어요.

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

@ExtendWith and @Tag can be declared on a test interface so that classes that implement the interface automatically inherit its tags and extensions. See Before and After Test Execution Callbacks for the source code of the TimingExtension.
>`@ExtendWit`h 및 `@Tag`는 인터페이스를 구현하는 클래스가 태그와 확장을 자동으로 상속하도록 테스트 인터페이스에서 선언할 수 있습니다. `TimingExtension`의 소스 코드는 `테스트 실행 콜백 전후`를 참조하십시오.

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

Running the `TestInterfaceDemo` results in output similar to the following:
>TestInterfaceDemo를 실행하면 다음과 유사한 결과가 출력됩니다.

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

Another possible application of this feature is to write tests for interface contracts. For example, you can write tests for how implementations of Object.equals or Comparable.compareTo should behave as follows.
>이 기능의 또 다른 가능한 응용 프로그램은 인터페이스 계약에 대한 테스트를 작성하는 것입니다. 예를 들어 `Object.equals` 또는 `Comparable.compareTo`의 구현이 다음과 같이 동작해야 하는 방법에 대한 테스트를 작성할 수 있습니다.

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

In your test class you can then implement both contract interfaces thereby inheriting the corresponding tests. Of course you’ll have to implement the abstract methods.
>그런 다음 테스트 클래스에서 두 계약 인터페이스를 모두 구현하여 해당 테스트를 상속할 수 있습니다. 물론 abstract method를 구현해야 합니다.

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

>>The above tests are merely meant as examples and therefore not complete.
>위의 테스트는 예시일 뿐이므로 완전하지 않습니다.

2.14. Repeated Tests

JUnit Jupiter provides the ability to repeat a test a specified number of times by annotating a method with @RepeatedTest and specifying the total number of repetitions desired. Each invocation of a repeated test behaves like the execution of a regular @Test method with full support for the same lifecycle callbacks and extensions.
>JUnit Jupiter는 `@RepeatedTest`로 메소드에 annotation을 달고 원하는 총 반복 횟수를 지정하여 지정된 횟수만큼 테스트를 반복할 수 있는 기능을 제공합니다. 반복되는 테스트의 각 호출은 동일한 수명 주기 콜백 및 확장을 완벽하게 지원하는 일반 `@Test` 메소드의 실행처럼 작동합니다.

The following example demonstrates how to declare a test named repeatedTest() that will be automatically repeated 10 times.
>다음 예제는 자동으로 10번 반복되는 `repeatTest()`라는 테스트를 선언하는 방법을 보여줍니다.

```java
@RepeatedTest(10)
void repeatedTest() {
    // ...
}
```

In addition to specifying the number of repetitions, a custom display name can be configured for each repetition via the name attribute of the @RepeatedTest annotation. Furthermore, the display name can be a pattern composed of a combination of static text and dynamic placeholders. The following placeholders are currently supported.
>반복 횟수를 지정하는 것 외에도 `@RepeatedTest` annotation의 name 속성을 통해 각 반복에 대해 사용자 지정 display name을 구성할 수 있습니다. 또한 display name은 정적 텍스트와 동적 자리 표시자의 조합으로 구성된 패턴일 수 있습니다. 다음 자리 표시자가 현재 지원됩니다.

- {displayName}: display name of the @RepeatedTest method
- {currentRepetition}: the current repetition count
- {totalRepetitions}: the total number of repetitions

>- `{displayName}`: `@RepeatedTest` 메소드의 display name
>- `{currentRepetition}`: 현재 반복 횟수
>- `{totalRepetitions}`: 총 반복 횟수

The default display name for a given repetition is generated based on the following pattern: "repetition {currentRepetition} of {totalRepetitions}". Thus, the display names for individual repetitions of the previous repeatedTest() example would be: repetition 1 of 10, repetition 2 of 10, etc. If you would like the display name of the @RepeatedTest method included in the name of each repetition, you can define your own custom pattern or use the predefined RepeatedTest.LONG_DISPLAY_NAME pattern. The latter is equal to "{displayName} :: repetition {currentRepetition} of {totalRepetitions}" which results in display names for individual repetitions like repeatedTest() :: repetition 1 of 10, repeatedTest() :: repetition 2 of 10, etc.
>지정된 반복의 기본 display name은 `"repetition {currentRepetition} of {totalRepetitions}"` 패턴을 기반으로 생성됩니다. 따라서 이전 `repeatTest()` 예제의 개별 반복에 대한 display name은 `10번의 반복 1, 10의 반복 2` 등입니다. 각 반복의 이름에 `@RepeatedTest` 메소드의 display name을 포함하려면, 사용자 정의 패턴을 정의하거나 사전 정의된 `RepeatedTest.LONG_DISPLAY_NAME` 패턴을 사용할 수 있습니다. 후자는 `"{displayName} :: repeat {currentRepetition} of {totalRepetitions}"`와 같으며, 이는 `repeatTest() :: 반복 1/10, repeatTest() :: 반복 2/10 `등과 같은 개별 반복에 대한 display name을 생성합니다.

In order to retrieve information about the current repetition and the total number of repetitions programmatically, a developer can choose to have an instance of RepetitionInfo injected into a @RepeatedTest, @BeforeEach, or @AfterEach method.
>프로그래밍 방식으로 현재 반복 및 총 반복 횟수에 대한 정보를 검색하기 위해 개발자는 `RepetitionInfo` 인스턴스를 `@RepeatedTest`, `@BeforeEach` 또는 `@AfterEach` 메소드에 주입하도록 선택할 수 있습니다.

2.14.1. Repeated Test Examples

The RepeatedTestsDemo class at the end of this section demonstrates several examples of repeated tests.
>이 섹션의 끝에 있는 RepeatedTestsDemo 클래스는 `Repeated test`의 몇 가지 예를 보여줍니다.

The repeatedTest() method is identical to example from the previous section; whereas, repeatedTestWithRepetitionInfo() demonstrates how to have an instance of RepetitionInfo injected into a test to access the total number of repetitions for the current repeated test.
>`repeatTest()` method는 이전 섹션의 예제와 동일합니다. 반면, `repeatTestWithRepetitionInfo()`는 현재 반복되는 테스트의 총 반복 횟수에 액세스하기 위해 테스트에 `RepetitionInfo` 인스턴스를 주입하는 방법을 보여줍니다.

The next two methods demonstrate how to include a custom @DisplayName for the @RepeatedTest method in the display name of each repetition. customDisplayName() combines a custom display name with a custom pattern and then uses TestInfo to verify the format of the generated display name. Repeat! is the {displayName} which comes from the @DisplayName declaration, and 1/1 comes from {currentRepetition}/{totalRepetitions}. In contrast, customDisplayNameWithLongPattern() uses the aforementioned predefined RepeatedTest.LONG_DISPLAY_NAME pattern.
>다음 두 method는 각 반복의 display name에 `@RepeatedTest` 메소드에 대한 사용자 지정 `@DisplayName`을 포함하는 방법을 보여줍니다. `customDisplayName()`은 사용자 지정 display name을 사용자 지정 패턴과 결합한 다음 `TestInfo`를 사용하여 생성된 display name의 형식을 확인합니다. `Repeat!`는 `@DisplayName` 선언에서 가져온 `{displayName}`이고 `1/1`은 `{currentRepetition}/{totalRepetitions}`에서 가져옵니다. 대조적으로 `customDisplayNameWithLongPattern()`은 앞서 정의된 `RepeatedTest.LONG_DISPLAY_NAME` 패턴을 사용합니다.

repeatedTestInGerman() demonstrates the ability to translate display names of repeated tests into foreign languages — in this case German, resulting in names for individual repetitions such as: Wiederholung 1 von 5, Wiederholung 2 von 5, etc.
>`repeatTestInGerman()`는 repeated test의 display name을 외국어로 번역하는 기능을 보여줍니다.  — 이 경우 독일어, 결과적으로 `Wiederholung 1 von 5, Wiederholung 2 von 5` 등과 같은 개별 반복에 대한 이름이 생성됩니다.

Since the beforeEach() method is annotated with @BeforeEach it will get executed before each repetition of each repeated test. By having the TestInfo and RepetitionInfo injected into the method, we see that it’s possible to obtain information about the currently executing repeated test. Executing RepeatedTestsDemo with the INFO log level enabled results in the following output.
>`beforeEach()` method는 `@BeforeEach` annotation이 달려 있으므로 각 repeated test가 반복되기 전에 실행됩니다. `TestInfo`와 `RepetitionInfo`를 메소드에 주입함으로써 현재 실행 중인 repeated test에 대한 정보를 얻을 수 있음을 알 수 있습니다. `INFO` 로그 수준이 활성화된 상태에서 `RepeatedTestsDemo`를 실행하면 다음 출력이 생성됩니다.

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

When using the ConsoleLauncher with the unicode theme enabled, execution of RepeatedTestsDemo results in the following output to the console.
>유니코드 테마가 활성화된 `ConsoleLauncher`를 사용할 때 `RepeatedTestsDemo`를 실행하면 콘솔에 다음과 같은 출력이 표시됩니다.

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

2.15. Parameterized Tests

Parameterized tests make it possible to run a test multiple times with different arguments. They are declared just like regular @Test methods but use the @ParameterizedTest annotation instead. In addition, you must declare at least one source that will provide the arguments for each invocation and then consume the arguments in the test method.
>parameterized test를 사용하면 다른 인수로 테스트를 여러 번 실행할 수 있습니다. 그것들은 일반 `@Test` 메소드처럼 선언되지만 대신 [@ParameterizedTest](https://junit.org/junit5/docs/current/api/org.junit.jupiter.params/org/junit/jupiter/params/ParameterizedTest.html) annotation을 사용합니다. 또한 각 호출에 대한 인수를 제공한 다음 테스트 메소드에서 인수를 사용할 소스를 하나 이상 선언해야 합니다.

The following example demonstrates a parameterized test that uses the @ValueSource annotation to specify a String array as the source of arguments.
>다음 예제는 `@ValueSource` annotation을 사용하여 `String` 배열을 인수의 소스로 지정하는 parameterized test를 보여줍니다.

```java
@ParameterizedTest
@ValueSource(strings = { "racecar", "radar", "able was I ere I saw elba" })
void palindromes(String candidate) {
    assertTrue(StringUtils.isPalindrome(candidate));
}
```

When executing the above parameterized test method, each invocation will be reported separately. For instance, the ConsoleLauncher will print output similar to the following.
>위의 parameterized test 방법을 실행할 때 각 호출은 별도로 보고됩니다. 예를 들어, `ConsoleLauncher`는 다음과 유사한 출력을 인쇄합니다.

```
palindromes(String) ✔
├─ [1] candidate=racecar ✔
├─ [2] candidate=radar ✔
└─ [3] candidate=able was I ere I saw elba ✔
```

2.15.1. Required Setup

In order to use parameterized tests you need to add a dependency on the junit-jupiter-params artifact. Please refer to Dependency Metadata for details.
>parameterized test를 사용하려면 `junit-jupiter-params` artifact에 종속성을 추가해야 합니다. 자세한 내용은 `Dependency Metadata`를 참조하세요.

2.15.2. Consuming Arguments

Parameterized test methods typically consume arguments directly from the configured source (see Sources of Arguments) following a one-to-one correlation between argument source index and method parameter index (see examples in @CsvSource). However, a parameterized test method may also choose to aggregate arguments from the source into a single object passed to the method (see Argument Aggregation). Additional arguments may also be provided by a ParameterResolver (e.g., to obtain an instance of TestInfo, TestReporter, etc.). Specifically, a parameterized test method must declare formal parameters according to the following rules.
>`parameterized test` method는 일반적으로 인수 소스 인덱스와 메소드 매개변수 인덱스 간의 일대일 상관 관계에 따라 구성된 소스('Source of Arguments' 참조)에서 직접 인수를 사용합니다(`@CsvSource`의 예 참조). 그러나 `parameterized test` method는 소스의 인수를 메소드에 전달된 단일 개체로 집계하도록 선택할 수도 있습니다(`Argument Aggregation` 참조). `ParameterResolver`에서 추가 인수를 제공할 수도 있습니다(예: `TestInfo`, `TestReporter` 등의 인스턴스를 얻기 위해). 특히 `parameterized test` 방법은 다음 규칙에 따라 형식 매개변수를 선언해야 합니다.

- Zero or more indexed arguments must be declared first.
- Zero or more aggregators must be declared next.
- Zero or more arguments supplied by a ParameterResolver must be declared last.

>- 0개 이상의 *indexed arguments 인덱싱된 인수*를 먼저 선언해야 합니다.
>- 다음에 0개 이상의 *aggregators집계자*를 선언해야 합니다.
>- `ParameterResolver`에서 제공하는 0개 이상의 인수는 마지막에 선언되어야 합니다.

In this context, an indexed argument is an argument for a given index in the Arguments provided by an ArgumentsProvider that is passed as an argument to the parameterized method at the same index in the method’s formal parameter list. An aggregator is any parameter of type ArgumentsAccessor or any parameter annotated with @AggregateWith.
>이 컨텍스트에서 인덱싱된 인수는 메소드의 형식 매개 변수 목록에서 동일한 인덱스의 매개 변수화된 메소드에 인수로 전달되는 *ArgumentsProvider*에서 제공하는 *Arguments*의 지정된 인덱스에 대한 인수입니다. *aggregator집계자*는 *ArgumentsAccessor* 유형의 매개변수 또는 *@AggregateWith*로 annotation이 달린 매개변수입니다.

>>AutoCloseable arguments
Arguments that implement java.lang.AutoCloseable (or java.io.Closeable which extends java.lang.AutoCloseable) will be automatically closed after @AfterEach methods and AfterEachCallback extensions have been called for the current parameterized test invocation.
>*AutoCloseable arguments*
`java.lang.AutoCloseable`(또는 `java.lang.AutoCloseable`을 확장하는 `java.io.Closeabl`e)을 구현하는 인수는 `@AfterEach` 메소드 및 `AfterEachCallback` 확장이 현재 `parameterized test` 호출에 대해 호출된 후 자동으로 닫힙니다.
>>To prevent this from happening, set the autoCloseArguments attribute in @ParameterizedTest to false. Specifically, if an argument that implements AutoCloseable is reused for multiple invocations of the same parameterized test method, you must annotate the method with @ParameterizedTest(autoCloseArguments = false) to ensure that the argument is not closed between invocations.
이를 방지하려면 `@ParameterizedTest`의 `autoCloseArguments` 속성을 `false`로 설정하십시오. 특히 `AutoCloseable`을 구현하는 인수가 동일한 매개 변수화된 테스트 메소드의 여러 호출에 재사용되는 경우 호출 간에 인수가 닫히지 않도록 `@ParameterizedTest(autoCloseArguments = false)`로 메소드에 annotation을 달아야 합니다.

2.15.3. Sources of Arguments

Out of the box, JUnit Jupiter provides quite a few source annotations. Each of the following subsections provides a brief overview and an example for each of them. Please refer to the Javadoc in the org.junit.jupiter.params.provider package for additional information.
>JUnit Jupiter는 기본적으로 몇 가지 소스 annotation을 제공합니다. 다음 subsection은 각각에 대한 간략한 개요와 예를 제공합니다. 추가 정보는 [org.junit.jupiter.params.provider](https://junit.org/junit5/docs/current/api/org.junit.jupiter.params/org/junit/jupiter/params/provider/package-summary.html) 패키지의 Javadoc을 참조하십시오.

@ValueSource
@ValueSource is one of the simplest possible sources. It lets you specify a single array of literal values and can only be used for providing a single argument per parameterized test invocation.
>`@ValueSource`는 가장 간단한 가능한 소스 중 하나입니다. 이를 통해 리터럴 값의 단일 배열을 지정할 수 있으며 매개 변수화된 테스트 호출당 단일 인수를 제공하는 데만 사용할 수 있습니다.

The following types of literal values are supported by @ValueSource.
>`@ValueSource`는 다음 유형의 리터럴 값을 지원합니다.

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

For example, the following @ParameterizedTest method will be invoked three times, with the values 1, 2, and 3 respectively.
>예를 들어 다음 `@ParameterizedTes`t method는 각각 1, 2, 3 값으로 세 번 호출됩니다.

```java
@ParameterizedTest
@ValueSource(ints = { 1, 2, 3 })
void testWithValueSource(int argument) {
    assertTrue(argument > 0 && argument < 4);
}
```

*Null and Empty Sources*

In order to check corner cases and verify proper behavior of our software when it is supplied bad input, it can be useful to have null and empty values supplied to our parameterized tests. The following annotations serve as sources of null and empty values for parameterized tests that accept a single argument.
>코너 케이스를 확인하고 잘못된 입력이 제공될 때 소프트웨어의 올바른 동작을 확인하려면 `parameterized test`에 null 및 빈 값을 제공하는 것이 유용할 수 있습니다. 다음 annotation은 단일 인수를 허용하는 `parameterized test`에 대한 null 및 빈 값의 소스 역할을 합니다.

- @NullSource: provides a single null argument to the annotated @ParameterizedTest method.
    - @NullSource cannot be used for a parameter that has a primitive type.
- @EmptySource: provides a single empty argument to the annotated @ParameterizedTest method for parameters of the following types: java.lang.String, java.util.List, java.util.Set, java.util.Map, primitive arrays (e.g., int[], char[][], etc.), object arrays (e.g.,String[], Integer[][], etc.).
    - Subtypes of the supported types are not supported.
- @NullAndEmptySource: a composed annotation that combines the functionality of @NullSource and @EmptySource.

>- [@NullSource](https://junit.org/junit5/docs/current/api/org.junit.jupiter.params/org/junit/jupiter/params/provider/NullSource.html): annotation이 달린 `@ParameterizedTest` 메소드에 단일 null 인수를 제공합니다.
    - `@NullSource`는 프리미티브 유형의 매개변수에 사용할 수 없습니다.
>- [@EmptySource](https://junit.org/junit5/docs/current/api/org.junit.jupiter.params/org/junit/jupiter/params/provider/EmptySource.html): `java.lang.String`, `java.util.List`, `java.util.Set`, `java.util.Map`, 기본 배열(예: , `int[]`, `char[][]` 등), 객체 배열(예: `String[]`, `Integer[][]` 등).
    - 지원되는 유형의 subtype은 지원되지 않습니다.
>- [@NullAndEmptySource](https://junit.org/junit5/docs/current/api/org.junit.jupiter.params/org/junit/jupiter/params/provider/NullAndEmptySource.html): `@NullSource`와 `@EmptySource`의 기능을 결합한 합성 annotation.

If you need to supply multiple varying types of blank strings to a parameterized test, you can achieve that using @ValueSource — for example, @ValueSource(strings = {" ", "   ", "\t", "\n"}).
>`parameterized test`에 다양한 유형의 빈 문자열을 제공해야 하는 경우 `@ValueSource` — 를 사용하여 이를 달성할 수 있습니다(예: `@ValueSource(strings = {" ", " ", "\t", "\n"}`) .

You can also combine @NullSource, @EmptySource, and @ValueSource to test a wider range of null, empty, and blank input. The following example demonstrates how to achieve this for strings.
>`@NullSource`, `@EmptySource` 및 `@ValueSource`를 결합하여 더 넓은 범위의 `null`, 공백 및 공백 입력을 테스트할 수도 있습니다. 다음 예제는 문자열에 대해 이를 달성하는 방법을 보여줍니다.

```java
@ParameterizedTest
@NullSource
@EmptySource
@ValueSource(strings = { " ", "   ", "\t", "\n" })
void nullEmptyAndBlankStrings(String text) {
    assertTrue(text == null || text.trim().isEmpty());
}
```

Making use of the composed @NullAndEmptySource annotation simplifies the above as follows.
>합성된 `@NullAndEmptySource` annotation을 사용하면 위의 내용이 다음과 같이 단순화됩니다.

```java
@ParameterizedTest
@NullAndEmptySource
@ValueSource(strings = { " ", "   ", "\t", "\n" })
void nullEmptyAndBlankStrings(String text) {
    assertTrue(text == null || text.trim().isEmpty());
}
```

>>Both variants of the nullEmptyAndBlankStrings(String) parameterized test method result in six invocations: 1 for null, 1 for the empty string, and 4 for the explicit blank strings supplied via @ValueSource.
>`nullEmptyAndBlankStrings(String)` 매개 변수화된 테스트 메소드의 두 변형은 모두 6개의 호출을 생성합니다. `null`의 경우 1, 빈 문자열의 경우 1, `@ValueSource`를 통해 제공된 명시적 빈 문자열의 경우 4입니다.

@EnumSource

@EnumSource provides a convenient way to use Enum constants.
>`@EnumSource`는 Enum 상수를 사용하는 편리한 방법을 제공합니다.

```java
@ParameterizedTest
@EnumSource(ChronoUnit.class)
void testWithEnumSource(TemporalUnit unit) {
    assertNotNull(unit);
}
```
The annotation’s value attribute is optional. When omitted, the declared type of the first method parameter is used. The test will fail if it does not reference an enum type. Thus, the value attribute is required in the above example because the method parameter is declared as TemporalUnit, i.e. the interface implemented by ChronoUnit, which isn’t an enum type. Changing the method parameter type to ChronoUnit allows you to omit the explicit enum type from the annotation as follows.
>annotation의 `value` 속성은 선택 사항입니다. 생략하면 첫 번째 메소드 매개변수의 선언된 유형이 사용됩니다. 열거형을 참조하지 않으면 테스트가 실패합니다. 따라서 위의 예에서 `value` 속성은 메소드 매개변수가 `TemporalUnit`, 즉 열거형이 아닌 `ChronoUnit`에 의해 구현된 인터페이스로 선언되었기 때문에 필요합니다. 메소드 매개변수 유형을 `ChronoUnit`으로 변경하면 다음과 같이 annotation에서 명시적 열거형 유형을 생략할 수 있습니다.

```java
@ParameterizedTest
@EnumSource
void testWithEnumSourceWithAutoDetection(ChronoUnit unit) {
    assertNotNull(unit);
}
```

The annotation provides an optional names attribute that lets you specify which constants shall be used, like in the following example. If omitted, all constants will be used.
>annotation은 다음 예제와 같이 사용할 상수를 지정할 수 있는 선택적 `name` attribute를 제공합니다. 생략하면 모든 상수가 사용됩니다.

```java
@ParameterizedTest
@EnumSource(names = { "DAYS", "HOURS" })
void testWithEnumSourceInclude(ChronoUnit unit) {
    assertTrue(EnumSet.of(ChronoUnit.DAYS, ChronoUnit.HOURS).contains(unit));
}
```

The @EnumSource annotation also provides an optional mode attribute that enables fine-grained control over which constants are passed to the test method. For example, you can exclude names from the enum constant pool or specify regular expressions as in the following examples.
>`@EnumSource` annotation은 또한 테스트 메소드에 전달되는 상수를 세밀하게 제어할 수 있는 선택적 `mode` attribute를 제공합니다. 예를 들어 열거형 상수 풀에서 이름을 제외하거나 다음 예와 같이 정규식을 지정할 수 있습니다.

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

@MethodSource

@MethodSource allows you to refer to one or more factory methods of the test class or external classes.
>`@MethodSource`를 사용하면 테스트 클래스 또는 외부 클래스의 하나 이상의 `factory` 메소드를 참조할 수 있습니다.

Factory methods within the test class must be static unless the test class is annotated with @TestInstance(Lifecycle.PER_CLASS); whereas, factory methods in external classes must always be static. In addition, such factory methods must not accept any arguments.
>테스트 클래스가 `@TestInstance(Lifecycle.PER_CLASS)`로 annotation 처리되지 않는 한 테스트 클래스 내의 factory method는 `static`이어야 합니다. 반면 외부 클래스의 factory method는 항상 `static`이어야 합니다. 또한 이러한 factory method는 인수를 허용하지 않아야 합니다.

Each factory method must generate a stream of arguments, and each set of arguments within the stream will be provided as the physical arguments for individual invocations of the annotated @ParameterizedTest method. Generally speaking this translates to a Stream of Arguments (i.e., Stream<Arguments>); however, the actual concrete return type can take on many forms. In this context, a "stream" is anything that JUnit can reliably convert into a Stream, such as Stream, DoubleStream, LongStream, IntStream, Collection, Iterator, Iterable, an array of objects, or an array of primitives. The "arguments" within the stream can be supplied as an instance of Arguments, an array of objects (e.g., Object[]), or a single value if the parameterized test method accepts a single argument.
>각 factory method는 인수 stream을 생성해야 하며 stream 내의 각 인수 세트는 annotation이 달린 `@ParameterizedTest` 메소드의 개별 호출에 대한 물리적 인수로 제공됩니다. 일반적으로 이것은 `stream of arguments 인수 스트림`(예: `Stream<Arguments>`)으로 변환됩니다. 그러나 실제 구체적인 반환 유형은 다양한 형태를 취할 수 있습니다. 이 컨텍스트에서 "stream"은 `Stream`, `DoubleStream`, `LongStream`, `IntStream`, `Collection`, `Iterator`, `Iterable`, `객체 배열` 또는 `기본 요소 배열`과 같이 JUnit이 Stream으로 안정적으로 변환할 수 있는 모든 것입니다. stream 내의 "인수"는 `Arguments`의 인스턴스, 객체 배열(예: `Object[]`) 또는 `parameterized test` 메소드가 단일 인수를 허용하는 경우 단일 값으로 제공될 수 있습니다.

If you only need a single parameter, you can return a Stream of instances of the parameter type as demonstrated in the following example.
>단일 매개변수만 필요한 경우 다음 예제와 같이 매개변수 유형 인스턴스의 `Stream`을 반환할 수 있습니다.

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

If you do not explicitly provide a factory method name via @MethodSource, JUnit Jupiter will search for a factory method that has the same name as the current @ParameterizedTest method by convention. This is demonstrated in the following example.
>`@MethodSource`를 통해 `factory` 메소드 이름을 명시적으로 제공하지 않으면 JUnit Jupiter는 규칙에 따라 현재 `@ParameterizedTest` 메소드와 동일한 이름을 갖는 factory method를 검색합니다. 이것은 다음 예에서 보여줍니다.

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

Streams for primitive types (DoubleStream, IntStream, and LongStream) are also supported as demonstrated by the following example.
>다음 예제와 같이 기본 유형(`DoubleStream`, `IntStream` 및 `LongStream`)에 대한 stream도 지원됩니다.

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

If a parameterized test method declares multiple parameters, you need to return a collection, stream, or array of Arguments instances or object arrays as shown below (see the Javadoc for @MethodSource for further details on supported return types). Note that arguments(Object…​) is a static factory method defined in the Arguments interface. In addition, Arguments.of(Object…​) may be used as an alternative to arguments(Object…​).
>매개 변수화된 테스트 메소드가 여러 매개 변수를 선언하는 경우 아래와 같이 `Arguments` 인스턴스 또는 개체 배열의 collection, stream 또는 배열을 반환해야 합니다(지원되는 반환 유형에 대한 자세한 내용은 `@MethodSource`에 대한 Javadoc 참조). `arguments(Object…​)`는 Arguments 인터페이스에 정의된 정적 factory method입니다. 또한 `Arguments.of(Object…​)`는 `arguments(Object…​)`의 대안으로 사용될 수 있습니다.

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

An external, static factory method can be referenced by providing its fully qualified method name as demonstrated in the following example.
>다음 예제와 같이 *정규화된 메소드 이름*을 제공하여 외부의 `static` *factory* method를 참조할 수 있습니다.

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

@CsvSource

@CsvSource allows you to express argument lists as comma-separated values (i.e., CSV String literals). Each string provided via the value attribute in @CsvSource represents a CSV record and results in one invocation of the parameterized test. The first record may optionally be used to supply CSV headers (see the Javadoc for the useHeadersInDisplayName attribute for details and an example).
>`@CsvSource`를 사용하면 인수 목록을 쉼표로 구분된 값(즉, CSV 문자열 리터럴)으로 표현할 수 있습니다. `@CsvSource`의 값 속성을 통해 제공된 각 문자열은 CSV 레코드를 나타내며 `parameterized test`가 한 번 호출됩니다. 첫 번째 레코드는 선택적으로 CSV 헤더를 제공하는 데 사용할 수 있습니다(자세한 내용과 예제는 `useHeadersInDisplayName` 속성에 대한 Javadoc 참조).

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

The default delimiter is a comma (,), but you can use another character by setting the delimiter attribute. Alternatively, the delimiterString attribute allows you to use a String delimiter instead of a single character. However, both delimiter attributes cannot be set simultaneously.
>기본 구분자는 쉼표(`,`)이지만 `delimiter` 속성을 설정하여 다른 문자를 사용할 수 있습니다. 또는 `delimiterString` 속성을 사용하면 단일 문자 대신 문자열 구분 기호를 사용할 수 있습니다. 그러나 두 구분 기호 속성을 동시에 설정할 수는 없습니다.

By default, @CsvSource uses a single quote (') as its quote character, but this can be changed via the quoteCharacter attribute. See the 'lemon, lime' value in the example above and in the table below. An empty, quoted value ('') results in an empty String unless the emptyValue attribute is set; whereas, an entirely empty value is interpreted as a null reference. By specifying one or more nullValues, a custom value can be interpreted as a null reference (see the NIL example in the table below). An ArgumentConversionException is thrown if the target type of a null reference is a primitive type.
>기본적으로 @CsvSource는 작은따옴표(`'`)를 인용 문자로 사용하지만 이는 `quoteCharacter` 속성을 통해 변경할 수 있습니다. 위의 예와 아래 표에서 `'레몬, 라임'` 값을 참조하십시오. 비어 있고 따옴표로 묶인 값('')은 `emptyValue` 속성이 설정되지 않은 한 빈 `String`이 됩니다. 반면 완전히 비어 있는 값은 `null` 참조로 해석됩니다. 하나 이상의 `nullValue를` 지정하여 사용자 지정 값을 null 참조로 해석할 수 있습니다(아래 표의 NIL 예 참조). null 참조의 대상 유형이 기본 유형인 경우 `ArgumentConversionException`이 발생합니다.

An unquoted empty value will always be converted to a null reference regardless of any custom values configured via the nullValues attribute.
>따옴표가 없는 빈 값은 `nullValues` 속성을 통해 구성된 사용자 지정 값에 관계없이 항상 null 참조로 변환됩니다.

Except within a quoted string, leading and trailing whitespace in a CSV column is trimmed by default. This behavior can be changed by setting the ignoreLeadingAndTrailingWhitespace attribute to true.
>따옴표로 묶인 문자열 내를 제외하고 CSV 열의 선행 및 후행 공백은 기본적으로 잘립니다. 이 동작은 `ignoreLeadingAndTrailingWhitespace` 속성을 `true`로 설정하여 변경할 수 있습니다.

|Example Input|Resulting Argument List|
|---|---|
|@CsvSource({ "apple, banana" })|`"apple"`, `"banana"`|
|@CsvSource({ "apple, 'lemon, lime'" })|`"apple"`, `"lemon`, `lime"`|
|@CsvSource({ "apple, ''" })|`"apple", ""`|
|@CsvSource({ "apple, " })|`"apple"`, `nul`l`|
|@CsvSource(value = { "apple, banana, NIL" }, nullValues = "NIL")|`"apple"`, `"banana"`, `null`|
|@CsvSource(value = { " apple , banana" }, ignoreLeadingAndTrailingWhitespace = false)|`" apple "`, `" banana"`|

If the programming language you are using supports text blocks — for example, Java SE 15 or higher — you can alternatively use the textBlock attribute of @CsvSource. Each record within a text block represents a CSV record and results in one invocation of the parameterized test. The first record may optionally be used to supply CSV headers by setting the useHeadersInDisplayName attribute to true as in the example below.
>사용 중인 프로그래밍 언어가 텍스트 블록 (예: Java SE 15 이상 )을 지원하는 경우 `@CsvSource`의 `textBlock` 속성을 대신 사용할 수 있습니다. 텍스트 블록 내의 각 레코드는 CSV 레코드를 나타내며 `parameterized test`가 한 번 호출됩니다. 첫 번째 레코드는 선택적으로 아래 예와 같이 `useHeadersInDisplayName` 속성을 `true`로 설정하여 CSV 헤더를 제공하는 데 사용할 수 있습니다.

Using a text block, the previous example can be implemented as follows.
>텍스트 블록을 사용하여 이전 예제를 다음과 같이 구현할 수 있습니다.

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

The generated display names for the previous example include the CSV header names.
>이전 예에서 생성된 display name에는 CSV 헤더 이름이 포함됩니다.

```
[1] FRUIT = apple, RANK = 1
[2] FRUIT = banana, RANK = 2
[3] FRUIT = lemon, lime, RANK = 0xF1
[4] FRUIT = strawberry, RANK = 700_000
```

In contrast to CSV records supplied via the value attribute, a text block can contain comments. Any line beginning with a # symbol will be treated as a comment and ignored. Note, however, that the # symbol must be the first character on the line without any leading whitespace. It is therefore recommended that the closing text block delimiter (""") be placed either at the end of the last line of input or on the following line, left aligned with the rest of the input (as can be seen in the example below which demonstrates formatting similar to a table).
>값 속성을 통해 제공되는 CSV 레코드와 달리 텍스트 블록에는 annotation이 포함될 수 있습니다. `#` 기호로 시작하는 줄은 annotation으로 처리되어 무시됩니다. 그러나 `#` 기호는 선행 공백 없이 행의 첫 번째 문자여야 합니다. 따라서 닫는 텍스트 블록 구분 기호(`"""`)를 입력의 마지막 줄이나 다음 줄에 배치하고 나머지 입력과 왼쪽 정렬하는 것이 좋습니다(아래 예에서 볼 수 있음). 표와 유사한 형식을 보여줍니다.

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

>>Java’s text block feature automatically removes incidental whitespace when the code is compiled. However other JVM languages such as Groovy and Kotlin do not. Thus, if you are using a programming language other than Java and your text block contains comments or new lines within quoted strings, you will need to ensure that there is no leading whitespace within your text block.
>Java의 [text block](https://docs.oracle.com/en/java/javase/15/text-blocks/index.html) 기능은 코드가 컴파일될 때 부수적인 공백을 자동으로 제거합니다. 그러나 Groovy 및 Kotlin과 같은 다른 JVM 언어는 그렇지 않습니다. 따라서 Java 이외의 프로그래밍 언어를 사용 중이고 텍스트 블록에 따옴표 붙은 문자열 내에 annotation이나 새 줄이 포함되어 있는 경우 텍스트 블록 내에 선행 공백이 없는지 확인해야 합니다.

@CsvFileSource

@CsvFileSource lets you use comma-separated value (CSV) files from the classpath or the local file system. Each record from a CSV file results in one invocation of the parameterized test. The first record may optionally be used to supply CSV headers. You can instruct JUnit to ignore the headers via the numLinesToSkip attribute. If you would like for the headers to be used in the display names, you can set the useHeadersInDisplayName attribute to true. The examples below demonstrate the use of numLinesToSkip and useHeadersInDisplayName.
>`@CsvFileSource`를 사용하면 클래스 경로 또는 로컬 파일 시스템에서 쉼표로 구분된 값(CSV) 파일을 사용할 수 있습니다. CSV 파일의 각 레코드는 `parameterized test`를 한 번 호출합니다. 첫 번째 레코드는 선택적으로 CSV 헤더를 제공하는 데 사용할 수 있습니다. `numLinesToSkip` 속성을 통해 헤더를 무시하도록 JUnit에 지시할 수 있습니다. display name에 헤더를 사용하려면 `useHeadersInDisplayName` 속성을 `true`로 설정하면 됩니다. 아래 예는 `numLinesToSkip` 및 `useHeadersInDisplayName` 사용을 보여줍니다.

The default delimiter is a comma (,), but you can use another character by setting the delimiter attribute. Alternatively, the delimiterString attribute allows you to use a String delimiter instead of a single character. However, both delimiter attributes cannot be set simultaneously.
>기본 구분자는 쉼표(`,`)이지만 `delimiter` 속성을 설정하여 다른 문자를 사용할 수 있습니다. 또는 `delimiterString` 속성을 사용하면 단일 문자 대신 문자열 구분 기호를 사용할 수 있습니다. 그러나 두 구분 기호 속성을 동시에 설정할 수는 없습니다.

>>Comments in CSV files
Any line beginning with a # symbol will be interpreted as a comment and will be ignored.
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

The following listing shows the generated display names for the first two parameterized test methods above.
>다음 목록은 위의 처음 두 개의 `parameterized test` 방법에 대해 생성된 display name을 보여줍니다.

```
[1] country=Sweden, reference=1
[2] country=Poland, reference=2
[3] country=United States of America, reference=3
[4] country=France, reference=700_000
```

The following listing shows the generated display names for the last parameterized test method above that uses CSV header names.
>다음 목록은 CSV 헤더 이름을 사용하는 위의 마지막 `parameterized test` 방법에 대해 생성된 display name을 보여줍니다.

```
[1] COUNTRY = Sweden, REFERENCE = 1
[2] COUNTRY = Poland, REFERENCE = 2
[3] COUNTRY = United States of America, REFERENCE = 3
[4] COUNTRY = France, REFERENCE = 700_000
```

In contrast to the default syntax used in @CsvSource, @CsvFileSource uses a double quote (") as the quote character by default, but this can be changed via the quoteCharacter attribute. See the "United States of America" value in the example above. An empty, quoted value ("") results in an empty String unless the emptyValue attribute is set; whereas, an entirely empty value is interpreted as a null reference. By specifying one or more nullValues, a custom value can be interpreted as a null reference. 
>`@CsvSource`에서 사용되는 기본 구문과 달리 `@CsvFileSourc`e는 기본적으로 큰따옴표(")를 인용 문자로 사용하지만 이는 `quoteCharacter` 속성을 통해 변경할 수 있습니다. 위의 예에서 `"United States of America"` 값을 참조하십시오. . 따옴표로 묶인 비어 있는 값(`""`)은 `emptyValue` 속성이 설정되지 않은 한 빈 `String`이 됩니다. 반면에 완전히 비어 있는 값은 `null` 참조로 해석됩니다. 

>>An ArgumentConversionException is thrown if the target type of a null reference is a primitive type.
>하나 이상의 `nullValue`를 지정하면 사용자 정의 값을 `null` 참조 `null` 참조의 대상 유형이 기본 유형인 경우 `ArgumentConversionException`이 발생합니다.

An unquoted empty value will always be converted to a null reference regardless of any custom values configured via the nullValues attribute.
Except within a quoted string, leading and trailing whitespace in a CSV column is trimmed by default. This behavior can be changed by setting the ignoreLeadingAndTrailingWhitespace attribute to true.
>따옴표가 없는 빈 값은 `nullValues` 속성을 통해 구성된 사용자 지정 값에 관계없이 항상 `null` 참조로 변환됩니다.
따옴표로 묶인 문자열 내를 제외하고 CSV 열의 선행 및 후행 공백은 기본적으로 잘립니다. 이 동작은 `ignoreLeadingAndTrailingWhitespace` 속성을 `true`로 설정하여 변경할 수 있습니다.

@ArgumentsSource

@ArgumentsSource can be used to specify a custom, reusable ArgumentsProvider. Note that an implementation of ArgumentsProvider must be declared as either a top-level class or as a static nested class.
>`@ArgumentsSource`는 재사용 가능한 맞춤형 `ArgumentsProvider`를 지정하는 데 사용할 수 있습니다. `ArgumentsProvider`의 구현은 최상위 클래스 또는 static nested 클래스로 선언되어야 합니다.

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

2.15.4. Argument Conversion

Widening Conversion
>확대 전환

JUnit Jupiter supports Widening Primitive Conversion for arguments supplied to a @ParameterizedTest. For example, a parameterized test annotated with @ValueSource(ints = { 1, 2, 3 }) can be declared to accept not only an argument of type int but also an argument of type long, float, or double.
>JUnit Jupiter는 `@ParameterizedTest`에 제공된 인수에 대해 [확대 기본 변환](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.1.2)을 지원합니다. 예를 들어` @ValueSource(ints = { 1, 2, 3 })` annotation이 달린 `parameterized test`는 `int` 유형의 인수뿐만 아니라 `long`, `float` 또는 `double` 유형의 인수도 허용하도록 선언할 수 있습니다.

Implicit Conversion
>암시적 변환

To support use cases like @CsvSource, JUnit Jupiter provides a number of built-in implicit type converters. The conversion process depends on the declared type of each method parameter.
>`@CsvSource`와 같은 사용 사례를 지원하기 위해 JUnit Jupiter는 여러 built-in 암시적 유형 변환기를 제공합니다. 변환 프로세스는 각 메소드 매개변수의 선언된 유형에 따라 다릅니다.

For example, if a @ParameterizedTest declares a parameter of type TimeUnit and the actual type supplied by the declared source is a String, the string will be automatically converted into the corresponding TimeUnit enum constant.
>예를 들어 `@ParameterizedTest`가 `TimeUnit` 유형의 매개변수를 선언하고 선언된 소스에서 제공한 실제 유형이 `String`인 경우 문자열은 해당 `TimeUnit` 열거형 상수로 자동 변환됩니다.

```java
@ParameterizedTest
@ValueSource(strings = "SECONDS")
void testWithImplicitArgumentConversion(ChronoUnit argument) {
    assertNotNull(argument.name());
}
```

String instances are implicitly converted to the following target types.
>문자열 인스턴스는 암시적으로 다음 대상 유형으로 변환됩니다.

Decimal, hexadecimal, and octal String literals will be converted to their integral types: byte, short, int, long, and their boxed counterparts.
>10진수, 16진수 및 8진수 문자열 리터럴은 `byte`, `short`, `int`, `long` 및 아래 표의 대응과 같은 정수 유형으로 변환됩니다.

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

Fallback String-to-Object Conversion
>대체 문자열-객체 변환

In addition to implicit conversion from strings to the target types listed in the above table, JUnit Jupiter also provides a fallback mechanism for automatic conversion from a String to a given target type if the target type declares exactly one suitable factory method or a factory constructor as defined below.
위 표에 나열된 대상 유형으로 `String`을 암시적으로 변환하는 것 외에도 JUnit Jupiter는 대상 유형이 정확히 하나의 적절한 *factory method* 또는 *factory constructor*를 아래에 정의되어 있습니다.

- factory method: a non-private, static method declared in the target type that accepts a single String argument and returns an instance of the target type. The name of the method can be arbitrary and need not follow any particular convention.

- factory constructor: a non-private constructor in the target type that accepts a single String argument. Note that the target type must be declared as either a top-level class or as a static nested class.

>- *factory method*: 단일 `String` 인수를 받아들이고 대상 형식의 인스턴스를 반환하는 대상 형식에 선언된 private가 아닌 static method입니다. 메소드 이름은 임의적일 수 있으며 특정 규칙을 따를 필요는 없습니다.

>- *factory constructor*: 단일 String 인수를 허용하는 대상 유형의 private가 아닌 생성자. 대상 유형은 최상위 클래스 또는 *static* nested 클래스로 선언되어야 합니다.

>>If multiple factory methods are discovered, they will be ignored. If a factory method and a factory constructor are discovered, the factory method will be used instead of the constructor.
For example, in the following @ParameterizedTest method, the Book argument will be created by invoking the Book.fromTitle(String) factory method and passing "42 Cats" as the title of the book.
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

Explicit Conversion

Instead of relying on implicit argument conversion you may explicitly specify an ArgumentConverter to use for a certain parameter using the @ConvertWith annotation like in the following example. Note that an implementation of ArgumentConverter must be declared as either a top-level class or as a static nested class.
>암시적 인수 변환에 의존하는 대신 다음 예제와 같이 `@ConvertWith` annotation을 사용하여 특정 매개변수에 사용할 `ArgumentConverter`를 명시적으로 지정할 수 있습니다. `ArgumentConverter`의 구현은 최상위 클래스 또는 static nested 클래스로 선언되어야 합니다.

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

If the converter is only meant to convert one type to another, you can extend TypedArgumentConverter to avoid boilerplate type checks.
>변환기가 한 유형을 다른 유형으로 변환하기 위한 것이라면 `TypedArgumentConverter`를 확장하여 상용구 유형 검사(boilerplate type check)를 피할 수 있습니다.

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
Explicit argument converters are meant to be implemented by test and extension authors. Thus, junit-jupiter-params only provides a single explicit argument converter that may also serve as a reference implementation: JavaTimeArgumentConverter. It is used via the composed annotation JavaTimeConversionPattern.
>명시적 인수 변환기는 테스트 및 확장 작성자가 구현해야 합니다. 따라서 `junit-jupiter-params`는 참조 구현으로도 사용할 수 있는 단일 명시적 인수 변환기인 `JavaTimeArgumentConverter`만 제공합니다. 작성된 annotation `JavaTimeConversionPattern`을 통해 사용됩니다.

```java
@ParameterizedTest
@ValueSource(strings = { "01.01.2017", "31.12.2017" })
void testWithExplicitJavaTimeConverter(
        @JavaTimeConversionPattern("dd.MM.yyyy") LocalDate argument) {

    assertEquals(2017, argument.getYear());
}
```

2.15.5. Argument Aggregation

By default, each argument provided to a @ParameterizedTest method corresponds to a single method parameter. Consequently, argument sources which are expected to supply a large number of arguments can lead to large method signatures.
>기본적으로 `@ParameterizedTest` 메소드에 제공된 각 인수는 단일 메소드 매개변수에 해당합니다. 결과적으로 많은 수의 인수를 제공할 것으로 예상되는 인수 소스는 큰 메소드 서명으로 이어질 수 있습니다.

In such cases, an ArgumentsAccessor can be used instead of multiple parameters. Using this API, you can access the provided arguments through a single argument passed to your test method. In addition, type conversion is supported as discussed in Implicit Conversion.
>이러한 경우 여러 매개변수 대신 `ArgumentsAccessor`를 사용할 수 있습니다. 이 API를 사용하면 테스트 메소드에 전달된 단일 인수를 통해 제공된 인수에 액세스할 수 있습니다. 또한 암시적 변환에서 설명한 대로 형식 변환이 지원됩니다.

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

An instance of ArgumentsAccessor is automatically injected into any parameter of type ArgumentsAccessor.
>`ArgumentsAccessor`의 인스턴스는 `ArgumentsAccessor` 유형의 매개변수에 자동으로 삽입됩니다.

Custom Aggregators

Apart from direct access to a @ParameterizedTest method’s arguments using an ArgumentsAccessor, JUnit Jupiter also supports the usage of custom, reusable aggregators.
>`ArgumentsAccessor`를 사용하여 `@ParameterizedTest` 메소드의 인수에 직접 액세스하는 것 외에도 JUnit Jupiter는 재사용 가능한 사용자 지정 aggregator 사용을 지원합니다.

To use a custom aggregator, implement the ArgumentsAggregator interface and register it via the @AggregateWith annotation on a compatible parameter in the @ParameterizedTest method. The result of the aggregation will then be provided as an argument for the corresponding parameter when the parameterized test is invoked. Note that an implementation of ArgumentsAggregator must be declared as either a top-level class or as a static nested class.
>사용자 정의 수집기를 사용하려면 `ArgumentsAggregator` 인터페이스를 구현하고 `@ParameterizedTest` 메소드의 호환 가능한 매개변수에 `@AggregateWith` annotation을 통해 등록하십시오. aggregation 결과는 `parameterized test`가 호출될 때 해당 매개변수에 대한 인수로 제공됩니다. `ArgumentsAggregator`의 구현은 최상위 클래스 또는 static nested 클래스로 선언되어야 합니다.

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

If you find yourself repeatedly declaring @AggregateWith(MyTypeAggregator.class) for multiple parameterized test methods across your codebase, you may wish to create a custom composed annotation such as @CsvToMyType that is meta-annotated with @AggregateWith(MyTypeAggregator.class). The following example demonstrates this in action with a custom @CsvToPerson annotation.
>코드베이스에서 여러 `parameterized test` 메소드에 대해 `@AggregateWith(MyTypeAggregator.class)`를 반복적으로 선언하는 경우 `@AggregateWith(MyTypeAggregator.class`)로 메타 annotation이 달린` @CsvToMyType`과 같은 사용자 정의 합성 annotation을 생성할 수 있습니다. 다음 예제는 사용자 정의 `@CsvToPerson` annotation을 사용하여 이를 실제로 보여줍니다.

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

2.15.6. Customizing Display Names

By default, the display name of a parameterized test invocation contains the invocation index and the String representation of all arguments for that specific invocation. Each of them is preceded by the parameter name (unless the argument is only available via an ArgumentsAccessor or ArgumentAggregator), if present in the bytecode (for Java, test code must be compiled with the -parameters compiler flag).
>기본적으로 parameterized test 호출의 display name에는 호출 인덱스와 해당 특정 호출에 대한 모든 인수의 `String` 표현이 포함됩니다. 바이트코드에 있는 경우(Java의 경우 테스트 코드는 `-parameters` 컴파일러 플래그로 컴파일해야 함) 매개변수 이름이 각각 앞에 옵니다(인수가 `ArgumentsAccessor` 또는 `ArgumentAggregator`를 통해서만 사용 가능한 경우 제외).

However, you can customize invocation display names via the name attribute of the @ParameterizedTest annotation like in the following example.
>그러나 다음 예제와 같이 `@ParameterizedTest` annotation의 name 속성을 통해 호출 display name을 사용자 정의할 수 있습니다.

```java
@DisplayName("Display name of container")
@ParameterizedTest(name = "{index} ==> the rank of ''{0}'' is {1}")
@CsvSource({ "apple, 1", "banana, 2", "'lemon, lime', 3" })
void testWithCustomDisplayNames(String fruit, int rank) {
}
```

When executing the above method using the ConsoleLauncher you will see output similar to the following.
>`ConsoleLauncher`를 사용하여 위의 방법을 실행하면 다음과 유사한 출력이 표시됩니다.

```
Display name of container ✔
├─ 1 ==> the rank of 'apple' is 1 ✔
├─ 2 ==> the rank of 'banana' is 2 ✔
└─ 3 ==> the rank of 'lemon, lime' is 3 ✔
```

Please note that name is a MessageFormat pattern. Thus, a single quote (') needs to be represented as a doubled single quote ('') in order to be displayed.
>이름은 `MessageFormat` 패턴입니다. 따라서 작은따옴표(`'`)를 표시하려면 큰따옴표(`''`)로 표시해야 합니다.

The following placeholders are supported within custom display names.
>다음 자리 표시자는 사용자 지정 display name 내에서 지원됩니다.

|Placeholder|Description|
|---|---|
|{displayName}|the display name of the method|
|{index}|the current invocation index (1-based)|
|{arguments}|the complete, comma-separated arguments list|
|{argumentsWithNames}|the complete, comma-separated arguments list with parameter names|
|{0}, {1}, …​|an individual argument|

>>When including arguments in display names, their string representations are truncated if they exceed the configured maximum length. The limit is configurable via the junit.jupiter.params.displayname.argument.maxlength configuration parameter and defaults to 512 characters.
>display name에 인수를 포함할 때 문자열 표현이 구성된 최대 길이를 초과하면 잘립니다. 제한은 `junit.jupiter.params.displayname.argument.maxlength` 구성 매개변수를 통해 구성할 수 있으며 기본값은 512자입니다.

When using @MethodSource or @ArgumentSource, you can give names to arguments. This name will be used if the argument is included in the invocation display name, like in the example below.
>`@MethodSource` 또는 `@ArgumentSource`를 사용할 때 인수에 이름을 지정할 수 있습니다. 이 이름은 아래 예와 같이 호출 display name에 인수가 포함된 경우 사용됩니다.

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

If you’d like to set default name pattern for all parameterized tests in your project, you can add the following configuration to junit-platform.properties
>프로젝트의 모든 parameterized test에 대한 기본 이름 패턴을 설정하려면 다음 구성을 `junit-platform.properties`에 추가할 수 있습니다.

```
junit.jupiter.params.displayname.default = {index}
```

the display name for a parameterized method is determined according to the following precedence rules:
>매개 변수화된 메소드의 display name은 다음 우선 순위 규칙에 따라 결정됩니다.

1. name of @ParameterizedTest, if present
1. the value of junit.jupiter.params.displayname.default (from junit-platform.properties), if present
1. DEFAULT_DISPLAY_NAME constant defined in @ParameterizedTest

>1. `@ParameterizedTest`의 이름(이 있는 경우)
>1. `junit.jupiter.params.displayname.default` 값(junit-platform.properties에서)(이 있는 경우)
>1. `@ParameterizedTest`에 정의된 `DEFAULT_DISPLAY_NAME` 상수

2.15.7. Lifecycle and Interoperability

Each invocation of a parameterized test has the same lifecycle as a regular @Test method. For example, @BeforeEach methods will be executed before each invocation. Similar to Dynamic Tests, invocations will appear one by one in the test tree of an IDE. You may at will mix regular @Test methods and @ParameterizedTest methods within the same test class.
>parameterized test의 각 호출은 일반 `@Test` 메소드와 동일한 수명 주기를 갖습니다. 예를 들어 `@BeforeEach` method는 각 호출 전에 실행됩니다. `dynamic test`와 유사하게 호출은 IDE의 테스트 트리에 하나씩 나타납니다. 동일한 테스트 클래스 내에서 일반 `@Test` 메소드와 `@ParameterizedTest `메소드를 마음대로 혼합할 수 있습니다.

You may use ParameterResolver extensions with @ParameterizedTest methods. However, method parameters that are resolved by argument sources need to come first in the argument list. Since a test class may contain regular tests as well as parameterized tests with different parameter lists, values from argument sources are not resolved for lifecycle methods (e.g. @BeforeEach) and test class constructors.
>`@ParameterizedTest` 메소드와 함께 `ParameterResolver` 확장을 사용할 수 있습니다. 그러나 인수 소스에 의해 확인되는 메소드 매개변수는 인수 목록에서 맨 처음에 와야 합니다. 테스트 클래스에는 일반 테스트와 다른 매개변수 목록이 있는 parameterized test가 포함될 수 있으므로 인수 소스의 값은 수명 주기 메소드(예: `@BeforeEach`) 및 테스트 클래스 생성자에 대해 확인되지 않습니다.

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

2.16. Test Templates

A @TestTemplate method is not a regular test case but rather a template for test cases. As such, it is designed to be invoked multiple times depending on the number of invocation contexts returned by the registered providers. Thus, it must be used in conjunction with a registered TestTemplateInvocationContextProvider extension. Each invocation of a test template method behaves like the execution of a regular @Test method with full support for the same lifecycle callbacks and extensions. Please refer to Providing Invocation Contexts for Test Templates for usage examples.
>[@TestTemplate](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestTemplate.html) 메소드는 일반 테스트 케이스가 아니라 테스트 케이스를 위한 템플릿입니다. 따라서 등록된 공급자가 반환하는 호출 컨텍스트의 수에 따라 여러 번 호출되도록 설계되었습니다. 따라서 등록된 [TestTemplateInvocationContextProvider](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestTemplateInvocationContextProvider.html) 확장과 함께 사용해야 합니다. 테스트 템플릿 메소드의 각 호출은 동일한 수명 주기 콜백 및 확장을 완벽하게 지원하는 일반 `@Test` 메소드 실행처럼 작동합니다. 사용 예는 `테스트 템플릿에 대한 호출 컨텍스트 제공`을 참조하십시오.

>>Repeated Tests and Parameterized Tests are built-in specializations of test templates.
>`Repeated test` 및 `parameterized test`는 테스트 템플릿의 기본 제공 전문화입니다.

2.17. Dynamic Tests

The standard @Test annotation in JUnit Jupiter described in Annotations is very similar to the @Test annotation in JUnit 4. Both describe methods that implement test cases. These test cases are static in the sense that they are fully specified at compile time, and their behavior cannot be changed by anything happening at runtime. Assumptions provide a basic form of dynamic behavior but are intentionally rather limited in their expressiveness.
>`Annotation`에 설명된 JUnit Jupiter의 표준  `@Test`  annotation은 JUnit 4의  `@Test`  annotation과 매우 유사합니다. 둘 다 테스트 케이스를 구현하는 메소드를 설명합니다. 이러한 테스트 케이스는 컴파일 시간에 완전히 지정된다는 점에서 정적이며 런타임에 발생하는 어떤 것으로도 그 동작을 변경할 수 없습니다. *assumption은 기본적인 형태의 동적 행동을 제공하지만 의도적으로 표현이 다소 제한됩니다.*

In addition to these standard tests a completely new kind of test programming model has been introduced in JUnit Jupiter. This new kind of test is a dynamic test which is generated at runtime by a factory method that is annotated with @TestFactory.
>이러한 표준 테스트 외에도 완전히 새로운 종류의 테스트 프로그래밍 모델이 JUnit Jupiter에 도입되었습니다. 이 새로운 종류의 테스트는 `@TestFactory`로 annotation이 달린 factory method에 의해 런타임에 생성되는 dynamic test입니다.

In contrast to @Test methods, a @TestFactory method is not itself a test case but rather a factory for test cases. Thus, a dynamic test is the product of a factory. Technically speaking, a @TestFactory method must return a single DynamicNode or a Stream, Collection, Iterable, Iterator, or array of DynamicNode instances. Instantiable subclasses of DynamicNode are DynamicContainer and DynamicTest. DynamicContainer instances are composed of a display name and a list of dynamic child nodes, enabling the creation of arbitrarily nested hierarchies of dynamic nodes. DynamicTest instances will be executed lazily, enabling dynamic and even non-deterministic generation of test cases.
>`@Tes`t 메소드와 달리 `@TestFactory` 메소드는 그 자체가 테스트 케이스가 아니라 테스트 케이스를 위한 팩토리입니다. 따라서 `dynamic test`는 factory의 산물입니다. 기술적으로 말하면 `@TestFactory `method는 단일 `DynamicNode` 또는 `Stream`, `Collection`, `Iterable`, `Iterator` 또는 `DynamicNode` 인스턴스의 배열을 반환해야 합니다. `DynamicNode`의 인스턴스화 가능한 subclass는 `DynamicContainer` 및 `DynamicTest`입니다. `DynamicContainer` 인스턴스는 display name과 동적 자식 노드 목록으로 구성되어 동적 노드의 임의 중첩 계층을 생성할 수 있습니다. DynamicTest 인스턴스는 느리게 실행되어 테스트 사례의 동적 및 비결정적 생성을 가능하게 합니다.

Any Stream returned by a @TestFactory will be properly closed by calling stream.close(), making it safe to use a resource such as Files.lines().
>`@TestFactory`에서 반환된 모든 `stream`은 `stream.close()`를 호출하여 적절하게 닫히므로 `Files.lines()`와 같은 리소스를 안전하게 사용할 수 있습니다.

As with @Test methods, @TestFactory methods must not be private or static and may optionally declare parameters to be resolved by ParameterResolvers.
>`@Test` 메소드와 마찬가지로 `@TestFactory` method는 *private*이거나 *static*이어서는 안 되며 선택적으로 `ParameterResolvers`에서 확인할 매개변수를 선언할 수 있습니다.

A DynamicTest is a test case generated at runtime. It is composed of a display name and an Executable. Executable is a @FunctionalInterface which means that the implementations of dynamic tests can be provided as lambda expressions or method references.
>`DynamicTest`는 런타임에 생성되는 테스트 케이스입니다. display name과 `실행` 파일로 구성됩니다. `실행` 파일은 `@FunctionalInterface`입니다. 즉, dynamic test의 구현이 *lambda expressions* 또는 *메소드 참조*로 제공될 수 있습니다.

>>*Dynamic Test Lifecycle*
>>The execution lifecycle of a dynamic test is quite different than it is for a standard @Test case. Specifically, there are no lifecycle callbacks for individual dynamic tests. This means that @BeforeEach and @AfterEach methods and their corresponding extension callbacks are executed for the @TestFactory method but not for each dynamic test. In other words, if you access fields from the test instance within a lambda expression for a dynamic test, those fields will not be reset by callback methods or extensions between the execution of individual dynamic tests generated by the same @TestFactory method.
>>*Dynamic Test Lifecycle*
>`dynamic test`의 실행 수명 주기는 표준 `@Test` 경우와 상당히 다릅니다. 특히 개별 `dynamic test`에는 수명 주기 콜백이 없습니다. 즉, `@BeforeEach` 및 `@AfterEach` 메소드와 해당 확장 콜백은 `@TestFactory` 메소드에 대해 실행되지만 각 `dynamic test`에 대해서는 실행되지 않습니다. 즉, *dynamic test*를 위한 lambda expressions 내 테스트 인스턴스의 필드에 액세스하는 경우 해당 필드는 동일한 `@TestFactory` 메소드에 의해 생성된 개별 *dynamic test* 실행 사이의 확장 또는 콜백 메소드에 의해 재설정되지 않습니다.

As of JUnit Jupiter 5.8.2, dynamic tests must always be created by factory methods; however, this might be complemented by a registration facility in a later release.
>JUnit Jupiter 5.8.2부터 *dynamic test*는 항상 factory method로 생성해야 합니다. 그러나 이는 이후 릴리스의 등록 기능으로 보완될 수 있습니다.

2.17.1. Dynamic Test Examples

The following DynamicTestsDemo class demonstrates several examples of test factories and dynamic tests.
>다음 `DynamicTestsDemo` 클래스는 테스트 팩토리 및 dynamic test의 몇 가지 예를 보여줍니다.

The first method returns an invalid return type. Since an invalid return type cannot be detected at compile time, a JUnitException is thrown when it is detected at runtime.
>첫 번째 method는 잘못된 반환 유형을 반환합니다. 유효하지 않은 반환 유형은 컴파일 시간에 감지할 수 없으므로 런타임에 감지되면 `JUnitException`이 발생합니다.

The next six methods are very simple examples that demonstrate the generation of a Collection, Iterable, Iterator, array, or Stream of DynamicTest instances. Most of these examples do not really exhibit dynamic behavior but merely demonstrate the supported return types in principle. However, dynamicTestsFromStream() and dynamicTestsFromIntStream() demonstrate how easy it is to generate dynamic tests for a given set of strings or a range of input numbers.
>다음 6가지 방법은 `Collection`, `Iterable`, `Iterator`, `array` 또는 DynamicTest 인스턴스의 `Stream` 생성을 보여주는 매우 간단한 예입니다. 이러한 예제의 대부분은 실제로 동적 동작을 나타내지 않고 원칙적으로 지원되는 반환 유형을 보여줍니다. 그러나 `dynamicTestsFromStream()` 및 `dynamicTestsFromIntStream()`은 주어진 문자열 세트 또는 입력 숫자 범위에 대한 dynamic test를 생성하는 것이 얼마나 쉬운지를 보여줍니다.

The next method is truly dynamic in nature. generateRandomNumberOfTests() implements an Iterator that generates random numbers, a display name generator, and a test executor and then provides all three to DynamicTest.stream(). Although the non-deterministic behavior of generateRandomNumberOfTests() is of course in conflict with test repeatability and should thus be used with care, it serves to demonstrate the expressiveness and power of dynamic tests.
>다음 방법은 본질적으로 동적입니다. `generateRandomNumberOfTests()`는 난수를 생성하는 `Iterator`, display name 생성기 및 테스트 실행기를 구현한 다음 세 가지 모두를 `DynamicTest.stream()`에 제공합니다. 물론 `generateRandomNumberOfTests()`의 비결정적 동작은 테스트 반복성과 충돌하므로 주의해서 사용해야 하지만 dynamic test의 표현력과 능력을 입증하는 역할을 합니다.

The next method is similar to generateRandomNumberOfTests() in terms of flexibility; however, dynamicTestsFromStreamFactoryMethod() generates a stream of dynamic tests from an existing Stream via the DynamicTest.stream() factory method.
>다음 방법은 유연성 측면에서 `generateRandomNumberOfTests()`와 유사합니다. 그러나 `dynamicTestsFromStreamFactoryMethod()`는 `DynamicTest.stream()` factory method를 통해 기존 stream에서 dynamic test stream을 생성합니다.

For demonstration purposes, the dynamicNodeSingleTest() method generates a single DynamicTest instead of a stream, and the dynamicNodeSingleContainer() method generates a nested hierarchy of dynamic tests utilizing DynamicContainer.
>데모를 위해 `dynamicNodeSingleTest()` method는 stream 대신 단일 DynamicTest를 생성하고 `dynamicNodeSingleContainer()` method는 DynamicContainer를 활용하는 dynamic test의 중첩 계층을 생성합니다.

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

2.17.2. URI Test Sources for Dynamic Tests

The JUnit Platform provides TestSource, a representation of the source of a test or container used to navigate to its location by IDEs and build tools.
>JUnit 플랫폼은 IDE 및 빌드 도구에서 해당 위치로 이동하는 데 사용되는 테스트 또는 컨테이너의 소스를 나타내는 `TestSource`를 제공합니다.

The TestSource for a dynamic test or dynamic container can be constructed from a java.net.URI which can be supplied via the DynamicTest.dynamicTest(String, URI, Executable) or DynamicContainer.dynamicContainer(String, URI, Stream) factory method, respectively. The URI will be converted to one of the following TestSource implementations.
>dynamic test 또는 동적 컨테이너에 대한 `TestSource`는 각각` DynamicTest.dynamicTest(String, URI, Executable)` 또는 `DynamicContainer.dynamicContainer(String, URI, Stream)` factory method를 통해 제공될 수 있는 `java.net.URI`에서 구성될 수 있습니다. `URI`는 다음 `TestSource` 구현 중 하나로 변환됩니다.

`ClasspathResourceSource`
If the URI contains the classpath scheme — for example, classpath:/test/foo.xml?line=20,column=2.
>`URI`에 classpath 체계 — 가 포함된 경우(예: `classpath:/test/foo.xml?line=20,column=2`).

`DirectorySource`
If the URI represents a directory present in the file system.
>`URI`가 파일 시스템에 있는 디렉토리를 나타내는 경우.

`FileSource`
If the URI represents a file present in the file system.
>`URI`가 파일 시스템에 있는 파일을 나타내는 경우.

`MethodSource`
If the URI contains the method scheme and the fully qualified method name (FQMN) — for example, method:org.junit.Foo#bar(java.lang.String, java.lang.String[]). Please refer to the Javadoc for DiscoverySelectors.selectMethod(String) for the supported formats for a FQMN.
>`URI`에 메소드 체계와 FQMN(정규화된 메소드 이름) (예: `method:org.junit.Foo#bar(java.lang.String, java.lang.String[])`)이 포함된 경우. FQMN에 지원되는 형식은 `DiscoverySelectors.selectMethod(String)`에 대한 Javadoc을 참조하십시오.

`ClassSource`
If the URI contains the class scheme and the fully qualified class name — for example, class:org.junit.Foo?line=42.
>`URI`에 `클래스` 체계와 정규화된 클래스 이름이 포함된 경우(예: `class:org.junit.Foo?line=42`).

`UriSource`
If none of the above TestSource implementations are applicable.
>위의 `TestSource` 구현이 적용되지 않는 경우.

2.18. Timeouts

The @Timeout annotation allows one to declare that a test, test factory, test template, or lifecycle method should fail if its execution time exceeds a given duration. The time unit for the duration defaults to seconds but is configurable.
>`@Timeout` annotation을 사용하면 실행 시간이 주어진 기간을 초과하는 경우 테스트, 테스트 팩토리, 테스트 템플릿 또는 수명 주기 메소드가 실패해야 한다고 선언할 수 있습니다. 지속 시간의 시간 단위는 기본적으로 초로 설정되지만 구성할 수 있습니다.

The following example shows how @Timeout is applied to lifecycle and test methods.
>다음 예제는 `@Timeout`이 수명 주기 및 테스트 메소드에 어떻게 적용되는지 보여줍니다.

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

Contrary to the assertTimeoutPreemptively() assertion, the execution of the annotated method proceeds in the main thread of the test. If the timeout is exceeded, the main thread is interrupted from another thread. This is done to ensure interoperability with frameworks such as Spring that make use of mechanisms that are sensitive to the currently running thread — for example, ThreadLocal transaction management.
>`assertTimeoutPreemptively()` assertion과 달리 annotation이 달린 메소드의 실행은 테스트의 메인 스레드에서 진행됩니다. 타임아웃이 초과되면 메인 쓰레드는 다른 쓰레드에서 인터럽트된다. 이는 현재 실행 중인 스레드(예: `ThreadLocal` 트랜잭션 관리)에 민감한 메커니즘을 사용하는 Spring과 같은 프레임워크와의 상호 운용성을 보장하기 위해 수행됩니다.

To apply the same timeout to all test methods within a test class and all of its @Nested classes, you can declare the @Timeout annotation at the class level. It will then be applied to all test, test factory, and test template methods within that class and its @Nested classes unless overridden by a @Timeout annotation on a specific method or @Nested class. Please note that @Timeout annotations declared at the class level are not applied to lifecycle methods.
>테스트 클래스 내의 모든 테스트 메소드와 모든 `@Nested` 클래스에 동일한 시간 초과를 적용하려면 클래스 수준에서 `@Timeout` annotation을 선언할 수 있습니다. 그런 다음 특정 메소드 또는 `@Nested` 클래스에 대한 `@Timeout` annotation으로 재정의되지 않는 한 해당 클래스 및 해당 `@Nested` 클래스 내의 모든 테스트, 테스트 팩토리 및 테스트 템플릿 메소드에 적용됩니다. 클래스 수준에서 선언된 `@Timeout` annotation은 수명 주기 메소드에 적용되지 않습니다.

Declaring @Timeout on a @TestFactory method checks that the factory method returns within the specified duration but does not verify the execution time of each individual DynamicTest generated by the factory. Please use assertTimeout() or assertTimeoutPreemptively() for that purpose.
>`@TestFactory` 메소드에서 `@Timeout`을 선언하면 factory method가 지정된 기간 내에 반환되는지 확인하지만 팩토리에서 생성된 각 개별 DynamicTest의 실행 시간은 확인하지 않습니다. 그 목적을 위해 `assertTimeout()` 또는 `assertTimeoutPreemptively()`를 사용하십시오.

If @Timeout is present on a @TestTemplate method — for example, a @RepeatedTest or @ParameterizedTest — each invocation will have the given timeout applied to it.
>`@Timeout`이 `@TestTemplate` 메소드(예: `@RepeatedTest` 또는 `@ParameterizedTest`)에 있는 경우 각 호출에 지정된 시간 제한이 적용됩니다.

The following configuration parameters can be used to specify global timeouts for all methods of a certain category unless they or an enclosing test class is annotated with @Timeout:
>다음 `구성 매개변수(configuration parameters)`를 사용하여 특정 범주의 모든 메소드 또는 둘러싸는 테스트 클래스에 `@Timeout` annotation이 지정되지 않는 한 전역 시간 초과를 지정할 수 있습니다.

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

More specific configuration parameters override less specific ones. For example, junit.jupiter.execution.timeout.test.method.default overrides junit.jupiter.execution.timeout.testable.method.default which overrides junit.jupiter.execution.timeout.default.
>보다 구체적인 구성 매개변수는 덜 구체적인 구성 매개변수보다 우선합니다. 예를 들어, `junit.jupiter.execution.timeout.test.method.default`는 `junit.jupiter.execution.timeout.default`를 재정의하는 `junit.jupiter.execution.timeout.testable.method.default`를 재정의합니다.

The values of such configuration parameters must be in the following, case-insensitive format: `<number>` [ns|μs|ms|s|m|h|d]. The space between the number and the unit may be omitted. Specifying no unit is equivalent to using seconds.
>이러한 구성 매개변수의 값은 대소문자를 구분하지 않는 형식이어야 합니다. `<number> [ns|μs|ms|s|m|h|d]`. 숫자와 단위 사이의 공백은 생략할 수 있습니다. 단위를 지정하지 않는 것은 초를 사용하는 것과 같습니다.

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

2.18.1. Using @Timeout for Polling Tests

When dealing with asynchronous code, it is common to write tests that poll while waiting for something to happen before performing any assertions. In some cases you can rewrite the logic to use a CountDownLatch or another synchronization mechanism, but sometimes that is not possible — for example, if the subject under test sends a message to a channel in an external message broker and assertions cannot be performed until the message has been successfully sent through the channel. Asynchronous tests like these require some form of timeout to ensure they don’t hang the test suite by executing indefinitely, as would be the case if an asynchronous message never gets successfully delivered.
>비동기 코드를 처리할 때 assertion을 수행하기 전에 어떤 일이 일어나기를 기다리는 동안 폴링하는 테스트를 작성하는 것이 일반적입니다. 어떤 경우에는 `CountDownLatch` 또는 다른 동기화 메커니즘을 사용하도록 논리를 다시 작성할 수 있지만 때로는 불가능합니다. 예를 들어 테스트 대상이 외부 메시지 브로커의 채널에 메시지를 보내고 assertion은 메시지가 채널을 통해 성공적으로 전송되었습니다. 이와 같은 비동기식 테스트는 비동기식 메시지가 성공적으로 전달되지 않는 경우와 같이 무기한 실행하여 test suite를 중단하지 않도록 하기 위해 일종의 시간 초과가 필요합니다.

By configuring a timeout for an asynchronous test that polls, you can ensure that the test does not execute indefinitely. The following example demonstrates how to achieve this with JUnit Jupiter’s @Timeout annotation. This technique can be used to implement "poll until" logic very easily.
>폴링하는 비동기 테스트에 대한 시간 초과를 구성하여 테스트가 무기한 실행되지 않도록 할 수 있습니다. 다음 예제는 JUnit Jupiter의 `@Timeout` annotation으로 이를 달성하는 방법을 보여줍니다. 이 기술은 "poll until" 논리를 매우 쉽게 구현하는 데 사용할 수 있습니다.

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

>>If you need more control over polling intervals and greater flexibility with asynchronous tests, consider using a dedicated library such as Awaitility.
>폴링 간격을 더 많이 제어하고 비동기 테스트를 통해 더 큰 유연성이 필요한 경우 `Awaitility`와 같은 전용 라이브러리를 사용하는 것이 좋습니다.

2.18.2. Disable @Timeout Globally

When stepping through your code in a debug session, a fixed timeout limit may influence the result of the test, e.g. mark the test as failed although all assertions were met.
>디버그 세션에서 코드를 단계별로 실행할 때 고정된 시간 제한이 테스트 결과에 영향을 미칠 수 있습니다. 모든 주장이 충족되었지만 테스트를 실패로 표시합니다.

JUnit Jupiter supports the junit.jupiter.execution.timeout.mode configuration parameter to configure when timeouts are applied. There are three modes: enabled, disabled, and disabled_on_debug. The default mode is enabled. A VM runtime is considered to run in debug mode when one of its input parameters starts with -agentlib:jdwp. This heuristic is queried by the disabled_on_debug mode.
>JUnit Jupiter는 제한 시간이 적용될 때 구성하기 위해 `junit.jupiter.execution.timeout.mode `구성 매개변수를 지원합니다. `enabled`, `disabled` 및 `disabled_on_debug`의 세 가지 모드가 있습니다. 기본 모드는 `enabled`입니다. VM 런타임은 입력 매개변수 중 하나가 `-agentlib:jdwp`로 시작하는 경우 debug mode에서 실행되는 것으로 간주됩니다. 이 휴리스틱은 `disabled_on_debug` 모드에서 쿼리됩니다.

2.19. Parallel Execution

>>Parallel test execution is an experimental feature
You’re invited to give it a try and provide feedback to the JUnit team so they can improve and eventually promote this feature.
>*병렬 테스트 실행은 실험적인 기능입니다.*
JUnit 팀이 이 기능을 개선하고 궁극적으로 `홍보`할 수 있도록 시도해 보고 피드백을 제공하도록 초대받았습니다.

By default, JUnit Jupiter tests are run sequentially in a single thread. Running tests in parallel — for example, to speed up execution — is available as an opt-in feature since version 5.3. To enable parallel execution, set the junit.jupiter.execution.parallel.enabled configuration parameter to true — for example, in junit-platform.properties (see Configuration Parameters for other options).
>기본적으로 JUnit Jupiter 테스트는 단일 스레드에서 순차적으로 실행됩니다.   병렬 테스트 실행 —예: 실행 속도 향상 — 은 버전 5.3부터 옵트인 기능으로 사용할 수 있습니다. 병렬 실행을 활성화하려면 `junit.jupiter.execution.parallel.enabled` 구성 매개변수를 `true` — 로 설정합니다(예: junit-platform.properties에서 구성 매개변수 참조)

Please note that enabling this property is only the first step required to execute tests in parallel. If enabled, test classes and methods will still be executed sequentially by default. Whether or not a node in the test tree is executed concurrently is controlled by its execution mode. The following two modes are available.
>이 속성을 활성화하는 것은 테스트를 병렬로 실행하는 데 필요한 첫 번째 단계일 뿐입니다. 활성화된 경우 테스트 클래스 및 method는 기본적으로 계속 순차적으로 실행됩니다. 테스트 트리의 노드가 동시에 실행되는지 여부는 실행 모드에 의해 제어됩니다. 다음 두 가지 모드를 사용할 수 있습니다.

`SAME_THREAD`
Force execution in the same thread used by the parent. For example, when used on a test method, the test method will be executed in the same thread as any @BeforeAll or @AfterAll methods of the containing test class.
>부모가 사용하는 동일한 스레드에서 강제 실행합니다. 예를 들어, 테스트 메소드에서 사용될 때 테스트 메소드는 포함하는 테스트 클래스의 `@BeforeAll` 또는 `@AfterAll` 메소드와 동일한 스레드에서 실행됩니다.

`CONCURRENT`
Execute concurrently unless a resource lock forces execution in the same thread.
>리소스 잠금이 동일한 스레드에서 강제로 실행되지 않는 한 동시에 실행합니다.

By default, nodes in the test tree use the SAME_THREAD execution mode. You can change the default by setting the junit.jupiter.execution.parallel.mode.default configuration parameter. Alternatively, you can use the @Execution annotation to change the execution mode for the annotated element and its subelements (if any) which allows you to activate parallel execution for individual test classes, one by one.
>기본적으로 테스트 트리의 노드는 `SAME_THREAD` 실행 모드를 사용합니다. `junit.jupiter.execution.parallel.mode.default` 구성 매개변수를 설정하여 기본값을 변경할 수 있습니다. 또는 `@Execution` annotation을 사용하여 annotation이 달린 요소와 해당 subelements(가 있는 경우)의 실행 모드를 변경하여 개별 테스트 클래스에 대해 하나씩 병렬 실행을 활성화할 수 있습니다.

*Configuration parameters to execute all tests in parallel*

```
junit.jupiter.execution.parallel.enabled = true
junit.jupiter.execution.parallel.mode.default = concurrent
```

The default execution mode is applied to all nodes of the test tree with a few notable exceptions, namely test classes that use the Lifecycle.PER_CLASS mode or a MethodOrderer (except for MethodOrderer.Random). In the former case, test authors have to ensure that the test class is thread-safe; in the latter, concurrent execution might conflict with the configured execution order. Thus, in both cases, test methods in such test classes are only executed concurrently if the @Execution(CONCURRENT) annotation is present on the test class or method.
>기본 실행 모드는 몇 가지 주목할 만한 예외, 즉 `Lifecycle.PER_CLASS` 모드 또는 [MethodOrderer](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.html) ([MethodOrderer.Random](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/MethodOrderer.Random.html) 제외) 사용하는 테스트 클래스를 제외하고 테스트 트리의 모든 노드에 적용됩니다. 전자의 경우 테스트 작성자는 테스트 클래스가 스레드로부터 안전한지 확인해야 합니다. 후자의 경우 동시 실행이 구성된 실행 순서와 충돌할 수 있습니다. 따라서 두 경우 모두 이러한 테스트 클래스의 테스트 method는 `@Execution(CONCURRENT)` annotation이 테스트 클래스 또는 메소드에 있는 경우에만 동시에 실행됩니다.

All nodes of the test tree that are configured with the CONCURRENT execution mode will be executed fully in parallel according to the provided configuration while observing the declarative synchronization mechanism. Please note that Capturing Standard Output/Error needs to be enabled separately.
>`CONCURRENT` 실행 모드로 구성된 테스트 트리의 모든 노드는 선언적 `동기화(synchronization)` 메커니즘을 관찰하면서 제공된 구성에 따라 완전히 병렬로 실행됩니다. `표준 출력/오류 캡처(Capturing Standard Output/Error)`는 별도로 활성화해야 합니다.

In addition, you can configure the default execution mode for top-level classes by setting the junit.jupiter.execution.parallel.mode.classes.default configuration parameter. By combining both configuration parameters, you can configure classes to run in parallel but their methods in the same thread:
>또한 `junit.jupiter.execution.parallel.mode.classes.default` 구성 매개변수를 설정하여 최상위 클래스에 대한 기본 실행 모드를 구성할 수 있습니다. 두 구성 매개변수를 결합하여 클래스가 병렬로 실행되도록 구성할 수 있지만 해당 method는 동일한 스레드에서 실행됩니다.

*Configuration parameters to execute top-level classes in parallel but methods in same thread*
```
junit.jupiter.execution.parallel.enabled = true
junit.jupiter.execution.parallel.mode.default = same_thread
junit.jupiter.execution.parallel.mode.classes.default = concurrent
```

The opposite combination will run all methods within one class in parallel, but top-level classes will run sequentially:
>반대 조합은 한 클래스 내의 모든 메소드를 병렬로 실행하지만 최상위 클래스는 순차적으로 실행됩니다.

*Configuration parameters to execute top-level classes sequentially but their methods in parallel*

```
junit.jupiter.execution.parallel.enabled = true
junit.jupiter.execution.parallel.mode.default = concurrent
junit.jupiter.execution.parallel.mode.classes.default = same_thread
```

The following diagram illustrates how the execution of two top-level test classes A and B with two test methods per class behaves for all four combinations of junit.jupiter.execution.parallel.mode.default and junit.jupiter.execution.parallel.mode.classes.default (see labels in first column).
>다음 다이어그램은 클래스당 두 개의 테스트 메소드가 있는 두 개의 최상위 테스트 클래스 A와 B의 실행이 `junit.jupiter.execution.parallel.mode.default` 및 `junit.jupiter.execution.parallel.mode.classes.default`의 네 가지 조합 모두에 대해 어떻게 작동하는지 보여줍니다. (첫 번째 열의 레이블 참조).

![junit5_guide_writing-tests_execution_mode](../images/junit5_guide_writing-tests_execution_mode.svg)

*Default execution mode configuration combinations*
>*기본 실행 모드 구성 조합*

If the junit.jupiter.execution.parallel.mode.classes.default configuration parameter is not explicitly set, the value for junit.jupiter.execution.parallel.mode.default will be used instead.
>`junit.jupiter.execution.parallel.mode.classes.default` 구성 매개변수가 명시적으로 설정되지 않은 경우 `junit.jupiter.execution.parallel.mode.default` 값이 대신 사용됩니다.

2.19.1. Configuration

Properties such as the desired parallelism and the maximum pool size can be configured using a ParallelExecutionConfigurationStrategy. The JUnit Platform provides two implementations out of the box: dynamic and fixed. Alternatively, you may implement a custom strategy.
>`ParallelExecutionConfigurationStrategy`를 사용하여 원하는 병렬 처리 및 최대 풀 크기와 같은 속성을 구성할 수 있습니다. JUnit 플랫폼은 기본적으로 동적 및 고정의 두 가지 구현을 제공합니다. 또는 사용자 지정 전략을 구현할 수 있습니다.

To select a strategy, set the junit.jupiter.execution.parallel.config.strategy configuration parameter to one of the following options.
>전략을 선택하려면 `junit.jupiter.execution.parallel.config.strategy` 구성 매개변수를 다음 옵션 중 하나로 설정합니다.

`dynamic`
Computes the desired parallelism based on the number of available processors/cores multiplied by the junit.jupiter.execution.parallel.config.dynamic.factor configuration parameter (defaults to 1).

`fixed`
Uses the mandatory junit.jupiter.execution.parallel.config.fixed.parallelism configuration parameter as the desired parallelism.

`custom`
Allows you to specify a custom ParallelExecutionConfigurationStrategy implementation via the mandatory junit.jupiter.execution.parallel.config.custom.class configuration parameter to determine the desired configuration.

>`dynamic`
&nbsp;&nbsp;&nbsp;사용 가능한 프로세서/코어 수에 `junit.jupiter.execution.parallel.config.dynamic.factor` 구성 매개변수를 곱하여 원하는 병렬도를 계산합니다(기본값은 `1`).
>`fixed`
&nbsp;&nbsp;&nbsp;필수 `junit.jupiter.execution.parallel.config.fixed.parallelism` 구성 매개변수를 원하는 병렬 처리로 사용합니다.
>`custom`
&nbsp;&nbsp;&nbsp;필수 `junit.jupiter.execution.parallel.config.custom.class` 구성 매개변수를 통해 사용자 지정 `ParallelExecutionConfigurationStrategy` 구현을 지정하여 원하는 구성을 결정할 수 있습니다.

If no configuration strategy is set, JUnit Jupiter uses the dynamic configuration strategy with a factor of 1. Consequently, the desired parallelism will be equal to the number of available processors/cores.
>구성 전략이 설정되지 않은 경우 JUnit Jupiter는 인수 `1`로 동적 구성 전략을 사용합니다. 결과적으로 원하는 병렬 처리는 사용 가능한 프로세서/코어 수와 동일합니다.

>>Parallelism does not imply maximum number of concurrent threads
>*병렬 처리는 최대 동시 스레드 수를 의미하지 않습니다.*
>>JUnit Jupiter does not guarantee that the number of concurrently executing tests will not exceed the configured parallelism. For example, when using one of the synchronization mechanisms described in the next section, the ForkJoinPool that is used behind the scenes may spawn additional threads to ensure execution continues with sufficient parallelism. Thus, if you require such guarantees in a test class, please use your own means of controlling concurrency.
>JUnit Jupiter는 동시에 실행되는 테스트 수가 구성된 병렬 처리를 초과하지 않을 것이라고 보장하지 않습니다. 예를 들어, 다음 섹션에서 설명하는 동기화 메커니즘 중 하나를 사용할 때 장면 뒤에서 사용되는 `ForkJoinPool`은 실행이 충분한 병렬 처리로 계속되도록 하기 위해 추가 스레드를 생성할 수 있습니다. 따라서 테스트 클래스에서 이러한 보장이 필요한 경우 동시성을 제어하는 자체 수단을 사용하십시오.

2.19.2. Synchronization

In addition to controlling the execution mode using the @Execution annotation, JUnit Jupiter provides another annotation-based declarative synchronization mechanism. The @ResourceLock annotation allows you to declare that a test class or method uses a specific shared resource that requires synchronized access to ensure reliable test execution. The shared resource is identified by a unique name which is a String. The name can be user-defined or one of the predefined constants in Resources: SYSTEM_PROPERTIES, SYSTEM_OUT, SYSTEM_ERR, LOCALE, or TIME_ZONE.
>`@Execution` annotation을 사용하여 실행 모드를 제어하는 것 외에도 JUnit Jupiter는 또 다른 annotation 기반 선언적 동기화 메커니즘을 제공합니다. `@ResourceLock` annotation을 사용하면 테스트 클래스 또는 메소드가 안정적인 테스트 실행을 보장하기 위해 동기화된 액세스가 필요한 특정 공유 리소스를 사용한다고 선언할 수 있습니다. 공유 리소스는 문자열인 고유한 이름으로 식별됩니다. 이름은 사용자 정의이거나 [리소스](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/parallel/Resources.html)에 있는 사전 정의된 상수 중 하나일 수 있습니다. `SYSTEM_PROPERTIES`, `SYSTEM_OUT`, `SYSTEM_ERR`, `LOCALE` 또는 `TIME_ZONE`.

If the tests in the following example were run in parallel without the use of @ResourceLock, they would be flaky. Sometimes they would pass, and at other times they would fail due to the inherent race condition of writing and then reading the same JVM System Property.
>다음 예제의 테스트가 `@ResourceLock`을 사용하지 않고 병렬로 실행되면 불안정합니다. 때로는 통과하고 다른 때는 동일한 JVM 시스템 속성을 쓰고 읽는 고유한 경쟁 조건으로 인해 실패합니다.

When access to shared resources is declared using the @ResourceLock annotation, the JUnit Jupiter engine uses this information to ensure that no conflicting tests are run in parallel.
>`@ResourceLock` annotation을 사용하여 공유 리소스에 대한 액세스가 선언되면 JUnit Jupiter 엔진은 이 정보를 사용하여 충돌하는 테스트가 병렬로 실행되지 않도록 합니다.

>>*Running tests in isolation*
>*격리된 테스트 실행*
>>If most of your test classes can be run in parallel without any synchronization but you have some test classes that need to run in isolation, you can mark the latter with the @Isolated annotation. Tests in such classes are executed sequentially without any other tests running at the same time.
>대부분의 테스트 클래스를 동기화 없이 병렬로 실행할 수 있지만 일부 테스트 클래스를 격리하여 실행해야 하는 경우 후자를 `@Isolated` annotation으로 표시할 수 있습니다. 이러한 클래스의 테스트는 동시에 실행되는 다른 테스트 없이 순차적으로 실행됩니다.

In addition to the String that uniquely identifies the shared resource, you may specify an access mode. Two tests that require READ access to a shared resource may run in parallel with each other but not while any other test that requires READ_WRITE access to the same shared resource is running.
>공유 리소스를 고유하게 식별하는 문자열 외에도 액세스 모드를 지정할 수 있습니다. 공유 리소스에 대한 `READ` 액세스가 필요한 두 테스트는 서로 병렬로 실행될 수 있지만 동일한 공유 리소스에 대한 `READ_WRITE` 액세스가 필요한 다른 테스트가 실행되는 동안에는 실행되지 않습니다.

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

2.20. Built-in Extensions

While the JUnit team encourages reusable extensions to be packaged and maintained in separate libraries, the JUnit Jupiter API artifact includes a few user-facing extension implementations that are considered so generally useful that users shouldn’t have to add another dependency.
>JUnit 팀은 재사용 가능한 확장을 별도의 라이브러리에 패키징하고 유지하도록 권장하지만 JUnit Jupiter API artifact는 사용자가 다른 종속성을 추가할 필요가 없어야 하는 일반적으로 유용한 것으로 간주되는 몇 가지 사용자 대면 확장 구현을 포함합니다.

2.20.1. The TempDirectory Extension

>>`@TempDir`* is an experimental feature*
You’re invited to give it a try and provide feedback to the JUnit team so they can improve and eventually promote this feature.
>`@TempDir`*은 실험적인 기능입니다.*
JUnit 팀이 이 기능을 개선하고 궁극적으로 *홍보*할 수 있도록 시도해 보고 피드백을 제공하도록 초대받았습니다.

The built-in TempDirectory extension is used to create and clean up a temporary directory for an individual test or all tests in a test class. It is registered by default. To use it, annotate a field of type java.nio.file.Path or java.io.File with @TempDir or add a parameter of type java.nio.file.Path or java.io.File annotated with @TempDir to a lifecycle method or test method.
>기본 제공 `TempDirectory` 확장은 개별 테스트 또는 테스트 클래스의 모든 테스트를 위한 임시 디렉터리를 만들고 정리하는 데 사용됩니다. 기본적으로 등록되어 있습니다. 그것을 사용하려면 `@TempDir`로 `java.nio.file.Path` 또는 `java.io.File` 유형의 필드에 annotation을 추가하거나 `@TempDir`로 annotation이 달린 `java.nio.file.Path` 또는 `java.io.File` 유형의 매개변수를 추가하십시오. 수명 주기 방법 또는 테스트 방법.

For example, the following test declares a parameter annotated with @TempDir for a single test method, creates and writes to a file in the temporary directory, and checks its content.
>예를 들어 다음 테스트는 단일 테스트 메소드에 대해 `@TempDir` annotation이 달린 매개변수를 선언하고 임시 디렉토리에 파일을 생성 및 기록하고 그 내용을 확인합니다.

*A test method that requires a temporary directory*

```java
@Test
void writeItemsToFile(@TempDir Path tempDir) throws IOException {
    Path file = tempDir.resolve("test.txt");

    new ListWriter(file).write("a", "b", "c");

    assertEquals(singletonList("a,b,c"), Files.readAllLines(file));
}
```

You can inject multiple temporary directories by specifying multiple annotated parameters.
>annotation이 달린 여러 매개변수를 지정하여 여러 임시 디렉토리를 삽입할 수 있습니다.

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

>>To revert to the old behavior of using a single temporary directory for the entire test class or method (depending on which level the annotation is used), you can set the junit.jupiter.tempdir.scope configuration parameter to per_context. However, please note that this option is deprecated and will be removed in a future release.
>전체 테스트 클래스 또는 메소드에 대해 단일 임시 디렉터리를 사용하는 이전 동작으로 되돌리려면(어노테이션이 사용된 수준에 따라 다름) `junit.jupiter.tempdir.scope` 구성 매개변수를 `per_context`로 설정할 수 있습니다. 그러나 이 옵션은 더 이상 사용되지 않으며 향후 릴리스에서 제거될 예정입니다.

@TempDir is not supported on constructor parameters. If you wish to retain a single reference to a temp directory across lifecycle methods and the current test method, please use field injection by annotating an instance field with @TempDir.
>`@TempDir`은 생성자 매개변수에서 지원되지 않습니다. 수명 주기 메소드와 현재 테스트 메소드에서 임시 디렉터리에 대한 단일 참조를 유지하려면 인스턴스 필드에 `@TempDir` annotation을 추가하여 필드 주입을 사용하세요.

The following example stores a shared temporary directory in a static field. This allows the same sharedTempDir to be used in all lifecycle methods and test methods of the test class. For better isolation, you should use an instance field so that each test method uses a separate directory.
>다음 예는 공유 임시 디렉토리를 정적 필드에 저장합니다. 이를 통해 테스트 클래스의 모든 수명 주기 메소드 및 테스트 메소드에서 동일한 `sharedTempDir`을 사용할 수 있습니다. 더 나은 격리를 위해 각 테스트 메소드가 별도의 디렉터리를 사용하도록 인스턴스 필드를 사용해야 합니다.

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
