*Apache Maven 3.5.2*

Maven是一个项目管理工具。

当执行Maven构建时，Maven会根据POM中的定义，自动下载相关依赖和插件。

# 安装和配置

## 安装

1. 安装JDK；
2. 下载并解压Maven；
3. 将环境变量`M2_HOME`设置为Maven的安装目录；
4. 将`M2_HOME/bin`目录添加到`Path`环境变量中。

## 配置

### 全局配置

全局配置可以在两个地方：

+ 环境变量`MAVEN_OPTS` ；
+ 配置文件`M2_HOME/conf/settings.xml`。

环境变量`MAVEN_OPTS`通常用于设置一些用于启动JVM的参数，例如：`-Xms256m -Xmx1024m`（用于避免在较大项目中，需要大量内存，而造成的`java.lang.OutOfMemeoryError`错误）。

### 用户级的配置

用户级的配置文件位于：`~/.m2/settings.xml`，它只对当前用户有效果。

### 项目级的配置

项目级的配置文件位于项目根目录下的`.mvn`目录中的`maven.config`和`extensions.xml`。



# 入门

## 生成项目

```bash
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

如果是使用Powershell，要记得给形如“-Dxxx”的参数加上引号：

```powershell
mvn archetype:generate '-DgroupId=com.mycompany.app' '-DartifactId=my-app2' '-DarchetypeArtifactId=maven-archetype-quickstart' '-DinteractiveMode=false'
```

上面命令，将自动创建一个`my-app`目录，并将它作为项目的根目录。

`archetype`是Maven的**插件**，`generate`是该插件的一个**目标（goal）**。

### Maven标准项目结构

```
my-app
|-- pom.xml
|-- LICENSE  （Project's license）
|-- NOTICE  (Notices and attributions required by libraries that the project depends on)
|-- README.md  （Project's readme）
|-- src
|   |-- main
|   |   |-- java  （Application/Library sources）
|   |   |   `-- com
|   |   |       `-- mycompany
|   |   |           `-- app
|   |   |               `-- App.java
|   |   |-- resources  （Application/Library resources） 
|   |   |-- filters  （Resource filter files）
|   |   |-- webapp  （Web application sources）
|   |-- test
|   |   |-- java  （Test sources）
|   |   |   `-- com
|   |   |       `-- mycompany
|   |   |           `-- app
|   |   |               `-- AppTest.java
|   |   |-- resources  （Test resources）
|   |   |-- filters  （Test resource filter files）
|   |-- it （Integration Tests (primarily for plugins)）
|   |-- assembly  （Assembly descriptors）
|   |-- site  （Site）
|-- target  （Output of the build）
```

在一个Maven项目，只有`pom.xml`是必须的，其他内容都是可选的。

## 配置POM

pom.xml：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
 
  <name>Maven Quick Start Archetype</name>
  <url>http://maven.apache.org</url>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

## 编写代码

略。

## 构建项目

```bash
cd my-app
mvn package
```

这里`package`是Maven的一个**阶段（phase）**。阶段是**构建生成周期（build lifecycle）**的一个步骤，而构建生命周期则是多个阶段组成的有序序列。

当执行一个阶段时，Maven将依次执行该阶段之前的所有阶段，然后再执行该阶段。

> 在`mvn`之后，形如“xxx:xxx”的是目标，而只有一个单词的是阶段。
>
> 在同一条mvn命令中，可以同时包含有目标和阶段：
>
> ```bash
> mvn clean dependency:copy-dependencies package
> ```

## 运行应用

```bash
java -cp target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App
```

## 生成项目站点

```bash
mvn site
```

生成的项目站点在`target/site`目录下。

# 项目对象模型（POM）

POM定义了项目的基本信息，它描述项目是如何构建，声明了项目依赖等等。

POM通常定义在项目根目录下的`pom.xml`文件中。

## POM的结构

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <!-- The Basics -->
  <groupId>...</groupId>
  <artifactId>...</artifactId>
  <version>...</version>
  <packaging>...</packaging>
  <dependencies>...</dependencies>
  <parent>...</parent>
  <dependencyManagement>...</dependencyManagement>
  <modules>...</modules>
  <properties>...</properties>
 
  <!-- Build Settings -->
  <build>...</build>
  <reporting>...</reporting>
 
  <!-- More Project Information -->
  <name>...</name>
  <description>...</description>
  <url>...</url>
  <inceptionYear>...</inceptionYear>
  <licenses>...</licenses>
  <organization>...</organization>
  <developers>...</developers>
  <contributors>...</contributors>
 
  <!-- Environment Settings -->
  <issueManagement>...</issueManagement>
  <ciManagement>...</ciManagement>
  <mailingLists>...</mailingLists>
  <scm>...</scm>
  <prerequisites>...</prerequisites>
  <repositories>...</repositories>
  <pluginRepositories>...</pluginRepositories>
  <distributionManagement>...</distributionManagement>
  <profiles>...</profiles>
</project>
```

> POM的详细说明参见官网的 [POM Reference](http://maven.apache.org/pom.html) 。

## Super POM

*Super POM*是Maven的默认POM。所有POM都自动继承自Super POM，除非显式指定父POM。

每个版本的Maven的Super POM可能都有点不一样，具体参见官网。例如：[Maven 3.5.2 的Super POM](http://maven.apache.org/ref/3-LATEST/maven-model-builder/super-pom.html#)：

```xml
<project>
  <modelVersion>4.0.0</modelVersion>

  <repositories>
    <repository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>

  <pluginRepositories>
    <pluginRepository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <releases>
        <updatePolicy>never</updatePolicy>
      </releases>
    </pluginRepository>
  </pluginRepositories>

  <build>
    <directory>${project.basedir}/target</directory>
    <outputDirectory>${project.build.directory}/classes</outputDirectory>
    <finalName>${project.artifactId}-${project.version}</finalName>
    <testOutputDirectory>${project.build.directory}/test-classes</testOutputDirectory>
    <sourceDirectory>${project.basedir}/src/main/java</sourceDirectory>
    <scriptSourceDirectory>${project.basedir}/src/main/scripts</scriptSourceDirectory>
    <testSourceDirectory>${project.basedir}/src/test/java</testSourceDirectory>
    <resources>
      <resource>
        <directory>${project.basedir}/src/main/resources</directory>
      </resource>
    </resources>
    <testResources>
      <testResource>
        <directory>${project.basedir}/src/test/resources</directory>
      </testResource>
    </testResources>
    <pluginManagement>
      <!-- NOTE: These plugins will be removed from future versions of the super POM -->
      <!-- They are kept for the moment as they are very unlikely to conflict with lifecycle mappings (MNG-4453) -->
      <plugins>
        <plugin>
          <artifactId>maven-antrun-plugin</artifactId>
          <version>1.3</version>
        </plugin>
        <plugin>
          <artifactId>maven-assembly-plugin</artifactId>
          <version>2.2-beta-5</version>
        </plugin>
        <plugin>
          <artifactId>maven-dependency-plugin</artifactId>
          <version>2.8</version>
        </plugin>
        <plugin>
          <artifactId>maven-release-plugin</artifactId>
          <version>2.3.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>

  <reporting>
    <outputDirectory>${project.build.directory}/site</outputDirectory>
  </reporting>

  <profiles>
    <!-- NOTE: The release profile will be removed from future versions of the super POM -->
    <profile>
      <id>release-profile</id>

      <activation>
        <property>
          <name>performRelease</name>
          <value>true</value>
        </property>
      </activation>

      <build>
        <plugins>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-source-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-sources</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-javadoc-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-javadocs</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-deploy-plugin</artifactId>
            <configuration>
              <updateReleaseInfo>true</updateReleaseInfo>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </profile>
  </profiles>
</project>
```

## 最小的POM

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>org.codehaus.mojo</groupId>
  <artifactId>my-project</artifactId>
  <version>1.0</version>
</project>
```

## Maven坐标

POM中的`groupId`、`artifactId`、`version`、`packaging`（可选）和`classifier`（可选）五个元素构成了所谓的**Maven坐标**——它唯一定位一个Maven项目。

Maven坐标在控制台输出时，常写作如下形式：

+ groupId:artifactId:version
+ groupId:artifactId:packaging:version
+ groupId:artifactId:packaging:classifier:version

`packaging`当前可取：`jar`（默认值）、`pom`、 `maven-plugin`、`ejb`、`war`、`ear`、`rar`、`par` 。

# 依赖管理

# 项目生命周期

## Clean生命周期

| `pre-clean`  | execute processes needed prior to the actual project cleaning |
| ------------ | ------------------------------------------------------------ |
| `clean`      | remove all files generated by the previous build             |
| `post-clean` | execute processes needed to finalize the project cleaning    |

## 默认生命周期

| `validate`                | validate the project is correct and all necessary information is available. |
| ------------------------- | ------------------------------------------------------------ |
| `initialize`              | initialize build state, e.g. set properties or create directories. |
| `generate-sources`        | generate any source code for inclusion in compilation.       |
| `process-sources`         | process the source code, for example to filter any values.   |
| `generate-resources`      | generate resources for inclusion in the package.             |
| `process-resources`       | copy and process the resources into the destination directory, ready for packaging. |
| `compile`                 | compile the source code of the project.                      |
| `process-classes`         | post-process the generated files from compilation, for example to do bytecode enhancement on Java classes. |
| `generate-test-sources`   | generate any test source code for inclusion in compilation.  |
| `process-test-sources`    | process the test source code, for example to filter any values. |
| `generate-test-resources` | create resources for testing.                                |
| `process-test-resources`  | copy and process the resources into the test destination directory. |
| `test-compile`            | compile the test source code into the test destination directory |
| `process-test-classes`    | post-process the generated files from test compilation, for example to do bytecode enhancement on Java classes. For Maven 2.0.5 and above. |
| `test`                    | run tests using a suitable unit testing framework. These tests should not require the code be packaged or deployed. |
| `prepare-package`         | perform any operations necessary to prepare a package before the actual packaging. This often results in an unpacked, processed version of the package. (Maven 2.1 and above) |
| `package`                 | take the compiled code and package it in its distributable format, such as a JAR. |
| `pre-integration-test`    | perform actions required before integration tests are executed. This may involve things such as setting up the required environment. |
| `integration-test`        | process and deploy the package if necessary into an environment where integration tests can be run. |
| `post-integration-test`   | perform actions required after integration tests have been executed. This may including cleaning up the environment. |
| `verify`                  | run any checks to verify the package is valid and meets quality criteria. |
| `install`                 | install the package into the local repository, for use as a dependency in other projects locally. |
| `deploy`                  | done in an integration or release environment, copies the final package to the remote repository for sharing with other developers and projects. |

## Site生命周期

| `pre-site`    | execute processes needed prior to the actual project site generation |
| ------------- | ------------------------------------------------------------ |
| `site`        | generate the project's site documentation                    |
| `post-site`   | execute processes needed to finalize the site generation, and to prepare for site deployment |
| `site-deploy` | deploy the generated site documentation to the specified web server |



# 插件

# Maven仓库

# Profile

# Archetype

# 版本管理

# 配置代理

基于安全因素，有时候需要通过安全认证的代理访问因特网。Maven可以通过在配置文件中添加如下配置，来配置代理：

```xml
<proxies>
    <proxy>
        <id>optional</id>
        <active>true</active>
        <protocol>http</protocol>
        <username>proxyuser</username>
        <password>proxypass</password>
        <host>proxy.host.net</host>
        <port>80</port>
        <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>
</proxies>
```

`nonProxyHosts`用来指定哪些主机名不需要代理，可以使用“|”来分隔多个主机名，也可以使用通配符，例如：`*.google.com`表示所有以`google.com`结尾的域名。

`proxies`下可以有多个`proxy`，并且默认情况第一个被激活的（即`<active>true</active>`）proxy会生效。

