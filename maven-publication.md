# DRAFT
# How to publish your artifact into Maven Central and Github Packages

## Maven Central publication only

In this case we publish release versions into Maven Central. The modern Maven Central repository doesn't support snapshot versions and require several mandatory things:

- All artifacts must be signed by GPG
- Javadoc and source code artifacts must be provided along side with main JAR artifact 

### POM file additions

#### Distribution management

Under `project` add a new section

```xml
    <distributionManagement>
        <repository>
            <id>central</id>
            <name>Central Maven Repository</name>
        </repository>
    </distributionManagement>
```

#### Build plugins

In a sectin `project.build.plugins` add next entries:

```xml
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-gpg-plugin</artifactId>
                <version>3.2.7</version>
                <executions>
                    <execution>
                        <id>sign-artifacts</id>
                        <phase>verify</phase>
                        <goals>
                            <goal>sign</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-source-plugin</artifactId>
                <version>3.3.0</version>
                <executions>
                    <execution>
                        <id>attach-sources</id>
                        <goals>
                            <goal>jar-no-fork</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-javadoc-plugin</artifactId>
                <version>3.6.3</version>
                <executions>
                    <execution>
                        <id>attach-javadocs</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
```

## Maven Central and Github Packages publication

In this case we publish release versions into Maven Central and use Github Packages to publish development (snapshot) versions.

### POM file additions

#### Repositories description

In a sectin `project.profiles` add two entries:

```xml
    <profiles>
        <profile>
            <id>central</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>org.sonatype.central</groupId>
                        <artifactId>central-publishing-maven-plugin</artifactId>
                        <version>0.6.0</version>
                        <extensions>true</extensions>
                        <configuration>
                            <publishingServerId>central</publishingServerId>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>
        <profile>
            <id>github</id>
            <distributionManagement>
                <repository>
                    <id>github</id>
                    <name>GitHub Packages</name>
                    <url>https://maven.pkg.github.com/OWNER/REPOSITORY</url>
                </repository>
            </distributionManagement>
        </profile>
    </profiles>
```

#### Build plugins

In a sectin `project.build.plugins` add next entries:

```xml
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-gpg-plugin</artifactId>
                <version>3.2.7</version>
                <executions>
                    <execution>
                        <id>sign-artifacts</id>
                        <phase>verify</phase>
                        <goals>
                            <goal>sign</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-source-plugin</artifactId>
                <version>3.3.0</version>
                <executions>
                    <execution>
                        <id>attach-sources</id>
                        <goals>
                            <goal>jar-no-fork</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-javadoc-plugin</artifactId>
                <version>3.6.3</version>
                <executions>
                    <execution>
                        <id>attach-javadocs</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
```

### GitHub Actions

#### GitHub Action to publish into Maven Central

Create a new yaml file in `.github/workflows` folder.
Copy and paste the code below

```yaml



```
