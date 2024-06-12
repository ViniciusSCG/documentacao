# Guideline FrontEnd

## Pré-requisitos

Antes de iniciar o desenvolvimento ou a execução do ambiente de desenvolvimento do ChatGPTeacher, certifique-se de ter instalado em sua máquina as seguintes ferramentas:

- **Node.Js 17**: O Node.js é um ambiente de tempo de execução JavaScript que permite executar JavaScript no lado do servidor. A versão 17 é necessária para garantir compatibilidade com as dependências do projeto. A instalação pode ser feita através do site oficial [Node](https://nodejs.org/en).
- **Yarn**: Yarn é um gerenciador de pacotes JavaScript que oferece melhor desempenho e segurança em comparação com o npm (Node Package Manager). Você pode instalar o Yarn através das instruções disponíveis no site oficial do [Yarn](https://yarnpkg.com).

## Subir ambiente de Dev

Para começar, certifique-se de que o Node.js 17 e o Yarn estejam instalados em sua máquina, conforme mencionado nos pré-requisitos.

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

Antes de iniciar o desenvolvimento ou a execução do ambiente de desenvolvimento do ChatGPTeacher no backend, certifique-se de ter instalado em sua máquina as seguintes ferramentas:

- **Java 17**: Java é uma linguagem de programação e uma plataforma de computação amplamente utilizada. A versão 17 é necessária para garantir compatibilidade com as dependências do projeto. Você pode baixar e instalar o JDK 17 a partir do site oficial da Oracle ou utilizar uma distribuição OpenJDK. [JAVA](https://www.oracle.com/java/technologies/javase-jdk17-downloads.html).
- **Maven**: Maven: Maven é uma ferramenta de gerenciamento de projetos amplamente utilizada para construir, gerenciar e documentar projetos Java. Certifique-se de ter o Maven instalado em sua máquina. Você pode baixar e instalar o Maven a partir do site oficial da Apache Maven. [Maven](https://maven.apache.org/download.cgi).

#### Extensões VSCode

- **Extension Pack for Java**: Esta extensão inclui tudo o que você precisa para começar a desenvolver em Java no Visual Studio Code. Ela inclui suporte ao Language Server Protocol (LSP), Debugger para Java, Maven, entre outras ferramentas úteis.

- **Spring Boot Extension Pack**: Esta extensão fornece suporte aprimorado para o desenvolvimento de aplicativos Spring Boot, incluindo realce de sintaxe, snippets e integração com o Spring Initializr.


## Configuração de Ambiente

Após instalar o Java 17, o Spring Boot e o Maven, você pode seguir os passos abaixo para configurar o ambiente do backend:

### Clonar o Repositório

Clone o repositório do projeto a partir do Azure DevOps:

 ```
 git clone https://LEXP@dev.azure.com/LEXP/ChatGPTeacher/_git/chatgpteacher-api
 ```

### Instalar Dependências

O Maven gerencia as dependências automaticamente com base no arquivo de configuração pom.xml. Navegue até o diretório raiz do projeto clonado e execute o seguinte comando para baixar e instalar as dependências:

 ```
 mvn install
 ```

### Configurar o Banco de Dados

 configure as informações de conexão no arquivo de configuração apropriado (application.properties) para garantir que o aplicativo possa se conectar ao banco de dados corretamente.


### Executar o Projeto

Inicie o projeto executando a classe principal que contém o método main() do Spring Boot. Isso iniciará o servidor de aplicativos e disponibilizará os endpoints conforme configurado no projeto.

É possível iniciar o projeto por terminal sem a nececssidade de abrir uma IDE usando o comando:

 ```
 mvn spring-boot:run
 ```

 Após iniciar o projeto, você pode acessar os endpoints localmente em `http://localhost:8080`.

 ### Documentação da API

 Documente os endpoints da API, incluindo suas entradas, saídas e qualquer informação relevante para facilitar o uso da API pelos desenvolvedores frontend. É realizado a documentação utilizando o Swagger, pode ser acessado em `http://localhost:8080/swagger-ui/index.html#/`
 Certifique-se de que a documentação da API esteja sempre atualizada conforme novos endpoints são adicionados ou alterados no projeto.