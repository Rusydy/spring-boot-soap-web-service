# notes

## Issues

- [x] Issue 1 - error on tns

  - line:

    ```xml
    <xs:element name="getCountryResponse">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="country" type="tns:country"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
    ```

  - error message:

    ```md
    s4s-att-invalid-value: Invalid attribute value for 'type' in element 'element'. Recorded reason: UndeclaredPrefix: Cannot resolve 'tns:country' as a QName: the prefix 'tns' is not declared
    ```

  - solution:

    Don't forget to specify tns and targetNamespace in the schema.

    ```xml
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" xmlns:tns="http://www.soapSpringAPI.com/country" targetNamespace="http://www.soapSpringAPI.com/country">
    ```

- [x] Issue 2 - error when adding plugins

  - line:

  ```xml
    <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>jaxb2-maven-plugin</artifactId>
        <version>1.6</version>
        <executions>
            <execution>
                <id>xjc</id>
                <goals>
                    <goal>xjc</goal>
                </goals>
            </execution>
        </executions>
        <configuration>
            <schemaDirectory>${project.basedir}/src/main/resources/</schemaDirectory>
            <outputDirectory>${project.basedir}/src/main/java</outputDirectory>
            <clearOutputDir>false</clearOutputDir>
        </configuration>
    </plugin>
  ```

  - error message:

    ```md
    Element name 'plugin' is invalid.

    One of the following is expected:
    - packaging
    - name
    - description
    ...

    Error indicated by:
    {http://maven.apache.org/POM/4.0.0}
    with code:xml(cvc-complex-type.2.4.a)
    ```

  - solution:
    on new version of maven, the plugin should be added in the build section.

    ```xml
        <build>
            <plugins>
                <plugin>
                    <groupId>org.codehaus.mojo</groupId>
                    <artifactId>jaxb2-maven-plugin</artifactId>
                    <version>1.6</version>
                    <executions>
                        <execution>
                            <id>xjc</id>
                            <goals>
                                <goal>xjc</goal>
                            </goals>
                        </execution>
                    </executions>
                    <configuration>
                        <schemaDirectory>${project.basedir}/src/main/resources/</schemaDirectory>
                        <outputDirectory>${project.basedir}/src/main/java</outputDirectory>
                        <clearOutputDir>false</clearOutputDir>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    ```

- [ ] Issue 3 - Generate the Domain Java Classes ERROR

  - line/description:

  we'll generate the Java classes from the XSD file defined in the previous section. The `jaxb2-maven-plugin` will do this automatically during build time. The plugin uses the XJC tool as a code generation engine. XJC compiles the XSD schema file into fully annotated Java classes.
  
  - error message:

  ```bash
    ➜  spring-boot-soap-web-service git:(main) ✗ mvn compile -e
    [INFO] Error stacktraces are turned on.
    [INFO] Scanning for projects...
    [INFO]
    [INFO] -----------< com.soapSpringAPI:spring-boot-soap-web-service >-----------
    [INFO] Building spring-boot-soap-web-service 1.0-SNAPSHOT
    [INFO] --------------------------------[ jar ]---------------------------------
    [INFO]
    [INFO] --- jaxb2-maven-plugin:3.1.0:xjc (xjc) @ spring-boot-soap-web-service ---
    [INFO] Created EpisodePath [/Users/rusydymuhiddin/Documents/java-learn/spring-boot-soap-web-service/src/main/java/META-INF/JAXB]: true
    [INFO] Ignored given or default xjbSources [/Users/rusydymuhiddin/Documents/java-learn/spring-boot-soap-web-service/src/main/xjb], since it is not an existent file or directory.
    [INFO] Ignored given or default sources [/Users/rusydymuhiddin/Documents/java-learn/spring-boot-soap-web-service/src/main/xsd], since it is not an existent file or directory.
    [WARNING] No XSD files found. Please check your plugin configuration.
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD FAILURE
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  0.240 s
    [INFO] Finished at: 2022-11-25T02:47:37+07:00
    [INFO] ------------------------------------------------------------------------
    [ERROR] Failed to execute goal org.codehaus.mojo:jaxb2-maven-plugin:3.1.0:xjc (xjc) on project spring-boot-soap-web-service: : MojoExecutionException: NoSchemasException -> [Help 1]
    org.apache.maven.lifecycle.LifecycleExecutionException: Failed to execute goal org.codehaus.mojo:jaxb2-maven-plugin:3.1.0:xjc (xjc) on project spring-boot-soap-web-service:
  ```

  - references:
    - [MojoExecutionException](https://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException)

    - [JAXB 2 Maven Plugin » 3.1.0](https://mvnrepository.com/artifact/org.codehaus.mojo/jaxb2-maven-plugin/3.1.0)
