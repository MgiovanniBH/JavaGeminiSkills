# 📋 Questionário de Configuração do Novo Job Batch

Por favor, preencha as respostas abaixo para guiar a criação do novo projeto de processamento em lote.

---

### 1. Nome do Projeto & Identificadores
* **1.1. Qual é o nome do novo projeto/repositório?** (Ex: `desktop-financeiro-cobranca-batch`)
  > desktop-fiscal-invoice-email-batch
* **1.2. Qual será o `spring.application.name` no `application.yml`?** (Ex: `financeiro-cobranca-batch`)
  > desktop-fiscal-invoice-email-batch

### 2. Estrutura de Pacotes Java
* **2.1. Qual será o pacote base Java?** (Ex: `br.com.desktop.financeiro.cobranca.batch`)
  > br.com.desktop.fiscal.invoice.email.batch;

### 3. Origem dos Dados (Reader)
* **3.1. Qual tabela e colunas serão lidas na origem?** (Ex: tabela `adminclone.cobrancas`, colunas `id`, `valor`, `status`)
  > adminclone.faturas, adminclone.demonstrativos e adminclone.faturas_email
* **3.2. Qual é a query SQL (ou critérios de filtro) para buscar os registros pendentes?** (Ex: buscar cobranças vencidas e com status 'PENDENTE')
  > SELECT d.id
FROM faturas f
JOIN demonstrativos d on
    f.demonstrativo = d.id
LEFT JOIN
    faturas_email fe ON
	fe.demonstrativo = d.id
where
	f.status = 'E'	
	AND f.emissao LIKE CONCAT(DATE_FORMAT(NOW(), '%Y%m'), '%')
    AND fe.id IS NULL AND d.tipodocumento = 'J'
* **3.3. Quais campos (e tipos) compõem o DTO de entrada?**
  > id: long, ultima_atualizacao: datetime

### 4. Regra de Negócio & Processamento (Processor)
* **4.1. Quais validações de dados (Jakarta Validation) e regras de negócio devem ser aplicadas ao registro?** (Ex: valor deve ser maior que zero, CPF/CNPJ válido)
  > Antes de processar qualquer registro deverá ser consultada a tabela de parametrização de jobs, vendo se esta task está liberada para ser executada, abaixo o select:
  Task name: desktop-fiscal-invoice-email-create-request-task
  Select: select id from batch_config
where nome_task = 'desktop-fiscal-invoice-email-create-request-task'
and ativo = 1
  Caso não retorne nada, o job não deve ser executado e deverá ser logado um warning. Caso retorne algo, a task poderá ser executada normalmente.

* **4.2. Esse Job publicará eventos no **Kafka**? Se sim:**
  * Qual o nome do tópico de destino?
    > 
  * Quais campos devem ir no payload da mensagem (DTO de saída/envio)?
    > 
  * Qual campo deve ser usado como a chave (key) da mensagem do Kafka?
    > 

### 5. Destino & Escrita (Writer)
* **5.1. Após o processamento, os registros lidos no banco de origem devem ter seu status atualizado?** (Ex: de `PENDENTE` para `PROCESSADO` ou `ERRO`)
  > Não será atualizada a tabela de origem, o filtro será executado pelo registro existente na tabela fatura_email com o mesmo id da fatura.
* **5.2. Qual é a query SQL de `UPDATE` que o `Writer` deve executar?** (Ex: `UPDATE adminclone.cobrancas SET status = 'PROCESSADO' WHERE id = :id`)
  > INSERT INTO adminclone.faturas_email
(demonstrativo,ultima_atualizacao)
VALUES(:id,now());
* **5.3. Existe algum outro destino de banco ou escrita?**
  > Não.

### 6. Infraestrutura & CI/CD
* **6.1. Deseja manter o modelo padrão de **Dual DataSource** (um para o negócio/admin e outro para o Spring Batch)?**
  > Sim.
* **6.2. Há alguma necessidade especial de variáveis de ambiente ou alteração nos arquivos `Dockerfile`/`Jenkinsfile`?**
  > Não.
