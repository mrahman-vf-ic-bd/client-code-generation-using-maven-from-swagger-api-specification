# Client code generation using maven plugin in your java project from swagger API specification

1. Do a API call to generate swagger api specification of your Server.
Run following command and save the output file in `server-api.json` file.
 `curl -v -H "X-API-Key: secret" -H "accept:application/json" http://<server-IP>:<port>/api/docs -o server-api.json`
 For example
	 `curl -v -H "X-API-Key: secret" -H "accept:application/json" http://www.example.com:8081/api/docs -o server-api.json`
 
3. Put the `server-api.json` file in resources folder of Spring Boot application.
4. Add a plugin in `pom.xml` to generate code based on the `server-api.json` file. 
```
<plugin>
	<groupId>io.swagger</groupId>
	<artifactId>swagger-codegen-maven-plugin</artifactId>
	<version>2.3.1</version>
	<executions>
		<execution>
			<goals>
				<goal>generate</goal>
			</goals>
			<configuration>
				<inputSpec>${project.basedir}/src/main/resources/dns-api.json</inputSpec>
				<output>${project.build.directory}/generated-sources</output>
				<language>java</language>
				<modelPackage>${default.package}.client.model</modelPackage>
				<apiPackage>${default.package}.client.api</apiPackage>
				<invokerPackage>${default.package}.client.invoker</invokerPackage>
				<generateApiTests>false</generateApiTests>
				<configOptions>
					<sourceFolder>main/java</sourceFolder>
				</configOptions>
			</configuration>
		</execution>
	</executions>
</plugin>
```
Here `default.package` is defined in :
```
<properties>
	<default.package>dev.msr</default.package>
</properties>
``` 
5.  Add another plugin to the `pom.xml` file and This plugin will add the generated source to the project so that project can use the generated code:
    ```
				<plugin>
				<groupId>org.codehaus.mojo</groupId>
				<artifactId>build-helper-maven-plugin</artifactId>
				<executions>
					<execution>
						<id>add-source</id>
						<phase>generate-sources</phase>
						<goals>
							<goal>add-source</goal>
						</goals>
						<configuration>
							<sources>
								<source>${project.build.directory}/generated-sources/main/java/</source>
							</sources>
						</configuration>
					</execution>
				</executions>
			</plugin>
	```

That's all enjoy.
