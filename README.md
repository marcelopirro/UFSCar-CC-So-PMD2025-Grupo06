# UFSCar-CC-So-PMD2025-Grupo06

**Autores:**
* Marcelo Pirro – 800510
* Guilherme Campos Bortoletto – 801477

**Projeto:**
**Detecção de Fake News com Processamento em Larga Escala Utilizando MongoDB Atlas e Python**

**Ano:** 2025

---

## Resumo

O projeto tem como foco o desenvolvimento de um pipeline completo para a detecção automática de fake news, integrando tecnologias de armazenamento e processamento de dados. Utilizando o **MongoDB Atlas** como banco de dados NoSQL e a biblioteca **Pymongo** em um ambiente **Databricks** para a análise e processamento, o sistema é capaz de armazenar, transformar e analisar milhares de notícias jornalísticas.

A solução trabalha com um conjunto de dados obtido do Kaggle, contendo aproximadamente 44 mil notícias reais e falsas. Após o pré-processamento, os registros são inseridos no MongoDB como documentos JSON. A partir daí, são realizadas operações de limpeza, tokenização, vetorização e transformação com TF-IDF. Com os dados preparados, são treinados modelos de machine learning para classificar os textos como verdadeiros ou falsos.

Além da classificação automática, o projeto realiza uma análise exploratória linguística, identificando as palavras mais frequentes em conteúdos falsos, revelando padrões de escrita e vocabulário característicos. O pipeline completo demonstra a viabilidade técnica da integração de um banco de dados NoSQL robusto com bibliotecas de ciência de dados em Python para tarefas de Processamento de Linguagem Natural (NLP).

## Objetivo

Nosso objetivo é desenvolver um pipeline completo de detecção de notícias falsas (fake news) por meio da análise de padrões linguísticos em textos jornalísticos. Buscamos demonstrar a aplicação de tecnologias NoSQL e frameworks de processamento para:

* Armazenar de forma flexível e escalável um volume significativo de notícias reais e falsas;
* Processar os dados textuais em larga escala utilizando técnicas de NLP (*Natural Language Processing*);
* Treinar e avaliar um modelo de *machine learning* para classificar automaticamente notícias como verdadeiras ou falsas;
* Avaliar a viabilidade da arquitetura proposta no contexto de aplicações que demandam robustez e integração direta com bancos de dados;
* Identificar métricas linguísticas complementares, como as palavras mais frequentes em textos de *fake news*, fornecendo análises exploratórias sobre padrões de vocabulário e estilo.

## Tecnologias Escolhidas

### MongoDB Atlas

A escolha de um banco de dados NoSQL como o **MongoDB** é a mais adequada para este projeto porque lidamos com dados textuais e semi-estruturados (notícias) que variam em formato e conteúdo. A natureza dos dados tratados — notícias jornalísticas — envolve estruturas com campos como **título**, **categoria**, **conteúdo textual** e **data**. O **MongoDB Atlas** resolve esse problema ao utilizar um modelo de documentos **JSON**, que se adapta perfeitamente a esse tipo de dado. Além disso, trata-se de uma solução **gerenciada na nuvem**, com fácil escalabilidade horizontal.

Diferente dos bancos relacionais, que exigem esquemas rígidos, o MongoDB oferece uma **estrutura flexível**, possui **indexação** e **busca textual nativa**, e se integra diretamente com diversas linguagens e frameworks, permitindo o **processamento eficiente** dos dados.

### Mudança de Estratégia: De PySpark para Pymongo no Databricks

Inicialmente, o projeto previa o uso do **Apache Spark (PySpark)** para o processamento distribuído dos dados. A arquitetura original visava conectar o Spark ao MongoDB Atlas para realizar as transformações em um ambiente de larga escala, aproveitando a capacidade do Spark de distribuir o processamento em clusters.

Contudo, durante o desenvolvimento, enfrentamos uma barreira técnica significativa. A plataforma **Databricks Community Edition**, que era nossa escolha para o processamento distribuído com Spark, **removeu o acesso a clusters computacionais em seu plano livre**. Sem um cluster, a execução do PySpark e do conector `spark-mongodb` tornou-se inviável.

Diante desse obstáculo, adaptamos a estratégia. **Mantivemos o ambiente Databricks para a execução dos notebooks Python**, mas substituímos o processamento com PySpark pela biblioteca **Pymongo** para a interação direta com o MongoDB Atlas. Essa abordagem, embora não utilize processamento distribuído, ainda cumpre o requisito de integrar um modelo de dados NoSQL com uma plataforma de processamento de dados, permitindo a execução de todo o pipeline de análise e machine learning.

## Fonte de Dados

* **Dataset**: Fake and Real News Dataset – Kaggle
* **Link**: <https://www.kaggle.com/datasets/clmentbisaillon/fake-and-real-news-dataset>
* **Conteúdo**: Cerca de 44.000 artigos de notícias, divididos nos arquivos `Fake.csv` e `True.csv`.
* **Atributos**: título, conteúdo, categoria, data de publicação.

## Fluxo do Projeto

1.  **Obtenção dos Dados** – Download dos arquivos CSV do Kaggle.
2.  **Pré-processamento Inicial** – Limpeza e padronização em Python.
3.  **Inserção no MongoDB Atlas** – Cada notícia é armazenada como um documento JSON em sua respectiva *collection* (`Fake` ou `Real`).
4.  **Leitura com Pymongo no Databricks** – Conexão com o MongoDB Atlas e leitura dos documentos das *collections*.
5.  **Processamento e Análise** – Utilização da biblioteca `scikit-learn` para tokenização, vetorização com TF-IDF e preparação dos dados para o modelo.
6.  **Treinamento de Modelos** – Aplicação de algoritmos de Machine Learning, como Naive Bayes, para classificação.
7.  **Análise Textual Exploratória** – Identificação de padrões linguísticos e vocabulário frequente em fake news.

## Desenvolvimento e Implementação

A fase final do projeto concentrou-se na implementação do pipeline de leitura, processamento e classificação usando Python. O código foi executado em um **ambiente Databricks**, utilizando a biblioteca `pymongo` para interagir com o MongoDB Atlas.

Abaixo, um resumo das etapas implementadas no script:

#### 1. Conexão com o MongoDB Atlas

Primeiro, estabelecemos a conexão com o cluster do MongoDB Atlas usando a URI de conexão fornecida:

```python
from pymongo import MongoClient

conn_uri = "mongodb+srv://Databricks:1234@pmd2025marcelopirro.nhnozvy.mongodb.net/?retryWrites=true&w=majority"
client = MongoClient(conn_uri)
db = client["Fake_and_Real"]
```

#### 2. Carregamento dos Dados

Os dados, previamente inseridos nas *collections* `Fake` e `Real`, foram carregados em DataFrames do Pandas para facilitar a manipulação:

```python
collection_fake = db["Fake"]
fake_data = list(collection_fake.find())
df_fake = pd.DataFrame(fake_data)

collection_real = db["Real"]
real_data = list(collection_real.find())
df_real = pd.DataFrame(real_data)
```

#### 3. Pré-processamento e Unificação

Criamos uma coluna `label` para diferenciar as notícias (`0` para falsas, `1` para verdadeiras) e unificamos os dois conjuntos de dados. Em seguida, aplicamos uma função de limpeza nos textos para remover caracteres especiais, converter para minúsculas e retirar *stopwords*:

```python
df_fake["label"] = 0
df_real["label"] = 1
df_total = pd.concat([df_fake, df_real], ignore_index=True)

def preprocess_text(text):
    text = text.lower()
    text = re.sub(r'[^a-zA-Z\s]', '', text)
    tokens = text.split()
    tokens = [word for word in tokens if word not in stopwords.words('english')]
    return ' '.join(tokens)

df_total["clean_text"] = df_total["text"].apply(preprocess_text)
```

#### 4. Vetorização e Treinamento do Modelo

Utilizamos a técnica **TF-IDF (Term Frequency-Inverse Document Frequency)** para converter o texto em vetores numéricos, limitando o vocabulário às 5000 palavras mais relevantes. Por fim, dividimos os dados em conjuntos de treino e teste e treinamos um modelo de classificação **Multinomial Naive Bayes**:

```python
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import MultinomialNB

vectorizer = TfidfVectorizer(max_features=5000)
X_vect = vectorizer.fit_transform(df_total["clean_text"])
y = df_total["label"]

X_train, X_test, y_train, y_test = train_test_split(X_vect, y, test_size=0.2, random_state=42)

model = MultinomialNB()
model.fit(X_train, y_train)
```

#### 5. Avaliação dos Resultados

O desempenho do modelo foi avaliado com um relatório de classificação e uma matriz de confusão, que nos permitem medir a acurácia, precisão e outras métricas importantes para validar a eficácia da classificação.

## Resultados Esperados

Será desenvolvido um **pipeline funcional de ponta a ponta** para a detecção automática de *fake news*, abrangendo todas as etapas, desde a ingestão dos dados até a classificação final dos textos.

O modelo de *machine learning* será avaliado com métricas relevantes, como **acurácia**, **precisão** e **recall**, proporcionando uma análise quantitativa consistente sobre o desempenho da solução.

Além disso, o projeto visa demonstrar a **viabilidade técnica e prática** da integração entre **MongoDB Atlas** e **Python (com Pymongo e Scikit-learn)** em um ambiente **Databricks**, utilizando essas tecnologias em um cenário real de **classificação de textos em larga escala**.
