# Pré-requisitos

-   **Acesso administrativo** à sua própria conta AWS (**Conta A**) e à
    **Conta de Destino (Conta B)**.
-   Os passos referentes à Conta B podem ser executados por qualquer
    **usuário da Conta B** com permissões administrativas.

------------------------------------------------------------------------

# 1. Configuração da Role na Conta de Destino (Conta B)

A Conta B precisa disponibilizar uma **IAM Role** para ser assumida pela
Conta A.\
Existem dois métodos --- utilize **apenas um**, conforme sua
necessidade.

------------------------------------------------------------------------

## 1.1 Método A --- Criar a Role via CloudFormation (Recomendado)

**Arquivo da Stack:** [stack-ar.yaml](./stack-ar.yaml) (presente neste repositório)

Esta Stack cria automaticamente **uma IAM Role com permissões
administrativas completas** na Conta B.\
**IMPORTANTE:**\
A Role criada por este método possui **permissão administrativa
(AdministratorAccess)**.\
Use este método apenas se esse nível de privilégio for realmente
necessário.

Se você precisa de permissões mais restritas, utilize o **Método B**,
onde é possível selecionar exatamente quais permissões conceder.

### Como usar este método

1.  Acesse **CloudFormation** na Conta B.\
2.  Clique em **Create stack → With new resources (standard)**.\
3.  Faça upload do arquivo **`stack-ar.yaml`**.\
4.  Preencha os parâmetros solicitados (como `ExternalId` e
    `SourceAccountId`).\
5.  Crie a Stack e aguarde a finalização.\
6.  A Role será criada automaticamente.

------------------------------------------------------------------------

## 1.1 Método B --- Criar a Role manualmente via Console da AWS (Permissões personalizáveis)

Use este método se você **não deseja conceder permissões
administrativas** e prefere definir permissões específicas.

1.  Acesse **IAM → Roles → Create role**.\
2.  Em **Trusted entity**, selecione **AWS Account**.\
3.  Informe o **Account ID da Conta A**.\
4.  (**Recomendado**) Ative **External ID** e defina um valor
    previamente acordado.\
5.  Clique em **Next**.

### Atribuir permissões

Selecione as permissões desejadas, como:

-   **ReadOnlyAccess**, ou\
-   Políticas específicas conforme a necessidade.

Clique em **Next** e finalize especificando:

-   **Nome da Role**\
-   **Sessão máxima permitida** (ex.: 1h)

Crie a Role para concluir.

------------------------------------------------------------------------

## 1.2 Conferir a *Trust Policy* (Opcional, mas recomendado)

Após criar a Role (via Método A ou B):

1.  Abra a Role criada na Conta B.\
2.  Acesse **Trust relationships**.\
3.  Verifique se a trust aponta corretamente para a Conta A e, se
    aplicável, utiliza o External ID configurado.

------------------------------------------------------------------------

## 1.3 Informações que a Conta B deve enviar para a Conta A

-   **Account ID da Conta B**\
-   **Nome da Role criada**\
-   **External ID** (quando configurado)

------------------------------------------------------------------------

# 2. Acessando a Role a partir da Conta A

1.  Faça login na Conta A.\

2.  No canto superior direito, clique no nome da sua conta.\

3.  Selecione **Switch Role**.\

4.  Informe:

    -   **Account ID**: ID da Conta B\
    -   **Role name**: Nome da Role criada\
    -   **Display name**: Nome amigável que aparecerá na barra\
    -   **Color**: opcional

5.  Clique em **Switch Role**.

Você estará agora operando recursos da Conta B através da Role assumida.

------------------------------------------------------------------------

# 3. Encerrando a Sessão

Para voltar à Conta A:

1.  Clique no **banner colorido** indicando a Role ativa.\
2.  Selecione **Back to *sua conta*** ou **Sign Out**, conforme a
    interface.
