# Documentação RUN StudentCare

### RUN

![](https://i.ibb.co/vJybNtM/imagem-2024-06-30-012402688.png)

Run é o nome dado ao botão que é criado no Dashboard, o objetivo do botão é pegar todos os alunos que estão filtrados. Ao clicar o usuário terá acesso a todos os alunos em uma tabela, onde podera criar tickets, plano de ações, comunicações e pesquisas de forma massiva.

![](https://i.ibb.co/X4mqpt3/imagem-2024-06-30-012542230.png)

#### Componente

O componente é localizado em `src/app/(app)/analytics/components/ReportPowerBi.js`

Nesse caso, o componente integra um relatório do Power BI na aplicação, permitindo a interação com o relatório e a exportação de dados específicos.

```javascript
	const { reportidPbi, sectionidPbi, id } = props
	const [token, setToken] = useState()
	const router = useRouter()
	const { data: user } = useSession()
	const { setStudent } = useStudentStore((state) => state)
	let report = null

	useEffect(() => {
		getTokenPowerBi().then((response) => {
			setToken(response)
		})
	}, [])
```

##### Inicialização e Autenticação
O componente ReportPowerBi recebe propriedades como reportidPbi, sectionidPbi, e id, que são usados para configurar o relatório do Power BI a ser embutido.
Utiliza o hook useState para armazenar o token de acesso obtido através da função getTokenPowerBi, que é chamada dentro de um useEffect para garantir que o token seja obtido na montagem do componente.
##### Configuração do Relatório do Power BI
O componente utiliza a biblioteca powerbi-client para embutir o relatório dentro da aplicação web.

```javascript
<Report
						embedType="report"
						tokenType="Aad"
						accessToken={token.access_token} 
						embedUrl={'https://app.powerbi.com/reportEmbed'} 
						embedId={reportidPbi}
						pageName={sectionidPbi}
						reportMode="view"
						extraSettings={extraSettings}
						permissions="All"
						style={{ height: '800px', borderWidth: 0 }}
						pageView="fitToWidth"
						onLoad={handleReportLoad}
						onRender={handleReportRender}
						onSelectData={handleDataSelected}
						onPageChange={handlePageChange}
						onTileClicked={handleTileClicked}
						onButtonClicked={handleButtonClicked}
					/>
```

##### Exportação de Dados:
A função handleButtonClicked é o ponto chave para a exportação de dados. Ela é acionada quando o botão no relatório é clicado.
```javascript
const handleButtonClicked = ({ id, title, type, bookmark }) => {
		if (report) {
			report.getPages().then(function (pages) {
				let activePage = pages.find((page) => page.isActive)
				activePage.getVisuals().then(function (visuals) {
					let visual = visuals.find((visual) => visual.title === 'DADOS_ALUNOS')
					visual.exportData(models.ExportDataType.ExportDataType).then((result) => {
						const resultData = parseData(result.data, [
							{ Header: 'Aluno', accessor: 'ALUNO' },
							{ Header: 'Nome', accessor: 'NOME_COMPL' },
							{ Header: 'E-Mail', accessor: 'E_MAIL' },
							{ Header: 'Unidade', accessor: 'NM_UNIDADE' },
							{ Header: 'Curso', accessor: 'CURSO' },
						])
						setStudent(
							resultData.data.map((student) => ({
								name: student?.NOME_COMPL,
								codeEnrollment: student.ALUNO,
								course: student.CURSO,
								unidade: student.NM_UNIDADE,
							}))
						)
						router.push('/analytics/step1')
					})
				})
			})
		}
	}
```

- Dentro dessa função, o código busca por páginas ativas no relatório e, em seguida, procura por um visual específico com o título 'DADOS_ALUNOS' (Nome da Tabela).
- Utilizando o método exportData do visual encontrado, os dados são exportados.
- Os dados exportados são então processados pela função parseData, que organiza os dados em um formato específico, mapeando campos como 'Aluno', 'Nome', 'E-Mail', 'Unidade', e 'Curso'.

##### Atualização do Estado e Navegação:

- Após a exportação e processamento dos dados, o estado da aplicação é atualizado com os dados dos alunos através da função setStudent, que é obtida do store useStudentStore (Store utilizada para salvar os dados e exibir na próxima tela) .

- A aplicação navega para a rota `analytics/step1` onde irá consumir os dados da useStudentStore para exibir os dados na tabela







