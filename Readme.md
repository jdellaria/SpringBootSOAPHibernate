Spring Boot SOAP Service with Hibernate Example

#1. Introduction

Java Persistence API (JPA) is Java’s standard API specification for object-relational mapping. Hibernate is a JPA provider and provides a framework for mapping an object-oriented domain model to a relational database. Spring Boot defines a list of starter projects, in which each project includes a set of default component dependencies and an automatic configuration of components. The Spring Web Service starter project enables developers to write the contract-first SOAP service easily.

In this example, I will create a SOAP service with Hibernate in a Spring Boot application.

#2. Technologies Used

The example code in this article was built and run using:

    Java 1.8.101
    Maven 3.3.9
    Eclipse Oxygen
    Spring Boot 1.5.16
    Hibernate 5.0.12.Final
    H2 1.4.197

#3. Maven Project

Spring Boot Starters provides more than 30 starters to ease the dependency management for your projects. The easiest way to generate a Spring Boot Web Service with Hibernate via Spring Initializer:

    Go to https://start.spring.io/.
    Select Maven Project with Java and Spring Boot version 1.5.16. Add JPA, Web Services, and H2 in the “search for dependencies” section.
    Enter the group name: jcg.zheng.demo and artifact: spring-boot-soap-hibernate.
    Click the Generate Project button.

A maven project will be generated and downloaded to your workstation. Import it into your Eclipse workspace.

#3.1 Dependencies

The generated pom.xml includes H2, spring-boot-starter-data-jpa, spring-boot-starter-web-services, and spring-boot-starter-test. I will include wsdl4j and maven-jaxb2-plugin.

pom.xml

#3.2 Spring Boot Application

In this step, I will modify the generated Spring Boot application to include a step to save test data.

SpringBootSoapHibernateApplication.java

#3.3 Application Properties

In this step, I will include two properties to set the Hibernate SQL logger level to DEBUG. Click here to see the default properties.

applicaiton.properties

#4. Employee WSDL

The Web Services Description Language (WSDL) is an XML-based interface definition language that is used for describing the functionality offered by a web service. In this step, employee.wsdl is an XML document which describes the EmployeeLookup web service and specifies how to access and use its methods.

employee.wsdl

#4.1 WSDL Validation

Predic8 provides a free online tool to validate the WSDL file. I used it to validate employee.wsdl. The validation results included reports for TargetNamespace, Message, PortType, Operation, Binding, and Service.

#4.2 Generated Java Files

The Maven plug-in, maven-jaxb2-plugin, scans src/main/resource/wsdl/employee.wsdl. Execute mvn clean install to generate the Java stubs under C:\MZheng_Java_workspace\Java Code Geek Examples\spring-boot-soap-hibernate\target\generated-sources\xjc.

No modification needed to the generated files.

Generated Java Stubs

#5. Employee Service

In this step, I will create a Spring service to look up the employee based on its identifier and transform it to EmployeeInfo.
5.1 Employee Entity

@javax.persistence.Entity annotation defines that a class can be mapped to a table. An entity class must have a field annotated with @Id.

In this step, I will create an Employee entity to map to a T_Employee table.

Employee.java

#5.2 EmployeeTransformer

In this step, I will create an EmployeeTransformer which transforms the Employee entity to the generated EmployeeInfo class.

EmployeeTransformer.java

#5.3 EmployeeRepository

Spring data scans the base package and all its sub-packages for any interfaces extending @Repository or one of its sub-interfaces. For each interface found, Spring creates the appropriate bean to handle invocation of the query methods. In this step, I will create an EmployeeRespository interface.

EmployeeRepository.java


#5.4 EmployeeService

In this step, I will create an EmployeeService which injects EmployeeRepository and EmployeeTransformer to look up an employee and convert it to EmployeeInfo.

EmployeeService.java

#6. Employee SOAP Service

I will create an EmployeeServiceEndPoint which defines the employeeLookup web method and exposes it as a SOAP service.

#6.1 EmployeeServiceEndPoint

In this step, I will create an EmployeeServiceEndPoint annotate it with @Endpoint. I will annotate the web method with @PayloadRoot and @ResponsePayload. It uses the generated Java stubs in step 4.2 and the EmployeeService created in step 5.4.

EmployeeServiceEndPoint.java

#6.2 SoapServiceConfiguration

In this step, I will create a SoapServiceConfiguration with @EnableWs and configure a SOAP service with employee.wsdl.

SoapServiceConfiguration.java

#7. Demo

SoapUI is a great tool for testing web services. Click here to download it.

Start the Spring Boot application created in step 3.2. Verify that the service is started with four employees in the server log.

Server Log

In this step, I will create a new SOAP project:

    Click File->New SOAP Project
    Enter the Initial WSDL: http://localhost:8080/soap/ws/employee.wsdl and Click OK
    Expand the newly created project, then click employeeLookup, and Request 1
    A SOAP message is populated, replace ? with the test data
    Submit the request
    Wait for a response

SoapUI Input

#8. Spring Boot SOAP Service with Hibernate- Summary

In this article, I demonstrated how to create a SOAP web service using Hibernate in a Spring Boot Maven project in four steps:

    Generate Java stubs from a WSDL file.
    Create an Entity class and Repository Interface
    Create a web service endpoint
    Configure a SimpleWsdl11Definition with WSDL
