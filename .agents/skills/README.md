# 🧠 Catálogo de Skills da IA Antigravity (.agent/skills)

Este diretório contém o conjunto de **Skills Especializadas (IA Skills)** integradas ao assistente Antigravity para automatizar revisões técnicas, garantir padrões de engenharia e otimizar o ciclo de desenvolvimento do projeto.

---

## 🗺️ Matriz de Categorias das Skills

Para facilitar a navegação, as 18 skills disponíveis estão agrupadas por domínio técnico:

| 🟢 Qualidade de Código | 📐 Arquitetura e Padrões | 💾 Banco de Dados & I/O | 🔒 Segurança e Auditorias | 🛠️ Ciclo de Vida e Ferramentas |
| :--- | :--- | :--- | :--- | :--- |
| [Clean Code](#3-clean-code) | [SOLID Principles](#16-solid-principles) | [JPA & Hibernate](#11-jpa-patterns) | [Security Audit](#15-security-audit) | [IntelliJ Inspections](#8-intellij-code-review) |
| [Java Code Review](#9-java-code-review) | [Design Patterns](#5-design-patterns) | [Concurrency Review](#4-concurrency-review) | [API Contract Review](#1-api-contract-review) | [Maven Audit](#13-maven-dependency-audit) |
| [Test Quality](#18-test-quality) | [Spring Boot Expert](#17-spring-boot-multi-version) | [Logging Patterns](#12-logging-patterns) | | [Full Code Review](#6-full-code-review) |
| [Performance Smells](#14-performance-smell) | | | | [Java Migration](#10-java-migration) |
| | | | | [Git Commit Helper](#7-git-commit) |
| | | | | [Changelog Gen](#2-changelog-generator) |

---

## 🛠️ Catálogo Detalhado das 18 Skills

---

### 1. API Contract Review
*   **Pasta:** `api-contract-review/`
*   **Resumo:** Analisa e revisa contratos de APIs REST sob a ótica de semântica HTTP, padronização de endpoints, versionamento, paginação e compatibilidade retroativa.
*   **Como Utilizar:**
    *   *Trigger:* `@[/api-contract-review] revisar o controller X`
    *   *Exemplo de Prompt:* `"@[/api-contract-review] revise as rotas expostas em ValidationController para checar conformidade com padrões RESTful."`

---

### 2. Changelog Generator
*   **Pasta:** `changelog-generator/`
*   **Resumo:** Gera registros de alterações (*changelogs*) automatizados a partir do histórico de commits do Git, agrupando modificações em recursos, correções e alterações críticas.
*   **Como Utilizar:**
    *   *Trigger:* `@[/changelog-generator] gerar changelog desde a última tag`
    *   *Exemplo de Prompt:* `"@[/changelog-generator] crie o CHANGELOG.md cobrindo todos os commits da sprint atual."`

---

### 3. Clean Code
*   **Pasta:** `clean-code/`
*   **Resumo:** Inspeciona o código visando legibilidade e manutenibilidade (DRY, KISS, YAGNI), eliminação de complexidade ciclomática, boas práticas de nomenclatura e refatorações (cláusulas de guarda, eliminação de magic numbers).
*   **Como Utilizar:**
    *   *Trigger:* `@[/clean-code] revisar a classe X`
    *   *Exemplo de Prompt:* `"@[/clean-code] analise o arquivo BuscarValidacoesItemWriterConfig.java e sugira refatorações para simplificar a legibilidade."`

---

### 4. Concurrency Review
*   **Pasta:** `concurrency-review/`
*   **Resumo:** Realiza auditorias de concorrência em Java. Valida thread safety, previne race conditions/deadlocks, e avalia o uso de padrões assíncronos modernos (`Virtual Threads`, `CompletableFuture`, `@Async`).
*   **Como Utilizar:**
    *   *Trigger:* `@[/concurrency-review] avaliar a concorrência na classe X`
    *   *Exemplo de Prompt:* `"@[/concurrency-review] avalie o despacho assíncrono pós-commit do Kafka em BuscarValidacoesItemWriterConfig.java."`

---

### 5. Design Patterns
*   **Pasta:** `design-patterns/`
*   **Resumo:** Guia a implementação e refatoração de padrões de projeto consagrados (Gang of Four) adequados para o contexto Java e Spring (Factory, Strategy, Builder, Decorator, Observer).
*   **Como Utilizar:**
    *   *Trigger:* `@[/design-patterns] aplicar o padrão X no fluxo Y`
    *   *Exemplo de Prompt:* `"@[/design-patterns] sugira como podemos refatorar a validação de layouts XML aplicando o padrão Strategy."`

---

### 6. Full Code Review
*   **Pasta:** `full-code-review/`
*   **Resumo:** O "Master Orchestrator". Executa um pente-fino integrado cobrindo segurança, performance, SOLID, clean code, padrões de projeto e logging. Retorna um relatório conciso com plano de ação de correções.
*   **Como Utilizar:**
    *   *Trigger:* `@[/full-code-review] em todo o projeto`
    *   *Exemplo de Prompt:* `"@[/full-code-review] para todo o projeto antes da liberação da release."`

---

### 7. Git Commit Helper
*   **Pasta:** `git-commit/`
*   **Resumo:** Cria mensagens de commits semânticos aderentes às diretrizes convencionais (Conventional Commits) baseando-se estritamente nas alterações de código identificadas no `git diff`.
*   **Como Utilizar:**
    *   *Trigger:* `@[/git-commit] gerar commit`
    *   *Exemplo de Prompt:* `"@[/git-commit] crie uma mensagem de commit adequada para a refatoração de null-safety que acabamos de fazer."`

---

### 8. IntelliJ Code Review
*   **Pasta:** `intellij-code-review/`
*   **Resumo:** Executa auditorias estáticas emulando a engine de JetBrains IntelliJ IDEA Code Inspections, identificando problemas como importações corrompidas, redundâncias de tipos e avisos de compilador.
*   **Como Utilizar:**
    *   *Trigger:* `@[/intellij-code-review] para todo o projeto`
    *   *Exemplo de Prompt:* `"@[/intellij-code-review] valide o projeto para remover qualquer import corrompido ou wildcard não utilizado."`

---

### 9. Java Code Review
*   **Pasta:** `java-code-review/`
*   **Resumo:** Checklist de revisão sistemática com foco profundo no ecossistema Java: prevenção estrita de `NullPointerException`, gestão robusta de exceções (chaining), recursos (`try-with-resources`) e tipos de coleções.
*   **Como Utilizar:**
    *   *Trigger:* `@[/java-code-review] revisar o pacote X`
    *   *Exemplo de Prompt:* `"@[/java-code-review] audite a segurança de nulos e o tratamento de exceções em ValidationProducerService.java."`

---

### 10. Java Migration
*   **Pasta:** `java-migration/`
*   **Resumo:** Framework de apoio técnico para atualização e modernização de código entre versões principais do Java (ex: migração de Java 8 para 11, 17, 21 ou 25).
*   **Como Utilizar:**
    *   *Trigger:* `@[/java-migration] migrar projeto para Java X`
    *   *Exemplo de Prompt:* `"@[/java-migration] forneça um plano de migração para atualizar nossa stack do Java 17 para o Java 21."`

---

### 11. JPA Patterns
*   **Pasta:** `jpa-patterns/`
*   **Resumo:** Detecção e mitigação de gargalos comuns de persistência com Hibernate/JPA: problemas de N+1 queries, controle transacional inadequado, lazy loading exceptions, paginação ausente e caching.
*   **Como Utilizar:**
    *   *Trigger:* `@[/jpa-patterns] auditar consultas JPA`
    *   *Exemplo de Prompt:* `"@[/jpa-patterns] verifique a query de busca de faturas fiscais para garantir que não temos ineficiências na busca por lote."`

---

### 12. Logging Patterns
*   **Pasta:** `logging-patterns/`
*   **Resumo:** Padronização de logs estruturados (SLF4J, Logback, JSON). Ensina o uso correto de MDC (Mapped Diagnostic Context) para rastreamento de requisições concorrentes e formatação otimizada para monitoramento (ELK/Splunk).
*   **Como Utilizar:**
    *   *Trigger:* `@[/logging-patterns] auditar logs`
    *   *Exemplo de Prompt:* `"@[/logging-patterns] revise as chamadas de log.error para garantir que todas passam a exceção de origem e usam placeholders corretamente."`

---

### 13. Maven Dependency Audit
*   **Pasta:** `maven-dependency-audit/`
*   **Resumo:** Varre dependências do projeto mapeando conflitos de bibliotecas, transitividades que quebram o classpath, e dependências legadas ou com vulnerabilidades conhecidas (CVEs).
*   **Como Utilizar:**
    *   *Trigger:* `@[/maven-dependency-audit] analisar dependências`
    *   *Exemplo de Prompt:* `"@[/maven-dependency-audit] audite o build.gradle em busca de dependências conflitantes ou desatualizadas."`

---

### 14. Performance Smell Detection
*   **Pasta:** `performance-smell-detection/`
*   **Resumo:** Identifica gargalos silenciosos em tempo de execução: compilação repetitiva de Regex em loops, concatenação ineficiente de Strings, custos ocultos de autoboxing/unboxing e redimensionamento de coleções.
*   **Como Utilizar:**
    *   *Trigger:* `@[/performance-smell-detection] na classe X`
    *   *Exemplo de Prompt:* `"@[/performance-smell-detection] analise o loop de envio de mensagens do Kafka para verificar consumo de CPU ou de memória desnecessário."`

---

### 15. Security Audit
*   **Pasta:** `security-audit/`
*   **Resumo:** Análise de vulnerabilidades alinhada com o OWASP Top 10. Avalia segurança contra injeções de SQL, vazamento de credenciais em logs (*logging leak*), fragilidades de criptografia, e falhas de desserialização.
*   **Como Utilizar:**
    *   *Trigger:* `@[/security-audit] auditar segurança da camada X`
    *   *Exemplo de Prompt:* `"@[/security-audit] audite o arquivo AppProperties.java para garantir a segurança no tratamento de senhas."`

---

### 16. SOLID Principles
*   **Pasta:** `solid-principles/`
*   **Resumo:** Avaliação estrita dos cinco princípios fundamentais de design orientado a objetos (SRP, OCP, LSP, ISP, DIP). Ajuda no desacoplamento de classes gigantes ("God Classes") e inversão de controle.
*   **Como Utilizar:**
    *   *Trigger:* `@[/solid-principles] avaliar a estrutura de Y`
    *   *Exemplo de Prompt:* `"@[/solid-principles] verifique se a classe BuscarValidacoesItemWriterConfig obedece à responsabilidade única (SRP)."`

---

### 17. Spring Boot Multi-Version Expert
*   **Pasta:** `spring-boot/`
*   **Resumo:** Especialista técnico para Spring Boot adaptado estritamente à versão do ecossistema do seu projeto (Java 11, Java 17 ou Java 21). Ajusta sintaxe, injeções, transações e recursos concorrentes nativos (ex: Virtual Threads no Java 21).
*   **Como Utilizar:**
    *   *Trigger:* `@[/spring-boot] projetar componente na versão X`
    *   *Exemplo de Prompt:* `"@[/spring-boot] crie um serviço assíncrono para integração com banco de dados usando as melhores práticas para Java 17."`

---

### 18. Test Quality
*   **Pasta:** `test-quality/`
*   **Resumo:** Garante testes unitários e de integração limpos e assertivos. Focado no uso moderno de JUnit 5, asserts fluentes AssertJ, e isolamento por meio de Mockito (stubs e mocks robustos).
*   **Como Utilizar:**
    *   *Trigger:* `@[/test-quality] criar/revisar testes para X`
    *   *Exemplo de Prompt:* `"@[/test-quality] audite a cobertura e qualidade dos testes existentes em ValidationProducerServiceTest.java."`

---

## 💡 Como Obter os Melhores Resultados com o Agente?

1.  **Chame a Skill Diretamente:** Ao iniciar uma tarefa, referencie a skill correspondente utilizando a tag `@` (ex: `@[/clean-code]`). O agente automaticamente lerá as instruções contidas no `SKILL.md` correspondente antes de gerar código.
2.  **Combine com o Contexto Local:** A stack técnica de referência do nosso projeto é descrita em [AGENTS.md](../AGENTS.md). O agente cruzará os princípios das skills com as diretrizes específicas de nossa stack (Java 17 / Spring Boot 3.5.11 / Gradle).
