# Desktop Spring Batch Template - Documentação

Este projeto foi reestruturado para servir como o **Template Padrão (Starter)** para a criação de novos projetos de processamento em lote (Spring Batch) na organização.

## 🏛️ Arquitetura do Template

A arquitetura provê um fluxo resiliente de `Reader -> Processor -> Writer` totalmente integrado ao ecossistema do Desktop:
- **Cloud Config**: Resolução de propriedades centralizada.
- **Dual DataSource**: Conexões simultâneas a dois bancos de dados (`admin` para leitura da massa de dados e `batch` interno para controle de estado do Spring Batch).
- **Kafka Integration**: Envio/Recebimento de eventos padronizado para tópicos.
- **Spring Batch 5 + Spring Boot 3.5.x**: Core moderno com versionamento Java 17.

### Estrutura de Diretórios
A estrutura de diretórios e pacotes do projeto segue um padrão fixo baseado na funcionalidade das classes (arquitetura em camadas):
- `configuration/`: Configurações globais, banco de dados múltiplos, tarefas e configurações individuais de cada Job (ex: `InvoiceEmailBatchConfiguration`, `GenerateDocumentJobConfig`).
- `dto/`: Objetos de Transferência de Dados (`InvoiceEmailDTO`).
- `exception/`: Exceções customizadas da aplicação.
- `job/`: Contém as classes que executam as etapas do Batch, organizadas por sua funcionalidade:
  - `listener/`: Listeners de progresso e execução (ex: `StepProgressLoggingListener`).
  - `processor/`: Processadores de itens (ex: `GenerateDocumentProcessor`).
  - `reader/`: Leitores de itens (ex: `BuscarInvoicesItemReaderConfig`, `GenerateDocumentReaderConfig`).
  - `writer/`: Gravadores/escritores de itens (ex: `BuscarInvoicesItemWriterConfig`, `GenerateDocumentWriterConfig`).
- `properties/`: Properties mapeadas a partir de arquivos externos (`AppProperties`).
- `integration/` (sob a pasta de testes): Suíte de testes de integração pré-configurados que rodam contra servidores DEV reais (Config, BD, Kafka) via VPN.

---

## 🚀 Como Utilizar Este Template

Para criar um novo job baseado neste template, siga a receita de passos abaixo. Esta é uma lista de perguntas que funciona como um guia para alterar e adaptar o código base:

### 📋 Receita de Criação de Novos Jobs (Questionário)

1. **Qual é o nome do novo projeto/aplicação?**
   - *Ação:* Renomeie a pasta principal do projeto e atualize o `rootProject.name` no arquivo `settings.gradle`.
   - *Ação:* Atualize a propriedade `spring.application.name` no arquivo `src/main/resources/application.yml`.

2. **Qual é o propósito (domínio) deste Job?**
   - *Ação:* Altere os pacotes base de `br.com.desktop.fiscal.invoice.validation.batch` para o pacote adequado ao seu domínio (ex: `br.com.desktop.financeiro.cobranca.batch`).

3. **Quais dados serão lidos na origem?**
   - *Ação:* Crie/Altere os DTOs no diretório `dto/` para representar as entidades da tabela principal.
   - *Ação:* Atualize a query SQL (`app.sql1-reader`) nas propriedades do Cloud Config para a extração do seu cenário.
   - *Ação:* Crie/Refatore a configuração do `JdbcCursorItemReader` no pacote `job/reader/` para mapear a sua consulta para a classe DTO (usando o `RowMapper` pertinente).

4. **Como esses dados devem ser processados ou transformados?**
   - *Ação:* Crie/Altere o `ItemProcessor` no pacote `job/processor/` para efetuar a validação da regra de negócio ou montagem do payload.
   - *Ação:* Caso envolva enviar dados para o Kafka, verifique a criação de DTOs de envio (`Request` / `Response`) e altere o `KafkaTemplate` dentro do processador para publicar no tópico correto.

5. **Qual é o destino dos dados processados?**
   - *Ação:* Se for apenas atualizar o status do registro na base de leitura, altere a query de atualização (`app.sql1-writer`) no Cloud Config.
   - *Ação:* Crie/Refatore o `JdbcBatchItemWriter` no pacote `job/writer/` mapeando adequadamente as `named parameters` do DTO.
   - *Ação:* Crie/Refatore as configurações do Job e Step no pacote `configuration/`.

6. **Como você monitorará o andamento deste Job?**
   - *Ação:* Certifique-se de que a injeção do seu logger customizado (ex: `StepProgressLoggingListener`) continue funcional e registre as métricas necessárias. Você pode estender os eventos do `StepExecutionListener` para alertas.

7. **Quais Tópicos/Brokers serão afetados na comunicação externa?**
   - *Ação:* Valide se as propriedades do Kafka em `AppProperties.Kafka` atendem ao seu caso de uso (topics customizados, consumer groups, etc.) e os configure adequadamente no Cloud Config Server para os perfis `dev`, `hml` e `prd`.

---

## 🧪 Suíte de Testes e DevOps

A suíte de testes do projeto possui isolamento explícito via tags do JUnit 5 (`@Tag("integration")`) para não misturar os escopos.

- **Para rodar Testes Unitários:**
  ```bash
  ./gradlew.bat test
  ```
- **Para rodar Testes de Integração (Requer acesso a VPN p/ servidores DEV):**
  ```bash
  ./gradlew.bat integrationTest
  ```

### 💡 Atenção em Testes de Integração com Spring Batch
Sempre mantenha as seguintes propriedades em testes que sobem o contexto completo do Spring:

```java
@SpringBootTest(properties = {
        "spring.batch.job.enabled=false",
        "spring.cloud.task.closecontext_enabled=false",
        "spring.profiles.active=dev"
})
```

* **Por que?**
  1. Impede a execução automática do Job durante o boostrap do teste (Spring Boot/Batch default behaviour).
  2. Evita o fechamento prematuro do Contexto pelo Spring Cloud Task.
  3. Garante a resolução correta de placeholders de perfis em variáveis dependentes do Cloud Config Server (`${spring.profiles.active}`).
