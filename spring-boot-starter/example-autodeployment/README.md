# Spring Boot application with embedded Camunda engine and auto-deployed process

This example demonstrates how you can build Spring Boot application having following configured:
* Embedded Camunda engine
* Process application and BPMN process automatically deployed on engine startup
* Usage of @PostDeploy startup hook - occurs after the process application is deployed

## How is it done

1. To embed Camunda Engine you must add following dependency in your `wpom.xml`:

```xml
...
<dependency>
  <groupId>org.camunda.bpm.springboot</groupId>
  <artifactId>camunda-bpm-spring-boot-starter</artifactId>
  <version>2.3.0-SNAPSHOT</version>
</dependency>
...
```

2. With Spring Boot you usually create "application" class annotated with `@SpringBootApplication`. In order to have Camunda process application
registered, you can simply add annotation `@EnableProcessApplication` to the same class and also include `processes.xml` file in your `META-INF` folder:

```java
@SpringBootApplication
@EnableProcessApplication
public class AutoDeploymentApplication {

  public static void main(final String... args) throws Exception {
    SpringApplication.run(AutoDeploymentApplication.class, args);
  }

}
```

3. You can also put BPMN, CMMN and DMN files in your classpath, they will be automatically deployed and registered within process application.

4. When implementing process application in [standard manner](https://docs.camunda.org/manual/7.7/user-guide/process-applications/the-process-application-class/),
 you can use method-level annotations `@PostDeploy` and `@PreUndeploy` to process corresponding events. In case of Spring Boot 
 process application you can handle this events by using Spring @EventListener annotation with following event classes: 
`PostDeployEvent` and `PreUndeployEvent`.

```java
...
@EventListener
public void notify(final PostDeployEvent event) {
...
}
...
```
 
## Run the application and check the result

You can then build the application by calling `mvn clean install` and then run it with `java -jar` command.

Observe log entry similar to this: `Found deployed process: ProcessDefinitionEntity[Sample:1:215245a1-a1e2-11e7-8069-0a0027000006]`
