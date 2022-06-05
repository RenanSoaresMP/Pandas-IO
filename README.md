# Pandas-IO
Manipulação e tratamento de dados em vários formatos
# Trabalhando com dados em diferentes formatos

**Linguagem utilizada:** Python

**Bibliotecas utilizadas:** Numpy, Pandas, Seaborn

**Formatos de dados trabalhados:** JSON, HTML, CSV, Excel e banco SQL.

**Principais atividades feitas:**
- Manipulação de dados em diferentes formatos;
- Criação de diversas manipulações em listas e dataframes; 
- Criação de gráfico para análises;
- Limpeza de dados;
- Criação de Bancos SQL;
- Buscas através de uma querySQL.


# Introdução
Este exercício tem como base o curso ‘Pandas: Formatos diferentes de entrada e saída (IO)’, da plataforma Alura – Cursos Online de Tecnologia. O seu objetivo é trabalhar com dados nos formatos JSON, HTML, arquivos CSV, arquivos Excel e banco SQL.

# O Projeto

Nos tópicos seguintes faremos uma explicação de cada etapa do projeto, com os principais métodos e funções utilizadas no mesmo.

## Importando arquivos no formato JSON


Estamos criando um programa para uma escola fictícia de programação e, inicialmente, para criar uma base de dados com vários alunos, buscamos dados com nomes de pessoas numa API do site do IBGE, usando a função que lê arquivos JSON (pd.read_json), importamos os dados para as variáveis ‘mulheres’ e ‘homens’.

Criamos uma lista com todos os nomes, de ambos os sexos, em seguida concatenamos estes dados e usando a função to_frame(), com isso passamos essa lista para o formato dataframe, chamando de ‘nomes’. Na mesma linha de código colocamos o comando [‘nome’] para selecionarmos apenas a coluna que temos interesse dos dados, criando o dataframe ‘nomes’.

##  **Incluindo ID dos alunos**

Para criar o ID dos alunos, utilizamos a biblioteca Numpy, com sua função random.seed() e parâmetro 123, para que se gere sempre a mesma sequência de números aleatórios.

Na linha de código abaixo, criamos uma nova coluna no dataframe ‘nomes’, chamada id_aluno, com a função random.permutation, geramos Id’s aleatórios entre 1 a 1000 (que é o valor que está contido em ‘total_alunos’.


	nomes["id_aluno"] = np.random.permutation(total_alunos) + 1
	
A frente, no código, também foi criada mais uma coluna para o dataframe ‘nomes’, chamada de ‘domínio’ e uma coluna chamada ‘email’, que foi resultado da concatenação das colunas ‘nome’ e ‘domínio’, com a seguinte linha de código:


	nomes['email'] = nomes.nome.str.cat(nomes.dominio).str.lower()


A função random.choice(), também presente no programa, escolhe aleatoriamente dentro dos dados presentes na variável tipo lista ‘dominios’, qual dado irá preecher a coluna ‘dominio’, até chegar a última linha, este valor contido em ‘total_alunos’.

## **Criando a tabela 'Cursos' e lendo HTML**


Vamos importar uma tabela HTML que possui uma lista contendo 20 cursos, nosso objetivo é criar o dataframe ‘cursos’, com estas informações.
Inicialmente, precisamos instalar duas bibliotecas:


						!pip3 install html5lib

						!pip3 install lxml


E, importar uma:


						import html5lib

Com a url das informações em HTML, basta utilizarmos a função pd.read_html(), que estaremos carregando a tabela da página. Inicialmente, esta tabela, que chamamos de ‘cursos’, é do tipo lista.  Utilizando a instrução curso[0], passamos esta variável para o formato dataframe:


						cursos = cursos[0]
##  **Alterando o index do dataframe 'cursos'**



Utilizamos a função rename(), para renomearmos a coluna ‘Nome do curso’ para ‘nome_do_curso’, também geramos uma nova coluna, chamada ‘id’. Com a função set.index(), transformamos esta coluna ‘id’ na coluna index do dataframe:

			         cursos = cursos.set_index('id')


## **Matriculando os alunos nos cursos**


Nesta etapa vamos criar a coluna ‘matriculas’ no dataframe ‘nomes’:

	nomes['matriculas'] = np.ceil(np.random.exponential(size=total_alunos) * 1.5).astype(int)
	

Para preencher esta coluna utilizamos a função np.random.exponencial(), que é uma função exponencial presente na biblioteca Numpy, juntamente com a função np.ceil(), que arredonda esses valores gerados para cima, evitando que algum desses valores seja 0. Multiplicamos por 1.5 para que tenhamos valores um pouco maiores para as variáveis dentro da coluna ‘matriculas’.


						nomes.matriculas.describe()


No código acima, utilizamos a função describe(), que nos oferece algumas informações estatísticas a respeito da variável desejada, no nosso caso foi da coluna ‘matricula’ do dataframe ‘nomes’. Informações como a quantidade de matrículas, média, desvio padrão, dentre outras.

## **Gerando um histograma**



Utilizando a biblioteza Seaborn, geramos um histograma, que nos demonstra graficamente a relação de alunos matriculados por matéria, podemos verificar um número alto de alunos inscritos em uma ou duas matérias e um número baixo de alunos inscritos em 3 ou mais. O histograma foi gerado utilizando a função sns.displot():


					sns.distplot(nomes.matriculas)


Finalizando, com a utilização da função value_counts() saberemos a quantidade de alunos matriculados por quantidade de matérias, dados que foram representados anteriormente no histograma.
## Selecionando cursos


Nesta etapa do projeto o objetivo é criar um dataframe que nos forneça a quantidade de alunos matriculados por cada curso.

Na variável ‘todas_matriculas’, do tipo lista, serão armazenados os id’s e cursos relacionados as respectivas id’s (cursos estes que serão selecionados randomicamente, utilizando a função np.random.choice()).

Criamos um iterador _for_ que buscará cada index, linha por linha, do dataframe ‘nomes’, para buscar linha por linha, utilizamos a função interrows(). As variáveis ‘id’ e ‘matriculas’ foram criadas para armazenar as informações, de mesmo nome, que estão contidas no dataframe em questão.

Com a variável ‘mat’, armazenaremos o valor instâtaneo do ‘id’ e de um curso, que foi escolhido randomicamente, que na linha seguinte será salvo na lista ‘todas_matriculas.

Com o loop _for_ finalizado, criaremos o dataframe matriculas, com as informações armazenadas na lista ‘todas_matriculas’.

Com a linha de código abaixo, usamos a função groupby() para agrupar as variáveis iguais da ‘id_curso’ e a função count() para fazer o somatório deste agrupamentos, que nos darão a quantidade de alunos por curso.

	matriculas.groupby('id_curso').count().join(cursos['nome_do_curso']).rename(columns={'id_aluno':'quantidade_de_alunos'})

Finalmente, usando a função join(), uniremos a coluna ‘nome_do_curso’, do dataframe ‘cursos’, ao dataframe ‘matriculas’ e assim chegamos no nosso objetivo, chamamos este novo dataframe de ‘matriculas_por_curso’.



## **Saídas em diferentes formatos**


Exportamos o dataframe ‘matriculas_por_curso’ para os formatos CSV, JSON e HTML, com os seguintes comandos:

- Para CSV:

		matriculas_por_curso.to_csv('matriculas_por_curso.csv', index=False)
		
 O arquivo CSV criado fica disponível na aba ‘File’ do Google Colab e para ler o mesmo, utilizamos a função pd.read_csv().
 - Para JSON:


		matriculas_json = matriculas_por_curso.to_json()
 - Para HTML:


		matriculas_html = matriculas_por_curso.to_html()



## Criando o banco SQL




Nosso primeiro objetivo desta etapa do projeto é salvar o dataframe ‘matriculas_por_curso’ em um banco de dados local SQL. Para isso, instalamos a biblioteca sqlalchemy, e  a partir dela importamos as libs create_engine, MetaData, Table e inspect.

Em seguida, criamos a função que irá gerar o motor do banco de dados, a variável ‘engine’ receberá a chamada de create_engine, como parâmetro passaremos :

							'sqlite:///:memory:'

Sendo assim, nosso banco de dados será salvo na memória local.

Nesta etapa já temos condições de passar o dataframe para SQL, que chamaremos de ‘matriculas’, usamos a função to_sql, completamos com o nome que queremos dar ao banco de dados e com a engine.

Para possibilitar a visualização de tabelas, colunas e esquemáticos do banco de dados, precisamos importar o método inspect(), que executa uma inspeção desses dados pela engine criada, como é demonstrado nas duas linhas abaixo, retiradas do código principal. Com a função get_table_names(), exibimos as tabelas com o inspector.

						inspector = inspect(engine)
					print(inspector.get_table_names())

Com estas etapas o banco de dados SQL ‘matriculas’ foi criado.


## **Fazendo buscas no banco SQL**



Realizamos essa busca através de uma querySQL, criamos a variável ‘query’ e utilizamos o comando select*from, para selecionarmos os cursos que tem menos de 100 alunos matriculados, vide código abaixo:

	query = 'select * from matriculas where quantidade_de_alunos < 100'

Com a função pd.read_sql(), imprimimos o conteúdo da variável ‘query’ na tela.

Utilizamos também o comando pd.read_sql_tables(), com ele podemos filtrar quais colunas da tabela queremos visualizar.

Criamos uma nova consulta, que com o seu dataframe resultante, atribuiremos o nome de ‘muitas_matriculas’. Nesta consulta, buscamos os cursos com mais de 150 matrículas, utilizando outra função de buscas presente na biblioteca Pandas, chamada query():

	muitas_matriculas = muitas_matriculas.query('quantidade_de_alunos > 150')

## Escrevendo no banco SQL


Com o método to_sql(), criamos mais um campo no nosso banco de dados para ‘novas_matriculas’, é necessário passar o parâmetro con = engine (o motor do nosso banco). E assim, ao imprimirmos os nomes das tabelas presentes em nosso banco, teremos:

					['matriculas', 'muitas_matriculas']

##  **Nomes dos alunos e alunas da próxima turma**


Nosso objetivo nessa etapa é criar um dataframe com o nome dos alunos juntamente com o respectivo curso que ele está matriculado e , posteriormente, enviar este dataframe para um arquivo excel.

Vamos querer saber as informações referentes ao curso de número 16, ‘Estatística Básica’. Criaremos uma variável ‘id_curso’, atribuiremos o valor 16 a ela. Utilizaremos uma query, que buscará no dataframe ‘id_curso’, todos os alunos com a matrícula no curso 16, e armazenará na variável ‘proxima_turma’ , como é mostrado na linha de código abaixo:

							id_curso = 16

		proxima_turma = matriculas.query(f"id_curso == {id_curso}")

Para termos a visualização dos nomes destes alunos, vamos concatenar o dataframe ‘proxima_turma’, setando ele como index, com a coluna ‘nome’ do dataframe ‘nomes’, também setando seu index como ‘id_aluno’:

	proxima_turma.set_index('id_aluno').join(nomes.set_index('id_aluno'))['nome']

Utilizamos a função to_frame() para passarmos ‘proxima_turma’ para o formato dataframe, onde temos uma coluna com o ‘id_aluno’ e outra com o seu nome.

Por último, falta incluir o nome do curso no dataframe. Criaremos uma nova variável, ‘curso_escolhido’, que com o a utilização do método loc, armazenará na nova variável o  nome do curso de ‘id_curso’ = 16, que está presente no dataframe ‘cursos’.

E, finalente, vamos renomear a segunda coluna do dataframe ‘proxima_turma’, de modo que ela atualizará o nome do curso sempre que alterarmos o cursos escolhido:

	proxima_turma.rename(columns = {'nome':'Alunos do curso de {}'.format(curso_escolhido)})

## Escrevendo e lendo em excel




Basta utilizarmos os comandos to_excel() e pd.read_excel(), para escrever e ler respectivamente os arquivos excel.

Quando exportamos, o arquivo vai para a aba ‘files” do Google Colab, para importarmos um arquivo excel, ele tem que também estar na aba ‘files’.
