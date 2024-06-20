# Guideline FrontEnd

## Pré-requisitos

Antes de iniciar o desenvolvimento ou a execução do ambiente de desenvolvimento da Viitra, certifique-se de ter instalado em sua máquina as seguintes ferramentas:

- **Node.Js 20**: O Node.js é um ambiente de tempo de execução JavaScript que permite executar JavaScript no lado do servidor. A versão 20 é necessária para garantir compatibilidade com as dependências do projeto. A instalação pode ser feita através do site oficial [Node](https://nodejs.org/en).
- **Yarn**: Yarn é um gerenciador de pacotes JavaScript que oferece melhor desempenho e segurança em comparação com o npm (Node Package Manager). Você pode instalar o Yarn através das instruções disponíveis no site oficial do [Yarn](https://yarnpkg.com).

## Subir ambiente de Dev

Para começar, certifique-se de que o Node.js 20 e o Yarn estejam instalados em sua máquina, conforme mencionado nos pré-requisitos.

Clone o repositório do projeto a partir do Azure DevOps:


### Instalar dependências

Navegue até o diretório do projeto e instale as dependências usando o Yarn:

 ```
 cd nome-do-projeto
 yarn install
 ```


### Acessar a Aplicação

Após a instalação das dependências, você pode iniciar o servidor de desenvolvimento executando o seguinte comando:

```
 yarn dev
 ```

Isso iniciará o servidor de desenvolvimento e você poderá acessar o projeto localmente em `http://localhost:3000`.



### Compilação para Produção

Quando estiver pronto para implantar sua aplicação em um ambiente de produção, você pode compilar os recursos usando o seguinte comando:

 ```
 yarn build
 ```

Isso criará uma versão otimizada do seu projeto na pasta build, pronta para implantação em um servidor de produção.


# Guideline BackEnd

## Pré-requisitos

Diferente de outros projetos, aqui utilizamos o backend no próprio next onde ele é escrito em node, quando o ambiente é iniciado como 

```
 yarn dev
 ```

 O backend sobe junto com a aplicação


#### Prisma

Prisma é um ORM (Object-Relational Mapping) open-source para Node.js e TypeScript. Ele é usado para facilitar a interação com seu banco de dados.

###### Acessando Banco de Dados

É possível acessar o banco pelo próprio prisma studio, com o seguinte comando

 ```
 yarn prisma studio
 ```

Será aberto o banco na web, onde vai ser possível realizar consultas, criar, editar e deletar dados.

**IMPORTANTE**: Toda vez que for alterado algo no banco, como tabela, coluna e relacionamentos, deve ser atualizado o arquivo do prisma no projeto, 

```
yarn prisma generate
```


 # Aplicação Crud

 O conceito da aplicação é bem simples, tem o intuíto de liberar acesso para professores de diferentes munícipio e escolas onde os mesmos irão visualizar Dashboards com o filtro de cada escola que pertence

 A implementação dos dados foi realizado através de uma planilha inviada pelo cliente que foi implementada pelo time de dados.

### Perfis

Existem 5 perfis atualmente no produto.

**ADM**: Único perfil que consegue cadastrar usuários do tipo secretária

**SECRETARIA**: Único perfil que consegue cadastrar todos os outros tipos de usuários (DIRETOR, COORDENADOR, PROFESSOR). A secretaria é vinculada a um munícipio, quando é realizado o cadastro dos outros usuários, automaticamente é herdado o munícipio que está no cadastro da secretaria 

**DIRETOR**: Perfil que pode cadastrar coordenadores da escola em que atua, esse usuário possuí o munícipio herdado da secretária e a escola em que ele atua, ao cadastrar coordenadores, automaticamente é herdado a escola e o munícipio do usuário que realizou o cadastro.

**Coordenador**: Perfil que pode cadastrar professores da escola em que atua, esse usuário possuí o munícipio herdado do Diretor/Secretaria, escola do diretor e a série em que atua (2º, 5º, 9º), ao cadastrar professores, automaticamente é herdado munícipio, escola e série do usuário que realizou o cadastro.

**Professor**: Perfil que só acessa a plataforma e consome os dashboards, não existe nenhum cadastro permitido para esse grupo

**IMPORTANTE** Nenhum cadastro é inserido senha, **TODOS** os cadastros após serem finalizados o usuário irá receber uma senha gerada pelo email e após o acesso será solicitado a troca dessa senha.

### Redefinição de senha

Após o usuário solicitar redefinição de senha é gerado um token no banco, e enviado um email com o link de redefinição contendo o token que foi gerado ao usuário, ao acessar o link o usuário é redirecionado para um painel contendo um formulário, após finalizar é validado o token na request da api

```javascript
 const onFinished = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();

    try {
      await resetPasswordToken(params.token, senha, confirm);
      toast({
        title: "Senha redefinida com sucesso",
      });
      router.push("/login");
    } catch (error: any) {
      console.log("error", error);
      let errorMessage = error.message;
      toast({
        title: errorMessage,
        variant: "destructive",
      });
    }
  };
```

O endpoint resetPasswordToken pode ser encontrado no caminho `app/api/user/reset-password/resetpassword.ts`

```javascript
export async function resetPasswordToken(
  token: string,
  password: string,
  confirmPassword: string
) {
  try {
    if (
      !password ||
      typeof password !== "string" ||
      password !== confirmPassword
    ) {
      throw new Error("Senhas não conferem");
    }

    const passwordResetToken = await db.password_reset_token.findUnique({
      where: {
        token,
        createAt: { gt: new Date(Date.now() - 1000 * 60 * 60 * 4) },
        resetAt: null,
      },
    });

    if (!passwordResetToken) {
      throw new Error("Token inválido ou expirado");
    }

    const encrypted = await hash(password, 12);

    const updateUser = db.tb_usuario.update({
      where: { id: passwordResetToken.id_usuario },
      data: {
        senha: encrypted,
      },
    });

    const updateToken = db.password_reset_token.update({
      where: {
        id: passwordResetToken.id,
      },
      data: {
        resetAt: new Date(),
      },
    });

    await db.$transaction([updateUser, updateToken]);
  } catch (err: any) {
    throw new Error(err.message);
  }
}
```

##### Função `resetPasswordToken`

A função `resetPasswordToken` é utilizada para redefinir a senha de um usuário com base em um token de redefinição de senha. Ela executa os seguintes passos:

1. **Validação de Senhas**: Verifica se as senhas fornecidas são válidas e coincidem.
2. **Verificação do Token**: Consulta na base de dados pelo token de redefinição para garantir que seja válido (criado nas últimas 4 horas) e ainda não utilizado para resetar a senha.
3. **Criptografia da Senha**: Utiliza uma função de hash para criptografar a nova senha antes de armazená-la na base de dados.
4. **Atualização na Base de Dados**: Atualiza a senha do usuário com a senha criptografada e marca o token como utilizado, garantindo que não possa ser usado novamente.

##### Parâmetros

- `token` (string): O token de redefinição de senha recebido.
- `password` (string): A nova senha a ser definida para o usuário.
- `confirmPassword` (string): Confirmação da nova senha para verificação.

##### Retorno

Esta função não retorna nenhum valor diretamente, mas utiliza uma Promise para sinalizar a conclusão da operação de redefinição de senha.

#### Exceções

- Lança um erro se as senhas fornecidas não coincidirem.
- Lança um erro se o token fornecido for inválido ou expirado, impedindo a redefinição da senha.



