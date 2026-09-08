# Laboratório OGG

## 🎯 **Objetivos**

Descobrir como utilizar de forma prática o serviço OCI Oracle GoldenGate, criando uma extração de dados a partir do Oracle Database e replicando os dados para uma instância AIDP 

O que você aprenderá:

- Criar conexões no OCI GoldenGate.
- Criar e configurar diferentes tipos de deployments.
- Interconectar deployments de Goldengate.
- Criar e configurar extracts e replicats.

## 📌 Introdução

> **O laboratório tem a proposta de trazer a experiência de criação e configuração do Oracle Goldengate entre diferentes tecnologias** 

# **Parte 2 - Hands On AI OCI Goldengate**

## **1️⃣ Criação de Deployments**

**Navegue até* a console do **Oracle Goldengate**

![link_ogg](images/link_ogg.png)

Clique em **Deployments e depois crie o primeiro deployment para o DB Oracle**, preencha conforme o exemplo

![deploy01](images/deploy01.png)

![deploy02](images/deploy02.png)

![deploy03](images/deploy03.png)

**Clique em advanced options e selecione uma interface publica** para sua instância ogg

![deploy04](images/deploy04.png)

Clique em create

Agora **repita o processo de criação de deployment e crie** o ferramental que será utilizado com o **AIDP**

![deploy05](images/deploy05.png)

Note que nessa parte você tem que escolher **big data como tecnologia**

![deploy06](images/deploy06.png)

![deploy07](images/deploy07.png)

**Em advanced options** coloque também o deployment em uma **interface publica** e clique em create

![deploy08](images/deploy08.png)

Caso tenha concluido todas as etapas com sucesso, teremos 2 deployments: um do tipo Oracle e outro do tipo Big Data como no exemplo abaixo

> **⚠️ ATENÇÃO:** Os deployments demoram cerca de 15 minutos para provisionamento

![deploy09](images/deploy09.png)

## **2️⃣ Criação de Conexões**

**Selecione connections** no canto esquerdo da tela e **clique em create connection**, primeiramente vamos montar uma conexão com um database Oracle

Preencha conforme o exemplo

![conn01](images/conn01.png)

Uma vez selecionado o tipo de conexão, **vá até a parte inferior do formulário, clique em advanced options e retire a flag de use vault secrets**

![conn02](images/conn02.png)

Retorne a parte superior do formulário e selecione, **Enter database information**, coloque a string abaixo e as informações conforme o print screen

**String de conexão**
```sql
167.126.45.99:1521/prd01
```
![conn03](images/conn03.png)

Faça a criação da conexão.

Vamos configurar agora a conexão com o AIDP, para isso **abra o AIDP em outra guia do navegador e navegue até o seu cluster spark** criado anteriormente e **selecione a aba connection details**

![conn04](images/conn04.png)

De volta a nossa guia de conexões do OGG, **crie uma nova conexão preenchendo conforme o exemplo**

![conn05](images/conn05.png)

Insira a URL do conector JDBC conforme o exemplo e selecione **Use current user and tenancy**. Selecione a região que esta trabalhando com o seu workshop, no seu caso sera **GRU, adicione a fingerprint da chave PEM**

![conn06](images/conn06.png)

Agora vamos criar os conectores que representam cada um dos deployments do goldengate

**Volte a interface de conexão e crie o primeiro como o exemplo abaixo**

![conn07](images/conn07.png)

vá até a **parte inferior do formulário, clique em advanced options e retire a flag de use vault secrets**

![conn02](images/conn02.png)

Retorne a parte superior do formulário e selecione **Select Goldengate deployment**, preencha conforme o exemplo

![conn08](images/conn08.png)

Crie a segunda conexão de forma idêntica a anterior

![conn09](images/conn09.png)

vá até a **parte inferior do formulário, clique em advanced options e retire a flag de use vault secrets**

![conn02](images/conn02.png)

Retorne a parte superior do formulário e selecione **Select Goldengate deployment**, preencha conforme o exemplo

![conn10](images/conn10.png)

Por último vamos criar a conexão com o **object storage, crie conforme o exemplo**

![oss01](images/oss01.png)

Similar ao setup do AIDP, faça a configuração abaixo

![oss02](images/oss02.png)

Concluido o setup, teremos 5 conexões:
- Conexão Oracle será nosso data source
- Conexão AIDP será o nosso data target
- Conexão daa irá representar o deployment bigdata
- Conexão orcl irá representar o deployment oracle
- Conexão com o object storage

![conn11](images/conn11.png)

Agora retornamos a tela de deployments, clique no **orcl_deployment** e depois em **Assigned connections**

Na aba que irá se abrir busque pelo campo **Other assigned connectionts (na parte inferior da página) e clique em Assign connection**

Adicione as duas conexões como no exemplo

![conn12](images/conn12.png)

Retorne a aba de deployments e faça o mesmo procedimento para o deployment **daa_deployment**

**Adicione as 3 conexões restantes**

![conn13](images/conn13.png)

## **3️⃣ Configuração e setup Extract**

Nesse setup o nosso objetivo será executar a extração de dados a partir do DB Oracle configurado anteriormente para esse projeto

Retorna à aba de deployments, clique em **orcl_deployment** e launch console. Utilize as **credenciais oggadmin e senha WORKSHOPsec2019##**

![ext01](images/ext01.png)

![ext02](images/ext02.png)

Na interface inicial valide a conexão com o database de origem

Clique na aba conexões de banco de dados e no botão de teste de conexão

![ext03](images/ext03.png)

Caso nenhum erro seja apresentado você terá conectado com sucesso

Agora no canto esquerdo, **clique em Extrações e no botão adicionar extração**

![ext04](images/ext04.png)

Em nosso workshop estamos trabalhando com um único banco de dados Oracle como origem e os processos extratores devem ter nomes únicos, por isso **preencha o campo nome do processo com as 2 primeiras letras do seu nome e o dia de seu nascimento**. Exemplo Caio e 22, ficará CA22. **Selecione extração integrada**

![ext05](images/ext05.png)

Preencha conforme o exemplo

![ext06](images/ext06.png)

![ext07](images/ext07.png)

```python
TRANLOGOPTIONS SOURCE_OS_TIMEZONE America/Sao_Paulo
DDL INCLUDE MAPPED
SOURCECATALOG SWING
TABLE SOE.*;
```

Caso o setup esteja correto, teremos o ícone verde ao lado do extrator

![ext08](images/ext08.png)

Clicando em estatística, veremos as transações sendo capturadas

![ext09](images/ext09.png)

## **4️⃣ Configuração Distribution Service**

Agora vamos configurar o serviço de envio de trail files para o goldengate DAA

Na aba do canto esquerdo clique em Caminhos do Distribution Services e depois em Adicionar Caminho de Distribuição

Dê o nome de **dist01**

![dist01](images/dist01.png)

**Escolha o trail file a ser replicado**

![dist02](images/dist02.png)

Agora para a próxima aba, retorna a console do OCI e **busque pelo deployment do DAA, na aba Details busque pela URL de conexão do OGG**

![dist03](images/dist03.png)

**Cole a URL sem https, seleciona a porta 443, o nome do trail de ab e alias como daa_deployment**

![dist04](images/dist04.png)

Para o restante das configurações, pode aceitar o default e fazer a criação

Concluido o setup teremos o seguinte

![dist05](images/dist05.png)

## **5️⃣ Configuração e setup Replicat**

Agora vamos seguir com o setup do ambiente e replicação dos dados para o AIDP, **retorne na console do OCI, clique no daa_deployment, launch console e apresente as credenciais oggadmin e WORKSHOPsec2019##**

![dist03](images/dist03.png)

No canto esquerdo, **clique em conexões de banco de dados e no icone de teste de conexão com o aidp.** 

![rep01](images/rep01.png)

Em caso de sucesso a seguinte tela será apresentada

![rep02](images/rep02.png)

Agora no canto esquerdo, **clique em replicações e adicionar replicação**

Dê o nome de **REP01**

![rep03](images/rep03.png)

Preencha a próxima tela conforme o exemplo

![rep04](images/rep04.png)

Aceite as configurações default da proxima tela

Preencha a guia de parâmetros da seguinte maneira

![rep05](images/rep05.png)

```python
SOURCECATALOG SWING
MAP SOE.*, TARGET demo.default.*;
```

E na próxima tela teremos que fazer o **preenchimento de alguns campos: compartment OCID e oci-bucket-name**, abaixo o setup de exemplo, faça o preenchimento com os dados do seu ambiente

![rep06](images/rep06.png)

O ogg esta configurado com as políticas de replicação default, logo fará a replicação das transações a cada 3 min para dentro do AIDP. Os parâmetros e definições do handler AIDP podem ser encontrados no link https://docs.oracle.com/en/database/goldengate/big-data/26/gadbd/oracle-ai-data-platform.html

No OGG navegue até a guia de estatísticas e clique em atualizar para ver as transações sendo atualizadas

![rep07](images/rep07.png)

Em cerca de 3 min veremos alguns objetos sendo criados no bucket do object storage

![rep08](images/rep08.png)

No AIDP, crie um notebook e faça a consulta do número de linhas da tabela delta no AIDP

```python
spark.sql("SELECT count(*) FROM demo.default.stresstesttable").show(10, truncate=False)
```

![rep09](images/rep09.png)

Na SparkUI do cluster spark vemos o processo que foi feito sobre a tabela delta, uma tabela temporária é criada no fluxo e então temos o merge de registros

![rep10](images/rep10.png)

Na guia de métricas, conseguimos ver claramente o uso dos recursos a partir da execução dos jobs spark de carregamento de tabelas

![rep11](images/rep11.png)

------------------------------------------------------------------------

## **✅ Laboratório finalizado!**

Parabéns! Você concluiu o hands-on do **OCI Goldengate**, construindo uma replicação funcional entre duas tecnologias heterogêneas

## 👥 Agradecimentos

- **Autores** - Caio Oliveira e Marcio Carbonera
- **Autores Contribuintes** - Isabelle Anjos
- **Última atualização** - Agosto de 2026

## 🛡️ Declaração de Porto Seguro (Safe Harbor)

O tutorial apresentado tem como objetivo traçar a orientação dos nossos produtos em geral. É destinado somente a fins informativos e não pode ser incorporado a um contrato. Ele não representa um compromisso de entrega de qualquer tipo de material, código ou funcionalidade e não deve ser considerado em decisões de compra. O desenvolvimento, a liberação, a data de disponibilidade e a precificação de quaisquer funcionalidades ou recursos descritos para produtos da Oracle estão sujeitos a mudanças e são de critério exclusivo da Oracle Corporation.

Esta é a tradução de uma apresentação em inglês preparada para a sede da Oracle nos Estados Unidos. A tradução é realizada como cortesia e não está isenta de erros. Os recursos e funcionalidades podem não estar disponíveis em todos os países e idiomas. Caso tenha dúvidas, entre em contato com o representante de vendas da Oracle. 