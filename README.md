# Spring Boot Application Deploy on Heroku
## Using Maven plugin and fat jar

```xml
pom.xml

<properties>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
	<java.version>1.8</java.version>
	<full-artifact-name>target/${project.artifactId}-${project.version}.jar</full-artifact-name>
</properties>

...
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
		<plugin>
			<groupId>com.heroku.sdk</groupId>
			<artifactId>heroku-maven-plugin</artifactId>
			<version>1.2.0</version>
			<configuration>
				<appName>jade-firstapp</appName>
				<includeTarget>false</includeTarget>
				<includes>
					<include>${basedir}/${full-artifact-name}</include>
				</includes>
				<jdkVersion>1.8</jdkVersion>
				<processTypes>
					<web>java $JAVA_OPTS -Dserver.port=$PORT -jar ${full-artifact-name}</web>
				</processTypes>
			</configuration>
		</plugin>
	</plugins>
</build>
```

```sh
mvn clean package heroku:deploy
```
```
....

[INFO] -----> Packaging application...
[INFO]        - app: jade-firstapp
[INFO]        - including: target/heroku-app-demo-0.0.1-SNAPSHOT.jar
[INFO] -----> Creating build...
[INFO]        - file: target/heroku/slug.tgz
[INFO]        - size: 12MB
[INFO] -----> Uploading build...
[INFO]        - success
[INFO] -----> Deploying...
[INFO] remote: 
[INFO] remote: -----> heroku-maven-plugin app detected
[INFO] remote: -----> Installing OpenJDK 1.8... done
[INFO] remote: -----> Discovering process types
[INFO] remote:        Procfile declares types -> web
[INFO] remote: 
[INFO] remote: -----> Compressing...
[INFO] remote:        Done: 60.7M
[INFO] remote: -----> Launching...
[INFO] remote:        Released v6
[INFO] remote:        https://jade-firstapp.herokuapp.com/ deployed to Heroku
[INFO] remote: 
[INFO] -----> Done
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 01:10 min
[INFO] Finished at: 2017-08-17T17:58:01+09:00
[INFO] Final Memory: 42M/464M
[INFO] ------------------------------------------------------------------------

```


Note that if `-Dserver.port=$PORT` is not provided in <processType>, then `Error R10 (Boot timeout) -> Web process failed to bind to $PORT within 90 seconds of launch` will be occured
