# SpringBoot_Lesson10.2

## Propmt for the Code Agent (Codex, Gemini Code Assistant or Copilot)

**Context**:

I am writing a high-fidelity integration test for a Spring Boot application (Spring Boot 3.3, Java 17, Maven). 

The goal is to ensure the application works correctly with a real PostgreSQL database using Testcontainers.

**Task**:

Generate a JUnit 5 integration test that uses Testcontainers to provide a PostgreSQL database.

**Constraints**:

The test class must use @SpringBootTest to load the full application context.

The test must be annotated with @Testcontainers.

It must declare a PostgreSQLContainer field annotated with @Container.

Use @DynamicPropertySource to dynamically set:

spring.datasource.url

spring.datasource.username

spring.datasource.password

Also override spring.datasource.driver-class-name=org.postgresql.Driver and spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect (the project’s application.properties configures H2 by default).

Set spring.jpa.hibernate.ddl-auto=create-drop so Hibernate creates the schema during the test.

The test should inject a real TaskRepository bean and use it to save and retrieve a Task.

**Project specifics**:

Package: com.example.demo

Entity: Task with fields id (Long), description (String), and completed (boolean).

Repository: TaskRepository extends JpaRepository<Task, Long>.

**Steps**:

Add Testcontainers dependencies and the PostgreSQL JDBC driver to Maven:

org.testcontainers:junit-jupiter:test

org.testcontainers:postgresql:test

org.postgresql:postgresql:test

Generate TaskApplicationIntegrationTest.java under src/test/java/com/example/demo.

Define a static PostgreSQLContainer<?> (e.g., image postgres:16-alpine) annotated with @Container.

Implement a static @DynamicPropertySource method to set the datasource properties from the running container, including driver-class-name, dialect, and ddl-auto=create-drop.

Write a test method that injects TaskRepository.

Inside the test, save a new Task, then retrieve it and assert the data is correct.

Include the Maven command to run only this test.

**Deliverables**:

The required Maven dependencies for Testcontainers PostgreSQL and the PostgreSQL JDBC driver.

The full code for TaskApplicationIntegrationTest.java.

The command to run the test.

**Acceptance Criteria**:

The test class has @SpringBootTest and @Testcontainers.

A PostgreSQLContainer is defined and started by the test framework.

@DynamicPropertySource correctly configures datasource URL, username, password, driver-class-name, database-platform, and sets spring.jpa.hibernate.ddl-auto=create-drop.

When run with Docker available, logs show Testcontainers pulling/starting the PostgreSQL image and the application connecting to it.

The test successfully saves and retrieves data from the containerized PostgreSQL instance.

The test passes.

**Run command**:

mvn -Dtest=TaskApplicationIntegrationTest test

**Note**:

Docker Desktop (or an accessible Docker daemon) must be installed and running for Testcontainers.
