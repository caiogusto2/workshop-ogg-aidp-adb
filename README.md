# Workshop: OCI GoldenGate, AI Data Platform e Autonomous Database

Este repositório reúne os materiais e os passos do workshop de integração e replicação de dados usando **Oracle Cloud Infrastructure (OCI) GoldenGate**, **Oracle AI Data Platform (AIDP)** e **Oracle Autonomous Database (ADB)**.

## Objetivo

Ao final do workshop, você será capaz de configurar uma conexão segura, associá-la a um deployment do OCI GoldenGate e executar uma replicação de dados entre os ambientes configurados.

## Arquitetura

O fluxo proposto utiliza os seguintes componentes:

- **OCI GoldenGate**: orquestração e execução da replicação.
- **Autonomous Database (ADB)**: banco de dados de origem e/ou destino.
- **Oracle AI Data Platform (AIDP)**: ambiente de dados integrado ao cenário do workshop.
- **OCI Vault**: armazenamento seguro de senhas, wallets e outras credenciais.
- **IAM Dynamic Group e Policies**: autorização do deployment GoldenGate para acessar os secrets necessários.

## Pré-requisitos

Antes de iniciar, confirme que você possui:

- Uma tenancy OCI com permissões para criar e administrar recursos do GoldenGate.
- Um deployment OCI GoldenGate em execução.
- Instâncias ADB e AIDP disponíveis e com conectividade configurada.
- Usuários de banco de dados com os privilégios exigidos para captura e replicação.
- OCI Vault com os secrets de senha e, quando aplicável, wallet do banco.
- Acesso de rede entre o deployment GoldenGate e os endpoints de origem e destino.

## Roteiro do workshop

1. Prepare os bancos de dados de origem e destino.
2. Crie os secrets no OCI Vault para senhas e wallets necessários.
3. Configure o Dynamic Group e as políticas IAM do deployment GoldenGate.
4. Crie as conexões no OCI GoldenGate para ADB e AIDP.
5. Atribua as conexões ao deployment GoldenGate.
6. Crie os processos de Extract e Replicat.
7. Execute a carga inicial e inicie a replicação contínua.
8. Valide os dados replicados no destino.

## Limpeza dos recursos

Ao concluir o workshop, remova os recursos que não serão mais utilizados para evitar custos desnecessários:

- Processos de replicação e conexões GoldenGate.
- Deployments OCI GoldenGate de teste.
- Secrets e versões de secret criados exclusivamente para o workshop.
- Recursos temporários de banco de dados, rede e armazenamento.

## Referências

- [Documentação do OCI GoldenGate](https://docs.oracle.com/en/cloud/paas/goldengate-service/)
- [Políticas do OCI GoldenGate](https://docs.oracle.com/en/cloud/paas/goldengate-service/ocigg/oracle-cloud-infrastructure-goldengate-policies.html)
- [OCI Vault](https://docs.oracle.com/pt-br/iaas/Content/KeyManagement/Concepts/keyoverview.htm)
