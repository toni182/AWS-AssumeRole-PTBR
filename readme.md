# Pré-requisitos

-   **Acesso administrativo** à sua conta AWS (Conta A) e à conta do
    cliente (Conta B).\
    Os passos referentes à Conta B podem ser realizados por qualquer
    usuário com acesso administrativo nela.

------------------------------------------------------------------------

# 1. Configuração na Conta do Cliente (Conta B)

O cliente precisa criar uma **IAM Role** que você irá assumir.

## 1.1 Criar a Role

1.  Acesse **IAM → Roles → Create role**.\
2.  Em **Trusted entity**, selecione **AWS Account**.\
3.  Informe o **Account ID da sua conta** (Conta A).\
4.  (**Recomendado**) Ative **External ID** e defina um valor combinado
    com você.\
5.  Clique em **Next**.

------------------------------------------------------------------------

## 1.2 Atribuir Permissões

O cliente deve selecionar as permissões que deseja conceder, como:

-   **ReadOnlyAccess**, ou\
-   Políticas específicas conforme a necessidade.

Depois, clique em **Next** e finalize configurando:

-   **Nome da Role**\
-   **Sessão máxima permitida** (ex.: 1h)

Finalize criando a Role.

------------------------------------------------------------------------

## 1.3 Conferir a *Trust Policy* (Opcional, porém recomendado)

Após a criação da Role:

1.  Abra a Role e acesse a guia **Trust relationships**.\
2.  Clique em **Edit trust relationship** e confirme que a trust está
    apontando para sua conta (ou uma role específica da sua conta).

Exemplo de *trust policy* típica:

``` json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<SUA-CONTA-ID>:root"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": "ID-EXTERNO-ACORDADO"
        }
      }
    }
  ]
}
```

O cliente deve enviar para você:

-   **Account ID** da conta dele\
-   **Nome da Role**\
-   **External ID** (se aplicável)

------------------------------------------------------------------------

# 2. Acessando a Role na Sua Conta (Conta A)

Agora você fará o acesso à Role do cliente pelo console da AWS usando
**Switch Role**.

1.  Faça login normalmente na sua conta (Conta A).\

2.  No canto superior direito da Console, clique no nome da sua
    conta/usuário.\

3.  Selecione **Switch Role**.\

4.  Preencha:

    -   **Account ID**: ID da conta do cliente\
    -   **Role name**: nome da Role criada pelo cliente\
    -   **Display name**: nome amigável que aparecerá na barra superior\
    -   **Color** (opcional)

5.  Clique em **Switch Role**.

Agora você está operando na conta do cliente assumindo a Role criada.

------------------------------------------------------------------------

# 3. Encerrando a Sessão

Para voltar para sua conta:

1.  Clique no **banner superior colorido** (que mostra que você está em
    uma Role).\
2.  Selecione **Back to *sua conta*** ou **Sign Out**, dependendo da
    interface exibida.
