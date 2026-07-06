# 📋 Questionário de Configuração de Novo Job no Projeto Existente

Este questionário serve como guia para planejar e coletar os requisitos necessários para a implementação de um novo processamento em lote (Spring Batch Job) dentro do projeto existente `desktop-fiscal-invoice-email-batch`.

---

### 1. Identificação do Novo Job
* **1.1. Qual será o nome do Bean do novo Job (em camelCase)?** (Ex: `sendSmsTask`, `generateInvoicePdfTask`)
  > generateDocumentTask
* **1.2. Qual será a propriedade `app.jobName` no arquivo de propriedades (ou Cloud Config)?** (Ex: `job2Name: sendSms`)
  > requestInteractionTask
* **1.3. Qual será a task cadastrada na tabela `batch_config`?** 
  *(Nota: O sistema resolve e valida a permissão da task buscando o nome inferido em kebab-case a partir do nome do Bean: `desktop-fiscal-invoice-email-[nome-do-bean-kebab-case]`. Por exemplo, se o Bean for `sendSmsTask`, a task correspondente será `desktop-fiscal-invoice-email-send-sms-task`)*
  > request-interaction-task

### 2. Pacotes e Estrutura de Classes
* **2.1. Deseja organizar as novas classes (Reader, Writer, Config) sob uma estrutura de pacotes específica para este Job ou dentro das pastas globais?**
  * *Opção Recomendada:* Manter no padrão geral do projeto, alocando as classes de acordo com a sua funcionalidade junto com as outras classes iguais (ex: `br.com.desktop.fiscal.invoice.email.batch.job.reader` para leitores, `writer` para escritores, `processor` para processadores, e `configuration` para as configurações).
  > Manter no padrão geral do projeto (pastas globais por funcionalidade)
* **2.2. Qual será o nome do DTO de entrada para este Job?** (Ex: `InvoiceSmsDTO.java`)
  > InvoiceEmailDTO

### 3. Origem & Leitura dos Dados (Reader)
* **3.1. Quais tabelas e colunas serão lidas na origem (banco de leitura/negócio)?**
  > tabela: faturas_email
  > colunas: id, demonstrativo, cobranca, status, ultima_atualizacao, caminho, interacao_uuid, descricao_erro
* **3.2. Qual é a query SQL (ou critérios de filtro) para buscar os registros pendentes?**
  > select id, demonstrativo, status, ultima_atualizacao, caminho, interacao_uuid, descricao_erro  
from faturas_email
where status = 'preparado'
* **3.3. Quais campos (e tipos Java correspondentes) compõem o DTO de entrada?**
  > id (Long)
  > demonstrativo (String)
  > cobranca (string)
  > status (String)
  > ultima_atualizacao (LocalDateTime)
  > caminho (String)
  > interacao_uuid (String)
  > descricao_erro (String)

### 4. Processamento & Regra de Negócio (Processor)
* **4.1. Quais validações de dados (Jakarta Validation/Lombok/Custom) devem ser aplicadas ao registro no processador?**
  > demonstrativo e cobranca não pode ser nulo
* **4.2. Quais transformações ou lógica de negócios serão executadas?**
  > Preparar o body para chamada a api de interação, gerar o UUID String no formato padrão (8-4-4-4-12)
  > Coletar o Num e Nome do cliente da tabela usuarios, hash da tabela fatura 
  > A partir do campo item.caminho fazer a substituição para dois tipos de documento 1 "danfe,demonstrativo" e "xml" que deverá ser utilizado no json do body campo "attachmentsPath"
  > deverá ser chamado a api http://{srv-api-interaction}/{caminho} e analisar o retorno
  > Body Exemplo:
{
  "interactionUuid": "uuid",
  "num": "1355172",
  "eventType": "NOTA_FISCAL",
  "originSystem": "fiscal-invoice",
  "attachmentsPath": [
    "http://apiserver-dev.intradesk/customer-bill-fiscal-invoice/v1/fiscal-invoices/92340643/documents/danfe,demonstrativo/download",
    "http://apiserver-dev.intradesk/customer-bill-fiscal-invoice/v1/fiscal-invoices/92340643/documents/xml/download"
  ],
  "metadata": {
    "nomeCliente": "Bruce Wayne",
    "numeroBoleto": "202605000124",
    "linkDanfe": "16854315681324684321",
    "uuid": "uuid",
    "email": null,
    "status": null,
    "data": null,
    "erro": null
  }
}
* **4.3. Este Job publicará eventos em tópicos Kafka? Se sim:**
  * Qual é o nome do tópico de destino?
  * Quais campos devem ir no payload da mensagem (DTO de saída/envio)?
  * Qual campo deve ser usado como a chave (key) da mensagem do Kafka?
  > 

### 5. Destino & Escrita (Writer)
* **5.1. Qual operação de persistência (INSERT, UPDATE ou DELETE) deve ser feita na tabela de destino?**
  > caso o retorno da api seja ok, atualizar a tabela faturas_email na coluna status para 'solicitado'
  > caso o retorno da api seja diferente de ok, atualizar a tabela faturas_email na coluna status para 'erro' e descricao_erro para a mensagem de erro
* **5.2. Qual é a query SQL de escrita que o Writer deve executar no banco de negócios?** (Ex: `INSERT INTO ...` ou `UPDATE ...`)
  > update faturas_email
set interacao_uuid = %s, status = %s, descricao_erro = %s
where id = %s
A coluna interacao_uuid somente devera ser preenchida caso seja sucesso, no caso de erro deverá ser enviado null

* **5.3. Qual(is) campo(s) do DTO serão passados como parâmetro na query SQL?** (Ex: `:id`, `:status`)
  > :id, :interacao_uuid, :status, :descricao_erro

### 6. Parâmetros de Execução & Resiliência
* **6.1. Qual o tamanho do lote (`chunkSize`) ideal para processamento em cada transação?** (Padrão do projeto: 100)
  > campo app.chunk_size recebido do cloud config server
* **6.2. Quais exceções devem ser ignoradas/puladas (Skip) e qual o limite máximo de pulos (`maxSkipCount`) aceitável?** (Padrão: `RuntimeException` com limite 100)
  > (Padrão: `RuntimeException` com limite 100)
  > Deverá ser gerado um teste de integração validando que os registros estão sendo processados mesmo com algum dando erro e sem interromper o fluxo da task
* **6.3. Há necessidade de novos parâmetros de configuração em `application.yml` específicos para este Job?**
  > sim, o campo app.srv-api-interaction
  >app.srv-api-interaction: http://apiserver-dev.intradesk
  
