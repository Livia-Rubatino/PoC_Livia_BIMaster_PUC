# PoC_Livia_BIMaster_PUC
Proof of Concept to conclusion of MBA Business Intelligence Master PUC Rio

# Nome do projeto: Desenvolvimento de Chatbot RAG para análise inteligente de informações patentárias utilizando LLM e LangChain.

#### Aluno: [Lívia Rubatino de Faria](https://github.com/Livia-Rubatino)
#### Orientador: [Leonardo Alfredo Forero Mendoza](https://github.com/leofome8)

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

<!-- para os links a seguir, caso os arquivos estejam no mesmo repositório que este README, não há necessidade de incluir o link completo: basta incluir o nome do arquivo, com extensão, que o GitHub completa o link corretamente -->
- [Link para o código](https://github.com/Livia-Rubatino/PoC_Livia_BIMaster_PUC/blob/main/Trabalho%20Final_BI%20Master%202022.2%20(2).ipynb)


---

### Resumo
A recuperação e a análise de informações patentárias não são atividades triviais devido à grande quantidade de patentes farmacêuticas em determinado contexto, como o das vacinas de RNA mensageiro, ao excesso de terminologia jurídica e ao alto custo das bases de dados patenteários curados. Devido ao grande sucesso do Processamento de Linguagem Natural (NLP) na análise de conteúdos textuais, este trabalho apresenta um modelo de chatbot que utiliza Retrieval-Augmented Generation (RAG) para aprimorar os Large Language Models (LLM) através da integração com fontes de dados externas (planilha de Excel) além de possibilitar a recuperação de informações relevantes de patentes, baseadas em similaridade de contexto. O modelo proposto utiliza ainda Langchain para: a divisão do texto em pequenos blocos com significado semântico (chunks); a geração dos embeddings via OpenAI text-embedding-ada-002; a otimização do prompt via PromptTemplate e a utilização do RunnablePassthrough. O modelo utiliza também o banco vetorial Chroma DB para armazenamento dos vetores gerados e aplica o modelo GPT-3.5-turbo para obtenção de um chatbot especializado no contexto das vacinas de RNA mensageiro. De todos os seis modelos testados, o chatbot proposto pelo Modelo 1 apresentou respostas técnicas relevantes, mostrando-se uma ferramenta gratuita e de fácil utilização para automatizar a análise de patentes farmacêuticas e facilitar a tomada de decisão baseada em informações precisas e de qualidade.


### Abstract 
Patent information retrieval and analysis are not a easy activity, due to the high quantity of pharmaceutical patents in a specific context, such as messenger RNA vaccines, the high content of legal terminology, and the high cost of curated patent databases. Given the success of Natural Language Processing (NLP) in textual content analysis, this paper presents a chatbot model that uses Retrieval-Augmented Generation (RAG) to enhance Large Language Models (LLM) by integrating external data sources (e.g. Excel spreadsheets) and enabling the retrieval of relevant patent information based on content similarity. The proposed model uses Langchain to split text into meaningful chunks; generate embeddings via OpenAI's text-embedding-ada-002; optimize the prompt using PromptTemplate; and use RunnablePassthrough. The model also uses the Chroma DB vector bank to store the generated vectors and applies the GPT-3.5-turbo model to create a specialized chatbot in the messenger RNA vaccines content. Among the six models that were tested, the chatbot proposed by Model 1 provided relevant technical responses as a free, user-friendly tool for pharmaceutical patents analysis, which supports informed decision-making based on accurate and high-quality information.


### 1. Introdução

Uma patente é um direito legal que concede ao seu titular a exclusividade de produzir, usar e comercializar uma invenção pelo período de 20 anos. A patente é um contrato entre o Estado e o titular, onde o Estado protege a invenção em troca da sua divulgação pública [2].

Desta forma, as patentes são divulgadas em bancos de dados públicos e disponibilizadas para que população em geral tenha acesso às informações tecnológicas mais atuais, incentivando, assim, a inovação e o desenvolvimento tecnológico dos países. 

Entretanto, a recuperação e a análise das informações tecnológicas divulgadas nas patentes não são atividades triviais. Somente profissionais experientes na área de propriedade industrial conseguem recuperar patentes realmente relevantes para uma determinada área de conhecimento. E mesmo quando as patentes recuperadas aparentam ser relevantes, somente após a análise de cada uma delas é possível se ter certeza o quanto cada patente pode contribuir ou, não, em determinada área e contexto. E este cenário torna-se ainda mais trabalhoso quando se utiliza as bases de dados patentários públicas e gratuitas.

O acesso às bases de dados patentárias curadas é privado, tem alto custo e nem todas as instituições brasileiras conseguem custear sua assinatura anual, sendo necessário, então, utilizar as bases de dados de acesso público e gratuito disponíveis na internet, que nem sempre conseguem suprir a necessidade do usuário de ter acesso a informações detalhadas e específicas, em determinado contexto.

No contexto biotecnológico, as patentes apresentam informações tecnológicas atualizadas provenientes da fronteira do conhecimento obtido por importantes instituições e empresas que atuam em áreas específicas da saúde humana, como no desenvolvimento novas vacinas, novos anticorpos monoclonais, na terapia gênica, no diagnóstico de doenças entre outros. As patentes farmacêuticas na área de biotecnologia são, portanto, uma fonte abundante e altamente específica de conhecimento biotecnológico implícito, que tem sido amplamente inacessível devido ao excesso de terminologia jurídica e ao alto custo para o acesso às bases de dados patentários curadas [5]. 

Especificamente no campo tecnológico das vacinas, as vacinas de RNA mensageiro (RNAm) surgiram como uma ferramenta crucial para conter a disseminação do vírus SARS-COV-2 e reduzir as mortes pelo COVID-19 durante a pandemia [3]. 

A vacina de RNAm baseia-se na entrega das moléculas de ácido nucleico (RNAm) nas células do indivíduo, induzindo as células a fabricarem proteínas específicas que conseguem ensinar o sistema imunológico do indivíduo a se proteger. Dessa forma, caso o indivíduo tenha contato com a doença, o seu sistema imunológico já treinado, conseguirá responderá rapidamente, protegendo totalmente o indivíduo da doença ou amenizando drasticamente os sintomas da doença [3].

Embora o COVID-19 tenha ressaltado a eficácia das vacinas de RNAm, a tecnologia de RNAm não é nova e, por isso, conta com grande volume de documentos de patentes e de vasta literatura científica, disponibilizados nos mais diversos meios de comunicação, como a internet.

Devido ao grande sucesso do Processamento de Linguagem Natural na análise de conteúdos textuais patentários [1], os Large Language Models (LLM) possibilitam o desenvolvimento de sofisticados chatbots de pergunta e resposta (Question-Answering chatbots) utilizando a técnica do Retrieval-Augmented Generation (RAG) [4]. 

O RAG aprimora os LLMs, integrando-os a fontes de dados externas, estruturadas e não estruturadas, como bancos de dados, documentação e APIs, sendo uma excelente ferramenta para a recuperação de informações relevantes de patentes, baseadas em similaridade de contexto [7]. 

Ao se adicionar um gerador de modelos que consegue entender contextos e gerar texto, como o GPT-3.5-turbo, e as ferramentas de aperfeiçoamento do modelo de linguagem oferecidas pelo LangChain, é possível se obter um poderoso chatbot capaz de entender o contexto e responder perguntas do usuário, gerando respostas que facilitam a tomada de decisão baseada em informações relevantes [6].

Desta forma, buscando trazer uma solução gratuita e de fácil utilização pelo usuário, este trabalho apresenta um chatbot, especializado no contexto das vacinas de RNA mensageiro, capaz de responder as perguntas do usuário, de forma técnica e baseada em informações relevantes obtidas a partir de patentes farmacêuticas.


### 2. Modelagem

O banco de dados utilizado baseia-se em planilha de Excel contendo informações textuais de patentes farmacêuticas relacionadas à tecnologia de RNA mensageiro. 

Utilizou-se o notebook Jupyter Google Colaboratory para criação e execução do código desenvolvido na linguagem de programação Python.

Abaixo, a modelagem do código:

![image](https://github.com/user-attachments/assets/102f1323-4b62-4406-b626-31ddfa63a649)

Foram testadas diversas alterações nos parâmetros do modelo proposto afim de se identificar quais seriam os valores ideais para o conjunto de dados analisado. Também foi verificado o impacto da otimização no prompt quando comparado ao prompt inicial.

O prompt inicial e o prompt otimizado utilizados neste trabalho são:  

![image](https://github.com/user-attachments/assets/7f76407b-426f-4ff3-ada6-5f238edc4a3c)


### 3. Resultados

Abaixo, estão registrados seis dos diversos modelos testados neste trabalho. Os seis modelos foram os que apresentaram os melhores resultados para a proposta deste trabalho.  

Na Tabela 1, estão compiladas as alterações paramétricas realizadas e estão sinalizados os modelos que tiveram seus prompts otimizados. Ao final, seguem-se ainda as métricas obtidas em cada um dos modelos testados.

Tabela 1 – Detalhamento dos modelos testados e as métricas obtidas.

![image](https://github.com/user-attachments/assets/042495a6-5880-44a8-b97a-2e22e61a77bc)

![image](https://github.com/user-attachments/assets/351ed74c-289c-4880-9df0-6722c6a3800e)

Como pode ser observado acima, todos os modelos se mostraram satisfatórios para o conjunto de dados analisado, sendo o Modelo 3 o que apresentou as métricas mais altas dentre todos os outros modelos. 

Entretanto, ao se verificar as respostas geradas pelo Modelo 3 pode-se observar erro na resposta à pergunta: “Qual vacina contém N1-metilpseudourina?”. 

Ao se comparar a resposta obtida pelo Modelo 3 com a resposta obtida pelo Modelo 1, por exemplo, pode-se perceber que o Modelo 1 respondeu corretamente à mesma pergunta. O Modelo 1 também teve seu prompt ajustado, mas obteve as métricas Bleu, Rouge e BERTScore pouco inferiores às métricas obtidas no Modelo 3. Possivelmente, a razão para a diferença de métricas e da resposta encontrada entre os dois modelos deve-se ao valor de Threshold na função def retrieve_with_threshold_fn que, no Modelo 1 era 0.7 e no Modelo 3 era 0.9. Valores altos de Threshold, embora aumentem a precisão do modelo, também aumentam o risco do modelo perder informações relevantes e não encontrar o contexto correto em algumas perguntas. Esta talvez seja a razão para que o Modelo 3, embora mais preciso, não tenha conseguido responder corretamente a pergunta sobre a N1-metilpseudourina, enquanto o Modelo 1 conseguiu responder corretamente.

Comparando ainda o impacto de diferentes valores de Threshold nos modelos testados, pode-se observar que o Modelo Inicial (com Threshold = 0.7) e o Modelo 2 (com Threshold = 0.9) geraram a mesma resposta correta, sendo que o Modelo 2 gerou uma resposta com mais detalhes e mais qualidade quando comparado com o Modelo Inicial. O excesso de detalhes nas respostas obtidas pelos dois modelos deve-se ao fato de seus prompts não terem sido ajustados para respostas mais objetivas e limitadas em até 20 palavras.

Avaliando agora o impacto de alterações no K da função def retrieve_with_threshold_fn que, no Modelo 3 era 10 e no Modelo 5 era 5, pode-se observar que o Modelo 3 obteve métricas mais altas quando comparado com o Modelo 5, possivelmente devido ao fato do valor de K mais alto representar um conjunto maior de documentos similares resgatados. No caso, o K=10 resgatou os 10 documentos mais similares a pergunta enquanto o K=5 resgatou 5 documentos. Provavelmente, documentos importantes não foram resgatados com o K mais baixo do Modelo 5, resultando em métricas mais baixas.

Verificando, por fim, o impacto da otimização no prompt inicial, pode-se verificar que os modelos que tiveram seus prompts otimizados (Modelos 1 e 3) obtiveram as métricas Coverage e BERTScore mais altas quando comparados com os modelos sem otimização do prompt (Modelos Inicial e 2). Este resultado se deve à obtenção de respostas contendo termos e detalhes mais relevantes (Coverage alto) e com alta similaridade semântica e compreensão do contexto da pergunta, mesmo que as palavras exatas usadas sejam diferentes entre si (BERTScore). Logo, a otimização do prompt melhorou a qualidade e a precisão das respostas geradas.

Por fim, após a verificação das respostas e das métricas obtidas por cada um dos modelos testados, entende-se que o Modelo 1 é o que mais se adequou ao conjunto de dados de entrada, mesmo quando comparado ao Modelo 3 que, embora tenha obtido métricas mais altas, gerou resposta errada e fora do contexto esperado.



### 4. Conclusões

Por fim, conclui-se que o chatbot proposto pelo Modelo 1, especializado no contexto das vacinas de RNA mensageiro apresentou respostas técnicas adequadas, mostrando-se uma ferramenta gratuita e de fácil utilização para automatizar a análise de patentes farmacêuticas e facilitar a tomada de decisão baseada em informações precisas, relevantes e de qualidade.



### Referências bibliográficas
1.	Bai Z., Zhang R., Chen L. et al. (2024) PatentGPT: A Large Language Model for Intellectual Property. arXiv, https://doi.org/10.48550/arXiv.2404.18255 
2.	BRASIL. LEI Nº 9.279, 14 de maio de 1996 (1996) Regula direitos e obrigações relativos à propriedade industrial. Diário Oficial da União, Brasília, DF, [1996]. 
3.	Fang, E., Liu, X., Li, M. et al. (2022) Advances in COVID-19 mRNA vaccine development. Sig Transduct Target Ther 7, 94. https://doi.org/10.1038/s41392-022-00950-y
4.	INTERNET. (2025) Build a Retrieval Augmented Generation (RAG) App. https://python.langchain.com/v0.2/docs/tutorials/rag/
5.	Kosonocky, C. W., Wilke, C. O., Marcotte E. M. et al. (2024) Mining patents with large language models elucidate the chemical function landscape. Digital Discovery, 2024, 3, 1150. DOI:10.1039/D4DD00011K. https://pubs.rsc.org/en/content/articlehtml/2024/dd/d4dd00011k
6.	Jiang, L., Zhang, C., Scherz, P. A. (2024) Can Large Language Models Generate High-quality Patent Claims?  https://doi.org/10.48550/arXiv.2406.19465 
7.	 Lee, K.-Y.; Bai, J. (2025) PAI-NET: Retrieval-Augmented Generation Patent Network Using Prior Art Information. Systems 2025, 13, 259. https://doi.org/10.3390/systems13040259 

---

Matrícula: 222.100.339

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*

