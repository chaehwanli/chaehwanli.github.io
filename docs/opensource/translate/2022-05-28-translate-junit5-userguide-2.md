---
layout : post
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
|Overview||XX|
|Writing Tests||XX|
|Migrating from JUnit 4||3|
|Running Tests||3|
|Extension Model||3|
|Advanced Topics||3|
|API Evolution||3|
|Contributors||3|
|OverRelease Notesview||3|
|Appendix||3|

3. Migrating from JUnit 4

Although the JUnit Jupiter programming model and extension model will not support JUnit 4 features such as Rules and Runners natively, it is not expected that source code maintainers will need to update all of their existing tests, test extensions, and custom build test infrastructure to migrate to JUnit Jupiter.
>JUnit Jupiter 프로그래밍 모델 및 확장 모델은 기본적으로 규칙 및 Runner와 같은 JUnit 4 기능을 지원하지 않지만 소스 코드 유지 관리자는 migration을 위해 기존 테스트, 테스트 확장 및 사용자 정의 빌드 테스트 인프라를 모두 업데이트해야 할 것으로 예상되지 않습니다. JUnit 목성에.

Instead, JUnit provides a gentle migration path via a JUnit Vintage test engine which allows existing tests based on JUnit 3 and JUnit 4 to be executed using the JUnit Platform infrastructure. Since all classes and annotations specific to JUnit Jupiter reside under a new org.junit.jupiter base package, having both JUnit 4 and JUnit Jupiter in the classpath does not lead to any conflicts. It is therefore safe to maintain existing JUnit 4 tests alongside JUnit Jupiter tests. Furthermore, since the JUnit team will continue to provide maintenance and bug fix releases for the JUnit 4.x baseline, developers have plenty of time to migrate to JUnit Jupiter on their own schedule.
>대신 JUnit은 JUnit 3 및 JUnit 4를 기반으로 하는 기존 테스트가 JUnit 플랫폼 인프라를 사용하여 실행될 수 있도록 하는 JUnit Vintage 테스트 엔진을 통해 부드러운 migration 경로를 제공합니다. JUnit Jupiter와 관련된 모든 클래스와 annotation은 새로운 `org.junit.jupiter` 기본 패키지 아래에 있으므로 클래스 경로에 JUnit 4와 JUnit Jupiter가 모두 있으면 충돌이 발생하지 않습니다. 따라서 JUnit Jupiter 테스트와 함께 기존 JUnit 4 테스트를 유지하는 것이 안전합니다. 또한 JUnit 팀이 JUnit 4.x 기준선에 대한 유지 관리 및 버그 수정 릴리스를 계속 제공할 것이기 때문에 개발자는 자신의 일정에 따라 JUnit Jupiter로 migration할 충분한 시간이 있습니다.

3.1. Running JUnit 4 Tests on the JUnit Platform

Just make sure that the junit-vintage-engine artifact is in your test runtime path. In that case JUnit 3 and JUnit 4 tests will automatically be picked up by the JUnit Platform launcher.
>`junit-vintage-engine` artifact가 테스트 런타임 경로에 있는지 확인하십시오. 이 경우 JUnit 3 및 JUnit 4 테스트는 JUnit 플랫폼 런처에 의해 자동으로 선택됩니다.

See the example projects in the junit5-samples repository to find out how this is done with Gradle and Maven.
>[junit5-samples](https://github.com/junit-team/junit5-samples) repository의 예제 프로젝트를 참조하여 Gradle 및 Maven으로 이 작업을 수행하는 방법을 알아보세요.

3.1.1. Categories Support

For test classes or methods that are annotated with @Category, the JUnit Vintage test engine exposes the category’s fully qualified class name as a tag of the corresponding test identifier. For example, if a test method is annotated with @Category(Example.class), it will be tagged with "com.acme.Example". Similar to the Categories runner in JUnit 4, this information can be used to filter the discovered tests before executing them (see Running Tests for details).
>`@Category`로 annotation이 달린 테스트 클래스 또는 메소드의 경우 JUnit Vintage 테스트 엔진은 카테고리의 정규화된 클래스 이름을 해당 테스트 식별자의 `태그`로 노출합니다. 예를 들어 테스트 메소드에 `@Category(Example.class)` annotation이 달린 경우 `"com.acme.Example`"로 태그가 지정됩니다. JUnit 4의 카테고리 runner와 유사하게 이 정보는 검색된 테스트를 실행하기 전에 필터링하는 데 사용할 수 있습니다(자세한 내용은 `테스트 실행(Runnig Test)` 참조).

3.2. Migration Tips

The following are topics that you should be aware of when migrating existing JUnit 4 tests to JUnit Jupiter.
>다음은 기존 JUnit 4 테스트를 JUnit Jupiter로 migration할 때 알고 있어야 하는 주제입니다.

- Annotations reside in the org.junit.jupiter.api package.
- Assertions reside in org.junit.jupiter.api.Assertions.
    - Note that you may continue to use assertion methods from org.junit.Assert or any other assertion library such as AssertJ, Hamcrest, Truth, etc.
- Assumptions reside in org.junit.jupiter.api.Assumptions.
    - Note that JUnit Jupiter 5.4 and later versions support methods from JUnit 4’s org.junit.Assume class for assumptions. Specifically, JUnit Jupiter supports JUnit 4’s AssumptionViolatedException to signal that a test should be aborted instead of marked as a failure.
- @Before and @After no longer exist; use @BeforeEach and @AfterEach instead.
- @BeforeClass and @AfterClass no longer exist; use @BeforeAll and @AfterAll instead.
- @Ignore no longer exists: use @Disabled or one of the other built-in execution conditions instead
    - See also JUnit 4 @Ignore Support.
- @Category no longer exists; use @Tag instead.
- @RunWith no longer exists; superseded by @ExtendWith.
- @Rule and @ClassRule no longer exist; superseded by @ExtendWith and @RegisterExtension
    - See also Limited JUnit 4 Rule Support.

>- Annotation은 `org.junit.jupiter.api` 패키지에 있습니다.
>- Assertion은 `org.junit.jupiter.api.Assertions`에 있습니다.
>   - `org.junit.Assert` 또는 [AssertJ](https://joel-costigliola.github.io/assertj/), [Hamcrest](https://hamcrest.org/JavaHamcrest/), [Truth](https://truth.dev/) 등과 같은 다른 주장 라이브러리의 주장 방법을 계속 사용할 수 있습니다.
>- Assumption은 `org.junit.jupiter.api.Assumptions`에 있습니다.
>   - JUnit Jupiter 5.4 이상 버전은 가정을 위해 JUnit 4의 `org.junit.Assume` 클래스에서 메소드를 지원합니다. 특히 JUnit Jupiter는 JUnit 4의 `AssumptionViolatedException`을 지원하여 테스트가 실패로 표시되는 대신 중단되어야 함을 알립니다.
>- `@Before` 및 `@After`는 더 이상 존재하지 않습니다. 대신 `@BeforeEac`h 및 `@AfterEach`를 사용하십시오.
>- `@BeforeClass` 및 `@AfterClass`는 더 이상 존재하지 않습니다. 대신 `@BeforeAll` 및 `@AfterAll`을 사용하십시오.
>- `@Ignore`가 더 이상 존재하지 않음: 대신 `@Disable`d 또는 다른 built-in 실행 조건 중 하나를 사용하십시오.
>   - `JUnit 4 @Ignore 지원(JUnit 4 @Ignore Support)`도 참조하십시오.
>- `@Category`는 더 이상 존재하지 않습니다. 대신 `@Tag`를 사용하십시오.
>- `@RunWith`는 더 이상 존재하지 않습니다. `@ExtendWith`로 대체되었습니다.
>- `@Rule` 및 `@ClassRule`은 더 이상 존재하지 않습니다. `@ExtendWith` 및 `@RegisterExtension`으로 대체됨 
>   - `Limited JUnit 4 Rule Support` 참조하세요.

3.3. Limited JUnit 4 Rule Support

As stated above, JUnit Jupiter does not and will not support JUnit 4 rules natively. The JUnit team realizes, however, that many organizations, especially large ones, are likely to have large JUnit 4 code bases that make use of custom rules. To serve these organizations and enable a gradual migration path the JUnit team has decided to support a selection of JUnit 4 rules verbatim within JUnit Jupiter. This support is based on adapters and is limited to those rules that are semantically compatible to the JUnit Jupiter extension model, i.e. those that do not completely change the overall execution flow of the test.
>위에서 언급했듯이 JUnit Jupiter는 기본적으로 JUnit 4 규칙을 지원하지 않으며 지원하지도 않습니다. 그러나 JUnit 팀은 많은 조직, 특히 대규모 조직이 사용자 지정 규칙을 사용하는 대규모 JUnit 4 코드 기반을 가질 가능성이 높다는 것을 알고 있습니다. 이러한 조직에 서비스를 제공하고 점진적인 migration 경로를 활성화하기 위해 JUnit 팀은 JUnit Jupiter 내에서 JUnit 4 규칙을 그대로 지원하기로 결정했습니다. 이 지원은 어댑터를 기반으로 하며 JUnit Jupiter 확장 모델과 의미적으로 호환되는 규칙, 즉 테스트의 전체 실행 흐름을 완전히 변경하지 않는 규칙으로 제한됩니다.

The junit-jupiter-migrationsupport module from JUnit Jupiter currently supports the following three Rule types including subclasses of these types:
>JUnit Jupiter의 `junit-jupiter-migrationsupport` 모듈은 현재 이러한 유형의 subclasses를 포함하여 다음 세 가지 규칙 유형을 지원합니다.

- `org.junit.rules.ExternalResource` (including `org.junit.rules.TemporaryFolder`)
- `org.junit.rules.Verifier` (including `org.junit.rules.ErrorCollector`)
- `org.junit.rules.ExpectedException`

As in JUnit 4, Rule-annotated fields as well as methods are supported. By using these class-level extensions on a test class such Rule implementations in legacy code bases can be left unchanged including the JUnit 4 rule import statements.
>JUnit 4에서와 같이 규칙 annotation 필드와 메소드가 지원됩니다. 테스트 클래스에서 이러한 클래스 수준 확장을 사용하면 JUnit 4 규칙 가져오기 문을 포함하여 레거시 코드 기반의 `규칙` 구현을 변경하지 않은 상태로 둘 수 있습니다.

This limited form of Rule support can be switched on by the class-level annotation @EnableRuleMigrationSupport. This annotation is a composed annotation which enables all rule migration support extensions: VerifierSupport, ExternalResourceSupport, and ExpectedExceptionSupport. You may alternatively choose to annotate your test class with @EnableJUnit4MigrationSupport which registers migration support for rules and JUnit 4’s @Ignore annotation (see JUnit 4 @Ignore Support).
>이 제한된 형태의 규칙 지원은 클래스 수준 annotation [@EnableRuleMigrationSupport](https://junit.org/junit5/docs/current/api/org.junit.jupiter.migrationsupport/org/junit/jupiter/migrationsupport/rules/EnableRuleMigrationSupport.html)에 의해 켜질 수 있습니다. 이 annotation은 모든 규칙 migration 지원 확장인 `VerifierSupport`, `ExternalResourceSupport` 및 `ExpectedExceptionSupport`를 활성화하는 구성된 annotation입니다. 또는 규칙 및 JUnit 4의 `@Ignore` annotation에 대한 migration 지원을 등록하는 `@EnableJUnit4MigrationSupport`로 테스트 클래스에 annotation을 달도록 선택할 수 있습니다(`JUnit 4 @Ignore 지원` 참조).

However, if you intend to develop a new extension for JUnit 5 please use the new extension model of JUnit Jupiter instead of the rule-based model of JUnit 4.
>그러나 JUnit 5에 대한 새 확장을 개발하려는 경우 JUnit 4의 규칙 기반 모델 대신 JUnit Jupiter의 새 확장 모델을 사용하십시오.

3.4. JUnit 4 @Ignore Support

In order to provide a smooth migration path from JUnit 4 to JUnit Jupiter, the junit-jupiter-migrationsupport module provides support for JUnit 4’s @Ignore annotation analogous to Jupiter’s @Disabled annotation.
>JUnit 4에서 JUnit Jupiter로의 원활한 migration 경로를 제공하기 위해 `junit-jupiter-migrationsupport` 모듈은 Jupiter의 [@Disabled](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Disabled.html) annotation과 유사한 JUnit 4의 `@Ignore` annotation에 대한 지원을 제공합니다.

To use @Ignore with JUnit Jupiter based tests, configure a test dependency on the junit-jupiter-migrationsupport module in your build and then annotate your test class with @ExtendWith(IgnoreCondition.class) or @EnableJUnit4MigrationSupport (which automatically registers the IgnoreCondition along with Limited JUnit 4 Rule Support). The IgnoreCondition is an ExecutionCondition that disables test classes or test methods that are annotated with @Ignore.
>JUnit Jupiter 기반 테스트와 함께 `@Ignore`를 사용하려면 빌드에서 junit-jupiter-migrationsupport 모듈에 대한 테스트 종속성을 구성한 다음 테스트 클래스에 `@ExtendWith(IgnoreCondition.class)` 또는 `@EnableJUnit4MigrationSupport`(이는 `IgnoreCondition`을 다음과 함께 자동으로 등록합니다. `제한된 JUnit 4 규칙 지원(Limited JUnit 4 Rule Support)`). `IgnoreCondition`은 `@Ignore` annotation이 달린 테스트 클래스 또는 테스트 메소드를 비활성화하는 []ExecutionCondition](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ExecutionCondition.html)입니다.

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

4. Running Tests

4.1. IDE Support

4.1.1. IntelliJ IDEA

IntelliJ IDEA supports running tests on the JUnit Platform since version 2016.2. For details please see the post on the IntelliJ IDEA blog. Note, however, that it is recommended to use IDEA 2017.3 or newer since these newer versions of IDEA will download the following JARs automatically based on the API version used in the project: junit-platform-launcher, junit-jupiter-engine, and junit-vintage-engine.
>IntelliJ IDEA는 버전 2016.2부터 JUnit 플랫폼에서 테스트 실행을 지원합니다. 자세한 내용은 [IntelliJ IDEA 블로그의 게시물](https://blog.jetbrains.com/idea/2016/08/using-junit-5-in-intellij-idea/)을 참조하세요. 그러나 이러한 최신 버전의 IDEA는 프로젝트에 사용된 API 버전을 기반으로 다음 JAR을 자동으로 다운로드하므로 IDEA 2017.3 이상을 사용하는 것이 좋습니다. `junit-platform-launcher`, `junit-jupiter-engine` 및 `junit-vintage-engine`.

>>IntelliJ IDEA releases prior to IDEA 2017.3 bundle specific versions of JUnit 5. Thus, if you want to use a newer version of JUnit Jupiter, execution of tests within the IDE might fail due to version conflicts. In such cases, please follow the instructions below to use a newer version of JUnit 5 than the one bundled with IntelliJ IDEA.
>IDEA 2017.3 이전 IntelliJ IDEA 릴리스는 JUnit 5의 특정 버전을 번들합니다. 따라서 JUnit Jupiter의 최신 버전을 사용하려는 경우 버전 충돌로 인해 IDE 내에서 테스트 실행이 실패할 수 있습니다. 이러한 경우 IntelliJ IDEA와 함께 번들로 제공되는 JUnit 5보다 최신 버전의 JUnit 5를 사용하려면 아래 지침을 따르십시오.

In order to use a different JUnit 5 version (e.g., 5.8.2), you may need to include the corresponding versions of the junit-platform-launcher, junit-jupiter-engine, and junit-vintage-engine JARs in the classpath.
>다른 JUnit 5 버전(예: 5.8.2)을 사용하려면 해당 버전의 `junit-platform-launcher`, `junit-jupiter-engine` 및 `junit-vintage-engine` JAR을 클래스 경로에 포함해야 할 수 있습니다.

*Additional Gradle Dependencies*

```
testImplementation(platform("org.junit:junit-bom:5.8.2"))
testRuntimeOnly("org.junit.platform:junit-platform-launcher") {
  because("Only needed to run tests in a version of IntelliJ IDEA that bundles older versions")
}
testRuntimeOnly("org.junit.jupiter:junit-jupiter-engine")
testRuntimeOnly("org.junit.vintage:junit-vintage-engine")
```

*Additional Maven Dependencies*
```
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

4.1.2. Eclipse

Eclipse IDE offers support for the JUnit Platform since the Eclipse Oxygen.1a (4.7.1a) release.
>Eclipse IDE는 Eclipse Oxygen.1a(4.7.1a) 릴리스부터 JUnit 플랫폼에 대한 지원을 제공합니다.

For more information on using JUnit 5 in Eclipse consult the official Eclipse support for JUnit 5 section of the Eclipse Project Oxygen.1a (4.7.1a) - New and Noteworthy documentation.
>Eclipse에서 JUnit 5를 사용하는 방법에 대한 자세한 내용은 [Eclipse Project Oxygen.1a(4.7.1a) - 새롭고 주목할만한(New and Noteworthy)](https://www.eclipse.org/eclipse/news/4.7.1a/#junit-5-support)문서의 JUnit 5에 대한 공식 Eclipse 지원 섹션을 참조하세요.

4.1.3. NetBeans

NetBeans offers support for JUnit Jupiter and the JUnit Platform since the Apache NetBeans 10.0 release.
>NetBeans는 [Apache NetBeans 10.0 릴리스](https://netbeans.apache.org/download/nb100/nb100.html) 이후 JUnit Jupiter 및 JUnit 플랫폼에 대한 지원을 제공합니다.

For more information consult the JUnit 5 section of the Apache NetBeans 10.0 release notes.
>자세한 내용은 [Apache NetBeans 10.0 릴리스 노트](https://netbeans.apache.org/download/nb100/index.html#_junit_5)의 JUnit 5 섹션을 참조하십시오.

4.1.4. Visual Studio Code

Visual Studio Code supports JUnit Jupiter and the JUnit Platform via the Java Test Runner extension which is installed by default as part of the Java Extension Pack.
>[Visual Studio Code](https://code.visualstudio.com/)는 기본적으로 [Java Extension Pack](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)의 일부로 설치되는 [Java Test Runner](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-test) 확장을 통해 JUnit Jupiter 및 JUnit 플랫폼을 지원합니다.

For more information consult the Testing section of the Java in Visual Studio Code documentation.
>자세한 내용은 [Visual Studio Code 설명서에서 Java의 테스트 섹션](https://code.visualstudio.com/docs/languages/java#_testing)을 참조하세요.

4.1.5. Other IDEs

If you are using an editor or IDE other than one of those listed in the previous sections, the JUnit team provides two alternative solutions to assist you in using JUnit 5. You can use the Console Launcher manually — for example, from the command line — or execute tests with a JUnit 4 based Runner if your IDE has built-in support for JUnit 4.
>이전 섹션에 나열된 것 이외의 편집기 또는 IDE를 사용하는 경우 JUnit 팀은 JUnit 5 사용을 지원하는 두 가지 대체 솔루션을 제공합니다. `콘솔 시작 관리자`를 수동으로 사용할 수 있습니다. 또는 IDE에 JUnit 4 지원 기능이 내장된 경우 `JUnit 4 기반 Runner`로 테스트를 실행하십시오.

4.2. Build Support

4.2.1. Gradle

The JUnit Platform Gradle Plugin has been discontinued
The junit-platform-gradle-plugin developed by the JUnit team was deprecated in JUnit Platform 1.2 and discontinued in 1.3. Please switch to Gradle’s standard test task.
>*JUnit 플랫폼 Gradle 플러그인이 중단되었습니다.*
JUnit 팀에서 개발한 `junit-platform-gradle-plugin`은 JUnit 플랫폼 1.2에서 더 이상 사용되지 않으며 1.3에서 중단되었습니다. Gradle의 표준 *테스트* 작업으로 전환하십시오.

Starting with version 4.6, Gradle provides native support for executing tests on the JUnit Platform. To enable it, you just need to specify useJUnitPlatform() within a test task declaration in build.gradle:
>[버전 4.6](https://docs.gradle.org/4.6/release-notes.html)부터 Gradle은 JUnit 플랫폼에서 테스트를 실행하기 위한 [기본 지원(native support)](https://docs.gradle.org/current/userguide/java_testing.html#using_junit5)을 제공합니다. 활성화하려면 `build.gradle`의 `test` 작업 선언 내에서 `useJUnitPlatform()`을 지정하기만 하면 됩니다.

```groovy
test {
    useJUnitPlatform()
}
```

Filtering by tags, tag expressions, or engines is also supported:
>`tags`, `tag expresskons` 또는 엔진별 필터링도 지원됩니다.

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

Please refer to the official Gradle documentation for a comprehensive list of options.
>전체 옵션 목록은 [공식 Gradle 문서](https://docs.gradle.org/current/userguide/java_plugin.html#sec:java_test)를 참조하세요.

Configuration Parameters

The standard Gradle test task currently does not provide a dedicated DSL to set JUnit Platform configuration parameters to influence test discovery and execution. However, you can provide configuration parameters within the build script via system properties (as shown below) or via the junit-platform.properties file.
>표준 Gradle 테스트 작업은 현재 테스트 검색 및 실행에 영향을 미치도록 JUnit 플랫폼 `구성 매개변수`를 설정하는 전용 DSL을 제공하지 않습니다. 그러나 시스템 속성(아래 참조) 또는 `junit-platform.properties` 파일을 통해 빌드 스크립트 내에서 구성 매개변수를 제공할 수 있습니다.

```groovy
test {
    // ...
    systemProperty("junit.jupiter.conditions.deactivate", "*")
    systemProperty("junit.jupiter.extensions.autodetection.enabled", true)
    systemProperty("junit.jupiter.testinstance.lifecycle.default", "per_class")
    // ...
}
```

Configuring Test Engines

In order to run any tests at all, a TestEngine implementation must be on the classpath.
>테스트를 전혀 실행하려면 `TestEngine` 구현이 클래스 경로에 있어야 합니다.

To configure support for JUnit Jupiter based tests, configure a testImplementation dependency on the dependency-aggregating JUnit Jupiter artifact similar to the following.
>
JUnit Jupiter 기반 테스트에 대한 지원을 구성하려면 다음과 유사한 종속성 집계 JUnit Jupiter artifact에 대한 `testImplementation` 종속성을 구성하십시오.

```groovy
dependencies {
    testImplementation("org.junit.jupiter:junit-jupiter:5.8.2")
}
```

The JUnit Platform can run JUnit 4 based tests as long as you configure a testImplementation dependency on JUnit 4 and a testRuntimeOnly dependency on the JUnit Vintage TestEngine implementation similar to the following.
>JUnit 플랫폼은 JUnit 4에 대한 `testImplementation` 종속성과 다음과 유사한 JUnit Vintage `TestEngine` 구현에 대한 `testRuntimeOnly` 종속성을 구성하는 한 JUnit 4 기반 테스트를 실행할 수 있습니다.

```groovy
dependencies {
    testImplementation("junit:junit:4.13.2")
    testRuntimeOnly("org.junit.vintage:junit-vintage-engine:5.8.2")
}
```

Configuring Logging (optional)

JUnit uses the Java Logging APIs in the java.util.logging package (a.k.a. JUL) to emit warnings and debug information. Please refer to the official documentation of LogManager for configuration options.
>JUnit은 경고 및 디버그 정보를 생성하기 위해 `java.util.loggin`g 패키지(JUL)의 Java 로깅 API를 사용합니다. 구성 옵션은 [LogManager](https://docs.oracle.com/javase/8/docs/api/java/util/logging/LogManager.html)의 공식 문서를 참조하십시오.

Alternatively, it’s possible to redirect log messages to other logging frameworks such as Log4j or Logback. To use a logging framework that provides a custom implementation of LogManager, set the java.util.logging.manager system property to the fully qualified class name of the LogManager implementation to use. The example below demonstrates how to configure Log4j 2.x (see Log4j JDK Logging Adapter for details).
>또는 [Log4j](https://logging.apache.org/log4j/2.x/) 또는 [Logback](https://logback.qos.ch/)과 같은 다른 로깅 프레임워크로 로그 메시지를 리디렉션할 수 있습니다. [LogManager](https://docs.oracle.com/javase/8/docs/api/java/util/logging/LogManager.html)의 사용자 정의 구현을 제공하는 로깅 프레임워크를 사용하려면 `java.util.logging.manager` 시스템 속성을 사용할 [LogManager](https://docs.oracle.com/javase/8/docs/api/java/util/logging/LogManager.html) 구현의 정규화된 클래스 이름으로 설정하십시오. 아래 예는 Log4j 2.x를 구성하는 방법을 보여줍니다(자세한 내용은 [Log4j JDK 로깅 어댑터(Log4j JDK Logging Adapter)](https://logging.apache.org/log4j/2.x/log4j-jul/index.html) 참조).

```groovy
test {
    systemProperty("java.util.logging.manager", "org.apache.logging.log4j.jul.LogManager")
}
```

Other logging frameworks provide different means to redirect messages logged using java.util.logging. For example, for Logback you can use the JUL to SLF4J Bridge by adding an additional dependency to the runtime classpath.
>다른 로깅 프레임워크는 `java.util.logging`을 사용하여 로깅된 메시지를 리디렉션하는 다른 방법을 제공합니다. 예를 들어, [Logback](https://logback.qos.ch/)의 경우 런타임 클래스 경로에 추가 종속성을 추가하여 [JUL에서 SLF4J로 브리지](https://www.slf4j.org/legacy.html#jul-to-slf4j)를 사용할 수 있습니다.

4.2.2. Maven

>>*The JUnit Platform Maven Surefire Provider has been discontinued*
>*JUnit 플랫폼 Maven Surefire 공급자가 중단되었습니다.*
>>The junit-platform-surefire-provider, which was originally developed by the JUnit team, was deprecated in JUnit Platform 1.3 and discontinued in 1.4. Please use Maven Surefire’s native support instead.
>원래 JUnit 팀에서 개발한 `junit-platform-surefire-provider`는 JUnit 플랫폼 1.3에서 더 이상 사용되지 않으며 1.4에서 중단되었습니다. 대신 Maven Surefire의 기본 지원을 사용하세요.

Starting with version 2.22.0, Maven Surefire and Maven Failsafe provide native support for executing tests on the JUnit Platform. The pom.xml file in the junit5-jupiter-starter-maven project demonstrates how to use the Maven Surefire plugin and can serve as a starting point for configuring your Maven build.
>[버전 2.22.0](https://issues.apache.org/jira/browse/SUREFIRE-1330)부터 Maven Surefire 및 Maven Failsafe는 JUnit 플랫폼에서 테스트를 실행하기 위한 [기본 지원(native support)](https://maven.apache.org/surefire/maven-surefire-plugin/examples/junit-platform.html)을 제공합니다. [junit5-jupiter-starter-maven](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-maven) 프로젝트의 `pom.xml` 파일은 Maven Surefire 플러그인을 사용하는 방법을 보여주며 Maven 빌드 구성을 위한 시작점 역할을 할 수 있습니다.

Configuring Test Engines
In order to have Maven Surefire or Maven Failsafe run any tests at all, at least one TestEngine implementation must be added to the test classpath.
>Maven Surefire 또는 Maven Failsafe가 모든 테스트를 실행하도록 하려면 테스트 클래스 경로에 하나 이상의 `TestEngine` 구현을 추가해야 합니다.

To configure support for JUnit Jupiter based tests, configure test scoped dependencies on the JUnit Jupiter API and the JUnit Jupiter TestEngine implementation similar to the following.
>JUnit Jupiter 기반 테스트에 대한 지원을 구성하려면 다음과 유사한 JUnit Jupiter API 및 JUnit Jupiter `TestEngine` 구현에 대한 테스트 범위 종속성을 구성하십시오.

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

Maven Surefire and Maven Failsafe can run JUnit 4 based tests alongside Jupiter tests as long as you configure test scoped dependencies on JUnit 4 and the JUnit Vintage TestEngine implementation similar to the following.
>Maven Surefire 및 Maven Failsafe는 JUnit 4 및 다음과 유사한 JUnit Vintage `TestEngine` 구현에 대한 `테스트` 범위 종속성을 구성하는 한 Jupiter 테스트와 함께 JUnit 4 기반 테스트를 실행할 수 있습니다.

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

Filtering by Test Class Names
The Maven Surefire Plugin will scan for test classes whose fully qualified names match the following patterns.
>Maven Surefire 플러그인은 정규화된 이름이 다음 패턴과 일치하는 테스트 클래스를 검색합니다.

- `**/Test*.java`
- `**/*Test.java`
- `**/*Tests.java`
- `**/*TestCase.java`

Moreover, it will exclude all nested classes (including static member classes) by default.
>또한 기본적으로 모든 nested 클래스(static member classes 포함)를 제외합니다.

Note, however, that you can override this default behavior by configuring explicit include and exclude rules in your pom.xml file. For example, to keep Maven Surefire from excluding static member classes, you can override its exclude rules as follows.
>그러나 `pom.xml` 파일에서 명시적 `include` 및 `exclude` 규칙을 구성하여 이 기본 동작을 재정의할 수 있습니다. 예를 들어 Maven Surefire가 static member classes를 제외하지 않도록 하려면 다음과 같이 제외 규칙을 재정의할 수 있습니다.

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

Please see the Inclusions and Exclusions of Tests documentation for Maven Surefire for details.
>자세한 내용은 Maven Surefire에 대한 [테스트 포함 및 제외 문서(Inclusions and Exclusions of Tests)](https://maven.apache.org/surefire/maven-surefire-plugin/examples/inclusion-exclusion.html)를 참조하세요.

Filtering by Tags
You can filter tests by tags or tag expressions using the following configuration properties.
>다음 구성 속성을 사용하여 `tags` 또는 `tag expresskons`으로 테스트를 필터링할 수 있습니다.

to include tags or tag expressions, use `groups`.

to exclude tags or tag expressions, use `excludedGroups`.

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

Configuration Parameters
You can set JUnit Platform configuration parameters to influence test discovery and execution by declaring the configurationParameters property and providing key-value pairs using the Java Properties file syntax (as shown below) or via the junit-platform.properties file.
>구성 `매개변수 속성`을 선언하고 Java `속성(Properties)` 파일 구문(아래 참조)을 사용하거나 `junit-platform.properties` 파일을 통해 키-값 쌍을 제공하여 JUnit 플랫폼 구성 매개변수를 설정하여 테스트 검색 및 실행에 영향을 줄 수 있습니다.

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

4.2.3. Ant
Starting with version 1.10.3 of Ant, a new junitlauncher task has been introduced to provide native support for launching tests on the JUnit Platform. The junitlauncher task is solely responsible for launching the JUnit Platform and passing it the selected collection of tests. The JUnit Platform then delegates to registered test engines to discover and execute the tests.
>[Ant](https://ant.apache.org/) 버전 `1.10.3`부터 JUnit 플랫폼에서 테스트를 시작하기 위한 기본 지원을 제공하기 위해 새로운 `junitlauncher` 작업이 도입되었습니다. `junitlauncher` 작업은 JUnit 플랫폼을 시작하고 선택한 테스트 모음을 전달하는 단독 책임입니다. 그런 다음 JUnit 플랫폼은 등록된 테스트 엔진에 위임하여 테스트를 검색하고 실행합니다.

The junitlauncher task attempts to align as close as possible with native Ant constructs such as resource collections for allowing users to select the tests that they want executed by test engines. This gives the task a consistent and natural feel when compared to many other core Ant tasks.
>`junitlauncher` 작업은 사용자가 테스트 엔진에서 실행하려는 테스트를 선택할 수 있도록 [리소스 컬렉션(resource collections)](https://ant.apache.org/manual/Types/resources.html#collection)과 같은 기본 Ant 구성과 최대한 가깝게 정렬하려고 시도합니다. 이것은 다른 많은 핵심 Ant 태스크와 비교할 때 태스크에 일관되고 자연스러운 느낌을 줍니다.

Starting with version 1.10.6 of Ant, the junitlauncher task supports forking the tests in a separate JVM.
>Ant 버전 `1.10.6`부터 `junitlauncher` 태스크는 별도의 [JVM에서 테스트 분기](https://ant.apache.org/manual/Tasks/junitlauncher.html#fork)를 지원합니다.

The build.xml file in the junit5-jupiter-starter-ant project demonstrates how to use the task and can serve as a starting point.
>[junit5-jupiter-starter-ant](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-ant) 프로젝트의 `build.xml` 파일은 작업 사용 방법을 보여주며 시작점 역할을 할 수 있습니다.

Basic Usage
The following example demonstrates how to configure the junitlauncher task to select a single test class (i.e., org.myapp.test.MyFirstJUnit5Test).
>다음 예제는 단일 테스트 클래스(예: `org.myapp.test.MyFirstJUnit5Test`)를 선택하도록 `junitlauncher` 작업을 구성하는 방법을 보여줍니다.

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

The test element allows you to specify a single test class that you want to be selected and executed. The classpath element allows you to specify the classpath to be used to launch the JUnit Platform. This classpath will also be used to locate test classes that are part of the execution.
>`테스트` 요소를 사용하면 선택 및 실행하려는 단일 테스트 클래스를 지정할 수 있습니다. `classpath` 요소를 사용하면 JUnit 플랫폼을 시작하는 데 사용할 클래스 경로를 지정할 수 있습니다. 이 클래스 경로는 실행의 일부인 테스트 클래스를 찾는 데에도 사용됩니다.

The following example demonstrates how to configure the junitlauncher task to select test classes from multiple locations.
>다음 예는 여러 위치에서 테스트 클래스를 선택하도록 `junitlauncher` 태스크를 구성하는 방법을 보여줍니다.

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

In the above example, the testclasses element allows you to select multiple test classes that reside in different locations.
>위의 예에서 `testclasses` 요소를 사용하면 다른 위치에 있는 여러 테스트 클래스를 선택할 수 있습니다.

For further details on usage and configuration options please refer to the official Ant documentation for the junitlauncher task.
>사용법 및 구성 옵션에 대한 자세한 내용은 `junitlauncher` [task](https://ant.apache.org/manual/Tasks/junitlauncher.html)에 대한 공식 Ant 문서를 참조하십시오.

4.3. Console Launcher
The ConsoleLauncher is a command-line Java application that lets you launch the JUnit Platform from the console. For example, it can be used to run JUnit Vintage and JUnit Jupiter tests and print test execution results to the console.
>[ConsoleLauncher](https://junit.org/junit5/docs/current/api/org.junit.platform.console/org/junit/platform/console/ConsoleLauncher.html)는 콘솔에서 JUnit 플랫폼을 시작할 수 있는 명령줄 Java 응용 프로그램입니다. 예를 들어 JUnit Vintage 및 JUnit Jupiter 테스트를 실행하고 테스트 실행 결과를 콘솔에 인쇄하는 데 사용할 수 있습니다.

An executable junit-platform-console-standalone-1.8.2.jar with all dependencies included is published in the Maven Central repository under the junit-platform-console-standalone directory. You can run the standalone ConsoleLauncher as shown below.
>모든 종속성이 포함된 실행 가능한 `junit-platform-console-standalone-1.8.2.jar`은 [junit-platform-console-standalone](https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/) 디렉토리 아래 [Maven Central](https://search.maven.org/) repository에 게시됩니다. 아래와 같이 독립형 ConsoleLauncher를 [실행](https://docs.oracle.com/javase/tutorial/deployment/jar/run.html)할 수 있습니다.

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

>>Exit Code
The ConsoleLauncher exits with a status code of 1 if any containers or tests failed. If no tests are discovered and the --fail-if-no-tests command-line option is supplied, the ConsoleLauncher exits with a status code of 2. Otherwise the exit code is 0.
>*Exit Code*
컨테이너 또는 테스트가 실패한 경우 `ConsoleLauncher`가 상태 코드 `1`과 함께 종료됩니다. 테스트가 검색되지 않고 `--fail-if-no-tests` 명령줄 옵션이 제공되면 `ConsoleLauncher`가 상태 코드 `2`와 함께 종료됩니다. 그렇지 않으면 종료 코드는 `0`입니다.

4.3.1. Options

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
  -e, --include-engine=ID    Provide the ID of an engine to be included in the test run.
                               This option can be repeated.
  -E, --exclude-engine=ID    Provide the ID of an engine to be excluded from the test
                               run. This option can be repeated.
      --config=KEY=VALUE     Set a configuration parameter for test discovery and
                               execution. This option can be repeated.
```

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

4.3.2. Argument Files (@-files)
On some platforms you may run into system limitations on the length of a command line when creating a command line with lots of options or with long arguments.
>일부 플랫폼에서는 옵션이 많거나 긴 인수가 있는 명령줄을 만들 때 명령줄 길이에 대한 시스템 제한이 발생할 수 있습니다.

Since version 1.3, the ConsoleLauncher supports argument files, also known as @-files. Argument files are files that themselves contain arguments to be passed to the command. When the underlying picocli command line parser encounters an argument beginning with the character @, it expands the contents of that file into the argument list.
>버전 1.3부터 `ConsoleLauncher`는 @-files라고도 하는 인수 파일을 지원합니다. 인수 파일은 명령에 전달할 인수를 자체적으로 포함하는 파일입니다. 기본 [picocli](https://github.com/remkop/picocli) 명령줄 파서는 `@` 문자로 시작하는 인수를 만나면 해당 파일의 내용을 인수 목록으로 확장합니다.

The arguments within a file can be separated by spaces or newlines. If an argument contains embedded whitespace, the whole argument should be wrapped in double or single quotes — for example, "-f=My Files/Stuff.java".
>파일 내의 인수는 공백이나 줄 바꿈으로 구분할 수 있습니다. 인수에 공백이 포함되어 있으면 전체 인수를 큰따옴표나 작은따옴표 로 묶어야 합니다(예: `"-f=My Files/Stuff.java"`).

If the argument file does not exist or cannot be read, the argument will be treated literally and will not be removed. This will likely result in an "unmatched argument" error message. You can troubleshoot such errors by executing the command with the picocli.trace system property set to DEBUG.
>인수 파일이 존재하지 않거나 읽을 수 없는 경우 인수는 문자 그대로 처리되고 제거되지 않습니다. 이로 인해 "일치하지 않는 인수" 오류 메시지가 나타날 수 있습니다. `picocli.trac`e 시스템 속성을 `DEBUG`로 설정하여 명령을 실행하여 이러한 오류를 해결할 수 있습니다.

Multiple @-files may be specified on the command line. The specified path may be relative to the current directory or absolute.
>명령줄에 여러 @-파일을 지정할 수 있습니다. 지정된 경로는 현재 디렉토리에 상대적이거나 절대 경로일 수 있습니다.

You can pass a real parameter with an initial @ character by escaping it with an additional @ symbol. For example, @@somearg will become @somearg and will not be subject to expansion.
>추가 `@` 기호로 이스케이프하여 초기 `@` 문자가 있는 실제 매개변수를 전달할 수 있습니다. 예를 들어 `@@somearg`는 `@somearg`가 되며 확장 대상이 아닙니다.

4.4. Using JUnit 4 to run the JUnit Platform

>>The JUnitPlatform runner has been deprecated
The JUnitPlatform runner was developed by the JUnit team as an interim solution for running test suites and tests on the JUnit Platform in a JUnit 4 environment.
>*`JUnitPlatform` runner는 더 이상 사용되지 않습니다.*
`JUnitPlatform` runner는 JUnit 4 환경의 JUnit 플랫폼에서 test suite 및 테스트를 실행하기 위한 임시 솔루션으로 JUnit 팀에 의해 개발되었습니다.

>>In recent years, all mainstream build tools and IDEs provide built-in support for running tests directly on the JUnit Platform.
>최근 몇 년 동안 모든 주류 빌드 도구와 IDE는 JUnit 플랫폼에서 직접 테스트를 실행하기 위한 내장 지원을 제공합니다.

>>In addition, the introduction of @Suite support provided by the junit-platform-suite-engine module makes the JUnitPlatform runner obsolete. See JUnit Platform Suite Engine for details.
>또한 `junit-platform-suite-engine` 모듈에서 제공하는 `@Suite` 지원의 도입으로 JUnitPlatform runner가 더 이상 사용되지 않습니다. 자세한 내용은 `JUnit 플랫폼 제품군 엔진(JUnit Platform Suite Engine)`을 참조하세요.

>>The JUnitPlatform runner and @UseTechnicalNames annotation have therefore been deprecated in JUnit Platform 1.8 and will be removed in JUnit Platform 2.0.
>따라서 `JUnitPlatform` runner 및 `@UseTechnicalNames` annotation은 JUnit 플랫폼 1.8에서 더 이상 사용되지 않으며 JUnit 플랫폼 2.0에서 제거됩니다.

>>If you are using the JUnitPlatform runner, please migrate to the @Suite support.
>`JUnitPlatform` runner를 사용하는 경우 `@Suite` 지원으로 migration하십시오.

The JUnitPlatform runner is a JUnit 4 based Runner which enables you to run any test whose programming model is supported on the JUnit Platform in a JUnit 4 environment — for example, a JUnit Jupiter test class.
>`JUnitPlatform` runner는 JUnit 4 환경 (예: JUnit Jupiter 테스트 클래스)의 JUnit 플랫폼에서 프로그래밍 모델이 지원되는 모든 테스트를 실행할 수 있는 JUnit 4 기반 `Runner`입니다.

Annotating a class with @RunWith(JUnitPlatform.class) allows it to be run with IDEs and build systems that support JUnit 4 but do not yet support the JUnit Platform directly.
>클래스에 `@RunWith(JUnitPlatform.class)` annotation을 추가하면 JUnit 4를 지원하지만 아직 JUnit 플랫폼을 직접 지원하지 않는 IDE 및 빌드 시스템으로 클래스를 실행할 수 있습니다.

>>Since the JUnit Platform has features that JUnit 4 does not have, the runner is only able to support a subset of the JUnit Platform functionality, especially with regard to reporting (see Display Names vs. Technical Names).
>JUnit 플랫폼에는 JUnit 4에 없는 기능이 있으므로 실행자는 특히 보고와 관련하여 JUnit 플랫폼 기능의 하위 집합만 지원할 수 있습니다(`Display names 대 Technical Names(Display Names vs. Technical Names)` 참조).

4.4.1. Setup

You need the following artifacts and their dependencies on the classpath. See Dependency Metadata for details regarding group IDs, artifact IDs, and versions.
>다음 artifact와 클래스 경로에 대한 종속성이 필요합니다. 그룹 ID, artifact ID 및 버전에 대한 자세한 내용은 `종속성 메타데이터(Dependency Metadata)`를 참조하세요.

Explicit Dependencies
- `junit-platform-runner` in test scope: location of the `JUnitPlatform` runner
- `junit-4.13.2.jar` in test scope: to run tests using JUnit 4
- `junit-jupiter-api` in test scope: API for writing tests using JUnit Jupiter, including `@Test`, etc.
- `junit-jupiter-engine` in test runtime scope: implementation of the `TestEngine` API for JUnit Jupiter

Transitive Dependencies
- `junit-platform-suite-api` in test scope
- `junit-platform-suite-commons` in test scope
- `junit-platform-launcher` in test scope
- `junit-platform-engine` in test scope
- `junit-platform-commons` in test scope
- `opentest4j` in test scope

4.4.2. Display Names vs. Technical Names

To define a custom display name for the class run via @RunWith(JUnitPlatform.class) annotate the class with @SuiteDisplayName and provide a custom value.
>`@RunWith(JUnitPlatform.class)`를 통해 실행되는 클래스의 사용자 정의 display name을 정의하려면 `@SuiteDisplayName`으로 클래스에 annotation을 달고 사용자 정의 값을 제공하십시오.

By default, display names will be used for test artifacts; however, when the JUnitPlatform runner is used to execute tests with a build tool such as Gradle or Maven, the generated test report often needs to include the technical names of test artifacts — for example, fully qualified class names — instead of shorter display names like the simple name of a test class or a custom display name containing special characters. To enable technical names for reporting purposes, declare the @UseTechnicalNames annotation alongside @RunWith(JUnitPlatform.class).
>기본적으로 display name은 테스트 artifact에 사용됩니다. 그러나 `JUnitPlatform` runner가 Gradle 또는 Maven과 같은 빌드 도구로 테스트를 실행하는 데 사용되는 경우 생성된 테스트 보고서에는 다음과 같은 짧은 display name 대신 테스트 artifact의 *technical name*(예: 정규화된 클래스 이름)이 포함되어야 하는 경우가 많습니다. 테스트 클래스의 단순 이름 또는 특수 문자가 포함된 사용자 지정 display name입니다. 보고 목적으로 기술 이름을 활성화하려면 `@RunWith(JUnitPlatform.class)`와 함께 `@UseTechnicalNames` annotation을 선언하십시오.

Note that the presence of @UseTechnicalNames overrides any custom display name configured via @SuiteDisplayName.
>`@UseTechnicalNames`가 있으면 `@SuiteDisplayName`을 통해 구성된 모든 사용자 지정 display name을 재정의합니다.

4.4.3. Single Test Class

One way to use the JUnitPlatform runner is to annotate a test class with @RunWith(JUnitPlatform.class) directly. Please note that the test methods in the following example are annotated with org.junit.jupiter.api.Test (JUnit Jupiter), not org.junit.Test (JUnit 4). Moreover, in this case the test class must be public; otherwise, some IDEs and build tools might not recognize it as a JUnit 4 test class.
>`JUnitPlatform` runner를 사용하는 한 가지 방법은 테스트 클래스에 `@RunWith(JUnitPlatform.class)` 직접 annotation을 추가하는 것입니다. 다음 예제의 테스트 method는 `org.junit.Test(JUnit 4)`가 아닌 `org.junit.jupiter.api.Test(JUnit Jupiter)`로 annotation 처리되어 있음을 유의하십시오. 더욱이 이 경우 테스트 클래스는 공개되어야 합니다. 그렇지 않으면 일부 IDE 및 빌드 도구에서 JUnit 4 테스트 클래스로 인식하지 못할 수 있습니다.

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

4.4.4. Test Suite
If you have multiple test classes you can create a test suite as can be seen in the following example.
>여러 테스트 클래스가 있는 경우 다음 예제에서 볼 수 있는 것처럼 test suite을 만들 수 있습니다.

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

The JUnitPlatformSuiteDemo will discover and run all tests in the example package and its subpackages. By default, it will only include test classes whose names either begin with Test or end with Test or Tests.
>`JUnitPlatformSuiteDemo`는 `예제` 패키지와 해당 하위 패키지의 모든 테스트를 검색하고 실행합니다. 기본적으로 이름이 `Test`로 시작하거나 `Test` 또는 `Tests`로 끝나는 테스트 클래스만 포함됩니다.

>>*Additional Configuration Options*
There are more configuration options for discovering and filtering tests than just @SelectPackages. Please consult the Javadoc of the org.junit.platform.suite.api package for further details.
>`@SelectPackages` 외에도 테스트 검색 및 필터링을 위한 구성 옵션이 더 있습니다. 자세한 내용은 `org.junit.platform.suite.api` 패키지의 Javadoc을 참조하십시오.

>>Test classes and suites annotated with @RunWith(JUnitPlatform.class) cannot be executed directly on the JUnit Platform (or as a "JUnit 5" test as documented in some IDEs). Such classes and suites can only be executed using JUnit 4 infrastructure.
>`@RunWith(JUnitPlatform.class)` annotation이 달린 테스트 클래스 및 스위트는 JUnit 플랫폼에서 직접 실행할 수 없습니다(또는 일부 IDE에 문서화된 "JUnit 5" 테스트로). 이러한 클래스와 제품군은 JUnit 4 인프라를 통해서만 실행할 수 있습니다.

4.5. Configuration Parameters

In addition to instructing the platform which test classes and test engines to include, which packages to scan, etc., it is sometimes necessary to provide additional custom configuration parameters that are specific to a particular test engine, listener, or registered extension. For example, the JUnit Jupiter TestEngine supports configuration parameters for the following use cases.
>포함할 테스트 클래스 및 테스트 엔진, 스캔할 패키지 등을 플랫폼에 지시하는 것 외에도 특정 테스트 엔진, 수신기 또는 등록된 확장과 관련된 추가 사용자 지정 구성 매개변수를 제공해야 하는 경우가 있습니다. 예를 들어 JUnit Jupiter `TestEngine`은 다음 사용 사례에 대한 구성 매개변수를 지원합니다.

- `Changing the Default Test Instance Lifecycle`
- `Enabling Automatic Extension Detection`
- `Deactivating Conditions`
- `Setting the Default Display Name Generator`

>- 기본 테스트 인스턴스 수명 주기 변경
>- 자동 확장 감지 활성화
>- 비활성화 조건
>- 기본 display name 생성기 설정

Configuration Parameters are text-based key-value pairs that can be supplied to test engines running on the JUnit Platform via one of the following mechanisms.
>구성 매개변수는 다음 메커니즘 중 하나를 통해 JUnit 플랫폼에서 실행되는 테스트 엔진에 제공할 수 있는 텍스트 기반 키-값 쌍(key-value pairs)입니다.

1. The configurationParameter() and configurationParameters() methods in the LauncherDiscoveryRequestBuilder which is used to build a request supplied to the Launcher API. When running tests via one of the tools provided by the JUnit Platform you can specify configuration parameters as follows:
    - Console Launcher: use the --config command-line option.
    - Gradle: use the systemProperty or systemProperties DSL.
    - Maven Surefire provider: use the configurationParameters property.
1. JVM system properties.
1. The JUnit Platform configuration file: a file named junit-platform.properties in the root of the class path that follows the syntax rules for a Java Properties file.

>1. `Launcher API`에 제공되는 요청을 빌드하는 데 사용되는 `LauncherDiscoveryRequestBuilder`의 `configurationParameter()` 및 `configurationParameters()` 메소드. JUnit 플랫폼에서 제공하는 도구 중 하나를 통해 테스트를 실행할 때 다음과 같이 구성 매개변수를 지정할 수 있습니다.
>   - `Console Launcher`: use the `--config command-line` option.
>   - `Gradle`: use the `systemProperty` or `systemProperties` DSL.
>   - `Maven Surefire provider`: use the `configurationParameters` property.
>1. JVM system properties :  a file named junit-platform.properties in the root of the class path that follows the syntax rules for a Java Properties file.
>1. JUnit 플랫폼 구성 파일: Java 속성 파일에 대한 구문 규칙을 따르는 클래스 경로의 루트에 있는 `junit-platform.properties`라는 파일입니다.

>>Configuration parameters are looked up in the exact order defined above. Consequently, configuration parameters supplied directly to the Launcher take precedence over those supplied via system properties and the configuration file. Similarly, configuration parameters supplied via system properties take precedence over those supplied via the configuration file.
>구성 매개변수는 위에 정의된 정확한 순서로 조회됩니다. 결과적으로 `Launcher`에 직접 제공된 구성 매개변수는 시스템 속성 및 구성 파일을 통해 제공된 매개변수보다 우선합니다. 마찬가지로 시스템 속성을 통해 제공되는 구성 매개변수는 구성 파일을 통해 제공되는 매개변수보다 우선합니다.

4.5.1. Pattern Matching Syntax

This section describes the pattern matching syntax that is applied to the configuration parameters used for the following features.
>이 섹션에서는 다음 기능에 사용되는 구성 매개변수에 적용되는 패턴 일치 구문에 대해 설명합니다.

-` Deactivating Conditions`
- `Deactivating a TestExecutionListener`

>- `Deactivating Conditions`
>- `Deactivating a TestExecutionListener`

If the value for the given configuration parameter consists solely of an asterisk (*), the pattern will match against all candidate classes. Otherwise, the value will be treated as a comma-separated list of patterns where each pattern will be matched against the fully qualified class name (FQCN) of each candidate class. Any dot (.) in a pattern will match against a dot (.) or a dollar sign ($) in a FQCN. Any asterisk (*) will match against one or more characters in a FQCN. All other characters in a pattern will be matched one-to-one against a FQCN.
>지정된 구성 매개변수의 값이 별표(`*`)로만 구성된 경우 패턴은 모든 후보 클래스와 일치합니다. 그렇지 않으면 값은 각 패턴이 각 후보 클래스의 FQCN(정규화된 클래스 이름)과 일치하는 쉼표로 구분된 패턴 목록으로 처리됩니다. 패턴의 모든 점(`.`)은 FQCN의 점(`.`) 또는 달러 기호(`$`)와 일치합니다. 별표(`*`)는 FQCN에서 하나 이상의 문자와 일치합니다. 패턴의 다른 모든 문자는 FQCN에 대해 일대일로 일치됩니다.

Examples:

- `*`: matches all candidate classes.
- `org.junit.*`: matches all candidate classes under the org.junit base package and any of its subpackages.
- `*.MyCustomImpl`: matches every candidate class whose simple class name is exactly MyCustomImpl.
- `*System*`: matches every candidate class whose FQCN contains System.
- `*System*+, +*Unit*`: matches every candidate class whose FQCN contains System or Unit.
- `org.example.MyCustomImpl`: matches the candidate class whose FQCN is exactly org.example.MyCustomImpl.
- `org.example.MyCustomImpl, org.example.TheirCustomImpl`: matches candidate classes whose FQCN is exactly org.example.MyCustomImpl or org.example.TheirCustomImpl.

>- `*`: 모든 후보 클래스와 일치합니다.
>- `org.junit.*`: `org.junit` 기본 패키지 및 해당 하위 패키지 아래의 모든 후보 클래스와 일치합니다.
>- `*.MyCustomImpl`: 단순 클래스 이름이 정확히 `MyCustomImpl`인 모든 후보 클래스와 일치합니다.
>- `*System*`: FQCN에 `System`이 포함된 모든 후보 클래스와 일치합니다.
>- `*System*+, +*Unit*`: FQCN에 `system` 또는 단위가 포함된 모든 후보 클래스와 일치합니다.
>- `org.example.MyCustomImpl`: FQCN이 정확히 `org.example.MyCustomImpl`인 후보 클래스와 일치합니다.
>- `org.example.MyCustomImpl, org.example.TheirCustomImpl`: FQCN이 정확히 `org.example.MyCustomImpl` 또는 `org.example.TheirCustomImpl`인 후보 클래스와 일치합니다.

4.6. Tags

Tags are a JUnit Platform concept for marking and filtering tests. The programming model for adding tags to containers and tests is defined by the testing framework. For example, in JUnit Jupiter based tests, the @Tag annotation (see Tagging and Filtering) should be used. For JUnit 4 based tests, the Vintage engine maps @Category annotations to tags (see Categories Support). Other testing frameworks may define their own annotation or other means for users to specify tags.
>태그는 테스트를 표시하고 필터링하기 위한 JUnit 플랫폼 개념입니다. 컨테이너 및 테스트에 태그를 추가하기 위한 프로그래밍 모델은 테스트 프레임워크에 의해 정의됩니다. 예를 들어 JUnit Jupiter 기반 테스트에서는 `@Tag` annotation(`태그 지정 및 필터링` 참조)을 사용해야 합니다. JUnit 4 기반 테스트의 경우 빈티지 엔진은 `@Category` annotation을 태그에 매핑합니다(`카테고리 지원` 참조). 다른 테스트 프레임워크는 사용자가 태그를 지정하기 위한 고유한 annotation 또는 기타 수단을 정의할 수 있습니다.

4.6.1. Syntax Rules for Tags

Regardless how a tag is specified, the JUnit Platform enforces the following rules:
>태그 지정 방법에 관계없이 JUnit 플랫폼은 다음 규칙을 적용합니다.

- A tag must not be null or blank.
- A trimmed tag must not contain whitespace.
- A trimmed tag must not contain ISO control characters.
- A trimmed tag must not contain any of the following reserved characters.
    - ,: comma
    - (: left parenthesis
    - ): right parenthesis
    - &: ampersand
    - |: vertical bar
    - !: exclamation point

>- 태그는 `null`이거나 공백일 수 없습니다.
>- 잘린 태그는 공백을 포함할 수 없습니다.
>- 트리밍된 태그에는 ISO 제어 문자가 포함되어서는 안 됩니다.
>- 트리밍된 태그에는 다음 예약 문자가 포함되어서는 안 됩니다.
>    - `,`: comma
>    - `(`: left parenthesis
>    - `)`: right parenthesis
>    - `&`: ampersand
>    - `|`: vertical bar
>    - `!`: exclamation point

>>In the above context, "trimmed" means that leading and trailing whitespace characters have been removed.
>위 컨텍스트에서 "trimmed"는 선행 및 후행 공백 문자가 제거되었음을 의미합니다.

4.6.2. Tag Expressions

Tag expressions are boolean expressions with the operators !, & and |. In addition, ( and ) can be used to adjust for operator precedence.
>태그 표현식은 `!`, `&` 및 `|` 연산자가 있는 부울 표현식입니다. 또한 `(` 및 `)` 연산자를 사용하여 연산자 우선 순위를 조정할 수 있습니다.

Two special expressions are supported, any() and none(), which select all tests with any tags at all, and all tests without any tags, respectively. These special expressions may be combined with other expressions just like normal tags.
>태그가 있는 모든 테스트와 태그가 없는 모든 테스트를 각각 선택하는 `any()` 및 `none()`의 두 가지 특수 표현식이 지원됩니다. 이러한 특수 표현식은 일반 태그와 마찬가지로 다른 표현식과 결합될 수 있습니다.

Table 2. Operators (in descending order of precedence)

|Operator|Meaning|Associativity|
|---|---|---|
|!|not|right|
|&|and|left|
|\||or|left|

If you are tagging your tests across multiple dimensions, tag expressions help you to select which tests to execute. When tagging by test type (e.g., micro, integration, end-to-end) and feature (e.g., product, catalog, shipping), the following tag expressions can be useful.
>여러 차원에 걸쳐 테스트에 태그를 지정하는 경우 태그 표현식은 실행할 테스트를 선택하는 데 도움이 됩니다. 테스트 유형(예: 마이크로, 통합, 종단 간) 및 기능(예: **제품, 카탈로그, 배송**)으로 태그를 지정할 때 다음 태그 표현식이 유용할 수 있습니다.

|Tag Expression|Selection|
|---|---|
|product|all tests for **product**|
|catalog \| shipping|all tests for catalog plus all tests for **shipping**|
|catalog & shipping|all tests for the intersection between **catalog** and **shipping**|
|product & !end-to-end|all tests for **product**, but not the end-to-end tests|
|(micro \| integration) & (product \| shipping)|all micro or integration tests for **product** or **shipping**|

4.7. Capturing Standard Output/Error

Since version 1.3, the JUnit Platform provides opt-in support for capturing output printed to System.out and System.err. To enable it, set the junit.platform.output.capture.stdout and/or junit.platform.output.capture.stderr configuration parameter to true. In addition, you may configure the maximum number of buffered bytes to be used per executed test or container using junit.platform.output.capture.maxBuffer.
>버전 1.3부터 JUnit 플랫폼은 `System.out` 및 `System.err`에 인쇄된 출력을 캡처하기 위한 옵트인 지원을 제공합니다. 활성화하려면 `junit.platform.output.capture.stdout` 및/또는 `junit.platform.output.capture.stderr` 구성 `매개변수`를 `true`로 설정하십시오. 또한 `junit.platform.output.capture.maxBuffer`를 사용하여 실행된 테스트 또는 컨테이너당 사용할 최대 버퍼링된 바이트 수를 구성할 수 있습니다.

If enabled, the JUnit Platform captures the corresponding output and publishes it as a report entry using the stdout or stderr keys to all registered TestExecutionListener instances immediately before reporting the test or container as finished.
>활성화된 경우 JUnit 플랫폼은 해당 출력을 캡처하고 테스트 또는 컨테이너를 완료된 것으로 보고하기 직전에 등록된 모든 `TestExecutionListener` 인스턴스에 `stdout` 또는 `stderr` 키를 사용하여 보고서 항목으로 게시합니다.

Please note that the captured output will only contain output emitted by the thread that was used to execute a container or test. Any output by other threads will be omitted because particularly when executing tests in parallel it would be impossible to attribute it to a specific test or container.
>캡처된 출력에는 컨테이너 또는 테스트를 실행하는 데 사용된 스레드에서 내보낸 출력만 포함됩니다. 특히 `병렬로 테스트를 실행(executing tests in parallel)`할 때 특정 테스트나 컨테이너에 속성을 부여하는 것이 불가능하기 때문에 다른 스레드의 출력은 생략됩니다.

4.8. Using Listeners
The JUnit Platform provides the following listener APIs that allow JUnit, third parties, and custom user code to react to events fired at various points during the discovery and execution of a TestPlan.
>JUnit 플랫폼은 JUnit, 타사 및 사용자 정의 사용자 코드가 `TestPlan`의 검색 및 실행 중 다양한 지점에서 발생한 이벤트에 반응할 수 있도록 하는 다음 리스너 API를 제공합니다.

- LauncherSessionListener: receives events when a LauncherSession is opened and closed.
- LauncherDiscoveryListener: receives events that occur during test discovery.
- TestExecutionListener: receives events that occur during test execution.

>- [LauncherSessionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSessionListener.html): [LauncherSession](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSession.html)이 열리고 닫힐 때 이벤트를 수신합니다.
>- [LauncherDiscoveryListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryListener.html): 테스트 검색 중에 발생하는 이벤트를 수신합니다.
>- [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html): 테스트 실행 중 발생하는 이벤트를 수신합니다.

The LauncherSessionListener API is typically implemented by build tools or IDEs and registered automatically for you in order to support some feature of the build tool or IDE.
>`LauncherSessionListener` API는 일반적으로 빌드 도구 또는 IDE에 의해 구현되며 빌드 도구 또는 IDE의 일부 기능을 지원하기 위해 자동으로 등록됩니다.

The LauncherDiscoveryListener and TestExecutionListener APIs are often implemented in order to produce some form of report or to display a graphical representation of the test plan in an IDE. Such listeners may be implemented and automatically registered by a build tool or IDE, or they may be included in a third-party library – potentially registered for you automatically. You can also implement and register your own listeners.
>`LauncherDiscoveryListener` 및 `TestExecutionListener` API는 보고서 형식을 생성하거나 IDE에서 테스트 계획의 그래픽 표현을 표시하기 위해 종종 구현됩니다. 이러한 리스너는 빌드 도구 또는 IDE에 의해 구현되고 자동으로 등록되거나 third-party 라이브러리에 포함될 수 있습니다(잠재적으로 자동으로 등록될 수 있음). 자체 리스너를 구현하고 등록할 수도 있습니다.

For details on registering and configuring listeners, see the following sections of this guide.
>리스너 등록 및 구성에 대한 자세한 내용은 이 가이드의 다음 섹션을 참조하십시오.

- `Registering a LauncherSessionListener`
- `Registering a LauncherDiscoveryListener`
- `Registering a TestExecutionListener`
- `Configuring a TestExecutionListener`
- `Deactivating a TestExecutionListener`

The JUnit Platform provides the following listeners which you may wish to use with your test suite.
>JUnit 플랫폼은 test suite와 함께 사용할 수 있는 다음 리스너를 제공합니다.

**`Flight Recorder Support`**
`FlightRecordingExecutionListener` and `FlightRecordingDiscoveryListener` that generate Java Flight Recorder events during test discovery and execution.

[LegacyXmlReportGeneratingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.reporting/org/junit/platform/reporting/legacy/xml/LegacyXmlReportGeneratingListener.html)
TestExecutionListener that generates XML reports compatible with the de facto standard for JUnit 4 based test reports. See JUnit Platform Reporting for details.

`LoggingListener`
TestExecutionListener for logging informational messages for all events via a BiConsumer that consumes Throwable and Supplier<String>.

`SummaryGeneratingListener`
TestExecutionListener that generates a summary of the test execution which can be printed via a PrintWriter.

`UniqueIdTrackingListener`
TestExecutionListener that that tracks the unique IDs of all tests that were skipped or executed during the execution of the TestPlan and generates a file containing the unique IDs once execution of the TestPlan has finished.

>**`Flight Recorder Support`**
테스트 검색 및 실행 중에 Java Flight Recorder 이벤트를 생성하는 `FlightRecordingExecutionListener` 및 `FlightRecordingDiscoveryListener`.

>[LegacyXmlReportGeneratingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.reporting/org/junit/platform/reporting/legacy/xml/LegacyXmlReportGeneratingListener.html)
JUnit 4 기반 테스트 보고서의 사실상 표준과 호환되는 XML 보고서를 생성하는 `TestExecutionListener`. 자세한 내용은 `JUnit 플랫폼 보고(JUnit Platform Reporting)`를 참조하세요.

>[LoggingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/LoggingListener.html)
`Throwable` 및 `Supplier<String>`을 사용하는 `BiConsumer`를 통해 모든 이벤트에 대한 정보 메시지를 로깅하기 위한 `TestExecutionListener`.

>[SummaryGeneratingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/SummaryGeneratingListener.html)
`PrintWriter`를 통해 인쇄할 수 있는 테스트 실행 요약을 생성하는 `TestExecutionListener`.

>[UniqueIdTrackingListener()](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/UniqueIdTrackingListener.html)
`TestPlan` 실행 중 건너뛰거나 실행된 모든 테스트의 고유 ID를 추적하고 `TestPlan` 실행이 완료되면 고유 ID를 포함하는 파일을 생성하는 `TestExecutionListener`.

4.8.1. Flight Recorder Support

Since version 1.7, the JUnit Platform provides opt-in support for generating Flight Recorder events. JEP 328 describes the Java Flight Recorder (JFR) as:
>버전 1.7부터 JUnit 플랫폼은 Flight Recorder 이벤트 생성을 위한 옵트인 지원을 제공합니다. [JEP 328](https://openjdk.java.net/jeps/328)은 JFR(Java Flight Recorder)을 다음과 같이 설명합니다.

>>Flight Recorder records events originating from applications, the JVM and the OS. Events are stored in a single file that can be attached to bug reports and examined by support engineers, allowing after-the-fact analysis of issues in the period leading up to a problem.
>Flight Recorder는 애플리케이션, JVM 및 OS에서 발생하는 이벤트를 기록합니다. 이벤트는 버그 보고서에 첨부할 수 있고 지원 엔지니어가 검사할 수 있는 단일 파일에 저장되므로 문제가 발생하기까지의 기간 동안 문제에 대한 사후 분석이 가능합니다.

In order to record Flight Recorder events generated while running tests, you need to:
>테스트를 실행하는 동안 생성된 Flight Recorder 이벤트를 기록하려면 다음을 수행해야 합니다.

1. Ensure that you are using either Java 8 Update 262 or higher or Java 11 or later.
1. Provide the org.junit.platform.jfr module (junit-platform-jfr-1.8.2.jar) on the class-path or module-path at test runtime.
1. Start flight recording when launching a test run. Flight Recorder can be started via java command line option:
```
-XX:StartFlightRecording:filename=...
```

>1. Java 8 업데이트 262 이상 또는 Java 11 이상을 사용하고 있는지 확인합니다.
>1. 테스트 런타임 시 클래스 경로 또는 모듈 경로에 `org.junit.platform.jfr` 모듈(`junit-platform-jfr-1.8.2.jar`)을 제공합니다.
>1. 테스트 실행을 시작할 때 비행 기록을 시작합니다. Flight Recorder는 Java 명령줄 옵션을 통해 시작할 수 있습니다.
>```
>-XX:StartFlightRecording:filename=...
>```

Please consult the manual of your build tool for the appropriate commands.
>적절한 명령에 대해서는 빌드 도구의 매뉴얼을 참조하십시오.

To analyze the recorded events, use the jfr command line tool shipped with recent JDKs or open the recording file with JDK Mission Control.
>기록된 이벤트를 분석하려면 최근 JDK와 함께 제공된 [jfr](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jfr.html) 명령줄 도구를 사용하거나 [JDK Mission Control](https://jdk.java.net/jmc/)로 기록 파일을 엽니다.

>>Flight Recorder support is currently an experimental feature. You’re invited to give it a try and provide feedback to the JUnit team so they can improve and eventually promote this feature.
>Flight Recorder 지원은 현재 실험적인 기능입니다. JUnit 팀이 이 기능을 개선하고 궁극적으로 `홍보`할 수 있도록 시도해 보고 피드백을 제공하도록 초대받았습니다.

5. Extension Model

5.1. Overview

In contrast to the competing Runner, TestRule, and MethodRule extension points in JUnit 4, the JUnit Jupiter extension model consists of a single, coherent concept: the Extension API. Note, however, that Extension itself is just a marker interface.
>JUnit 4의 경쟁적인 `Runner`, `TestRule` 및 `MethodRule` 확장 지점과 달리 JUnit Jupiter 확장 모델은 `Extension` API라는 일관된 단일 개념으로 구성됩니다. 그러나 `Extension` 자체는 마커 인터페이스일 뿐입니다.

5.2. Registering Extensions

Extensions can be registered declaratively via @ExtendWith, programmatically via @RegisterExtension, or automatically via Java’s ServiceLoader mechanism.
>확장은 `@ExtendWith`를 통해 선언적으로 등록하거나 `@RegisterExtension`을 통해 프로그래밍 방식으로 또는 Java의 `ServiceLoader` 메커니즘을 통해 자동으로 등록할 수 있습니다.

5.2.1. Declarative Extension Registration

Developers can register one or more extensions declaratively by annotating a test interface, test class, test method, or custom composed annotation with @ExtendWith(…​) and supplying class references for the extensions to register. As of JUnit Jupiter 5.8, @ExtendWith may also be declared on fields or on parameters in test class constructors, in test methods, and in @BeforeAll, @AfterAll, @BeforeEach, and @AfterEach lifecycle methods.
>개발자는 테스트 인터페이스, 테스트 클래스, 테스트 메소드 또는 사용자 정의 `구성 annotation(composed annotation)`에 `@ExtendWith(…​)` annotation을 달고 등록할 확장에 대한 클래스 참조를 제공하여 하나 이상의 확장을 선언적으로 등록할 수 있습니다. JUnit Jupiter 5.8부터` @ExtendWith`는 테스트 클래스 생성자의 필드 또는 매개변수, 테스트 메소드 및 `@BeforeAll`, `@AfterAll`, `@BeforeEach` 및 `@AfterEach` 수명 주기 메소드에서도 선언될 수 있습니다.

For example, to register a WebServerExtension for a particular test method, you would annotate the test method as follows. We assume the WebServerExtension starts a local web server and injects the server’s URL into parameters annotated with @WebServerUrl.
>예를 들어, 특정 테스트 방법에 대해 `WebServerExtension`을 등록하려면 다음과 같이 테스트 방법에 annotation을 답니다. `WebServerExtension`이 로컬 웹 서버를 시작하고 서버의 URL을 `@WebServerUrl`로 annotation이 달린 매개변수에 주입한다고 가정합니다.

```java
@Test
@ExtendWith(WebServerExtension.class)
void getProductList(@WebServerUrl String serverUrl) {
    WebClient webClient = new WebClient();
    // Use WebClient to connect to web server using serverUrl and verify response
    assertEquals(200, webClient.get(serverUrl + "/products").getResponseStatus());
}
```

To register the WebServerExtension for all tests in a particular class and its subclasses, you would annotate the test class as follows.
>특정 클래스 및 해당 subclasses의 모든 테스트에 대해 `WebServerExtension`을 등록하려면 다음과 같이 테스트 클래스에 annotation을 추가합니다.

```java
@ExtendWith(WebServerExtension.class)
class MyTests {
    // ...
}
```

Multiple extensions can be registered together like this:
>다음과 같이 여러 확장을 함께 등록할 수 있습니다.

```java
@ExtendWith({ DatabaseExtension.class, WebServerExtension.class })
class MyFirstTests {
    // ...
}
```

As an alternative, multiple extensions can be registered separately like this:
>대안으로 다음과 같이 여러 확장을 별도로 등록할 수 있습니다.

```java
@ExtendWith(DatabaseExtension.class)
@ExtendWith(WebServerExtension.class)
class MySecondTests {
    // ...
}
```

>>Extension Registration Order
Extensions registered declaratively via @ExtendWith at the class level, method level, or parameter level will be executed in the order in which they are declared in the source code. For example, the execution of tests in both MyFirstTests and MySecondTests will be extended by the DatabaseExtension and WebServerExtension, in exactly that order.
>*Extension Registration Order*
클래스 수준, 메소드 수준 또는 매개 변수 수준에서 `@ExtendWith`를 통해 선언적으로 등록된 확장은 소스 코드에서 선언된 순서대로 실행됩니다. 예를 들어 `MyFirstTests`와 `MySecondTests` 모두에서 테스트 실행은 `DatabaseExtension`과 `WebServerExtension`에 의해 **정확히 그 순서대로** 확장됩니다.

If you wish to combine multiple extensions in a reusable way, you can define a custom composed annotation and use @ExtendWith as a meta-annotation as in the following code listing. Then @DatabaseAndWebServerExtension can be used in place of @ExtendWith({ DatabaseExtension.class, WebServerExtension.class }).
>재사용 가능한 방식으로 여러 확장을 결합하려면 다음 코드 목록과 같이 사용자 정의 구성된 annotation을 정의하고 `@ExtendWith`를 meta-annotation으로 사용할 수 있습니다. 그런 다음 `@ExtendWith({ DatabaseExtension.class, WebServerExtension.class })` 대신 `@DatabaseAndWebServerExtension`을 사용할 수 있습니다.

```java
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@ExtendWith({ DatabaseExtension.class, WebServerExtension.class })
public @interface DatabaseAndWebServerExtension {
}
```

The above examples demonstrate how @ExtendWith can be applied at the class level or at the method level; however, for certain use cases it makes sense for an extension to be registered declaratively at the field or parameter level. Consider a RandomNumberExtension that generates random numbers that can be injected into a field or via a parameter in a constructor, test method, or lifecycle method. If the extension provides a @Random annotation that is meta-annotated with @ExtendWith(RandomNumberExtension.class) (see listing below), the extension can be used transparently as in the following RandomNumberDemo example.
>위의 예는 `@ExtendWith`가 클래스 수준이나 메소드 수준에서 어떻게 적용될 수 있는지 보여줍니다. 그러나 특정 사용 사례의 경우 확장을 필드 또는 매개변수 수준에서 선언적으로 등록하는 것이 좋습니다. 필드에 삽입하거나 생성자, 테스트 메소드 또는 수명 주기 메소드의 매개 변수를 통해 삽입할 수 있는 난수를 생성하는 `RandomNumberExtension`을 고려하십시오. 확장이 `@ExtendWith(RandomNumberExtension.class)`로 메타 annotation이 달린 `@Random` annotation을 제공하는 경우(아래 목록 참조) 확장은 다음 `RandomNumberDemo` 예제와 같이 투명하게 사용할 수 있습니다.

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
>>*Extension Registration Order for `@ExtendWith`on Fields*
Extensions registered declaratively via @ExtendWith on fields will be ordered relative to @RegisterExtension fields and other @ExtendWith fields using an algorithm that is deterministic but intentionally nonobvious. However, @ExtendWith fields can be ordered using the @Order annotation. See the Extension Registration Order tip for @RegisterExtension fields for details.
>*필드의 `@ExtendWith`에 대한 확장 등록 순서*
필드에서 `@ExtendWith`를 통해 선언적으로 등록된 확장은 결정적이지만 의도적으로 명확하지 않은 알고리즘을 사용하여 `@RegisterExtension` 필드 및 기타 `@ExtendWith` 필드를 기준으로 정렬됩니다. 그러나 `@ExtendWith` 필드는 `@Order` annotation을 사용하여 정렬할 수 있습니다. 자세한 내용은 `@RegisterExtension` 필드에 대한 `확장 등록 주문` 팁을 참조하십시오.

>>@ExtendWith fields may be either static or non-static. The documentation on Static Fields and Instance Fields for @RegisterExtension fields also applies to @ExtendWith fields.
>`@ExtendWith` 필드는 `정적`이거나 비정적일 수 있습니다. `@RegisterExtension` 필드의 `정적 필드(Static Fields)` 및 `인스턴스 필드(Instance Fields)`에 대한 문서는 `@ExtendWith` 필드에도 적용됩니다.

5.2.2. Programmatic Extension Registration

Developers can register extensions programmatically by annotating fields in test classes with @RegisterExtension.
>개발자는 [@RegisterExtension](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/RegisterExtension.html)으로 테스트 클래스의 필드에 annotation을 추가하여 프로그래밍 방식으로 확장을 등록할 수 있습니다.

When an extension is registered declaratively via @ExtendWith, it can typically only be configured via annotations. In contrast, when an extension is registered via @RegisterExtension, it can be configured programmatically — for example, in order to pass arguments to the extension’s constructor, a static factory method, or a builder API.
>확장이 `@ExtendWith`를 통해 선언적으로 등록되면 일반적으로 annotation을 통해서만 구성할 수 있습니다. 대조적으로, 확장이 `@RegisterExtension`을 통해 등록되면 확장의 생성자, 정적 팩토리 메소드 또는 빌더 API에 인수를 전달하기 위해 프로그래밍 방식으로 구성할 수 있습니다.

Extension Registration Order
By default, extensions registered programmatically via @RegisterExtension or declaratively via @ExtendWith on fields will be ordered using an algorithm that is deterministic but intentionally nonobvious. This ensures that subsequent runs of a test suite execute extensions in the same order, thereby allowing for repeatable builds. However, there are times when extensions need to be registered in an explicit order. To achieve that, annotate @RegisterExtension fields or @ExtendWith fields with @Order.
>기본적으로 `@RegisterExtension`을 통해 프로그래밍 방식으로 등록되거나 필드에서 `@ExtendWith`를 통해 선언적으로 등록된 확장은 결정적이지만 의도적으로 명확하지 않은 알고리즘을 사용하여 정렬됩니다. 이것은 test suite의 후속 실행이 동일한 순서로 확장을 실행하도록 하여 반복 가능한 빌드를 허용합니다. 그러나 확장을 명시적인 순서로 등록해야 하는 경우가 있습니다. 이를 달성하려면 `@RegisterExtension` 필드 또는` @ExtendWith `필드에 [@Order](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Order.html)를 추가하십시오.

Any @RegisterExtension field or @ExtendWith field not annotated with @Order will be ordered using the default order which has a value of Integer.MAX_VALUE / 2. This allows @Order annotated extension fields to be explicitly ordered before or after non-annotated extension fields. Extensions with an explicit order value less than the default order value will be registered before non-annotated extensions. Similarly, extensions with an explicit order value greater than the default order value will be registered after non-annotated extensions. For example, assigning an extension an explicit order value that is greater than the default order value allows before callback extensions to be registered last and after callback extensions to be registered first, relative to other programmatically registered extensions.
>`@Order`로 annotation이 지정되지 않은 `@RegisterExtension` 필드 또는 `@ExtendWith` 필드는 값이 `Integer.MAX_VALUE` / 2인 기본 순서를 사용하여 정렬됩니다. 이를 통해 `@Order` annotation이 있는 확장 필드를 annotation이 없는 확장 필드 앞이나 뒤에 명시적으로 정렬할 수 있습니다. 기본 순서 값보다 작은 명시적 순서 값을 가진 확장은 annotation이 없는 확장보다 먼저 등록됩니다. 마찬가지로 기본 순서 값보다 큰 명시적 순서 값을 가진 확장은 annotation이 없는 확장 후에 등록됩니다. 예를 들어, 기본 순서 값보다 큰 명시적 순서 값을 확장에 할당하면 프로그래밍 방식으로 등록된 다른 확장에 비해 콜백 확장 이전이 마지막으로 등록되고 콜백 확장 이후에 먼저 등록됩니다.

@RegisterExtension fields must not be null (at evaluation time) but may be either static or non-static.
>`@RegisterExtension` 필드는 `null`이 아니어야 하지만(평가 시) `정적`이거나 비정적일 수 있습니다.

Static Fields
If a @RegisterExtension field is static, the extension will be registered after extensions that are registered at the class level via @ExtendWith. Such static extensions are not limited in which extension APIs they can implement. Extensions registered via static fields may therefore implement class-level and instance-level extension APIs such as BeforeAllCallback, AfterAllCallback, TestInstancePostProcessor, and TestInstancePreDestroyCallback as well as method-level extension APIs such as BeforeEachCallback, etc.
>`@RegisterExtension` 필드가 정적이면 `@ExtendWith`를 통해 클래스 수준에서 등록된 확장 이후에 확장이 등록됩니다. 이러한 정적 확장은 구현할 수 있는 확장 API에 제한이 없습니다. 따라서 정적 필드를 통해 등록된 확장은 `BeforeAllCallback`, `AfterAllCallback`, `TestInstancePostProcessor` 및 `TestInstancePreDestroyCallback`과 같은 클래스 수준 및 인스턴스 수준 확장 API와 `BeforeEachCallback` 등과 같은 메소드 수준 확장 API를 구현할 수 있습니다.

In the following example, the server field in the test class is initialized programmatically by using a builder pattern supported by the WebServerExtension. The configured WebServerExtension will be automatically registered as an extension at the class level — for example, in order to start the server before all tests in the class and then stop the server after all tests in the class have completed. In addition, static lifecycle methods annotated with @BeforeAll or @AfterAll as well as @BeforeEach, @AfterEach, and @Test methods can access the instance of the extension via the server field if necessary.
>다음 예제에서 테스트 클래스의 서버 필드는 `WebServerExtension`에서 지원하는 빌더 패턴을 사용하여 프로그래밍 방식으로 초기화됩니다. 구성된 `WebServerExtension`은 클래스 수준 에서 확장으로 자동 등록됩니다. 또한 `@BeforeAll` 또는 `@AfterAll` annotation이 달린 정적 라이프사이클 메소드와 `@BeforeEach`, `@AfterEach`, `@Test` method는 필요한 경우 서버 필드를 통해 확장의 인스턴스에 액세스할 수 있습니다.

*Registering an extension via a static field in Java*
>Java에서 정적 필드를 통해 확장 등록

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

Static Fields in Kotlin
The Kotlin programming language does not have the concept of a static field. However, the compiler can be instructed to generate a private static field using the @JvmStatic annotation in Kotlin. If you want the Kotlin compiler to generate a public static field, you can use the @JvmField annotation instead.
>Kotlin 프로그래밍 언어에는 `static` 필드의 개념이 없습니다. 그러나 컴파일러는 Kotlin의 `@JvmStatic` annotation을 사용하여 `private static` 필드를 생성하도록 지시할 수 있습니다. Kotlin 컴파일러가 공용 정적 필드를 생성하도록 하려면 `@JvmField` annotation을 대신 사용할 수 있습니다.

The following example is a version of the WebServerDemo from the previous section that has been ported to Kotlin.
>다음 예제는 Kotlin으로 이식된 이전 섹션의 `WebServerDemo` 버전입니다.

`Registering an extension via a static field in Kotlin`
>Kotlin에서 정적 필드를 통해 확장 등록

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

Instance Fields
If a @RegisterExtension field is non-static (i.e., an instance field), the extension will be registered after the test class has been instantiated and after each registered TestInstancePostProcessor has been given a chance to post-process the test instance (potentially injecting the instance of the extension to be used into the annotated field). Thus, if such an instance extension implements class-level or instance-level extension APIs such as BeforeAllCallback, AfterAllCallback, or TestInstancePostProcessor, those APIs will not be honored. By default, an instance extension will be registered after extensions that are registered at the method level via @ExtendWith; however, if the test class is configured with @TestInstance(Lifecycle.PER_CLASS) semantics, an instance extension will be registered before extensions that are registered at the method level via @ExtendWith.
>`@RegisterExtension` 필드가 비정적(즉, 인스턴스 필드)인 경우 확장은 테스트 클래스가 인스턴스화되고 등록된 각 `TestInstancePostProcessor`에 테스트 인스턴스를 사후 처리할 기회가 주어진 후에 등록됩니다(잠재적으로 annotation 필드에 사용할 확장의 인스턴스). 따라서 이러한 *인스턴스 확장*이 `BeforeAllCallback`, `AfterAllCallback` 또는 `TestInstancePostProcessor`와 같은 클래스 수준 또는 인스턴스 수준 확장 API를 구현하는 경우 해당 API는 적용되지 않습니다. 기본적으로 인스턴스 확장은 `@ExtendWith`를 통해 메소드 수준에서 등록된 확장 이후에 등록됩니다. 그러나 테스트 클래스가 `@TestInstance(Lifecycle.PER_CLASS)` 의미 체계로 구성된 경우 인스턴스 확장은 `@ExtendWith`를 통해 메소드 수준에서 등록된 확장보다 먼저 등록됩니다.

In the following example, the docs field in the test class is initialized programmatically by invoking a custom lookUpDocsDir() method and supplying the result to the static forPath() factory method in the DocumentationExtension. The configured DocumentationExtension will be automatically registered as an extension at the method level. In addition, @BeforeEach, @AfterEach, and @Test methods can access the instance of the extension via the docs field if necessary.
>다음 예제에서 테스트 클래스의 문서 필드는 사용자 정의 `lookUpDocsDir()` 메소드를 호출하고 `DocumentationExtension`의 정적 `forPath()` 팩토리 메소드에 결과를 제공하여 프로그래밍 방식으로 초기화됩니다. 구성된 `DocumentationExtension`은 메소드 수준에서 자동으로 확장으로 등록됩니다. 또한 `@BeforeEach`, `@AfterEach`, `@Test` 메소드는 필요한 경우 `docs` 필드를 통해 확장의 인스턴스에 액세스할 수 있습니다.

`An extension registered via an instance field`
>인스턴스 필드를 통해 등록된 확장자

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

5.2.3. Automatic Extension Registration

In addition to declarative extension registration and programmatic extension registration support using annotations, JUnit Jupiter also supports global extension registration via Java’s ServiceLoader mechanism, allowing third-party extensions to be auto-detected and automatically registered based on what is available in the classpath.
>annotation을 사용한 `선언적 확장 등록(declarative extension registration)` 및 `프로그래밍 방식 확장 등록( programmatic extension registration)` 지원 외에도 JUnit Jupiter는 Java의 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 전역 확장 등록을 지원하므로 클래스 경로에서 사용 가능한 항목을 기반으로 타사 확장을 자동 감지하고 자동으로 등록할 수 있습니다.

Specifically, a custom extension can be registered by supplying its fully qualified class name in a file named org.junit.jupiter.api.extension.Extension within the /META-INF/services folder in its enclosing JAR file.
>특히, 사용자 정의 확장은 포함하는 JAR 파일의 `/META-INF/services` 폴더 내 `org.junit.jupiter.api.extension.Extension`이라는 파일에 정규화된 클래스 이름을 제공하여 등록할 수 있습니다.

Enabling Automatic Extension Detection
Auto-detection is an advanced feature and is therefore not enabled by default. To enable it, set the junit.jupiter.extensions.autodetection.enabled configuration parameter to true. This can be supplied as a JVM system property, as a configuration parameter in the LauncherDiscoveryRequest that is passed to the Launcher, or via the JUnit Platform configuration file (see Configuration Parameters for details).
>자동 감지는 고급 기능이므로 기본적으로 활성화되어 있지 않습니다. 활성화하려면 `junit.jupiter.extensions.autodetection.enabled` 구성 매개변수를 `true`로 설정하십시오. 이는 JVM 시스템 속성, `Launcher`에 전달되는 `LauncherDiscoveryRequest`의 구성 매개변수 또는 JUnit 플랫폼 구성 파일을 통해 제공될 수 있습니다(자세한 내용은 `구성 매개변수` 참조).

For example, to enable auto-detection of extensions, you can start your JVM with the following system property.
>예를 들어 확장 자동 감지를 활성화하려면 다음 시스템 속성으로 JVM을 시작할 수 있습니다.

`-Djunit.jupiter.extensions.autodetection.enabled=true`

When auto-detection is enabled, extensions discovered via the ServiceLoader mechanism will be added to the extension registry after JUnit Jupiter’s global extensions (e.g., support for TestInfo, TestReporter, etc.).
>자동 감지가 활성화되면 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 발견된 확장은 JUnit Jupiter의 전역 확장(예: `TestInfo`, `TestReporter` 등에 대한 지원) 이후 확장 레지스트리에 추가됩니다.

5.2.4. Extension Inheritance

Registered extensions are inherited within test class hierarchies with top-down semantics. Similarly, extensions registered at the class-level are inherited at the method-level. Furthermore, a specific extension implementation can only be registered once for a given extension context and its parent contexts. Consequently, any attempt to register a duplicate extension implementation will be ignored.
>등록된 확장은 하향식 의미 체계를 사용하여 테스트 클래스 계층 내에서 상속됩니다. 마찬가지로 클래스 수준에서 등록된 확장은 메소드 수준에서 상속됩니다. 또한 특정 확장 구현은 주어진 확장 컨텍스트 및 해당 상위 컨텍스트에 대해 한 번만 등록할 수 있습니다. 결과적으로 중복 확장 구현을 등록하려는 모든 시도는 무시됩니다.

5.3. Conditional Test Execution

ExecutionCondition defines the Extension API for programmatic, conditional test execution.
>[ExecutionCondition](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ExecutionCondition.html)은 프로그래밍 방식의 *조건부 테스트 실행*을 위한 `확장` API를 정의합니다.

An ExecutionCondition is evaluated for each container (e.g., a test class) to determine if all the tests it contains should be executed based on the supplied ExtensionContext. Similarly, an ExecutionCondition is evaluated for each test to determine if a given test method should be executed based on the supplied ExtensionContext.
>`ExecutionCondition`은 포함된 모든 테스트가 제공된 `ExtensionContext`를 기반으로 실행되어야 하는지 결정하기 위해 각 컨테이너(예: 테스트 클래스)에 대해 평가됩니다. 마찬가지로 `ExecutionCondition`은 제공된 `ExtensionContext`를 기반으로 주어진 테스트 메소드를 실행해야 하는지 여부를 결정하기 위해 각 테스트에 대해 평가됩니다.

When multiple ExecutionCondition extensions are registered, a container or test is disabled as soon as one of the conditions returns disabled. Thus, there is no guarantee that a condition is evaluated because another extension might have already caused a container or test to be disabled. In other words, the evaluation works like the short-circuiting boolean OR operator.
>여러 `ExecutionCondition` 확장이 등록되면 조건 중 하나가 *disabled*됨을 반환하는 즉시 컨테이너 또는 테스트가 비활성화됩니다. 따라서 다른 확장으로 인해 이미 컨테이너 또는 테스트가 비활성화되었을 수 있으므로 조건이 평가된다는 보장이 없습니다. 즉, 평가는 단락 부울 OR 연산자처럼 작동합니다.

See the source code of DisabledCondition and @Disabled for concrete examples.
>구체적인 예는 [DisabledCondition](https://github.com/junit-team/junit5/blob/r5.8.2/junit-jupiter-engine/src/main/java/org/junit/jupiter/engine/extension/DisabledCondition.java) 및 [@Disabled](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Disabled.html)의 소스 코드를 참조하십시오.

5.3.1. Deactivating Conditions
Sometimes it can be useful to run a test suite without certain conditions being active. For example, you may wish to run tests even if they are annotated with @Disabled in order to see if they are still broken. To do this, provide a pattern for the junit.jupiter.conditions.deactivate configuration parameter to specify which conditions should be deactivated (i.e., not evaluated) for the current test run. The pattern can be supplied as a JVM system property, as a configuration parameter in the LauncherDiscoveryRequest that is passed to the Launcher, or via the JUnit Platform configuration file (see Configuration Parameters for details).
>때로는 특정 조건이 활성화되지 않은 상태에서 test suite를 실행하는 것이 유용할 수 있습니다. 예를 들어, `@Disabled` annotation이 달린 경우에도 테스트를 실행하여 여전히 손상되었는지 확인할 수 있습니다. 이를 수행하려면 `junit.jupiter.conditions.deactivate` 구성 매개변수에 패턴을 제공하여 현재 테스트 실행에 대해 비활성화(즉, 평가되지 않음)해야 하는 조건을 지정합니다. 패턴은 JVM 시스템 속성, `Launcher`에 전달되는 `LauncherDiscoveryRequest`의 구성 매개변수 또는 JUnit 플랫폼 구성 파일을 통해 제공될 수 있습니다(자세한 내용은 `구성 매개변수` 참조).

For example, to deactivate JUnit’s @Disabled condition, you can start your JVM with the following system property.
>예를 들어, JUnit의 `@Disabled` 조건을 비활성화하려면 다음 시스템 속성으로 JVM을 시작할 수 있습니다.

`-Djunit.jupiter.conditions.deactivate=org.junit.*DisabledCondition`

Pattern Matching Syntax
Refer to `Pattern Matching Syntax` for details.

5.4. Test Instance Factories

TestInstanceFactory defines the API for Extensions that wish to create test class instances.
>`TestInstanceFactory`는 테스트 클래스 인스턴스를 생성하려는 `Extension`용 API를 정의합니다.

Common use cases include acquiring the test instance from a dependency injection framework or invoking a static factory method to create the test class instance.
>일반적인 사용 사례에는 종속성 주입 프레임워크에서 테스트 인스턴스를 획득하거나 테스트 클래스 인스턴스를 생성하기 위해 정적 팩토리 메소드를 호출하는 것이 포함됩니다.

If no TestInstanceFactory is registered, the framework will invoke the sole constructor for the test class to instantiate it, potentially resolving constructor arguments via registered ParameterResolver extensions.
>`TestInstanceFactory`가 등록되지 않은 경우 프레임워크는 테스트 클래스에 대한 유일한 생성자를 호출하여 인스턴스화하고 등록된 `ParameterResolver` 확장을 통해 잠재적으로 생성자 인수를 해결할 수 있습니다.

Extensions that implement TestInstanceFactory can be registered on test interfaces, top-level test classes, or @Nested test classes.
>`TestInstanceFactory`를 구현하는 확장은 테스트 인터페이스, 최상위 테스트 클래스 또는 `@Nested` 테스트 클래스에 등록할 수 있습니다.

>>Registering multiple extensions that implement TestInstanceFactory for any single class will result in an exception being thrown for all tests in that class, in any subclass, and in any nested class. Note that any TestInstanceFactory registered in a superclass or enclosing class (i.e., in the case of a @Nested test class) is inherited. It is the user’s responsibility to ensure that only a single TestInstanceFactory is registered for any specific test class.
>단일 클래스에 대해 `TestInstanceFactory`를 구현하는 여러 확장을 등록하면 해당 클래스, subclasses 및 nested 클래스의 모든 테스트에 대해 예외가 throw됩니다. 수퍼클래스 또는 *인클로징* 클래스(예: @Nested 테스트 클래스의 경우)에 등록된 모든 `TestInstanceFactory`는 *상속됩니다*. 특정 테스트 클래스에 대해 하나의 `TestInstanceFactory`만 등록되도록 하는 것은 사용자의 책임입니다.

5.5. Test Instance Post-processing

TestInstancePostProcessor defines the API for Extensions that wish to post process test instances.
>`TestInstancePostProcessor`는 테스트 인스턴스를 사후 처리하려는 `Extension`용 API를 정의합니다.

Common use cases include injecting dependencies into the test instance, invoking custom initialization methods on the test instance, etc.
>일반적인 사용 사례에는 테스트 인스턴스에 종속성 주입, 테스트 인스턴스에서 사용자 지정 초기화 메소드 호출 등이 포함됩니다.

For a concrete example, consult the source code for the MockitoExtension and the SpringExtension.
>구체적인 예는 [MockitoExtension](https://github.com/mockito/mockito/blob/release/2.x/subprojects/junit-jupiter/src/main/java/org/mockito/junit/jupiter/MockitoExtension.java) 및 [SpringExtension](https://github.com/spring-projects/spring-framework/blob/305055d6b1a42c7795891b7b389936ed80270505/spring-test/src/main/java/org/springframework/test/context/junit/jupiter/SpringExtension.java)에 대한 소스 코드를 참조하십시오.

5.6. Test Instance Pre-destroy Callback

TestInstancePreDestroyCallback defines the API for Extensions that wish to process test instances after they have been used in tests and before they are destroyed.
>`TestInstancePreDestroyCallback`은 테스트에서 사용된 *후* 소멸되기 *전*에 테스트 인스턴스를 처리하려는 확장용 API를 정의합니다.

Common use cases include cleaning dependencies that have been injected into the test instance, invoking custom de-initialization methods on the test instance, etc.
>일반적인 사용 사례에는 테스트 인스턴스에 주입된 종속성 정리, 테스트 인스턴스에서 사용자 지정 초기화 해제 메소드 호출 등이 있습니다.

5.7. Parameter Resolution

ParameterResolver defines the Extension API for dynamically resolving parameters at runtime.
>ParameterResolver는 런타임에 매개변수를 동적으로 해석하기 위한 확장 API를 정의합니다.

If a test class constructor, test method, or lifecycle method (see Test Classes and Methods) declares a parameter, the parameter must be resolved at runtime by a ParameterResolver. A ParameterResolver can either be built-in (see TestInfoParameterResolver) or registered by the user. Generally speaking, parameters may be resolved by name, type, annotation, or any combination thereof.
>*테스트 클래스* 생성자, 테스트 *메소드* 또는 *수명 주기* 메소드(`테스트 클래스 및 메소드(Test Classes and Methods)` 참조)가 매개변수를 선언하는 경우 매개변수는 `ParameterResolver`에 의해 런타임에 확인되어야 합니다. `ParameterResolver`는 built-in되거나(`TestInfoParameterResolver` 참조) `사용자가 등록(registered by the user)`할 수 있습니다. 일반적으로 매개변수는 이름, 유형, annotation 또는 이들의 조합으로 확인할 수 있습니다.

If you wish to implement a custom ParameterResolver that resolves parameters based solely on the type of the parameter, you may find it convenient to extend the TypeBasedParameterResolver which serves as a generic adapter for such use cases.
>매개변수 유형에만 기반하여 매개변수를 확인하는 사용자 지정 `ParameterResolver`를 구현하려는 경우 이러한 사용 사례에 대한 일반 어댑터 역할을 하는 `TypeBasedParameterResolver`를 확장하는 것이 편리할 수 있습니다.

For concrete examples, consult the source code for CustomTypeParameterResolver, CustomAnnotationParameterResolver, and MapOfListsTypeBasedParameterResolver.
>구체적인 예는 `CustomTypeParameterResolver`, `CustomAnnotationParameterResolver` 및 `MapOfListsTypeBasedParameterResolver`에 대한 소스 코드를 참조하십시오.

>>Due to a bug in the byte code generated by javac on JDK versions prior to JDK 9, looking up annotations on parameters directly via the core java.lang.reflect.Parameter API will always fail for inner class constructors (e.g., a constructor in a @Nested test class).
>JDK 9 이전 버전의 JDK에서 javac에 의해 생성된 바이트 코드의 버그로 인해 핵심 `java.lang.reflect.Parameter` API를 통해 직접 매개변수에 대한 annotation을 찾는 것은 내부 클래스 생성자(예: `@nested` 테스트 클래스).

>>The ParameterContext API supplied to ParameterResolver implementations therefore includes the following convenience methods for correctly looking up annotations on parameters. Extension authors are strongly encouraged to use these methods instead of those provided in java.lang.reflect.Parameter in order to avoid this bug in the JDK.
>따라서 `ParameterResolver` 구현에 제공되는 [ParameterContext](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ParameterContext.html) API에는 매개변수에 대한 annotation을 올바르게 조회하기 위한 다음과 같은 편리한 메소드가 포함됩니다. 확장 작성자는 JDK에서 이 버그를 방지하기 위해 `java.lang.reflect.Parameter`에 제공된 메소드 대신 이러한 메소드를 사용하는 것이 좋습니다.

>>- `boolean isAnnotated(Class<? extends Annotation> annotationType)`
>>- `Optional<A> findAnnotation(Class<A> annotationType)`
>>- `List<A> findRepeatableAnnotations(Class<A> annotationType)`

5.8. Test Result Processing

TestWatcher defines the API for extensions that wish to process the results of test method executions. Specifically, a TestWatcher will be invoked with contextual information for the following events.
>[TestWatcher](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestWatcher.html)는 테스트 메소드 실행 결과를 처리하려는 확장에 대한 API를 정의합니다. 특히 `TestWatcher`는 다음 이벤트에 대한 컨텍스트 정보와 함께 호출됩니다.

- testDisabled: invoked after a disabled test method has been skipped
- testSuccessful: invoked after a test method has completed successfully
- testAborted: invoked after a test method has been aborted
- testFailed: invoked after a test method has failed
>- `testDisabled`: 비활성화된 테스트 메소드를 건너뛴 후 호출됩니다.
>- `testSuccessful`: 테스트 메소드가 성공적으로 완료된 후 호출됨
>- `testAborted`: 테스트 메소드가 중단된 후 호출됨
>- `testFailed`: 테스트 메소드가 실패한 후 호출됨

>>In contrast to the definition of "test method" presented in Test Classes and Methods, in this context test method refers to any @Test method or @TestTemplate method (for example, a @RepeatedTest or @ParameterizedTest).
>`테스트 클래스 및 메서드`에 제시된 "테스트 메서드"의 정의와 달리 이 컨텍스트에서 테스트 메서드는 `@Test` 메서드 또는 @`TestTemplate` 메서드(예: @`RepeatedTest` 또는 @`ParameterizedTest`)를 나타냅니다.

Extensions implementing this interface can be registered at the method level or at the class level. In the latter case they will be invoked for any contained test method including those in @Nested classes.
>이 인터페이스를 구현하는 확장은 메소드 수준 또는 클래스 수준에서 등록할 수 있습니다. 후자의 경우 `@Nested` 클래스를 포함하여 포함된 모든 *테스트 메소드*에 대해 호출됩니다.


>>Any instances of ExtensionContext.Store.CloseableResource stored in the Store of the provided ExtensionContext will be closed before methods in this API are invoked (see Keeping State in Extensions). You can use the parent context’s Store to work with such resources.
>제공된 [ExtensionContext](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ExtensionContext.html)의 저장소에 저장된 `ExtensionContext.Store.CloseableResource`의 모든 인스턴스는 이 API의 메소드가 호출되기 *전*에 닫힙니다(`확장에서 상태 유지(Keeping State in Extensions)` 참조). 부모 컨텍스트의 `Store`를 사용하여 이러한 리소스로 작업할 수 있습니다.

5.9. Test Lifecycle Callbacks

The following interfaces define the APIs for extending tests at various points in the test execution lifecycle. Consult the following sections for examples and the Javadoc for each of these interfaces in the org.junit.jupiter.api.extension package for further details.
>다음 인터페이스는 테스트 실행 수명 주기의 다양한 지점에서 테스트를 확장하기 위한 API를 정의합니다. 예제는 다음 섹션을 참조하고 자세한 내용은 [org.junit.jupiter.api.extension](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/package-summary.html) 패키지의 이러한 각 인터페이스에 대한 Javadoc을 참조하십시오.

- [BeforeAllCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/BeforeAllCallback.html)
    - [BeforeEachCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/BeforeEachCallback.html)
        - [BeforeTestExecutionCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/BeforeTestExecutionCallback.html)
        - [AfterTestExecutionCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/AfterTestExecutionCallback.html)
    - [AfterEachCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/AfterEachCallback.html)
- [AfterAllCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/AfterAllCallback.html)

>>*Implementing Multiple Extension APIs*
Extension developers may choose to implement any number of these interfaces within a single extension. Consult the source code of the SpringExtension for a concrete example.
>확장 개발자는 단일 확장 내에서 이러한 인터페이스를 원하는 수만큼 구현하도록 선택할 수 있습니다. 구체적인 예는 [SpringExtension](https://github.com/spring-projects/spring-framework/blob/305055d6b1a42c7795891b7b389936ed80270505/spring-test/src/main/java/org/springframework/test/context/junit/jupiter/SpringExtension.java)의 소스 코드를 참조하십시오.

5.9.1. Before and After Test Execution Callbacks

BeforeTestExecutionCallback and AfterTestExecutionCallback define the APIs for Extensions that wish to add behavior that will be executed immediately before and immediately after a test method is executed, respectively. As such, these callbacks are well suited for timing, tracing, and similar use cases. If you need to implement callbacks that are invoked around @BeforeEach and @AfterEach methods, implement BeforeEachCallback and AfterEachCallback instead.
>[BeforeTestExecutionCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/BeforeTestExecutionCallback.html)과 [AfterTestExecutionCallback](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/AfterTestExecutionCallback.html) 은 각각 테스트 메소드가 실행되기 직전과 직후에 실행될 동작을 추가하고자 하는 Extension용 API를 정의합니다. 따라서 이러한 콜백은 타이밍, 추적 및 유사한 사용 사례에 매우 적합합니다. `@BeforeEach` 및 `@AfterEach` 메소드 주변에서 호출되는 콜백을 구현해야 하는 경우 대신 `BeforeEachCallback` 및 `AfterEachCallback`을 구현합니다.

The following example shows how to use these callbacks to calculate and log the execution time of a test method. TimingExtension implements both BeforeTestExecutionCallback and AfterTestExecutionCallback in order to time and log the test execution.
>다음 예제에서는 이러한 콜백을 사용하여 테스트 메소드의 실행 시간을 계산하고 기록하는 방법을 보여줍니다. `TimingExtension`은 테스트 실행의 시간을 측정하고 기록하기 위해 `BeforeTestExecutionCallback`과 `AfterTestExecutionCallback`을 모두 구현합니다.

`An extension that times and logs the execution of test methods`
>테스트 메소드의 실행 시간을 측정하고 기록하는 확장

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

Since the TimingExtensionTests class registers the TimingExtension via @ExtendWith, its tests will have this timing applied when they execute.
>`TimingExtensionTests` 클래스는 `@ExtendWith`를 통해 `TimingExtension`을 등록하기 때문에 테스트가 실행될 때 이 타이밍이 적용됩니다.

`A test class that uses the example TimingExtension`
>TimingExtension 예제를 사용하는 테스트 클래스

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

The following is an example of the logging produced when TimingExtensionTests is run.
>다음은 `TimingExtensionTests` 실행 시 생성되는 로깅의 예이다.

```
INFO: Method [sleep20ms] took 24 ms.
INFO: Method [sleep50ms] took 53 ms.
```

5.10. Exception Handling

Exceptions thrown during the test execution may be intercepted and handled accordingly before propagating further, so that certain actions like error logging or resource releasing may be defined in specialized Extensions. JUnit Jupiter offers API for Extensions that wish to handle exceptions thrown during @Test methods via TestExecutionExceptionHandler and for those thrown during one of test lifecycle methods (@BeforeAll, @BeforeEach, @AfterEach and @AfterAll) via LifecycleMethodExecutionExceptionHandler.
>테스트 실행 중에 발생한 예외는 추가로 전파되기 전에 가로채서 처리될 수 있으므로 오류 로깅 또는 리소스 해제와 같은 특정 작업이 특수 확장에 정의될 수 있습니다. JUnit Jupiter는 [TestExecutionExceptionHandler](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestExecutionExceptionHandler.html)를 통해 `@Test` 메소드 동안 throw된 예외와 [LifecycleMethodExecutionExceptionHandler](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/LifecycleMethodExecutionExceptionHandler.html)를 통해 테스트 수명 주기 메소드(`@BeforeAll`, `@BeforeEach`, `@AfterEach` 및 `@AfterAll`) 중 하나에서 throw된 예외를 처리하려는 `Extension`용 API를 제공합니다.

The following example shows an extension which will swallow all instances of IOException but rethrow any other type of exception.
>다음 예제는 `IOException`의 모든 인스턴스를 삼키지만 다른 유형의 예외를 다시 발생시키는 확장을 보여줍니다.

`An exception handling extension that filters IOExceptions in test execution`
>테스트 실행에서 IOException을 필터링하는 예외 처리 확장

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

Another example shows how to record the state of an application under test exactly at the point of unexpected exception being thrown during setup and cleanup. Note that unlike relying on lifecycle callbacks, which may or may not be executed depending on the test status, this solution guarantees execution immediately after failing @BeforeAll, @BeforeEach, @AfterEach or @AfterAll.
>또 다른 예는 설정 및 정리 중에 예기치 않은 예외가 발생하는 지점에서 테스트 중인 응용 프로그램의 상태를 정확히 기록하는 방법을 보여줍니다. 테스트 상태에 따라 실행되거나 실행되지 않을 수 있는 수명 주기 콜백에 의존하는 것과 달리 이 솔루션은 `@BeforeAll`, `@BeforeEach`, `@AfterEach` 또는 `@AfterAll` 실패 직후 실행을 보장합니다.

*An exception handling extension that records application state on error*
>오류 시 애플리케이션 상태를 기록하는 예외 처리 확장

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

Multiple execution exception handlers may be invoked for the same lifecycle method in order of declaration. If one of the handlers swallows the handled exception, subsequent ones will not be executed, and no failure will be propagated to JUnit engine, as if the exception was never thrown. Handlers may also choose to rethrow the exception or throw a different one, potentially wrapping the original.
>동일한 수명 주기 메소드에 대해 선언 순서대로 여러 실행 예외 처리기를 호출할 수 있습니다. 처리기 중 하나가 처리된 예외를 삼키면 후속 처리가 실행되지 않고 예외가 발생하지 않은 것처럼 JUnit 엔진에 오류가 전파되지 않습니다. 핸들러는 또한 예외를 다시 throw하거나 다른 예외를 throw하여 잠재적으로 원본을 래핑하도록 선택할 수 있습니다.

Extensions implementing LifecycleMethodExecutionExceptionHandler that wish to handle exceptions thrown during @BeforeAll or @AfterAll need to be registered on a class level, while handlers for BeforeEach and AfterEach may be also registered for individual test methods.
>`@BeforeAll` 또는 `@AfterAll` 중에 발생한 예외를 처리하려는 [LifecycleMethodExecutionExceptionHandler](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/LifecycleMethodExecutionExceptionHandler.html)를 구현하는 확장은 클래스 수준에서 등록해야 하는 반면, `BeforeEach` 및 `AfterEach`에 대한 처리기는 개별 테스트 메소드에 대해 등록할 수도 있습니다.

*Registering multiple exception handling extensions*
>여러 예외 처리 확장 등록

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

5.11. Intercepting Invocations

InvocationInterceptor defines the API for Extensions that wish to intercept calls to test code.
>[InvocationInterceptor](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/InvocationInterceptor.html) 테스트 코드에 대한 호출을 가로채려는 `확장`을 위한 API를 정의합니다.

The following example shows an extension that executes all test methods in Swing’s Event Dispatch Thread.
>다음 예제는 Swing의 Event Dispatch Thread에서 모든 테스트 메소드를 실행하는 확장을 보여줍니다.

*An extension that executes tests in a user-defined thread*
>사용자 정의 스레드에서 테스트를 실행하는 확장

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

5.12. Providing Invocation Contexts for Test Templates

A @TestTemplate method can only be executed when at least one TestTemplateInvocationContextProvider is registered. Each such provider is responsible for providing a Stream of TestTemplateInvocationContext instances. Each context may specify a custom display name and a list of additional extensions that will only be used for the next invocation of the @TestTemplate method.
>[@TestTemplate](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestTemplate.html) method는 하나 이상의 [TestTemplateInvocationContextProvider](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestTemplateInvocationContextProvider.html)가 등록된 경우에만 실행할 수 있습니다. 이러한 각 공급자는 [TestTemplateInvocationContext](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestTemplateInvocationContext.html) 인스턴스의 스트림을 제공할 책임이 있습니다. 각 컨텍스트는 사용자 지정 display name과 [@TestTemplate](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/TestTemplate.html) 메소드의 다음 호출에만 사용할 추가 확장 목록을 지정할 수 있습니다.

The following example shows how to write a test template as well as how to register and implement a TestTemplateInvocationContextProvider.
>다음 예제에서는 테스트 템플릿을 작성하는 방법과 [TestTemplateInvocationContextProvider](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestTemplateInvocationContextProvider.html)를 등록하고 구현하는 방법을 보여줍니다.

*A test template with accompanying extension*
>확장이 수반되는 테스트 템플릿

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

In this example, the test template will be invoked twice. The display names of the invocations will be apple and banana as specified by the invocation context. Each invocation registers a custom ParameterResolver which is used to resolve the method parameter. The output when using the ConsoleLauncher is as follows.
>이 예에서 테스트 템플릿은 두 번 호출됩니다. 호출의 display name은 호출 컨텍스트에 지정된 대로 `사과` 및 `바나나`입니다. 각 호출은 메소드 매개 변수를 확인하는 데 사용되는 사용자 지정 [ParameterResolver](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/ParameterResolver.html)를 등록합니다. ConsoleLauncher 사용 시 출력은 다음과 같습니다.

```
└─ testTemplate(String) ✔
   ├─ apple ✔
   └─ banana ✔
```

The TestTemplateInvocationContextProvider extension API is primarily intended for implementing different kinds of tests that rely on repetitive invocation of a test-like method albeit in different contexts — for example, with different parameters, by preparing the test class instance differently, or multiple times without modifying the context. Please refer to the implementations of Repeated Tests or Parameterized Tests which use this extension point to provide their functionality.
>[TestTemplateInvocationContextProvider](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/extension/TestTemplateInvocationContextProvider.html) 확장 API는 기본적으로 다른 컨텍스트(예: 다른 매개변수를 사용하여 테스트 클래스 인스턴스를 다르게 준비하거나 수정하지 않고 여러 번)에서라도 테스트와 유사한 메소드의 반복적인 호출에 의존하는 다양한 종류의 테스트를 구현하기 위한 것입니다. 문맥. 기능을 제공하기 위해 이 확장점을 사용하는 `repeated test` 또는 `parameterized test`의 구현을 참조하십시오.

5.13. Keeping State in Extensions

Usually, an extension is instantiated only once. So the question becomes relevant: How do you keep the state from one invocation of an extension to the next? The ExtensionContext API provides a Store exactly for this purpose. Extensions may put values into a store for later retrieval. See the TimingExtension for an example of using the Store with a method-level scope. It is important to remember that values stored in an ExtensionContext during test execution will not be available in the surrounding ExtensionContext. Since ExtensionContexts may be nested, the scope of inner contexts may also be limited. Consult the corresponding Javadoc for details on the methods available for storing and retrieving values via the Store.
>일반적으로 확장은 한 번만 인스턴스화됩니다. 따라서 질문은 관련이 있습니다. 한 번의 확장 호출에서 다음 호출로 상태를 유지하는 방법은 무엇입니까? `ExtensionContext` API는 정확히 이 목적을 위해 `Store`를 제공합니다. 확장은 나중에 검색하기 위해 값을 저장소에 저장할 수 있습니다. 메소드 수준 범위와 함께 저장소를 사용하는 예는 `TimingExtension`을 참조하세요. 테스트 실행 중에 `ExtensionContext`에 저장된 값은 주변 `ExtensionContext`에서 사용할 수 없다는 것을 기억하는 것이 중요합니다. `ExtensionContext`는 nested될 수 있으므로 내부 컨텍스트의 범위도 제한될 수 있습니다. `Store`를 통해 값을 저장하고 검색하는 데 사용할 수 있는 방법에 대한 자세한 내용은 해당 Javadoc을 참조하세요.

>>`ExtensionContext.Store.CloseableResource`
An extension context store is bound to its extension context lifecycle. When an extension context lifecycle ends it closes its associated store. All stored values that are instances of CloseableResource are notified by an invocation of their close() method in the inverse order they were added in.
>확장 컨텍스트 저장소는 확장 컨텍스트 수명 주기에 바인딩됩니다. 확장 컨텍스트 수명 주기가 끝나면 연결된 저장소가 닫힙니다. `CloseableResource`의 인스턴스인 모든 저장된 값은 추가된 역순으로 해당 `close()` 메소드를 호출하여 알림을 받습니다.

5.14. Supported Utilities in Extensions

The junit-platform-commons artifact exposes a package named org.junit.platform.commons.support that contains maintained utility methods for working with annotations, classes, reflection, and classpath scanning tasks. TestEngine and Extension authors are encouraged to use these supported methods in order to align with the behavior of the JUnit Platform.
>`junit-platform-commons` artifact는 annotation, 클래스, 리플렉션 및 클래스 경로 스캔 작업 작업을 위한 *유지 관리* 유틸리티 메소드가 포함된 `org.junit.platform.commons.support`라는 패키지를 노출합니다. `TestEngine` 및 `Extension` 작성자는 JUnit 플랫폼의 동작에 맞추기 위해 이러한 지원 방법을 사용하는 것이 좋습니다.

5.14.1. Annotation Support

AnnotationSupport provides static utility methods that operate on annotated elements (e.g., packages, annotations, classes, interfaces, constructors, methods, and fields). These include methods to check whether an element is annotated or meta-annotated with a particular annotation, to search for specific annotations, and to find annotated methods and fields in a class or interface. Some of these methods search on implemented interfaces and within class hierarchies to find annotations. Consult the Javadoc for AnnotationSupport for further details.
>`AnnotationSupport`는 annotation이 달린 요소(예: 패키지, annotation, 클래스, 인터페이스, 생성자, 메소드 및 필드)에서 작동하는 정적 유틸리티 메소드를 제공합니다. 여기에는 요소가 특정 annotation으로 annotation 처리되었는지 또는 meta-annotation 처리되었는지 확인하고, 특정 annotation을 검색하고, 클래스 또는 인터페이스에서 annotation이 달린 메소드 및 필드를 찾는 메소드가 포함됩니다. 이러한 메소드 중 일부는 구현된 인터페이스와 클래스 계층 내에서 검색하여 annotation을 찾습니다. 자세한 내용은 [AnnotationSupport](https://junit.org/junit5/docs/current/api/org.junit.platform.commons/org/junit/platform/commons/support/AnnotationSupport.html)에 대한 Javadoc을 참조하십시오.

5.14.2. Class Support

ClassSupport provides static utility methods for working with classes (i.e., instances of java.lang.Class). Consult the Javadoc for ClassSupport for further details.
>`ClassSupport`는 클래스(즉, `java.lang.Class`의 인스턴스) 작업을 위한 정적 유틸리티 메소드를 제공합니다. 자세한 내용은 [ClassSupport](https://junit.org/junit5/docs/current/api/org.junit.platform.commons/org/junit/platform/commons/support/ClassSupport.html)에 대한 Javadoc을 참조하십시오.

5.14.3. Reflection Support

ReflectionSupport provides static utility methods that augment the standard JDK reflection and class-loading mechanisms. These include methods to scan the classpath in search of classes matching specified predicates, to load and create new instances of a class, and to find and invoke methods. Some of these methods traverse class hierarchies to locate matching methods. Consult the Javadoc for ReflectionSupport for further details.
>`ReflectionSupport`는 표준 JDK 리플렉션 및 클래스 로딩 메커니즘을 보강하는 정적 유틸리티 메소드를 제공합니다. 여기에는 지정된 술어와 일치하는 클래스를 검색하여 클래스 경로를 스캔하고, 클래스의 새 인스턴스를 로드 및 생성하고, 메소드를 찾고 호출하는 메소드가 포함됩니다. 이러한 메소드 중 일부는 클래스 계층을 탐색하여 일치하는 메소드를 찾습니다. 자세한 내용은 [ReflectionSupport](https://junit.org/junit5/docs/current/api/org.junit.platform.commons/org/junit/platform/commons/support/ReflectionSupport.html)에 대한 Javadoc을 참조하십시오.

5.14.4. Modifier Support

ModifierSupport provides static utility methods for working with member and class modifiers — for example, to determine if a member is declared as public, private, abstract, static, etc. Consult the Javadoc for ModifierSupport for further details.
>`ModifierSupport`는 멤버 및 클래스 수정자  작업을 위한 정적 유틸리티 메소드를 제공합니다. 예를 들어 멤버가 `public`, `private`, `abstract`, `static` 등으로 선언되었는지 확인합니다. 자세한 내용은 [ModifierSupport](https://junit.org/junit5/docs/current/api/org.junit.platform.commons/org/junit/platform/commons/support/ModifierSupport.html)에 대한 Javadoc을 참조하세요.

5.15. Relative Execution Order of User Code and Extensions

When executing a test class that contains one or more test methods, a number of extension callbacks are called in addition to the user-supplied test and lifecycle methods.
>하나 이상의 테스트 메소드가 포함된 테스트 클래스를 실행할 때 사용자 제공 테스트 및 수명 주기 메소드 외에도 여러 확장 콜백이 호출됩니다.

>>See also: `Test Execution Order`

5.15.1. User and Extension Code

The following diagram illustrates the relative order of user-supplied code and extension code. User-supplied test and lifecycle methods are shown in orange, with callback code implemented by extensions shown in blue. The grey box denotes the execution of a single test method and will be repeated for every test method in the test class.
>다음 다이어그램은 사용자 제공 코드와 확장 코드의 상대적 순서를 보여줍니다. 사용자 제공 테스트 및 수명 주기 method는 주황색으로 표시되고 콜백 코드는 파란색으로 표시된 확장으로 구현됩니다. 회색 상자는 단일 테스트 메소드의 실행을 나타내며 테스트 클래스의 모든 테스트 메소드에 대해 반복됩니다.

extensions lifecycle
![image](../images/junit5_guide_extensions_lifecycle.png)
*User code and extension code*

The following table further explains the sixteen steps in the User code and extension code diagram.
>다음 표에서는 `사용자 코드 및 확장 코드 다이어그램`의 16단계에 대해 자세히 설명합니다.

|Step|Interface/Annotation|Description|
|---|---|---|
|1|interface org.junit.jupiter.api.extension.BeforeAllCallback|extension code executed before all tests of the container are executed|
|2|annotation org.junit.jupiter.api.BeforeAll|user code executed before all tests of the container are executed|
|3|interface org.junit.jupiter.api.extension.LifecycleMethodExecutionExceptionHandler #handleBeforeAllMethodExecutionException|extension code for handling exceptions thrown from @BeforeAll methods|
|4|interface org.junit.jupiter.api.extension.BeforeEachCallback|extension code executed before each test is executed|
|5|annotation org.junit.jupiter.api.BeforeEach|user code executed before each test is executed|
|6|interface org.junit.jupiter.api.extension.LifecycleMethodExecutionExceptionHandler #handleBeforeEachMethodExecutionException|extension code for handling exceptions thrown from @BeforeEach methods|
|7|interface org.junit.jupiter.api.extension.BeforeTestExecutionCallback|extension code executed immediately before a test is executed|
|8|annotation org.junit.jupiter.api.Test|user code of the actual test method|
|9|interface org.junit.jupiter.api.extension.TestExecutionExceptionHandler|extension code for handling exceptions thrown during a test|
|10|interface org.junit.jupiter.api.extension.AfterTestExecutionCallback|extension code executed immediately after test execution and its corresponding exception handlers|
|11|annotation org.junit.jupiter.api.AfterEach|user code executed after each test is executed|
|12|interface org.junit.jupiter.api.extension.LifecycleMethodExecutionExceptionHandler #handleAfterEachMethodExecutionException|extension code for handling exceptions thrown from @AfterEach methods|
|13|interface org.junit.jupiter.api.extension.AfterEachCallback|extension code executed after each test is executed|
|14|annotation org.junit.jupiter.api.AfterAll|user code executed after all tests of the container are executed|
|15|interface org.junit.jupiter.api.extension.LifecycleMethodExecutionExceptionHandler #handleAfterAllMethodExecutionException|extension code for handling exceptions thrown from @AfterAll methods|
|16|interface org.junit.jupiter.api.extension.AfterAllCallback|extension code executed after all tests of the container are executed|

>|단계|인터페이스/annotation|설명|
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

In the simplest case only the actual test method will be executed (step 8); all other steps are optional depending on the presence of user code or extension support for the corresponding lifecycle callback. For further details on the various lifecycle callbacks please consult the respective Javadoc for each annotation and extension.
>가장 단순한 경우에는 실제 테스트 방법만 실행됩니다(8단계). 다른 모든 단계는 해당 수명 주기 콜백에 대한 사용자 코드 또는 확장 지원의 존재 여부에 따라 선택 사항입니다. 다양한 수명 주기 콜백에 대한 자세한 내용은 각 annotation 및 확장에 대한 해당 Javadoc을 참조하십시오.

All invocations of user code methods in the above table can additionally be intercepted by implementing InvocationInterceptor.
>위 표의 모든 사용자 코드 메소드 호출은 `InvocationInterceptor`를 구현하여 추가로 가로챌 수 있습니다.

5.15.2. Wrapping Behavior of Callbacks

JUnit Jupiter always guarantees wrapping behavior for multiple registered extensions that implement lifecycle callbacks such as BeforeAllCallback, AfterAllCallback, BeforeEachCallback, AfterEachCallback, BeforeTestExecutionCallback, and AfterTestExecutionCallback.
>JUnit Jupiter는 `BeforeAllCallback`, `AfterAllCallback`, `BeforeEachCallback`, `AfterEachCallback`, `BeforeTestExecutionCallback` 및 `AfterTestExecutionCallback`과 같은 수명 주기 콜백을 구현하는 여러 등록된 확장에 대한 래핑 동작을 항상 보장합니다.

That means that, given two extensions Extension1 and Extension2 with Extension1 registered before Extension2, any "before" callbacks implemented by Extension1 are guaranteed to execute before any "before" callbacks implemented by Extension2. Similarly, given the two same two extensions registered in the same order, any "after" callbacks implemented by Extension1 are guaranteed to execute after any "after" callbacks implemented by Extension2. Extension1 is therefore said to wrap Extension2.
>즉, `Extension1`이 `Extension2`보다 먼저 등록된 `Extension1`과 `Extension2`가 두 개 있는 경우 `Extension1`에 의해 구현된 모든 "이전" 콜백은 `Extension2`에 의해 구현된 "이전" 콜백보다 **먼저** 실행되도록 보장됩니다. 유사하게, 동일한 순서로 등록된 두 개의 동일한 확장이 주어지면 `Extension1`에 의해 구현된 "이후" 콜백은 `Extension2`에 의해 구현된 모든 "이후" 콜백 **이후**에 실행되도록 보장됩니다. 따라서 `Extension1`은 `Extension2`를 래핑한다고 합니다.

JUnit Jupiter also guarantees wrapping behavior within class and interface hierarchies for user-supplied lifecycle methods (see Test Classes and Methods).
>JUnit Jupiter는 또한 사용자 제공 *수명 주기* 메소드에 대한 클래스 및 인터페이스 계층 내 *래핑* 동작을 보장합니다(`테스트 클래스 및 메소드` 참조).

- @BeforeAll methods are inherited from superclasses as long as they are not hidden or overridden. Furthermore, @BeforeAll methods from superclasses will be executed before @BeforeAll methods in subclasses.
    - Similarly, @BeforeAll methods declared in an interface are inherited as long as they are not hidden or overridden, and @BeforeAll methods from an interface will be executed before @BeforeAll methods in the class that implements the interface.

- @AfterAll methods are inherited from superclasses as long as they are not hidden or overridden. Furthermore, @AfterAll methods from superclasses will be executed after @AfterAll methods in subclasses.
    - Similarly, @AfterAll methods declared in an interface are inherited as long as they are not hidden or overridden, and @AfterAll methods from an interface will be executed after @AfterAll methods in the class that implements the interface.

- @BeforeEach methods are inherited from superclasses as long as they are not overridden. Furthermore, @BeforeEach methods from superclasses will be executed before @BeforeEach methods in subclasses.
    - Similarly, @BeforeEach methods declared as interface default methods are inherited as long as they are not overridden, and @BeforeEach default methods will be executed before @BeforeEach methods in the class that implements the interface.

- @AfterEach methods are inherited from superclasses as long as they are not overridden. Furthermore, @AfterEach methods from superclasses will be executed after @AfterEach methods in subclasses.
    - Similarly, @AfterEach methods declared as interface default methods are inherited as long as they are not overridden, and @AfterEach default methods will be executed after @AfterEach methods in the class that implements the interface.

>- `@BeforeAll` method는 *숨겨지거나 재정의되지 않는 한* 슈퍼클래스에서 *상속*됩니다. 또한 슈퍼클래스의 `@BeforeAll` method는 서브클래스의 `@BeforeAll` 메소드보다 **먼저** 실행됩니다.
>    - 마찬가지로 인터페이스에 선언된 `@BeforeAll` method는 *숨겨지거나 재정의되지 않는 한* *상속*되며, 인터페이스의 `@BeforeAll` method는 인터페이스를 구현하는 클래스의 `@BeforeAl`l 메소드보다 **먼저** 실행됩니다.
>- `@AfterAll` method는 *숨겨지거나 재정의되지 않는 한* 슈퍼클래스에서 *상속*됩니다. 또한 슈퍼클래스의 `@AfterAll` method는 서브클래스의 `@AfterAll` 메소드 **다음**에 실행됩니다.
>    - 마찬가지로 인터페이스에 선언된 `@AfterAll` 메소드는 *숨겨지거나 재정의되지 않는 한* *상속*되며, 인터페이스의 `@AfterAll` 메소드는 인터페이스를 구현하는 클래스의 `@AfterAl`l 메소드 **다음**에 실행됩니다.
>- `@BeforeEach` method는 *재정의되지 않는 한* 슈퍼클래스에서 *상속*됩니다. 또한 슈퍼클래스의 `@BeforeEach` method는 서브클래스의 `@BeforeEach` 메소드보다 **먼저** 실행됩니다.
>    - 마찬가지로 인터페이스 기본 메소드로 선언된 `@BeforeEach` method는 *재정의되지 않는 한* *상속*되며, `@BeforeEach` 기본 method는 인터페이스를 구현하는 클래스의 `@BeforeEach` 메소드보다 **먼저** 실행됩니다.
>- `@AfterEach` method는 *재정의되지 않는 한* 슈퍼클래스에서 *상속*됩니다. 또한 슈퍼클래스의 `@AfterEach` method는 서브클래스의 `@AfterEach` 메소드 **다음**에 실행됩니다.
>    - 마찬가지로, 인터페이스 기본 메소드로 선언된 `@AfterEach` 메소드는 *재정의되지 않는 한* *상속*되며, `@AfterEach` 기본 메소드는 인터페이스를 구현한 클래스의 `@AfterEach` 메소드 **이후**에 실행됩니다.

The following examples demonstrate this behavior. Please note that the examples do not actually do anything realistic. Instead, they mimic common scenarios for testing interactions with the database. All methods imported statically from the Logger class log contextual information in order to help us better understand the execution order of user-supplied callback methods and callback methods in extensions.
>다음 예제에서는 이 동작을 보여줍니다. 예제는 실제로 현실적이지 않다는 점에 유의하십시오. 대신 데이터베이스와의 상호 작용을 테스트하기 위한 일반적인 시나리오를 모방합니다. `Logger` 클래스에서 정적으로 가져온 모든 method는 확장에서 사용자 제공 콜백 메소드 및 콜백 메소드의 실행 순서를 더 잘 이해할 수 있도록 컨텍스트 정보를 기록합니다.

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

When the DatabaseTestsDemo test class is executed, the following is logged.
>`DatabaseTestsDemo` 테스트 클래스를 실행하면 다음과 같이 기록된다.

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

The following sequence diagram helps to shed further light on what actually goes on within the JupiterTestEngine when the DatabaseTestsDemo test class is executed.
>다음 시퀀스 다이어그램은 `DatabaseTestsDemo` 테스트 클래스가 실행될 때 `JupiterTestEngine` 내에서 실제로 무슨 일이 일어나는지 더 자세히 설명하는 데 도움이 됩니다.

![image](../images/juni5_guide_extensions_DatabaseTestsDemo.png)
*DatabaseTestsDemo*

JUnit Jupiter does not guarantee the execution order of multiple lifecycle methods that are declared within a single test class or test interface. It may at times appear that JUnit Jupiter invokes such methods in alphabetical order. However, that is not precisely true. The ordering is analogous to the ordering for @Test methods within a single test class.
>JUnit Jupiter는 단일 테스트 클래스 또는 테스트 인터페이스 내에서 선언된 여러 수명 주기 메소드의 실행 순서를 **보장하지 않습니다**. JUnit Jupiter가 알파벳 순서로 이러한 메소드를 호출하는 것처럼 보일 수 있습니다. 그러나 그것은 정확히 사실이 아닙니다. 순서는 단일 테스트 클래스 내에서 `@Test` 메소드의 순서와 유사합니다.

Lifecycle methods that are declared within a single test class or test interface will be ordered using an algorithm that is deterministic but intentionally non-obvious. This ensures that subsequent runs of a test suite execute lifecycle methods in the same order, thereby allowing for repeatable builds.
>단일 테스트 클래스 또는 테스트 인터페이스 내에서 선언된 수명 주기 method는 결정적이지만 의도적으로 명확하지 않은 알고리즘을 사용하여 정렬됩니다. 이렇게 하면 test suite의 후속 실행이 동일한 순서로 수명 주기 메소드를 실행하므로 반복 가능한 빌드가 가능합니다.

In addition, JUnit Jupiter does not support wrapping behavior for multiple lifecycle methods declared within a single test class or test interface.
>또한 JUnit Jupiter는 단일 테스트 클래스 또는 테스트 인터페이스 내에서 선언된 여러 수명 주기 메소드에 대한 래핑 동작을 **지원하지 않습니다**.

The following example demonstrates this behavior. Specifically, the lifecycle method configuration is broken due to the order in which the locally declared lifecycle methods are executed.
>다음 예제에서는 이 동작을 보여줍니다. 특히, 로컬로 선언된 수명 주기 메소드가 실행되는 순서로 인해 수명 주기 메소드 구성이 손상됩니다.

Test data is inserted before the database connection has been opened, which results in a failure to connect to the database.
>데이터베이스 연결이 열리기 *전에* 테스트 데이터가 삽입되어 데이터베이스 연결에 실패합니다.

The database connection is closed before deleting the test data, which results in a failure to connect to the database.
>테스트 데이터를 삭제하기 *전에* 데이터베이스 연결이 닫히므로 데이터베이스 연결에 실패합니다.

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

When the BrokenLifecycleMethodConfigDemo test class is executed, the following is logged.
>`BrokenLifecycleMethodConfigDemo` 테스트 클래스를 실행하면 다음과 같이 기록된다.

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

The following sequence diagram helps to shed further light on what actually goes on within the JupiterTestEngine when the BrokenLifecycleMethodConfigDemo test class is executed.
>다음 시퀀스 다이어그램은 `BrokenLifecycleMethodConfigDemo` 테스트 클래스가 실행될 때 `JupiterTestEngine` 내에서 실제로 무슨 일이 일어나는지 더 자세히 설명하는 데 도움이 됩니다.

![image](../images/junit5_guide_extensions_BrokenLifecycleMethodConfigDemo.png)
*BrokenLifecycleMethodConfigDemo*

>>Due to the aforementioned behavior, the JUnit Team recommends that developers declare at most one of each type of lifecycle method (see Test Classes and Methods) per test class or test interface unless there are no dependencies between such lifecycle methods.
>앞서 언급한 동작으로 인해 JUnit 팀은 개발자가 이러한 수명 주기 메소드 간에 종속성이 없는 경우를 제외하고 테스트 클래스 또는 테스트 인터페이스별로 각 유형의 *수명 주기 메소드*(`테스트 클래스 및 메소드` 참조)를 최대 하나씩 선언할 것을 권장합니다.

6. Advanced Topics

6.1. JUnit Platform Launcher API

One of the prominent goals of JUnit 5 is to make the interface between JUnit and its programmatic clients – build tools and IDEs – more powerful and stable. The purpose is to decouple the internals of discovering and executing tests from all the filtering and configuration that’s necessary from the outside.
>JUnit 5의 주요 목표 중 하나는 JUnit과 프로그래밍 방식 클라이언트(빌드 도구 및 IDE) 간의 인터페이스를 보다 강력하고 안정적으로 만드는 것입니다. 목적은 외부에서 필요한 모든 필터링 및 구성에서 테스트를 검색하고 실행하는 내부를 분리하는 것입니다.

JUnit 5 introduces the concept of a Launcher that can be used to discover, filter, and execute tests. Moreover, third party test libraries – like Spock, Cucumber, and FitNesse – can plug into the JUnit Platform’s launching infrastructure by providing a custom TestEngine.
>JUnit 5는 테스트를 검색, 필터링 및 실행하는 데 사용할 수 있는 `Launcher`의 개념을 도입했습니다. 또한 Spock, Cucumber 및 FitNesse와 같은 타사 테스트 라이브러리는 사용자 지정 [TestEngine](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/TestEngine.html)을 제공하여 JUnit 플랫폼의 출시 인프라에 연결할 수 있습니다.

The launcher API is in the junit-platform-launcher module.
>launcher API는 [junit-platform-launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/package-summary.html) 모듈에 있습니다

An example consumer of the launcher API is the ConsoleLauncher in the junit-platform-console project.
>launcher API의 예시 소비자는 [junit-platform-console](https://junit.org/junit5/docs/current/api/org.junit.platform.console/org/junit/platform/console/package-summary.html) 프로젝트의 [ConsoleLauncher](https://junit.org/junit5/docs/current/api/org.junit.platform.console/org/junit/platform/console/ConsoleLauncher.html)입니다.

6.1.1. Discovering Tests

Having test discovery as a dedicated feature of the platform itself frees IDEs and build tools from most of the difficulties they had to go through to identify test classes and test methods in previous versions of JUnit.
>플랫폼 자체의 전용 기능으로 *test discovery*을 사용하면 이전 버전의 JUnit에서 테스트 클래스와 테스트 방법을 식별하기 위해 겪었던 대부분의 어려움에서 IDE 및 빌드 도구를 자유롭게 할 수 있습니다.

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

You can select classes, methods, and all classes in a package or even search for all tests in the class-path or module-path. Discovery takes place across all participating test engines.
>패키지의 클래스, 메소드 및 모든 클래스를 선택하거나 클래스 경로 또는 모듈 경로에서 모든 테스트를 검색할 수도 있습니다. 검색은 참여하는 모든 테스트 엔진에서 발생합니다.

The resulting TestPlan is a hierarchical (and read-only) description of all engines, classes, and test methods that fit the LauncherDiscoveryRequest. The client can traverse the tree, retrieve details about a node, and get a link to the original source (like class, method, or file position). Every node in the test plan has a unique ID that can be used to invoke a particular test or group of tests.
>결과 `TestPlan`은 `LauncherDiscoveryRequest`에 맞는 모든 엔진, 클래스 및 테스트 방법에 대한 계층적(읽기 전용) 설명입니다. 클라이언트는 트리를 탐색하고 노드에 대한 세부 정보를 검색하고 원본 소스(클래스, 메소드 또는 파일 위치와 같은)에 대한 링크를 얻을 수 있습니다. 테스트 계획의 모든 노드에는 특정 테스트 또는 테스트 그룹을 호출하는 데 사용할 수 있는 *unique ID*가 있습니다.

Clients can register one or more LauncherDiscoveryListener implementations via the LauncherDiscoveryRequestBuilder to gain insight into events that occur during test discovery. By default, the builder registers an "abort on failure" listener that aborts test discovery after the first discovery failure is encountered. The default LauncherDiscoveryListener can be changed via the junit.platform.discovery.listener.default configuration parameter.
>클라이언트는 [LauncherDiscoveryRequestBuilder](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherDiscoveryRequestBuilder.html)를 통해 하나 이상의 [LauncherDiscoveryListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryListener.html) 구현을 등록하여 테스트 검색 중에 발생하는 이벤트에 대한 통찰력을 얻을 수 있습니다. 기본적으로 빌더는 첫 번째 검색 실패가 발생한 후 테스트 검색을 중단하는 "실패 시 중단" 수신기를 등록합니다. 기본 `LauncherDiscoveryListener`는 `junit.platform.discovery.listener.default` `구성 매개변수`를 통해 변경할 수 있습니다.

6.1.2. Executing Tests

To execute tests, clients can use the same LauncherDiscoveryRequest as in the discovery phase or create a new request. Test progress and reporting can be achieved by registering one or more TestExecutionListener implementations with the Launcher as in the following example.
>테스트를 실행하기 위해 클라이언트는 검색 단계에서와 동일한 `LauncherDiscoveryRequest`를 사용하거나 새 요청을 생성할 수 있습니다. 다음 예제와 같이 런처에 하나 이상의 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html) 구현을 등록하여 테스트 진행 및 보고를 수행할 수 있습니다.

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

There is no return value for the execute() method, but you can use a TestExecutionListener to aggregate the results. For examples see the SummaryGeneratingListener, LegacyXmlReportGeneratingListener, and UniqueIdTrackingListener.
>`execute()` 메소드에 대한 반환 값은 없지만 `TestExecutionListener`를 사용하여 결과를 집계할 수 있습니다. 예제는 [SummaryGeneratingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/SummaryGeneratingListener.html), [LegacyXmlReportGeneratingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.reporting/org/junit/platform/reporting/legacy/xml/LegacyXmlReportGeneratingListener.html) 및 [UniqueIdTrackingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/UniqueIdTrackingListener.html)를 참조하십시오.

6.1.3. Registering a TestEngine

JUnit provides three [TestEngine](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/TestEngine.html) implementations.

- [junit-jupiter-engine](https://junit.org/junit5/docs/current/api/org.junit.jupiter.engine/org/junit/jupiter/engine/package-summary.html): The core of JUnit Jupiter.
- [junit-vintage-engine](https://junit.org/junit5/docs/current/api/org.junit.vintage.engine/org/junit/vintage/engine/package-summary.html): A thin layer on top of JUnit 4 to allow running vintage tests with the launcher infrastructure.
- [junit-platform-suite-engine](https://junit.org/junit5/docs/current/api/org.junit.platform.suite.engine/org/junit/platform/suite/engine/package-summary.html): To execute declarative suites of tests with the launcher infrastructure.

Third parties may also contribute their own TestEngine by implementing the interfaces in the junit-platform-engine module and registering their engine. Engine registration is supported via Java’s ServiceLoader mechanism. For example, the junit-jupiter-engine module registers its org.junit.jupiter.engine.JupiterTestEngine in a file named org.junit.platform.engine.TestEngine within the /META-INF/services folder in the junit-jupiter-engine JAR.
>타사는 [junit-platform-engine](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/package-summary.html) 모듈에서 인터페이스를 구현하고 엔진을 등록하여 자체 `TestEngine`을 제공할 수도 있습니다. 엔진 등록은 Java의 `ServiceLoader` 메커니즘을 통해 지원됩니다. 예를 들어, `junit-jupiter-engine` 모듈은 `junit-jupiter-engine`의 `/META-INF/services` 폴더에 있는 `org.junit.platform.engine.TestEngine`이라는 파일에 `org.junit.jupiter.engine.JupiterTestEngine` JAR를 등록합니다.

>>HierarchicalTestEngine is a convenient abstract base implementation (used by the junit-jupiter-engine) that only requires implementors to provide the logic for test discovery. It implements execution of TestDescriptors that implement the Node interface, including support for parallel execution.
>[HierarchicalTestEngine](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/support/hierarchical/HierarchicalTestEngine.html)은 ([junit-jupiter-engine](https://junit.org/junit5/docs/current/api/org.junit.jupiter.engine/org/junit/jupiter/engine/package-summary.html)에서 사용하는) 편리한 추상 기본 구현으로, 구현자가 테스트 검색을 위한 논리를 제공하기만 하면 됩니다. 병렬 실행 지원을 포함하여 `node` 인터페이스를 구현하는 `TestDescriptors`의 실행을 구현합니다.

>>The junit- prefix is reserved for TestEngines from the JUnit Team
The JUnit Platform Launcher enforces that only TestEngine implementations published by the JUnit Team may use the junit- prefix for their TestEngine IDs.
>*`junit-` 접두사는 JUnit 팀의 TestEngines용으로 예약되어 있습니다.*
JUnit 플랫폼 `launcher`는 JUnit 팀에서 게시한 `TestEngine` 구현만 `TestEngine` ID에 대해 `junit-` 접두사를 사용할 수 있도록 합니다.

>>- If any third-party TestEngine claims to be junit-jupiter or junit-vintage, an exception will be thrown, immediately halting execution of the JUnit Platform.
>>- If any third-party TestEngine uses the junit- prefix for its ID, a warning message will be logged. Later releases of the JUnit Platform will throw an exception for such violations.
>- 타사 `TestEngine`이 `junit-jupiter` 또는 `junit-vintage`라고 주장하는 경우 예외가 발생하여 JUnit 플랫폼의 실행이 즉시 중단됩니다.
>- 타사 `TestEngine`이 ID로 `junit-` 접두사를 사용하는 경우 경고 메시지가 기록됩니다. JUnit 플랫폼의 이후 릴리스에서는 이러한 위반에 대해 예외가 발생합니다.

6.1.4. Registering a PostDiscoveryFilter

In addition to specifying post-discovery filters as part of a LauncherDiscoveryRequest passed to the Launcher API, PostDiscoveryFilter implementations will be discovered at runtime via Java’s ServiceLoader mechanism and automatically applied by the Launcher in addition to those that are part of the request.
>[Launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/Launcher.html) API에 전달된 [LauncherDiscoveryRequest](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryRequest.html)의 일부로 사후 검색 필터를 지정하는 것 외에도 [PostDiscoveryFilter](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/PostDiscoveryFilter.html) 구현은 런타임에 Java의 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 검색되고 요청의 일부인 것 외에 `Launcher`에 의해 자동으로 적용됩니다.

For example, an example.CustomTagFilter class implementing PostDiscoveryFilter and declared within the `/META-INF/services/org.junit.platform.launcher.PostDiscoveryFilter` file is loaded and applied automatically.
>예를 들어 `PostDiscoveryFilter`를 구현하고 `/META-INF/services/org.junit.platform.launcher.PostDiscoveryFilter` 파일 내에 선언된 `example.CustomTagFilter` 클래스가 자동으로 로드되어 적용됩니다.

6.1.5. Registering a LauncherSessionListener

Registered implementations of LauncherSessionListener are notified when a LauncherSession is opened (before a Launcher first discovers and executes tests) and closed (when no more tests will be discovered or executed). They can be registered programmatically via the LauncherConfig that is passed to the LauncherFactory, or they can be discovered at runtime via Java’s ServiceLoader mechanism and automatically registered with LauncherSession (unless automatic registration is disabled.)
>[LauncherSessionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSessionListener.html)의 등록된 구현은 [LauncherSession](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSession.html)이 열릴 때([Launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/Launcher.html)가 테스트를 처음 발견하고 실행하기 전) 닫힐 때(더 이상 테스트가 발견되거나 실행되지 않을 때) 알림을 받습니다. [LauncherFactory](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherFactory.html)에 전달되는 [LauncherConfig](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherConfig.html)를 통해 프로그래밍 방식으로 등록하거나 Java의 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 런타임에 검색하고 [LauncherSession](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSession.html)에 자동으로 등록할 수 있습니다(자동 등록이 비활성화되지 않은 경우).

A LauncherSessionListener is well suited for implementing once-per-JVM setup/teardown behavior since it’s called before the first and after the last test in a launcher session, respectively. The scope of a launcher session depends on the used IDE or build tool but usually corresponds to the lifecycle of the test JVM. A custom listener that starts an HTTP server before executing the first test and stops it after the last test has been executed, could look like this:
>[LauncherSessionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherSessionListener.html)는 각각 런처 세션의 첫 번째 테스트 이전과 마지막 테스트 이후에 호출되기 때문에 JVM당 한 번 설정/해제 동작을 구현하는 데 적합합니다. 런처 세션의 범위는 사용된 IDE 또는 빌드 도구에 따라 다르지만 일반적으로 테스트 JVM의 수명 주기에 해당합니다. 첫 번째 테스트를 실행하기 전에 HTTP 서버를 시작하고 마지막 테스트가 실행된 후에 중지하는 사용자 지정 수신기는 다음과 같습니다.

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

This sample uses the HTTP server implementation from the jdk.httpserver module that comes with the JDK but would work similarly with any other server or resource. In order for the listener to be picked up by JUnit Platform, you need to register it as a service by adding a resource file with the following name and contents to your test runtime classpath (e.g. by adding the file to src/test/resources):
>이 샘플은 JDK와 함께 제공되지만 다른 서버 또는 리소스와 유사하게 작동하는 jdk.httpserver 모듈의 HTTP 서버 구현을 사용합니다. JUnit 플랫폼에서 리스너를 선택하려면 테스트 런타임 클래스 경로에 다음 이름과 내용의 리소스 파일을 추가하여 서비스로 등록해야 합니다(예:` src/test/resources`에 파일 추가). :

*src/test/resources/META-INF/services/org.junit.platform.launcher.LauncherSessionListener
*
`example.session.GlobalSetupTeardownListener
`
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

6.1.6. Registering a LauncherDiscoveryListener

In addition to specifying discovery listeners as part of a LauncherDiscoveryRequest or registering them programmatically via the Launcher API, custom LauncherDiscoveryListener implementations can be discovered at runtime via Java’s ServiceLoader mechanism and automatically registered with the Launcher created via the LauncherFactory.
>[LauncherDiscoveryRequest](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryRequest.html)의 일부로 검색 수신기를 지정하거나 [Launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/Launcher.html) API를 통해 프로그래밍 방식으로 등록하는 것 외에도 사용자 지정 `LauncherDiscoveryListener` 구현은 Java의 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 런타임에 검색되고 [LauncherFactory](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherFactory.html)를 통해 생성된 `Launcher`에 자동으로 등록될 수 있습니다.

For example, an example.CustomLauncherDiscoveryListener class implementing LauncherDiscoveryListener and declared within the /META-INF/services/org.junit.platform.launcher.LauncherDiscoveryListener file is loaded and registered automatically.
>예를 들어 `LauncherDiscoveryListener`를 구현하고 `/META-INF/services/org.junit.platform.launcher.LauncherDiscoveryListener` 파일 내에 선언된 `example.CustomLauncherDiscoveryListener` 클래스가 자동으로 로드되어 등록됩니다.

6.1.7. Registering a TestExecutionListener

In addition to the public Launcher API method for registering test execution listeners programmatically, custom TestExecutionListener implementations will be discovered at runtime via Java’s ServiceLoader mechanism and automatically registered with the Launcher created via the LauncherFactory.
>프로그래밍 방식으로 테스트 실행 리스너를 등록하기 위한 공개 `Launcher` API 방법 외에도 사용자 지정 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html) 구현은 런타임 시 Java의 `ServiceLoader` 메커니즘을 통해 검색되고 `LauncherFactory`를 통해 생성된 Launcher에 자동으로 등록됩니다.

For example, an example.CustomTestExecutionListener class implementing TestExecutionListener and declared within the /META-INF/services/org.junit.platform.launcher.TestExecutionListener file is loaded and registered automatically.
>예를 들어 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html)를 구현하고 /`META-INF/services/org.junit.platform.launcher.TestExecutionListener` 파일 내에 선언된 `example.CustomTestExecutionListener` 클래스가 자동으로 로드되어 등록됩니다.

6.1.8. Configuring a TestExecutionListener

When a TestExecutionListener is registered programmatically via the Launcher API, the listener may provide programmatic ways for it to be configured — for example, via its constructor, setter methods, etc. However, when a TestExecutionListener is registered automatically via Java’s ServiceLoader mechanism (see Registering a TestExecutionListener), there is no way for the user to directly configure the listener. In such cases, the author of a TestExecutionListener may choose to make the listener configurable via configuration parameters. The listener can then access the configuration parameters via the TestPlan supplied to the testPlanExecutionStarted(TestPlan) and testPlanExecutionFinished(TestPlan) callback methods. See the UniqueIdTrackingListener for an example.
>[TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html)가 Launcher API를 통해 프로그래밍 방식으로 등록되면 리스너는 생성자, 설정자 메소드 등을 통해 구성할 프로그래밍 방식을 제공할 수 있습니다. 그러나 `TestExecutionListener`가 Java의 ServiceLoader 메커니즘(`Registering a TestExecutionListener` 참조), 사용자가 리스너를 직접 구성할 수 있는 방법이 없습니다. 이러한 경우 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html)의 작성자는 `구성 매개변수`를 통해 리스너를 구성할 수 있도록 선택할 수 있습니다. 그러면 리스너는 `testPlanExecutionStarted(TestPlan)` 및 `testPlanExecutionFinished(TestPlan)` 콜백 메소드에 제공된 TestPlan을 통해 구성 매개변수에 액세스할 수 있습니다. 예는 [UniqueIdTrackingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/listeners/UniqueIdTrackingListener.html)를 참조하십시오.

6.1.9. Deactivating a TestExecutionListener

Sometimes it can be useful to run a test suite without certain execution listeners being active. For example, you might have custom a TestExecutionListener that sends the test results to an external system for reporting purposes, and while debugging you might not want these debug results to be reported. To do this, provide a pattern for the junit.platform.execution.listeners.deactivate configuration parameter to specify which execution listeners should be deactivated (i.e. not registered) for the current test run.
>때로는 특정 실행 수신기가 활성화되지 않은 상태에서 test suite를 실행하는 것이 유용할 수 있습니다. 예를 들어, 보고 목적으로 테스트 결과를 외부 시스템으로 보내는 사용자 정의 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html)가 있을 수 있으며 디버깅하는 동안 이러한 디버그 결과가 보고되지 않기를 원할 수 있습니다. 이를 수행하려면 `junit.platform.execution.listeners.deactivate` *구성 매개변수*에 패턴을 제공하여 현재 테스트 실행에 대해 비활성화(즉, 등록되지 않음)해야 하는 실행 리스너를 지정합니다.

Only listeners registered via the ServiceLoader mechanism within the /META-INF/services/org.junit.platform.launcher.TestExecutionListener file can be deactivated. In other words, any TestExecutionListener registered explicitly via the LauncherDiscoveryRequest cannot be deactivated via the junit.platform.execution.listeners.deactivate configuration parameter.
>`/META-INF/services/org.junit.platform.launcher.TestExecutionListener` 파일 내에서 [ServiceLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/ServiceLoader.html) 메커니즘을 통해 등록된 리스너만 비활성화할 수 있습니다. 즉, [LauncherDiscoveryRequest](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryRequest.html)를 통해 명시적으로 등록된 `TestExecutionListener`는 `junit.platform.execution.listeners.deactivate` 구성 매개변수를 통해 비활성화할 수 없습니다.

In addition, since execution listeners are registered before the test run starts, the junit.platform.execution.listeners.deactivate configuration parameter can only be supplied as a JVM system property or via the JUnit Platform configuration file (see Configuration Parameters for details). This configuration parameter cannot be supplied in the LauncherDiscoveryRequest that is passed to the Launcher.
>또한 실행 리스너는 테스트 실행이 시작되기 전에 등록되기 때문에 `junit.platform.execution.listeners.deactivate` 구성 매개변수는 JVM 시스템 속성으로 또는 JUnit 플랫폼 구성 파일을 통해서만 제공될 수 있습니다(자세한 내용은 `구성 매개변수` 참조). 이 구성 매개변수는 [Launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/Launcher.html)에 전달되는 [LauncherDiscoveryRequest](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/LauncherDiscoveryRequest.html)에 제공할 수 없습니다.

Pattern Matching Syntax
Refer to `Pattern Matching Syntax` for details.

6.1.10. Configuring the Launcher

If you require fine-grained control over automatic detection and registration of test engines and listeners, you may create an instance of LauncherConfig and supply that to the LauncherFactory. Typically, an instance of LauncherConfig is created via the built-in fluent builder API, as demonstrated in the following example.
>테스트 엔진 및 리스너의 자동 감지 및 등록에 대한 세분화된 제어가 필요한 경우 [LauncherConfig](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherConfig.html) 인스턴스를 생성하고 이를 [LauncherFactory](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/core/LauncherFactory.html)에 제공할 수 있습니다. 일반적으로 `LauncherConfig`의 인스턴스는 다음 예제와 같이 내장된 Fluent *Builder* API를 통해 생성됩니다.

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

6.2. JUnit Platform Reporting

The junit-platform-reporting artifact contains TestExecutionListener implementations that generate test reports. These listeners are typically used by IDEs and build tools. The package org.junit.platform.reporting.legacy.xml currently contains the following implementation.
>`junit-platform-reporting` artifact는 테스트 보고서를 생성하는 [TestExecutionListener](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestExecutionListener.html) 구현을 포함합니다. 이러한 수신기는 일반적으로 IDE 및 빌드 도구에서 사용됩니다. `org.junit.platform.reporting.legacy.xml` 패키지에는 현재 다음 구현이 포함되어 있습니다.

LegacyXmlReportGeneratingListener generates a separate XML report for each root in the TestPlan. Note that the generated XML format is compatible with the de facto standard for JUnit 4 based test reports that was made popular by the Ant build system. The LegacyXmlReportGeneratingListener is used by the Console Launcher as well.
>[LegacyXmlReportGeneratingListener](https://junit.org/junit5/docs/current/api/org.junit.platform.reporting/org/junit/platform/reporting/legacy/xml/LegacyXmlReportGeneratingListener.html)는 [TestPlan](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestPlan.html)의 각 루트에 대해 별도의 XML 보고서를 생성합니다. 생성된 XML 형식은 Ant 빌드 시스템에서 널리 보급된 JUnit 4 기반 테스트 보고서의 사실상 표준과 호환됩니다. `LegacyXmlReportGeneratingListener`는 `Console Launcher`에서도 사용됩니다.

The junit-platform-launcher module also contains TestExecutionListener implementations that can be used for reporting purposes. See Using Listeners for details.
>[junit-platform-launcher](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/package-summary.html) 모듈에는 보고 목적으로 사용할 수 있는 `TestExecutionListener `구현도 포함되어 있습니다. 자세한 내용은 `리스너 사용(Using Listener)`을 참조하십시오.

6.3. JUnit Platform Suite Engine

The JUnit Platform supports the declarative definition and execution of suites of tests from any test engine using the JUnit Platform.
>JUnit 플랫폼은 JUnit 플랫폼을 사용하는 모든 테스트 엔진에서 test suite의 선언적 정의 및 실행을 지원합니다.

6.3.1. Setup

In addition to the junit-platform-suite-api and junit-platform-suite-engine artifacts, you need at least one other test engine and its dependencies on the classpath. See Dependency Metadata for details regarding group IDs, artifact IDs, and versions.
>`junit-platform-suite-api` 및 `junit-platform-suite-engine` artifact 외에도 적어도 하나의 다른 테스트 엔진과 클래스 경로에 대한 종속성이 필요합니다. 그룹 ID, artifact ID 및 버전에 대한 자세한 내용은 `종속성 메타데이터(Dependency Metadata)`를 참조하세요.

Required Dependencies
- junit-platform-suite-api in test scope: artifact containing annotations needed to configure a test suite
- junit-platform-suite-engine in test runtime scope: implementation of the TestEngine API for declarative test suites
>- 테스트 범위의 `junit-platform-suite-api`: test suite를 구성하는 데 필요한 annotation을 포함하는 artifact
>- 테스트 런타임 범위의 `junit-platform-suite-engine`: 선언적 test suite를 위한 `TestEngine` API 구현

>>Both of the required dependencies are aggregated in the junit-platform-suite artifact which can be declard in test scope instead of declaring explicit dependencies on junit-platform-suite-api and junit-platform-suite-engine.
>두 필수 종속성은 `junit-platform-suite-api` 및 `junit-platform-suite-engine`에 대한 명시적 종속성을 선언하는 대신 테스트 범위에서 선언할 수 있는 `junit-platform-suite` artifact에 집계됩니다.

Transitive Dependencies

- `junit-platform-suite-commons` in test scope
- `junit-platform-launcher` in test scope
- `junit-platform-engine` in test scope
- `junit-platform-commons` in test scope
- `opentest4j` in test scope

6.3.2. @Suite Example

By annotating a class with @Suite it is marked as a test suite on the JUnit Platform. As seen in the following example, selector and filter annotations can then be used to control the contents of the suite.
>`@Suite`로 클래스에 annotation을 추가하면 JUnit 플랫폼에서 test suite로 표시됩니다. 다음 예에서 볼 수 있듯이 선택기 및 필터 annotation을 사용하여 제품군의 내용을 제어할 수 있습니다.

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
There are numerous configuration options for discovering and filtering tests in a test suite. Please consult the Javadoc of the org.junit.platform.suite.api package for a full list of supported annotations and further details.
>test suite에서 테스트를 검색하고 필터링하기 위한 다양한 구성 옵션이 있습니다. 지원되는 annotation의 전체 목록과 추가 세부사항은 [org.junit.platform.suite.api](https://junit.org/junit5/docs/current/api/org.junit.platform.suite.api/org/junit/platform/suite/api/package-summary.html) 패키지의 Javadoc을 참조하십시오.

6.4. JUnit Platform Test Kit

The junit-platform-testkit artifact provides support for executing a test plan on the JUnit Platform and then verifying the expected results. As of JUnit Platform 1.4, this support is limited to the execution of a single TestEngine (see Engine Test Kit).
>`junit-platform-testkit` artifact는 JUnit 플랫폼에서 테스트 계획을 실행한 다음 예상 결과를 확인하기 위한 지원을 제공합니다. JUnit 플랫폼 1.4부터 이 지원은 단일 `TestEngine`의 실행으로 제한됩니다(`엔진 테스트 키트(Engine Test Kit)` 참조).

6.4.1. Engine Test Kit

The org.junit.platform.testkit.engine package provides support for executing a TestPlan for a given TestEngine running on the JUnit Platform and then accessing the results via a fluent API to verify the expected results. The key entry point into this API is the EngineTestKit which provides static factory methods named engine() and execute(). It is recommended that you select one of the engine() variants to benefit from the fluent API for building a LauncherDiscoveryRequest.
>[org.junit.platform.testkit.engine](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/package-summary.html) 패키지는 JUnit 플랫폼에서 실행되는 주어진 [TestEngine](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/TestEngine.html)에 대한 [TestPlan](https://junit.org/junit5/docs/current/api/org.junit.platform.launcher/org/junit/platform/launcher/TestPlan.html)을 실행한 다음 예상 결과를 확인하기 위해 유창한 API를 통해 결과에 액세스하기 위한 지원을 제공합니다. 이 API의 주요 진입점은 `engine()` 및 `execute()`라는 정적 팩토리 메소드를 제공하는 [EngineTestKit](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/EngineTestKit.html)입니다. `LauncherDiscoveryRequest`를 빌드하기 위한 유창한 API의 이점을 얻으려면 `engine()` 변형 중 하나를 선택하는 것이 좋습니다.

If you prefer to use the LauncherDiscoveryRequestBuilder from the Launcher API to build your LauncherDiscoveryRequest, you must use one of the execute() variants in EngineTestKit.
The following test class written using JUnit Jupiter will be used in subsequent examples.
>`Launcher` API에서 `LauncherDiscoveryRequestBuilder`를 사용하여 `LauncherDiscoveryRequest`를 빌드하려면 `EngineTestKit`에서 `execute()` 변형 중 하나를 사용해야 합니다.
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

For the sake of brevity, the following sections demonstrate how to test JUnit’s own JupiterTestEngine whose unique engine ID is "junit-jupiter". If you want to test your own TestEngine implementation, you need to use its unique engine ID. Alternatively, you may test your own TestEngine by supplying an instance of it to the EngineTestKit.engine(TestEngine) static factory method.
>간결함을 위해 다음 섹션에서는 고유한 엔진 ID가 `"junit-jupiter"`인 JUnit의 자체 `JupiterTestEngine`을 테스트하는 방법을 보여줍니다. 고유한 `TestEngine` 구현을 테스트하려면 고유한 엔진 ID를 사용해야 합니다. 또는 `EngineTestKit.engine(TestEngine)` 정적 팩토리 메소드에 인스턴스를 제공하여 고유한 `TestEngine`을 테스트할 수 있습니다.

6.4.2. Asserting Statistics

One of the most common features of the Test Kit is the ability to assert statistics against events fired during the execution of a TestPlan. The following tests demonstrate how to assert statistics for containers and tests in the JUnit Jupiter TestEngine. For details on what statistics are available, consult the Javadoc for EventStatistics.
>Test Kit의 가장 일반적인 기능 중 하나는 `TestPlan` 실행 중에 발생한 이벤트에 대한 통계를 주장하는 기능입니다. 다음 테스트는 JUnit Jupiter `TestEngine`에서 컨테이너 및 테스트에 대한 통계를 assertion하는 방법을 보여줍니다. 사용 가능한 통계에 대한 자세한 내용은 [EventStatistics](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/EventStatistics.html)에 대한 Javadoc을 참조하십시오.

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

In the verifyJupiterContainerStats() test method, the counts for the started and succeeded statistics are 2 since the JupiterTestEngine and the ExampleTestCase class are both considered containers.
>`verifyJupiterContainerStats()` 테스트 메소드에서 `JupiterTestEngine` 및 `ExampleTestCase` 클래스는 모두 컨테이너로 간주되기 때문에 시작 및 성공한 통계의 개수는 `2`입니다.

6.4.3. Asserting Events

If you find that asserting statistics alone is insufficient for verifying the expected behavior of test execution, you can work directly with the recorded Event elements and perform assertions against them.
>`assertion 통계(asserting statistics)`만으로는 테스트 실행의 예상 동작을 확인하는 데 충분하지 않은 경우 기록된 [이벤트](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/Event.html) 요소로 직접 작업하고 이에 대해 assertion을 수행할 수 있습니다.

For example, if you want to verify the reason that the skippedTest() method in ExampleTestCase was skipped, you can do that as follows.
>예를 들어, `ExampleTestCase`의 `skippedTest()` 메소드가 생략된 이유를 확인하려면 다음과 같이 하면 됩니다.

The assertThatEvents() method in the following example is a shortcut for org.assertj.core.api.Assertions.assertThat(events.list()) from the AssertJ assertion library.
>다음 예제의 `assertThatEvents()` method는 [AssertJ](https://joel-costigliola.github.io/assertj/) assertion 라이브러리의 `org.assertj.core.api.Assertions.assertThat(events.list())`에 대한 바로 가기입니다.

For details on what conditions are available for use with AssertJ assertions against events, consult the Javadoc for EventConditions.
>이벤트에 대한 AssertJ assertion과 함께 사용할 수 있는 조건에 대한 자세한 내용은 [EventConditions](https://junit.org/junit5/docs/current/api/org.junit.platform.testkit/org/junit/platform/testkit/engine/EventConditions.html)에 대한 Javadoc을 참조하십시오.

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

If you want to verify the type of exception thrown from the failingTest() method in ExampleTestCase, you can do that as follows.
>`ExampleTestCase`의 `failingTest()` 메소드에서 throw된 예외 유형을 확인하려면 다음과 같이 하면 됩니다.

>>For details on what conditions are available for use with AssertJ assertions against events and execution results, consult the Javadoc for EventConditions and TestExecutionResultConditions, respectively.
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

Although typically unnecessary, there are times when you need to verify all of the events fired during the execution of a TestPlan. The following test demonstrates how to achieve this via the assertEventsMatchExactly() method in the EngineTestKit API.
>일반적으로 불필요하지만 `TestPlan` 실행 중에 발생한 **모든** 이벤트를 확인해야 하는 경우가 있습니다. 다음 테스트는 `EngineTestKit` API의 `assertEventsMatchExactly()` 메소드를 통해 이를 달성하는 방법을 보여줍니다.

>>Since assertEventsMatchExactly() matches conditions exactly in the order in which the events were fired, ExampleTestCase has been annotated with @TestMethodOrder(OrderAnnotation.class) and each test method has been annotated with @Order(…​). This allows us to enforce the order in which the test methods are executed, which in turn allows our verifyAllJupiterEvents() test to be reliable.
>`assertEventsMatchExactly()`는 이벤트가 발생한 순서대로 조건을 정확히 일치시키기 때문에 `ExampleTestCase`는 `@TestMethodOrder(OrderAnnotation.class)`로 annotation 처리되었고 각 테스트 메소드는 `@Order(…​)`로 annotation 처리되었습니다. 이것은 우리가 테스트 메소드가 실행되는 순서를 강제할 수 있게 하여 우리의 `verifyAllJupiterEvents()` 테스트가 신뢰할 수 있게 해줍니다.

If you want to do a partial match with or without ordering requirements, you can use the methods assertEventsMatchLooselyInOrder() and assertEventsMatchLoosely(), respectively.
>순서 지정 요구 사항이 *있거나 없는* *부분* 일치를 수행하려는 경우 각각 `assertEventsMatchLooselyInOrder()` 및 `assertEventsMatchLoosely()` 메소드를 사용할 수 있습니다.

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

The debug() invocation from the preceding example results in output similar to the following.
>앞의 예제에서 `debug()`를 호출하면 다음과 유사한 출력이 나타납니다.

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

7. API Evolution

One of the major goals of JUnit 5 is to improve maintainers' capabilities to evolve JUnit despite its being used in many projects. With JUnit 4 a lot of stuff that was originally added as an internal construct only got used by external extension writers and tool builders. That made changing JUnit 4 especially difficult and sometimes impossible.
>JUnit 5의 주요 목표 중 하나는 JUnit이 많은 프로젝트에서 사용되고 있음에도 불구하고 JUnit을 발전시킬 수 있도록 유지 관리자의 능력을 향상시키는 것입니다. JUnit 4에서는 원래 내부 구성으로 추가되었던 많은 것들이 외부 확장 작성자와 도구 빌더에서만 사용되었습니다. 이로 인해 JUnit 4를 변경하는 것이 특히 어렵고 때로는 불가능했습니다.

That’s why JUnit 5 introduces a defined lifecycle for all publicly available interfaces, classes, and methods.
>이것이 JUnit 5가 공개적으로 사용 가능한 모든 인터페이스, 클래스 및 메소드에 대해 정의된 수명 주기를 도입한 이유입니다.

7.1. API Version and Status

Every published artifact has a version number <major>.<minor>.<patch>, and all publicly available interfaces, classes, and methods are annotated with @API from the @API Guardian project. The annotation’s status attribute can be assigned one of the following values.
>게시된 모든 artifact에는 버전 번호 `<major>`.`<minor>`.`<patch>`가 있으며 공개적으로 사용 가능한 모든 인터페이스, 클래스 및 method는 [@API Guardian](https://github.com/apiguardian-team/apiguardian) 프로젝트의 [@API](https://apiguardian-team.github.io/apiguardian/docs/current/api/org/apiguardian/api/package-summary.html)로 annotation 처리됩니다. annotation의 `상태` 속성은 다음 값 중 하나를 할당할 수 있습니다.

|Status|Description|
|---|---|
|INTERNAL|Must not be used by any code other than JUnit itself. Might be removed without prior notice.|
|DEPRECATED|Should no longer be used; might disappear in the next minor release.|
|EXPERIMENTAL|Intended for new, experimental features where we are looking for feedback. Use this element with caution; it might be promoted to MAINTAINED or STABLE in the future, but might also be removed without prior notice, even in a patch.|
|MAINTAINED|Intended for features that will not be changed in a backwards- incompatible way for at least the next minor release of the current major version. If scheduled for removal, it will be demoted to DEPRECATED first.|
|STABLE|Intended for features that will not be changed in a backwards- incompatible way in the current major version (5.*).|

|Status|Description|
|---|---|
|INTERNAL|JUnit 자체 이외의 다른 코드에서 사용하면 안 됩니다. 사전 통보 없이 삭제될 수 있습니다.|
|DEPRECATED|더 이상 사용되지 않아야 합니다. 다음 마이너 릴리스에서 사라질 수 있습니다.|
|EXPERIMENTAL|피드백을 찾고 있는 새롭고 실험적인 기능을 위한 것입니다. 이 요소는 주의해서 사용하십시오. 향후 `MAINTAINED` 또는 `STABLE`로 승격될 수 있지만 패치에서도 사전 통지 없이 제거될 수도 있습니다.|
|MAINTAINED|최소한 현재 주 버전의 다음 부 릴리스에 대해 이전 버전과 호환되지 않는 방식으로 변경되지 않는 기능을 위한 것입니다. 제거 예정인 경우 먼저 `DEPRECATED`로 강등됩니다.|
|STABLE|현재 주요 버전(5.*)에서 이전 버전과 호환되지 않는 방식으로 변경되지 않는 기능을 위한 것입니다.|

If the @API annotation is present on a type, it is considered to be applicable for all public members of that type as well. A member is allowed to declare a different status value of lower stability.
>`@API` annotation이 유형에 있는 경우 해당 유형의 모든 공개 멤버에도 적용 가능한 것으로 간주됩니다. 구성원은 낮은 안정성의 다른 `상태` 값을 선언할 수 있습니다.

7.2. Experimental APIs

The following table lists which APIs are currently designated as experimental via @API(status = EXPERIMENTAL). Caution should be taken when relying on such APIs.
>다음 표는 현재 `@API(status = EXPERIMENTAL)`를 통해 실험용으로 지정된 API를 나열합니다. 이러한 API에 의존할 때는 주의를 기울여야 합니다.

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

7.3. Deprecated APIs
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

7.4. @API Tooling Support
The @API Guardian project plans to provide tooling support for publishers and consumers of APIs annotated with @API. For example, the tooling support will likely provide a means to check if JUnit APIs are being used in accordance with @API annotation declarations.
>[@API Guardian](https://github.com/apiguardian-team/apiguardian) 프로젝트는 [@API](https://apiguardian-team.github.io/apiguardian/docs/current/api/org/apiguardian/api/package-summary.html) annotation이 달린 API의 게시자와 소비자를 위한 도구 지원을 제공할 계획입니다. 예를 들어, 도구 지원은 JUnit API가 `@API` annotation 선언에 따라 사용되고 있는지 확인하는 수단을 제공할 것입니다.

8. Contributors
Browse the [current list of contributors](https://github.com/junit-team/junit5/graphs/contributors) directly on GitHub.

9. Release Notes
The release notes are available [here](https://junit.org/junit5/docs/current/release-notes/index.html#release-notes).

10. Appendix

10.1. Reproducible Builds

Starting with version 5.7, JUnit 5 aims for its non-javadoc JARs to be reproducible.
>버전 5.7부터 JUnit 5는 non-javadoc JAR을 [재생산](https://reproducible-builds.org/)하는 것을 목표로 합니다.

Under identical build conditions, such as Java version, repeated builds should provide the same output byte-for-byte.
>Java 버전과 같은 동일한 빌드 조건에서 반복되는 빌드는 동일한 출력을 바이트 단위로 제공해야 합니다.

This means that anyone can reproduce the build conditions of the artifacts on Maven Central/Sonatype and produce the same output artifact locally, confirming that the artifacts in the repositories were actually generated from this source code.
>이는 누구나 Maven Central/Sonatype에서 artifact의 빌드 조건을 재현하고 동일한 출력 artifact를 로컬에서 생성할 수 있음을 의미하며, 저장소의 artifact가 실제로 이 소스 코드에서 생성되었음을 확인합니다.

10.2. Dependency Metadata
Artifacts for final releases and milestones are deployed to Maven Central, and snapshot artifacts are deployed to Sonatype’s snapshots repository under /org/junit.
>최종 릴리스 및 마일스톤에 대한 artifact는 [Maven Central](https://search.maven.org/)에 배포되고 스냅샷 artifact는 [/org/junit](https://oss.sonatype.org/content/repositories/snapshots/org/junit/) 아래의 Sonatype의 [snapshots repository](https://oss.sonatype.org/content/repositories/snapshots/)에 배포됩니다.

10.2.1. JUnit Platform

- Group ID: org.junit.platform
- Version: 1.8.2
- Artifact IDs:
&nbsp;&nbsp;&nbsp;`junit-platform-commons`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Common APIs and support utilities for the JUnit Platform. Any API annotated with `@API(status = INTERNAL)` is intended solely for usage within the JUnit framework itself. Any usage of internal APIs by external parties is not supported!
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼용 공통 API 및 지원 유틸리티. `@API(status = INTERNAL)` annotation이 달린 모든 API는 JUnit 프레임워크 자체 내에서만 사용하기 위한 것입니다. 외부 당사자의 내부 API 사용은 지원되지 않습니다!

&nbsp;&nbsp;&nbsp;`junit-platform-console`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Support for discovering and executing tests on the JUnit Platform from the console. See Console Launcher for details.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;콘솔에서 JUnit 플랫폼에 대한 테스트 검색 및 실행을 지원합니다. 자세한 내용은 `콘솔 런처(Console Launcher)`를 참조하십시오.

&nbsp;&nbsp;&nbsp;`junit-platform-console-standalone`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;An executable JAR with all dependencies included is provided in Maven Central under the junit-platform-console-standalone directory. See Console Launcher for details.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;모든 종속성이 포함된 실행 가능한 JAR은 Maven Central의 [junit-platform-console-standalone](https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/) 디렉토리 아래에 제공됩니다. 자세한 내용은 `콘솔 런처(Console Launcher)`를 참조하십시오.

&nbsp;&nbsp;&nbsp;`junit-platform-engine`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Public API for test engines. See Registering a TestEngine for details.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;테스트 엔진용 공개 API. 자세한 내용은 `TestEngine 등록`을 참조하십시오.

&nbsp;&nbsp;&nbsp;`junit-platform-jfr`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Provides a LauncherDiscoveryListener and TestExecutionListener for Java Flight Recorder events on the JUnit Platform. See Flight Recorder Support for details.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼의 Java Flight Recorder 이벤트에 대해 `LauncherDiscoveryListener` 및 `TestExecutionListener를` 제공합니다. 자세한 내용은 `비행 레코더 지원(Flight Recorder Support)`을 참조하십시오.

&nbsp;&nbsp;&nbsp;`junit-platform-launcher`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Public API for configuring and launching test plans — typically used by IDEs and build tools. See JUnit Platform Launcher API for details.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;테스트 계획 구성 및 실행을 위한 공개 API — 일반적으로 IDE 및 빌드 도구에서 사용합니다. 자세한 내용은 `JUnit 플랫폼 런처 API(JUnit Platform Launcher API)`를 참조하세요.

&nbsp;&nbsp;&nbsp;`junit-platform-reporting`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TestExecutionListener implementations that generate test reports — typically used by IDEs and build tools. See JUnit Platform Reporting for details.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;테스트 보고서  를 생성하는 `TestExecutionListener` 구현 -  일반적으로 IDE 및 빌드 도구에서 사용합니다. 자세한 내용은 `JUnit 플랫폼 보고(JUnit Platform Reporting)`를 참조하세요.

&nbsp;&nbsp;&nbsp;`junit-platform-runner`
Runner for executing tests and test suites on the JUnit Platform in a JUnit 4 environment. See Using JUnit 4 to run the JUnit Platform for details.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 4 환경의 JUnit 플랫폼에서 테스트 및 test suite를 실행하기 위한 runner. 자세한 내용은 `JUnit 4를 사용하여 JUnit 플랫폼 실행(Using JUnit 4 to run the JUnit Platform)`을 참조하십시오.

&nbsp;&nbsp;&nbsp;`junit-platform-suite`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit Platform Suite artifact that transitively pulls in dependencies on junit-platform-suite-api and junit-platform-suite-engine for simplified dependency management in build tools such as Gradle and Maven.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Gradle 및 Maven과 같은 빌드 도구에서 단순화된 종속성 관리를 위해 `junit-platform-suite-api` 및 `junit-platform-suite-engine`에 대한 종속성을 전이적으로 끌어오는 JUnit 플랫폼 제품군 artifact.

&nbsp;&nbsp;&nbsp;`junit-platform-suite-api`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Annotations for configuring test suites on the JUnit Platform. Supported by the JUnit Platform Suite Engine and the JUnitPlatform runner.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼에서 test suite를 구성하기 위한 annotation. `JUnit Platform Suite Engine` 및 `JUnitPlatform runner`에서 지원합니다.

&nbsp;&nbsp;&nbsp;`junit-platform-suite-commons`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Common support utilities for executing test suites on the JUnit Platform.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼에서 test suite를 실행하기 위한 공통 지원 유틸리티.

&nbsp;&nbsp;&nbsp;`junit-platform-suite-engine`
Engine that executes test suites on the JUnit Platform; only required at runtime. See JUnit Platform Suite Engine for details.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼에서 test suite를 실행하는 엔진. 런타임에만 필요합니다. 자세한 내용은` JUnit 플랫폼 제품군 엔진(JUnit Platform Suite Engine)`을 참조하세요.

&nbsp;&nbsp;&nbsp;`junit-platform-testkit`
Provides support for executing a test plan for a given TestEngine and then accessing the results via a fluent API to verify the expected results.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;주어진 `TestEngine`에 대한 테스트 계획을 실행한 다음 유창한 API를 통해 결과에 액세스하여 예상 결과를 확인하기 위한 지원을 제공합니다.

10.2.2. JUnit Jupiter

- Group ID: `org.junit.jupiter`
- Version: `5.8.2`
- Artifact IDs:

&nbsp;&nbsp;&nbsp;`junit-jupiter`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit Jupiter aggregator artifact that transitively pulls in dependencies on junit-jupiter-api, junit-jupiter-params, and junit-jupiter-engine for simplified dependency management in build tools such as Gradle and Maven.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Gradle 및 Maven과 같은 빌드 도구에서 단순화된 종속성 관리를 위해 `junit-jupiter-api`, `junit-jupiter-params` 및 `junit-jupiter-engine`에 대한 종속성을 전이적으로 끌어오는 JUnit Jupiter 수집기 artifact.

&nbsp;&nbsp;&nbsp;`junit-jupiter-api`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit Jupiter API for writing tests and extensions.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`테스트 작성(writing tests)` 및 `확장 작성(extensions)`하기 위한 JUnit Jupiter API.

&nbsp;&nbsp;&nbsp;`junit-jupiter-engine`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit Jupiter test engine implementation; only required at runtime.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit Jupiter 테스트 엔진 구현; 런타임에만 필요합니다.

&nbsp;&nbsp;&nbsp;`junit-jupiter-params`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Support for parameterized tests in JUnit Jupiter.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit Jupiter에서 `parameterized test`를 지원합니다.

&nbsp;&nbsp;&nbsp;`junit-jupiter-migrationsupport`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Support for migrating from JUnit 4 to JUnit Jupiter; only required for support for JUnit 4’s @Ignore annotation and for running selected JUnit 4 rules.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 4에서 JUnit Jupiter로의 migration 지원; JUnit 4의 @Ignore annotation을 지원하고 선택한 JUnit 4 규칙을 실행하는 데만 필요합니다.

10.2.3. JUnit Vintage

- Group ID: `org.junit.vintage`
- Version: `5.8.2`
- Artifact ID:
&nbsp;&nbsp;&nbsp;`junit-vintage-engine`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit Vintage test engine implementation that allows one to run vintage JUnit tests on the JUnit Platform. Vintage tests include those written using JUnit 3 or JUnit 4 APIs or tests written using testing frameworks built on those APIs.
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JUnit 플랫폼에서 빈티지 JUnit 테스트를 실행할 수 있게 해주는 JUnit 빈티지 테스트 엔진 구현. 빈티지 테스트에는 JUnit 3 또는 JUnit 4 API를 사용하여 작성된 테스트 또는 해당 API에 구축된 테스트 프레임워크를 사용하여 작성된 테스트가 포함됩니다.

10.2.4. Bill of Materials (BOM)

The Bill of Materials POM provided under the following Maven coordinates can be used to ease dependency management when referencing multiple of the above artifacts using Maven or Gradle.
>다음 Maven 좌표에서 제공되는 BOM POM은 [Maven](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Importing_Dependencies) 또는 [Gradle](https://docs.gradle.org/current/userguide/managing_transitive_dependencies.html#sec:bom_import)을 사용하여 위의 여러 artifact를 참조할 때 종속성 관리를 용이하게 하는 데 사용할 수 있습니다.

- Group ID: `org.junit`
- Artifact ID: `junit-bom`
- Version: `5.8.2`

10.2.5. Dependencies

Most of the above artifacts have a dependency in their published Maven POMs on the following @API Guardian JAR.
>위의 artifact 대부분은 다음 @API Guardian JAR에 게시된 Maven POM에 종속성이 있습니다.

- Group ID: `org.apiguardian`
- Artifact ID: `apiguardian-api`
- Version: `1.1.2`

In addition, most of the above artifacts have a direct or transitive dependency on the following OpenTest4J JAR.
>또한 위의 대부분의 artifact에는 다음 OpenTest4J JAR에 대한 직접적 또는 전이적 종속성이 있습니다.

- Group ID: `org.opentest4j`
- Artifact ID: `opentest4j`
- Version: `1.2.0`

10.3. Dependency Diagram
component diagram
![image](../images/juni5_guide_component-diagram.svg)
