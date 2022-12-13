# Goal 

Know the differenc between +Junit 5 and - Junit 5

# 1 - JUnit 5 Advantages
- #### we get more granularity and can import only what's necessary but in Junit4  imported on a unique jar file
- #### JUnit 5 allows multiple runners to work simultaneously. but in junit 4 only one runner execute tests 
- #### JUnit 5 makes good use of the Java 8 features.

junit5 rewrite the junit 5 and enhance it.

# 2 - JDifferences :

Junit 4 Now is divided into two modules that create Junit 5 

- **JUnit Platform –** scopes all extension for execute and report tests

- **JUnit Vintage –** this module allows backward compatibility with JUnit 4 or even JUnit 3.

## 2-1 Annotations :

| **Junit 4** | **Junit 5** |
| :---: | :---: |
| @Before  | @BeforeEach | 
| @Ignore  | @Disabled | 
| @After | @AfterEach | 
| @BeforeClass  | @BeforeAll | 
| @Test(expected = Exception.class) | Assertions.assertThrows(Exception.class, () -> {}); | 
| @Test(timeout = 1) | Assertions.assertTimeout(Duration.ofMillis(1), () -> Thread.sleep(10)); | 

## 2.2. Assertions
We can also write assertion messages in a lambda in JUnit 5 :

```java
@Test
void shouldFailBecauseTheNumbersAreNotEqual_lazyEvaluation() {
    Assertions.assertTrue(
      2 == 3, 
      () -> "Numbers " + 2 + " and " + 3 + " are not equal!");
}

```

## 2.3 Tagging and Filtering

In JUnit 4, we could group tests by using the @Category annotation. In JUnit 5, the @Category annotation is replaced by the @Tag annotation:

```java
@Tag("annotations")
@Tag("junit5")
class AnnotationTestExampleUnitTest {
    /*...*/
}
```

We can include/exclude particular tags using the maven-surefire-plugin:

```xml
<build>
    <plugins>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <configuration>
                <properties>
                    <includeTags>junit5</includeTags>
                </properties>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## 2.4. New Annotations for Running Tests
In Junit4 we Used **@RunWith(SpringJUnit4ClassRunner.class)**   Now in Junit5 we Use **@ExtendWith(SpringExtension.class)**

## 2.5. JUnit 5 Vintage

JUnit Vintage aids in the migration of JUnit tests by running JUnit 3 or JUnit 4 tests within the JUnit 5 context.

```xml
<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>
    <version>${junit5.vintage.version}</version>
    <scope>test</scope>
</dependency>
```