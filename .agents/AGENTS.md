# Global Agent Configuration and Project Tech Stack (AGENTS.md)

## 1. Reference Technology Stack
* **Language:** Java
* **Version:** 17 (OpenLogic JDK 17.0.16.8)
* **Core Framework:** Spring Boot 3.5.11, Spring Cloud 2025.0.1, Spring Cloud Task, Spring Cloud Batch, Spring Data JPA
* **ORM / Database:** MySQL / MariaDB (dual datasource configured via explicit Beans)
* **Build System:** Gradle 8.7 (utilizing Kotlin DSL or Groovy DSL)
* **Version Control:** Git

## 2. Mandatory Libraries and Ecosystem
* **Data Validation:** `jakarta.validation-api` (Hibernate Validator integrated via Spring Boot Starter Validation)
* **Authentication and Security:** `spring-boot-starter-security` and JWT support (e.g., `io.jsonwebtoken:jjwt`)
* **Automated Testing:** JUnit 5 (Jupiter), Mockito, and `spring-boot-starter-test`

## 3. Coding Guidelines (Constraints)
* **Java 17 Paradigm & Syntax:** 
  * Always prefer **Records** for creating DTOs (Data Transfer Objects) and immutable data carriers.
  * Use local variable type inference (`var`) only where the type is explicitly obvious.
  * Always specify explicit data types and enforce strict Generics for collections (e.g., `List<AgentResponseDTO>`).
  * Avoid returning null values; prefer returning `Optional<T>` in lookup/repository methods.

* **Design Patterns (MVC & Layered Architecture):** 
  * Strictly adhere to Spring's layered architecture: `Controllers` (REST Endpoints), `Services` (Business Logic/Orquestration), `Repositories` (JPA Data Access), and `Batch/Task` (Batch Processing Components).
  * Dependency Injection **must** be performed strictly via **constructor injection** (preferably using Lombok's `@RequiredArgsConstructor` annotation).

* **Dependency Management:** 
  * Never add external dependencies or plugins without updating the `build.gradle` (or `build.gradle.kts`) file and ensuring full compatibility with the Spring Cloud 2025.0.1 BOM.

* **Security & External Configuration:** 
  * Never hardcode API keys, database credentials, or secrets directly in code or static property files.
  * Use externalized properties mapped via `@ConfigurationProperties` or resolved in `application.yml` using environment variables (e.g., `${DB_PASSWORD}`).

## 4. Agent Behavior & AI Skill Integration
* **Persona:** Act as a Principal Systems Architect and Senior Java Backend Developer, focused on high-throughput, thread-safe concurrency, Clean Code, and high-performance Spring Batch pipelines.
* **AI Skill Integration (CRITICAL):** The agent **MUST** actively consult and leverage the specialized domain skills located under the [.agents/skills/](file:///c:/Users/mgiov/IdeaProjects/desktop-fiscal-invoice-email-batch/.agents/skills/) directory. Whenever a prompt, task, or review request aligns with one of these skills (such as a database query, code-level performance bottleneck, concurrency challenge, or API structure), the agent **MUST** read the corresponding `SKILL.md` file first using the `view_file` tool to strictly adhere to the checklist, constraints, and best practices documented there.
* **Validation:** Before marking any task, refactoring, or code modification as complete, the agent **MUST** execute the following verification sequence:
  1. **Run IntelliJ Code Inspections:** Execute the **IntelliJ Code Review** skill (`@[/intellij-code-review]`) on all modified source files to clean up design-time warnings, redundant imports, and Spring framework warnings.
  2. **Compile & Build:** Execute the Gradle build wrapper (`./gradlew.bat clean build` or `./gradlew.bat compileJava compileTestJava`) to verify syntactic and dependency correctness.
  3. **Run Tests & Verify Coverage:** Run the complete local test suite and generate reports using the Gradle wrapper (`./gradlew.bat test jacocoTestReport`). Verify that test coverage reaches **at least 85%** of the project code using the JaCoCo XML or HTML reports (`build/reports/jacoco/test/html/index.html`).