# Configuração Ambiente

## 🎯 **Objetivos**

Criar e configurar tenancy OCI para conseguirmos realizar o laboratórios das próximas sessões

O que faremos:

- Criar VCN com subnet publica e privada
- Criar vault e secrets.
- Criar instância AIDP.
- Criar um Autonomous DB.
- Criar catálogo, volume, workspace e cluster Spark.
- Criar dynamic group e policies OGG
- Criar bucket bulk upload OGG

# **Parte 1 - Configuração de ambiente**

## **1️⃣ Criação de VCN**

Primeiramente temos que **criar uma VCN com pelo menos uma subnet pública e privada**, navegue até a tela de administração de rede

![vcn01](images/vcn01.png)

Clique em **actions e selecione start VCN Wizard**

Selecione Create **VCN with internet connectivity** e clique em start VCN wizard

![vcn02](images/vcn02.png)

Dê o nome de **vcn01**, aceite todas as configuraçõe default e faça a criação

![vcn03](images/vcn03.png)

Após concluido, siga adiante

## **2️⃣ Criação de OCI Vault**

Siga até a tela do OCI Vault

![vault01](images/vault01.png)

Clique em **create vault** e preencha conforme o exemplo

![vault02](images/vault02.png)

Concluida a criação, clique na aba **Master encryption key**, clique em create key e preencha conforme o exemplo

![vault03](images/vault03.png)

Concluida a criação da master key, **clique no icone ao lado para retornar**

![vault04](images/vault04.png)

Selecione **Secret Management e clique em create secret**, preencha conforme o exemplo abaixo, utilize a senha **WORKSHOPsec2019##**

![vault05](images/vault05.png)

**Duplique a sua guia do OCI** e clique em user settings

![vault06](images/vault06.png)

Clique em **tokens and keys** e depois em **Add API Key. Faça o download da chave privada e também anote a `Fingerprint` em um bloco de notas**

![vault07](images/vault07.png)

Retorne à aba do navegador e **crie um novo secret** chamado `pem_key`, cole a chave privada da etapa anterior, conforme o exemplo

![vault08](images/vault08.png)

## **3️⃣ Criação de Instância AIDP**

![Link AIDP](images/link_aidp.png)

Dê um nome à sua instância e ao seu workspace e escolha as **políticas padrão**. Não há necessidade de preencher a seção que contém o Autonomous AI Lakehouse.

![Form01](images/form01.png)

![Form02](images/form02.png)

> **⚠️ ATENÇÃO:** A criação da sua instância AIDP deve demorar cerca de uns 10 minutos para conclusão.

## **4️⃣ Criação Autonomous DB**

![Link ADB](images/link_adb.png)

Crie um Autonomous Database utilizando as seguintes configurações.

![Form01_ADB](images/form01_adb.png)

> **⚠️ ATENÇÃO:** Garanta que a versão do Autonomous Database seja 26ai.

![Form02_ADB](images/form02_adb.png)

> **⚠️ ATENÇÃO:** A sugestão é utilizar a senha **WORKSHOPsec2019##**; contudo, você pode escolher outra senha, se desejar. O restante das configurações pode permanecer no padrão; em seguida, clique em **Create**.

![Form03_ADB](images/form03_adb.png)

> **⚠️ ATENÇÃO:** A criação da sua instância do Autonomous Database deve demorar cerca de 5 minutos para ser concluída.

## **5️⃣ Configuração AIDP**

Retorne à instância do AIDP e crie **um catálogo Standard**.

![caminho_aidp01](images/caminho_aidp01.png)

Clique em **Master Catalog**, depois em **Create Catalog**, dê o nome **demo** ao seu catálogo e clique em **Create**.

![catalog_create](images/catalog_create.png)

Clique no catálogo demo, no schema default, escolha volumes e crie **um volume virtual Standard** clicando no botão de + (próximo ao campo de filtragem). Dê o nome de vol01

![volume01](images/volume01.png)

Agora clique na aba do canto esquerdo em workspace, e crie **um Workspace** clicando no botão de + (próximo ao campo de filtragem). Dê o nome de workspace01.

![workspace01](images/workspace01.png)

Após concluida a criação do workspace, clique no mesmo, e na aba do canto esquerdo selecione compute. Crie **um cluster Spark**, clicando no botão de + (próximo ao campo de filtragem). Dê o nome de spark01.

![compute_spark01](images/compute_spark01.png)

Faça a integração do Autonomous Database com o AIDP, através da aba Master Catalog

Clique em create catalog, escolha o tipo external.

Na tela de detalhes do seu Autonomous Database, clique em **Database Connection** e faça o download de sua wallet

![autonomous_connection](images/autonomous_connection.png)

> **⚠️ ATENÇÃO:** A sugestão é utilizar a senha **WORKSHOPsec2019##**, contudo você pode escolher um outra senha se assim desejar.

Dê o nome de **adb01** ao catálogo, faça o upload da wallet no formulário do AIDP, escolha o serviço **Medium** e preencha as demais informações conforme o print abaixo.

![aidp_adb_connect](images/aidp_adb_connect.png)

## **6️⃣ Setup Dynamic Group e Policies**

Antes de iniciarmos o setup, vamos buscar pelo OCID de nosso compartment. **Navegue até a página de gestão de compartments**

![ocid01](images/ocid01.png)

Busque pelo seu compartment e **colete o OCID** igual ao exemplo. **Guarde essa informação pois usaremos ela em outras atividades**

![ocid02](images/ocid02.png)

Agora **navegue até a aba de domínios**

![iam01](images/iam01.png)

Selecione o compartment root e **clique no hyperlink default** na parte de cima da tela

![iam02](images/iam02.png)

Busque a guia **dynamic group** na tela que irá se abrir e clique em **create dynamic group**. Preencha conforme o exemplo, dê o nome de `ogg-dynamic-group`

![iam03](images/iam03.png)

```python
ALL {resource.type = 'goldengatedeployment', resource.compartment.id = 'SUBSTITUIR AQUI PELO SEU OCID DO SEU COMPARMENT'}
```

Agora **clique no canto esquerdo em policies e create new policy**

![iam04](images/iam04.png)

Preencha conforme o exemplo

![iam05](images/iam05.png)

```python
allow dynamic-group ogg-dynamic-group to manage object-family in tenancy
allow dynamic-group ogg-dynamic-group to use vaults in tenancy
allow dynamic-group ogg-dynamic-group to use keys in tenancy
allow dynamic-group ogg-dynamic-group to read secret-bundles in tenancy
```

## **7️⃣ Criação de bucket bulk upload OGG**

Siga até o object storage e crie um bucket chamado `bucket-load-aidp`

![oss01](images/oss01.png)

![oss02](images/oss02.png)

------------------------------------------------------------------------

## **✅ Laboratório finalizado!**

Parabéns! Você concluiu o processo de criação da infra estrutura que será utilizada nos proximos capitulos desse workshop.


## 👥 Agradecimentos

- **Autores** - Caio Oliveira
- **Autores Contribuintes** - Isabelle Anjos
- **Última atualização** - Agosto de 2026

## 🛡️ Declaração de Porto Seguro (Safe Harbor)

O tutorial apresentado tem como objetivo traçar a orientação dos nossos produtos em geral. É destinado somente a fins informativos e não pode ser incorporado a um contrato. Ele não representa um compromisso de entrega de qualquer tipo de material, código ou funcionalidade e não deve ser considerado em decisões de compra. O desenvolvimento, a liberação, a data de disponibilidade e a precificação de quaisquer funcionalidades ou recursos descritos para produtos da Oracle estão sujeitos a mudanças e são de critério exclusivo da Oracle Corporation.

Esta é a tradução de uma apresentação em inglês preparada para a sede da Oracle nos Estados Unidos. A tradução é realizada como cortesia e não está isenta de erros. Os recursos e funcionalidades podem não estar disponíveis em todos os países e idiomas. Caso tenha dúvidas, entre em contato com o representante de vendas da Oracle. 
