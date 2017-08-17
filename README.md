# springboot-deploy-heroku

```xml
pom.xml

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
