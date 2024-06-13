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


 # Aplicação

 ## Gerações

 As gerações são categorizadas por tipo:
    P - Planejamento
    A - Atividades
    M - Monitoria
    C - Comunicações

 ### Criar gerações novas

 Para realizar criação de novas gerações, basta inserir uma nova geração no Banco de Dados na tabela ``geracao_tipo``,

###### Observação

 Sempre que for criado uma geração nova deve ser inserido a categoria da geração na tabela ``geracao_tipo_categoria``, e em seguida os parametros na ``geracao_tipo_parametro``


### Listagem de gerações no Front
    A listagem é feita de forma bem simples através do endpoint ``/geracao-tipo``

```javascript
  export const buscaGeracaoTipo = () =>
  useQuery<IGeracaoTipoRecordGetResponse[]>(
    'generations',
    async (): Promise<IGeracaoTipoRecordGetResponse[]> => {
      const { data } = await Axios.get(`${base}/geracao-tipo`)
      return data
    },
    { refetchOnWindowFocus: false }
  )
```

Após a consulta é realizado um filter para exibir de acordo com o tipo da geração

```javascript
		{data?.filter((generation) => generation.tipo === 'P')
                  .map((generation) => (
                    <Cartao
                      title={generation.nome}
                      text={generation.descricao}
                      icon={generation.icone}
                      link={`/geracao/${generation.id}`}
                    />
    ))}
```


### Realizando a geração

Quando o usuário selecionar o tipo de geração que ele deseja gerar, será redirecionado para a rota `/geracao/[id]`

Nessa pagina é utilizado um hook para gerenciar os estados e funções, todos os parametros que o usuário selecionar é armazenado nesse hook, e a chamada da função para realizar a geração também



```javascript
  const {
    geracaoTipoCategorias,
    onGerar,
    onChange,
    isGerando,
    isModalSucessoOpen,
    onModalSucessoClose,
    isModalFalhaOpen,
    onModalFalhaClose,
    responseIntegracao,
    selectedBncc,
    setSelectedBncc,
    selectedSegmento,
    setSelectedSegmento,
    assunto,
    setAssunto,
    response,
    etapa,
    setEtapa,
    etapaAtual,
    setEtapaAtual,
    afeganistao,
    onGerarNovamente,
    selectedModalidade,
    setSelectedModalidade,
  } = useGeracaoState(geracaoTipo)

```

##### Função `onGerar` 

```javascript
const onGerar = async () => {
    setIsGerando(true)
    setResponseIntegracao(null)
    setResponse('')
    setEtapaAtual(etapaAtual + 1)
    updateSession()
    setContador(0)

    const intervalId = setInterval(() => {
      setContador((prevContador) => prevContador + 1)
    }, 1000)

    const body = {
      assunto: assunto,
      bnccId: selectedBncc,
      nivelEnsino: selectedSegmento,
      modalidade: selectedModalidade,
      parametros: dadosTexto
        .map((itemCategoria) =>
          itemCategoria.parametros?.map?.((itemParametro: any) => {
            let parametro = itemParametro.parametro
            if (parametro.tipo === 'CHECK') {
              parametro.valor = parametro.nome
            } else {
              parametro.valor = itemParametro.valor
            }

            return parametro
          })
        )
        .reduce((a, b) => a.concat(b), []),
      geracaoTipo: geracaoTipo,
    }

    gerar(body).then(async (response) => {
      if (response.status >= 200 && response.status < 300) {
        setResponseIntegracao(response.data)

        const generate = await fetch('/api/generate', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ messages: response.data.texto }),
        })
        setAfeganistao(afeganistao + 1)
        const data = generate.body
        if (!data) {
          return
        }

        const reader = data.getReader()
        const decoder = new TextDecoder()
        let done = false

        while (!done) {
          const { value, done: doneReading } = await reader.read()
          const chunkValue = decoder.decode(value)
          done = doneReading
          setResponse((prev) => {
            const resultado = prev + chunkValue || ''
            if (done) {
              clearInterval(intervalId)
              atualizaResultado(response.data.idGeracao, { resultado: resultado, gasto: contadorRef.current })
              setIsGerando(false)
            }
            return resultado
          })
        }
      } else {
        setIsModalFalhaOpen(true)
      }
    })
  }
```


Esta função `onGerar`, é responsável por gerar uma atividade com base nas seleções do professor. Aqui está uma explicação passo a passo do que a função faz:

1. Primeiro, a função define várias variáveis de estado para seus valores iniciais. Isso inclui definir `isGerando` como `true`, limpar as respostas anteriores e incrementar a etapa atual.

2. Em seguida, a função inicia um contador que aumenta a cada segundo. Isso pode ser usado para rastrear quanto tempo leva para gerar a atividade. (KPI Tempo Gasto e Economizado)

3. A função então cria um objeto `body` que contém todas as informações necessárias para gerar a atividade. Isso inclui o assunto, o ID BNCC selecionado, o nível de ensino, a modalidade, os parâmetros e o tipo de geração.

4. A função `gerar` é então chamada com o objeto `body` como argumento. Esta função faz uma chamada de API para um serviço que gera a atividade.

	```javascript
	export const gerar = (body: any) => {
	  return Axios.post<IGeracaoRecordChatGPT>(`${base}`, body)
	}
	```

5. Se a resposta da chamada de API for bem-sucedida, a função prossegue para processar a resposta.

6. A resposta é então enviada para outra API (`/api/generate`) que usa o modelo GPT para gerar o texto da atividade.

	```javascript
	export async function POST(req: Request): Promise<Response> {
	  const { messages } = await req.json()
	  const payload: OpenAIStreamPayload = {
		model: 'gpt-3.5-turbo',
		messages: JSON.parse(messages),
		temperature: 0.2,
		max_tokens: 3900,
		stream: true,
	  }
	  const stream = await OpenAIStream(payload)
	  return new Response(stream)
	}
	```

7. A função então lê a resposta da API `/api/generate` em chunks. Para cada chunk, ela decodifica o texto e o adiciona à resposta.

8. Quando todos os chunks foram lidos e a atividade foi totalmente gerada, a função limpa o intervalo do contador, atualiza o resultado com a atividade gerada e o tempo gasto, e define `isGerando` como `false`.

9. Se a chamada de API inicial falha, a função abre um modal de falha.


