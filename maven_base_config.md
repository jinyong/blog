# 定义变量（可选， 也可以写死）
<properties>
    <jdk.ver>1.6</jdk.ver>
    <spring.ver>4.3.22.RELEASE</spring.ver>
</properties>

# 各种依赖（可以引用变量）
<dependencies>
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-webmvc</artifactId>
	    <version>${spring.ver}</version>
	</dependency>
	  
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
</dependencies>

# 编译时执行
<build>
    <finalName></finalName>
    <pluginManagement>
	    <plugins>
	    	<plugin>
   				<groupId>org.apache.maven.plugins</groupId>
          		<artifactId>maven-compiler-plugin</artifactId>
          		<version>3.8.0</version>
          		<configuration>
          			<source>${jdk.ver}</source>
          			<target>${jdk.ver}</target>
          		</configuration>
	    	</plugin>
	    </plugins>
    </pluginManagement>
    <outputDirectory>src/main/webapp/WEB-INF/classes</outputDirectory>  
	<testOutputDirectory>src/main/webapp/WEB-INF/test-classes</testOutputDirectory>
</build>

## configuration
指定编译时默认jdk版本，防止maven update的时变回1.5

## outputDirecotry，testOutputDirectory
貌似默认target/classes?, target/test-classes?，防止maven update时变回去