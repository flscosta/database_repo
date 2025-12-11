# Configuração de uma Instância de Banco de Dados no Microsoft Azure

Este documento descreve o processo passo a passo para configurar uma instância de Banco de Dados no Microsoft Azure, focando no Azure SQL Database (um serviço gerenciado de banco de dados relacional). O guia é baseado na criação de um banco de dados único no tier de computação serverless usando o Portal do Azure. Inclui pré-requisitos, etapas de criação, opções de configuração e passos pós-criação.

## Pré-requisitos
- Uma assinatura ativa do Azure. Se não tiver, crie uma conta gratuita.
- Permissões necessárias:
  - Para criação via portal: Funções Azure RBAC como Contributor, SQL DB Contributor ou SQL Server Contributor.
  - Para criação via Transact-SQL: Permissões `CREATE DATABASE` (admin do servidor, admin Microsoft Entra ou membro da função `dbmanager`).

## Etapas de Criação

1. **Navegue até o Hub do Azure SQL**
   - Acesse o Portal do Azure em `https://portal.azure.com/#home`.
   - Em **Azure SQL Database**, selecione **Mostrar opções** e depois **Criar SQL Database**.

2. **Configure as Configurações Básicas**
   - **Assinatura**: Selecione sua assinatura do Azure.
   - **Grupo de Recursos**: Selecione **Criar novo**, insira um nome (ex.: `meuGrupoDeRecursos`) e clique em **OK**.
   - **Nome do Banco de Dados**: Insira um nome único (ex.: `meuBancoDeDadosExemplo`).
   - **Servidor**: Selecione **Criar novo** e preencha:
     - **Nome do Servidor**: Insira um nome globalmente único (ex.: `meuservidorsql` + sufixo).
     - **Localização**: Escolha uma região.
     - **Método de Autenticação**: Selecione **Usar autenticação SQL**.
     - **Senha**: Insira e confirme uma senha forte.
   - Clique em **OK**.

3. **Defina a Opção de Pool Elástico**
   - Defina **Deseja usar pool elástico SQL** como **Não**.

4. **Selecione o Ambiente de Carga de Trabalho**
   - Escolha **Desenvolvimento** (configura serverless, Propósito Geral, 1 vCore, backup redundante local, pausa automática após 1 hora).
   - (Opcional: Escolha **Produção** para backup geo-redundante, 2 vCores, 32 GB de armazenamento.)

5. **Configure Computação + Armazenamento**
   - Em **Computação + armazenamento**, selecione **Configurar banco de dados**.
   - Defina **Camada de Serviço** como **Propósito Geral (Mais econômico, computação serverless)**.
   - Defina **Camada de Computação** como **Serverless**.
   - Selecione **Aplicar**.

6. **Defina a Redundância de Armazenamento de Backup**
   - Escolha **Redundante Local** (para desenvolvimento) ou **Geo-redundante** (para produção).

7. **Configure a Rede**
   - **Método de Conectividade**: Selecione **Endpoint Público**.
   - **Regras de Firewall**: Ative **Adicionar endereço IP do cliente atual**.
   - Desative **Permitir que serviços e recursos do Azure acessem este servidor** (a menos que necessário).
   - **Política de Conexão**: Use **Padrão** com **Versão mínima de TLS** definida como **TLS 1.2**.

8. **Configurações Adicionais**
   - **Fonte de Dados**: Selecione **Exemplo** para criar um banco de dados de amostra `AdventureWorksLT`.
   - Opcionalmente, configure **Collation** ou **Janela de Manutenção**.

9. **Revise e Crie**
   - Selecione **Revisar + criar**.
   - Após a validação, selecione **Criar**.

## Etapas Pós-Criação

1. **Aguarde a Implantação**
   - A implantação pode levar vários minutos. Monitore o progresso no portal.

2. **Consulte o Banco de Dados**
   - Navegue até **Bancos de dados SQL** no portal.
   - Selecione seu banco de dados.
   - Abra o **Editor de Consultas (visualização)**.
   - Faça login usando autenticação SQL (login de admin/senha).
   - Execute uma consulta de amostra:
     ```sql
     SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
     FROM SalesLT.ProductCategory pc
     JOIN SalesLT.Product p
     ON pc.productcategoryid = p.productcategoryid;
     ```
   - Revise os resultados e feche o editor.

3. **Limpeza de Recursos (Quando Concluído)**
   - Vá para **Grupos de recursos**.
   - Selecione `meuGrupoDeRecursos`.
   - Selecione **Excluir grupo de recursos** e confirme.

Este processo cria um banco de dados Azure SQL único, serverless, com dados de amostra, configurado para uso em desenvolvimento, protegido por regras de firewall e pronto para consultas via Portal do Azure.
