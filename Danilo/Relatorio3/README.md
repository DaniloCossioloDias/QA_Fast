# **Relatório de Testes – API de Upload e Consulta de OS**

## **1️⃣ Objetivo**
O objetivo dos testes foi validar o funcionamento da API no **upload de arquivos CSV contendo ordens de serviço** e garantir que os registros fossem corretamente **inseridos no banco de dados** e consultados posteriormente.

---

## **2️⃣ Cenários Testados**
### **Teste 1 – Upload de CSV Válido**
- **Arquivo usado:** `testeUpload.csv`
- **Objetivo:** Enviar um **arquivo CSV correto** contendo colunas esperadas e valores válidos.
- **Resultado esperado:** API deve retornar **200** e **inserir os dados corretamente no banco**.
- **Resultado obtido:** API retornou `"0 ordens de serviço inseridas com sucesso."`, indicando que **nenhuma linha do CSV foi salva**.
- **Erro identificado:** Os registros estão sendo **descartados antes da inserção**.

---

### **Teste 2 – Upload de CSV com Colunas Erradas**
- **Arquivo usado:** `colunas_erradas.csv`
- **Objetivo:** Enviar um **CSV onde faltam colunas obrigatórias** para verificar se a API rejeita corretamente.
- **Resultado esperado:** API deve retornar **400 (Bad Request)** informando as colunas ausentes.
- **Resultado obtido:**
  - No **Swagger**, API retornou **400 corretamente**.
  - No **pytest**, API retornou **500**, indicando que a exceção está sendo capturada e modificada incorretamente no código.
- **Erro identificado:** O tratamento de erro na API precisa ser ajustado para que **400 seja retornado corretamente**.

---

### **Teste 3 – Upload de Arquivo que Não é CSV**
- **Arquivo usado:** `arquivo_errado.txt`
- **Objetivo:** Tentar enviar um **arquivo que não seja CSV** para testar a validação.
- **Resultado esperado:** API deve retornar **400** informando que o arquivo precisa ser CSV.
- **Resultado obtido:** API retornou **400 corretamente**.
- **API passou nesse teste sem erros.**

---

### **Teste 4 – Consulta de OS**
- **Objetivo:** Consultar todas as **ordens de serviço inseridas** e verificar se o banco contém registros válidos.
- **Resultado esperado:** API deve retornar **200** e listar todas as OS armazenadas.
- **Resultado obtido:**
  - Após o upload do CSV, a API retornou **"Nenhuma OS encontrada para análise."**, indicando que **os dados não estavam sendo salvos no banco**.
  - Após a **inserção manual de um registro via SQL**, o **GET funcionou corretamente** e retornou os dados inseridos.
- **Erro identificado:** O problema está na **inserção dos dados via upload CSV**, não na consulta.

---

### **Teste 5 – Upload de CSV com Valores Inválidos**
- **Arquivo usado:** `valores_errados.csv`
- **Objetivo:** Testar a API com valores incorretos, como **texto na coluna de valores numéricos** e **valores negativos**.
- **Resultado esperado:** API deve retornar **400 ou 500**, rejeitando os registros inválidos.
- **Resultado obtido:** API retornou **200**, mas `"0 ordens de serviço inseridas com sucesso."`, indicando que **os registros foram descartados**.
- **Erro identificado:** A API **não trata valores inválidos explicitamente**, descartando registros sem gerar erro.

---

## **3️⃣ Análise dos Problemas**
- **O upload do CSV não está inserindo registros no banco**  
- **A conversão da coluna `data` pode estar removendo todas as linhas**  
- **O pytest está alterando o retorno da API, convertendo erros 400 para 500**  
- **A consulta funciona corretamente quando os dados são inseridos manualmente**  
- **A API descarta registros inválidos sem retornar erro adequado**  

---

## **4️⃣ Possíveis Soluções**
- **O `try-except` captura erros esperados e altera o retorno da API**, fazendo com que 400 seja convertido em 500.  
- **A conversão de datas (`pd.to_datetime()`) pode estar removendo registros antes da inserção no banco.**  
- **O `await db.commit()` pode não estar salvando corretamente os dados após o upload do CSV.**  
- **Adicionar validação para valores inválidos (`cinquenta`, `-300`) e retornar erro adequado.**  
