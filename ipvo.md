# Guideline FrontEnd

## Pré-requisitos

Antes de iniciar o desenvolvimento ou a execução do ambiente de desenvolvimento da IPVO, certifique-se de ter instalado em sua máquina as seguintes ferramentas:

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

Antes de iniciar o desenvolvimento ou a execução do ambiente de desenvolvimento da IPVO no backend, certifique-se de ter instalado em sua máquina as seguintes ferramentas:

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
 git clone https://LEXP@dev.azure.com/LEXP/IPVO/_git/ipvo-api
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



# Guideline Mobile (Expo v49)

## Pré-requisitos

Antes de iniciar o desenvolvimento ou configurar o ambiente de desenvolvimento da IPVO Mobile, certifique-se de ter instalado em sua máquina as seguintes ferramentas:

- **Node.js 17**: O Node.js é um ambiente de tempo de execução JavaScript que permite executar JavaScript no lado do servidor. A versão 17 é necessária para garantir compatibilidade com as dependências do projeto. A instalação pode ser feita através do site oficial [Node](https://nodejs.org/en).
- **Yarn**: Yarn é um gerenciador de pacotes JavaScript que oferece melhor desempenho e segurança em comparação com o npm (Node Package Manager). Você pode instalar o Yarn através das instruções disponíveis no site oficial do [Yarn](https://yarnpkg.com).
- **Expo CLI**: O Expo CLI é necessário para criar e gerenciar projetos Expo. Instale o Expo CLI globalmente usando yarn:

 ```
 yarn global add expo-cli
 ```

 ## Subir ambiente de Dev

 Para começar, certifique-se de que o Node.js 17, Yarn e Expo CLI estejam instalados em sua máquina, conforme mencionado nos pré-requisitos.

 Clone o repositório do projeto a partir do Azure DevOps:

 ```
 git clone https://LEXP@dev.azure.com/LEXP/IPVO/_git/ipvo-app
 ```

 ### Instalar dependências

 Navegue até o diretório do projeto e instale as dependências usando Yarn:

  ```
  cd ipvo-app
  yarn install
  ```

 ### Iniciar o Projeto Expo

 Após a instalação das dependências, você pode iniciar o projeto utilizando Expo CLI:

  ```
  yarn expo start
  ```
  Isso iniciará o servidor de desenvolvimento do Expo e abrirá o Metro Bundler no navegador. Você pode acessar o projeto Expo Client em seu dispositivo móvel ou um emulador.

 ### Compilação para Produção

 Quando estiver pronto para compilar seu aplicativo Expo para produção, você pode usar o seguinte comando:


 ```
 yarn eas build
 ```

 Lembrando que são permitidos apenas 20 builds mensais no free tier

 ### Publicação nas lojas

 Quando o build estiver finalizado, você pode usar o seguinte comando para publicar o app nas lojas:

 ```
 yarn eas submit
 ```


 # Aplicação Crud

 A aplicação é bem simples apenas utilizada para exibir dados aos usuários, não existe solicitações dos mesmo.

 Toda alimentação do APP é realizado pelo CRUD WEB

### Boletim

![](https://i.ibb.co/ZHFsBg8/Imagem-do-Whats-App-de-2024-06-18-s-14-57-49-44b5b564.jpg)

O boletim é inserido em 3 partes, primeiro deve ser criado a reflexão (Texto que aparece no final da home do APP)

![](https://i.ibb.co/XJTYkVs/imagem-2024-06-18-150552595.png)

Após a reflexão criada, deve ser criado o boletim, que é vinculado a uma reflexão
![](https://i.ibb.co/k3qDgz8/imagem-2024-06-18-150416852.png)

E por ultimo as imagens que serão exibidas no boletim. Um boletim pode ter varias imagens, elas serão exibidas no carrosel na pagina inicial.
É possível inserir um link para quando o usuário clicar na imagem ele ser redirecionado.

![](https://i.ibb.co/Z8QP9B5/imagem-2024-06-18-150738687.png)

### Agenda Semanal

![](https://i.ibb.co/cN7F4Xd/Imagem-do-Whats-App-de-2024-06-18-s-15-09-48-a0060a1c.jpg)

O Cadastro de Agenda é intuítivo, unico detalhe é referente aos dias, os dias sempre serão fixo, quinta, sabado e domingo. 

![](https://i.ibb.co/pPncnNv/imagem-2024-06-18-151248676.png)

### Ministérios

![](https://i.ibb.co/rG0QFn4/Imagem-do-Whats-App-de-2024-06-18-s-15-27-10-ef02a0aa.jpg)

O cadastro de ministério são realizado em duas etapas

#####Criação de ministério

![](https://i.ibb.co/tMB1npF/imagem-2024-06-18-152918503.png)

####Vincular membro ao ministério

Ao cadastrar ministério, selecione ele dentro da lista de ministérios na parte superior do modal vai ter uma opção "adicionar membro", será redirecionado para uma outra pagina onde será possível adicionar varios membros ao ministério.

![](https://i.ibb.co/X5L28Gn/imagem-2024-06-18-153112878.png)

Para realizar a criação das escalas

### Escalas

![](https://i.ibb.co/2s20dnq/Imagem-do-Whats-App-de-2024-06-18-s-15-32-18-b32bd7a9.jpg)

A escala é cadastrada de forma bem intuitiva, apenas vinculado ao grupo, só existem duas escalas que são exibidas no boletim (HOJE), Berçário e diaconia.

### Aniversariantes 

![](https://i.ibb.co/fNpNpjs/Imagem-do-Whats-App-de-2024-06-18-s-15-36-58-4a242e3e.jpg)

Os aniversariantes são exibidos de acordo com a data que está listada no banco na tabela de membros.

### Bíblia

Para a exibição da bíblia no app, é utilizado uma API pública, segue o link abaixo da documentação

`https://www.abibliadigital.com.br/en`

### Membros

O primeiro cadastro de membro foi realizado por banco, pois ficaria muito demorado para o usuário cadastrar 500 membros pelo Crud, foi solicitado uma planilha com o nome, telefone, data de nascimento e a função de cada membro para a secretária da igreja, e rodado um script no banco para criar de forma massiva.

Existe a possíbilidade de cadastrar um a um no CRUD na aba de membresia

![](https://i.ibb.co/zJ7RMNz/imagem-2024-06-18-154157962.png)


