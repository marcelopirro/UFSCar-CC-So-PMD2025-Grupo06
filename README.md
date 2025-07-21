# Projeto de Disciplina – **Detecção de Fake News com Processamento em Larga Escala Utilizando MongoDB Atlas e Python**


**Universidade Federal de São Carlos**  
**Curso:** Bacharelado em Ciência da Computação de Sorocaba  
**Disciplina:** Processamento Massivo de Dados  
**Professora:** Profa. Dra. Sahudy Montenegro González  


**Grupo 06**  

**Integrantes:**
* Marcelo Pirro – 800510
* Guilherme Campos Bortoletto – 801477

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

## Fluxograma do Projeto

1.  **Obtenção dos Dados** – Download dos arquivos CSV do Kaggle.
2.  **Pré-processamento Inicial** – Limpeza e padronização em Python.
3.  **Inserção no MongoDB Atlas** – Cada notícia é armazenada como um documento JSON em sua respectiva *collection* (`Fake` ou `Real`).
4.  **Leitura com Pymongo no Databricks** – Conexão com o MongoDB Atlas e leitura dos documentos das *collections*.
5.  **Processamento e Análise** – Utilização da biblioteca `scikit-learn` para tokenização, vetorização com TF-IDF e preparação dos dados para o modelo.
6.  **Treinamento de Modelos** – Aplicação de algoritmos de Machine Learning, como Naive Bayes, para classificação.
7.  **Análise Textual Exploratória** – Identificação de padrões linguísticos e vocabulário frequente em fake news.

## 📊 Fluxo do Pipeline de Análise de Fake News


<img width="1920" height="1080" alt="fluxograma" src="https://github.com/user-attachments/assets/6611d4f0-298b-4efc-bca8-a38f65511e47" />

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
## Avaliação dos Resultados

O desempenho do modelo foi avaliado com um relatório de classificação e uma matriz de confusão, que nos permitem medir a acurácia, precisão e outras métricas importantes para validar a eficácia da classificação.

O modelo treinado com **Multinomial Naive Bayes** apresentou um excelente desempenho na tarefa de detecção automática de *fake news*. A avaliação do modelo no conjunto de teste resultou em uma **acurácia de 94%**, com métricas equilibradas para ambas as classes (notícias falsas e verdadeiras).

Abaixo, destacamos os principais resultados obtidos:

- **Precision** de 94% em ambas as classes, indicando uma baixa taxa de falsos positivos.
- **Recall** de 94% para a classe 0 (fake) e 93% para a classe 1 (real), refletindo uma boa capacidade de identificação dos casos reais.
- **F1-score** de 0.94 e 0.93, demonstrando equilíbrio entre precisão e recall.
- **Matriz de confusão** evidencia que a grande maioria das previsões foram corretas, com poucos erros relativos.

O pipeline, demonstrou ser eficaz ao integrar **MongoDB Atlas**, **Pymongo**, e **Scikit-learn** em um ambiente **Databricks**, viabilizando um fluxo completo de ingestão, processamento e classificação de textos jornalísticos em larga escala.

---

#### Métricas Principais (Para cada classe):

| **Métrica**   | **Classe 0 (Fake)** | **Classe 1 (Real)** | **Explicação**                                                                 |
|---------------|---------------------|----------------------|----------------------------------------------------------------------------------|
| Precision     | 0.94                | 0.94                 | 94% das previsões positivas estão corretas (baixa taxa de falsos positivos)      |
| Recall        | 0.94                | 0.93                 | 94% / 93% dos casos reais foram identificados (baixa taxa de falsos negativos)  |
| F1-Score      | 0.94                | 0.93                 | Média harmônica entre Precision e Recall (ótimo equilíbrio entre ambos)         |
| Support       | 4733                | 4247                 | Número de amostras em cada classe no conjunto de teste                          |

#### Métricas Globais:

| **Métrica**     | **Valor** | **Explicação**                                                                 |
|-----------------|-----------|----------------------------------------------------------------------------------|
| Accuracy        | 0.94      | 94% de acerto geral (excelente para problemas balanceados)                     |
| Macro Avg       | 0.94      | Média simples das métricas (igual peso para ambas as classes)                  |


A matriz de confusão abaixo representa o desempenho do modelo na tarefa de classificação de notícias falsas (Fake) e verdadeiras (Real):

```
[[4467  266]
 [ 285 3962]]
```

|                         | **Previsto: Fake (0)** | **Previsto: Real (1)** |
|-------------------------|------------------------|-------------------------|
| **Verdadeiro: Fake (0)**| 4467 ✅ (Verdadeiro Negativo - TN) | 266 ❌ (Falso Positivo - FP) |
| **Verdadeiro: Real (1)**| 285 ❌ (Falso Negativo - FN)       | 3962 ✅ (Verdadeiro Positivo - TP) |

---

##### 🧠 Interpretação:

- **4467** notícias falsas foram corretamente classificadas como falsas (**Verdadeiros Negativos**).
- **3962** notícias verdadeiras foram corretamente classificadas como verdadeiras (**Verdadeiros Positivos**).
- **266** notícias verdadeiras foram incorretamente classificadas como falsas (**Falsos Positivos**).
- **285** notícias falsas foram incorretamente classificadas como verdadeiras (**Falsos Negativos**).

---


## Análise Quantitativa
### 1. Palavras Mais Frequentes por Classe
A análise das palavras mais frequentes em notícias falsas e reais revela padrões linguísticos distintos entre os dois grupos. Nas fake news, termos como "hillary", "clinton", "obama", "trump" e "donald" dominam o vocabulário, indicando um forte viés político e a utilização de figuras polarizadoras para chamar atenção. Palavras como "us", "president", "people" e "state" sugerem um foco em temas nacionais e governamentais, frequentemente associados a teorias da conspiração ou desinformação estratégica. Além disso, o uso de termos como "would", "even", "like" e "said" aponta para um tom mais hipotético e emocional, característico de manchetes sensacionalistas.

Por outro lado, as notícias reais apresentam um perfil linguístico mais neutro e factual. Termos como "washington", "united", "states", "government" e "republican" refletem uma abordagem mais institucional e formal, típica de veículos jornalísticos tradicionais. Expressões como "told", "could", "last" e "house" indicam um discurso mais descritivo e menos carregado de emoção. Embora "trump" e "president" também apareçam com frequência, seu uso é menos dominante e mais contextualizado, sugerindo uma cobertura mais equilibrada.

Esses resultados destacam diferenças claras na linguagem utilizada por cada tipo de conteúdo. Notícias falsas tendem a explorar nomes próprios e termos emocionalmente carregados para criar narrativas persuasivas, enquanto notícias reais priorizam um vocabulário mais técnico e imparcial. Essa distinção pode ser útil no desenvolvimento de modelos de detecção de fake news, que podem se beneficiar da identificação desses padrões linguísticos.
<img width="1381" height="552" alt="image" src="https://github.com/user-attachments/assets/6fb4deb7-536c-42b8-a818-f4ea10d8ed5d" />

### 2. Nuvem de Palavras (Word Cloud)
As nuvens de palavras geradas para fake news e notícias reais reforçam e complementam os padrões identificados na análise de frequência de termos, oferecendo uma visualização intuitiva das diferenças linguísticas entre os dois grupos.

Fake News:
Na nuvem de palavras das fake news, destacam-se visualmente termos como "Trump", "Hillary", "Clinton", "Obama" e "president", confirmando o foco em figuras políticas polarizadoras. A presença marcante desses nomes próprios, muitas vezes em tamanhos maiores devido à sua alta frequência, sugere que as fake news frequentemente se aproveitam da notoriedade dessas personalidades para atrair atenção e engajamento. Além disso, palavras como "people", "us", "state" e "time" aparecem com destaque, indicando uma tendência a abordar temas de interesse nacional de forma sensacionalista ou manipuladora. A repetição de termos como "would" e "even" reforça o tom hipotético e emocional, característico de manchetes projetadas para provocar reações imediatas.

Notícias Reais:
Em contraste, a nuvem de palavras das notícias reais apresenta um vocabulário mais diversificado e menos centrado em indivíduos específicos. Termos como "Washington", "United", "States", "government" e "House" predominam, refletindo uma abordagem mais institucional e factual. A presença de palavras como "told", "last" e "new" sugere um discurso mais informativo e menos carregado de opinião ou emoção. Embora "Trump" e "president" ainda apareçam, seu tamanho relativo é menor, indicando uma cobertura mais equilibrada e menos focada em personalidades.
<img width="1185" height="307" alt="image" src="https://github.com/user-attachments/assets/4b4f1904-00c9-443c-9312-19daf86eaa4c" />

### 3. Distribuição de Tamanho dos Textos
A análise da distribuição do tamanho dos textos mostra que a maioria se concentra entre 0 e 5.000 caracteres, revelando uma predominância de textos relativamente curtos. Observa-se um pico em torno de 2.000 caracteres, principalmente na curva associada ao label 0 (falso), o que sugere que esses textos tendem a ser ligeiramente mais longos. Apesar dessas diferenças sutis, as distribuições entre os rótulos são bastante semelhantes, indicando que o tamanho dos textos, isoladamente, pode não ser um fator decisivo para a diferenciação entre as classes
<img width="886" height="557" alt="image" src="https://github.com/user-attachments/assets/180d2460-54a0-427c-84c2-d9b5b69f7736" />

### 4. Frequência de Bigramas (Pares de Palavras)
A análise de bigramas revela diferenças marcantes na construção linguística entre fake news e notícias reais. Nos conteúdos falsos, observa-se uma predominância de combinações envolvendo figuras políticas polarizadoras, como "donald trump", "hillary clinton" e "barack obama", que juntas representam a maioria dos pares mais frequentes. Este padrão sugere uma estratégia deliberada de associar a desinformação a personalidades já carregadas de significado político, aproveitando-se de vieses cognitivos pré-existentes no público. Além disso, a presença de termos institucionais como "white house" e "united states", frequentemente descontextualizados, parece servir para emprestar uma falsa aura de credibilidade ao conteúdo.

Em contraste, as notícias reais apresentam uma estrutura linguística mais diversificada e jornalisticamente convencional. Bigramas como "trump said" e "said statement" seguem o padrão profissional de atribuição clara de fontes, enquanto combinações como "prime minister" e "north korea" demonstram uma cobertura mais ampla de temas internacionais. A presença consistente de verbos de relato ("said") e a maior variedade temática indicam uma abordagem mais equilibrada e contextualizada dos fatos. Vale notar que alguns termos como "white house" aparecem em ambos os grupos, porém com usos distintos: nas fake news tendem a aparecer isolados, enquanto nas notícias reais estão normalmente inseridos em estruturas gramaticais mais completas.

Estes achados têm implicações práticas significativas para o combate à desinformação. Sistemas automatizados de detecção de fake news podem se beneficiar ao incorporar a análise de bigramas, priorizando alertas para sequências repetitivas de nomes próprios e a ausência de estruturas de atribuição típicas do jornalismo profissional. Da mesma forma, iniciativas de educação midiática deveriam enfatizar como identificar esses padrões linguísticos distintos, capacitando o público a reconhecer as estratégias discursivas típicas da desinformação. Para pesquisas futuras, seria produtivo investigar como esses padrões se modificam em contextos específicos, como períodos eleitorais ou crises internacionais, e como se relacionam com as dinâmicas de viralização nas redes sociais.
<img width="1350" height="536" alt="image" src="https://github.com/user-attachments/assets/2b0ec9da-d07b-4081-a148-5def1e163bbe" />

### 5. Frequência de Trigramas (Pares de Palavras)
A análise de trigramas (combinações de três palavras) revela padrões ainda mais distintos entre fake news e notícias reais do que os observados nos bigramas. Nos conteúdos falsos, destacam-se estruturas que combinam termos institucionais com alegações não verificadas, como "century wire says" e "video screen capture", que sugerem tentativas de validar informações através de supostas evidências visuais ou fontes obscuras. A presença marcante de "president barack obama", "president donald trump" e "president united states" em contextos desconexos indica a apropriação de cargos e instituições para dar aparência de legitimidade a conteúdos enganosos.

Notícias reais, por outro lado, apresentam trigramas que refletem práticas jornalísticas convencionais, como "white house said" e "reuters president donald", que demonstram preocupação com atribuição precisa de declarações. A variedade de combinações, incluindo "secretary state rex" (referindo-se a Rex Tillerson) e "president vladimir putin", mostra uma cobertura mais ampla e contextualizada de assuntos políticos. Curiosamente, enquanto as fake news usam "new york times" de forma isolada, possivelmente para se aproveitar da credibilidade do veículo, as notícias reais mencionam "new york routers" em contextos específicos de infraestrutura.

Estes padrões sugerem que:

- Fake news utilizam trigramas para criar falsas associações entre instituições, pessoas públicas e alegações não verificadas

- Notícias reais empregam combinações de três palavras principalmente para:

    - Atribuir declarações de forma precisa

    - Contextualizar informações dentro de estruturas narrativas mais complexas

    - Abordar uma gama mais diversificada de tópicos
<img width="1345" height="671" alt="image" src="https://github.com/user-attachments/assets/01acef1c-ee55-4989-a6c2-bfa105dc8640" />

### 6.Análise de Sentimento
O gráfico em formato de boxplot mostra a distribuição dos escores de sentimento para as classes "Fake" (vermelha) e "Real" (verde), variando de -1.0 (sentimento totalmente negativo) a +1.0 (totalmente positivo). As medianas de ambas as classes estão próximas de 0.00, sugerindo que os textos, em média, tendem ao sentimento neutro — independentemente do rótulo.

No entanto, há variações importantes que merecem atenção:

- Classe Fake: Exibe uma leve dispersão maior de sentimentos negativos, com alguns outliers indo para extremos negativos. Isso pode indicar que textos falsos tendem a carregar uma carga emocional mais negativa em certos casos.

- Classe Real: Embora também centrada em 0, possui uma distribuição similar, com outliers nos extremos positivos e negativos. Isso sugere que textos reais também apresentam polaridade emocional variada.

Apesar das medianas neutras, o boxplot revela a presença de outliers que indicam variações relevantes de polaridade entre as classes. Esses desvios podem ser úteis na modelagem preditiva, principalmente se combinados com outras variáveis linguísticas — como intensidade de polaridade, subjetividade ou presença de palavras-chave emocionais.
<img width="878" height="551" alt="image" src="https://github.com/user-attachments/assets/ced27d4f-09ee-47e3-a9bf-eeeb54087a29" />

### 7. Correlação entre Features
Os dados revelam padrões sutis, porém consistentes, na estrutura textual entre fake news (label 0) e notícias reais (label 1). Em média, os textos classificados como fake news apresentam maior extensão tanto em caracteres (1740.86 vs 1719.16) quanto em contagem de palavras (230.74 vs 226.66), com diferenças relativas de 1.26% e 1.8%, respectivamente. Esses resultados sugerem que conteúdos falsos podem adotar estratégias de redundância ou repetição para reforçar mensagens, contrariando a percepção comum de que seriam invariavelmente mais curtos.

A análise aponta para a relevância dessas features em modelos de detecção, especialmente quando combinadas com outros padrões já identificados (como uso de n-gramas e polaridade emocional). Notícias reais, por sua vez, demonstram maior concisão, possivelmente refletindo práticas editoriais mais rigorosas. Como próximo passo, sugere-se investigar métricas derivadas (como densidade caracteres/palavra) e a significância estatística dessas diferenças, além de cruzar esses dados com análises de complexidade lexical e origem dos conteúdos.

| Classe      | Média de Caracteres (text_length) | Média de Palavras (word_count) |
|-------------|----------------------------------|-------------------------------|
| Fake News (0) | 1740.86                          | 230.74                        |
| Notícias Reais (1) | 1719.16                          | 226.66                        |


## Conclusão:

O modelo errou relativamente pouco em ambos os sentidos:

- Baixo número de **falsos positivos** (notícia verdadeira sendo marcada como falsa);
- Baixo número de **falsos negativos** (notícia falsa sendo considerada verdadeira).

Esses resultados confirmam que o modelo está **bem equilibrado** e apresenta **excelente desempenho**, o que também é refletido nas métricas globais (accuracy, precision, recall, f1-score).

Este trabalho realizou uma análise detalhada de fake news e notícias reais utilizando uma infraestrutura tecnológica composta por MongoDB Atlas, PyMongo e Databricks. Essa combinação mostrou-se extremamente eficiente para todo o fluxo de trabalho, desde o armazenamento dos dados até a execução das análises textuais e estatísticas. O MongoDB Atlas foi utilizado como banco de dados principal, armazenando tanto os textos brutos quanto os metadados e resultados das análises processadas. Sua estrutura flexível de documentos JSON permitiu acomodar facilmente diferentes tipos de dados, desde o conteúdo textual original até as features extraídas, como n-gramas e scores de sentimento.

A integração com o PyMongo foi fundamental para conectar o ambiente de análise ao banco de dados. Essa biblioteca Python permitiu operações eficientes de inserção, consulta e atualização dos dados, garantindo que todas as etapas do processamento fossem adequadamente registradas. No Databricks, os notebooks proporcionaram um ambiente colaborativo e organizado para desenvolver as análises, com a vantagem adicional de poder visualizar os resultados diretamente no mesmo ambiente onde o código era executado. Essa combinação de tecnologias mostrou-se particularmente adequada para projetos de análise textual, oferecendo o equilíbrio ideal entre flexibilidade e poder de processamento.

Uma das principais vantagens dessa arquitetura foi sua escalabilidade. O MongoDB Atlas garantiu desempenho consistente mesmo com o crescimento do volume de dados, enquanto o Databricks forneceu a capacidade computacional necessária para as análises mais complexas. A natureza schemaless do MongoDB provou-se especialmente útil durante o desenvolvimento, permitindo adaptar a estrutura dos dados conforme novas necessidades de análise surgiam, sem exigir mudanças complexas no banco de dados. Essa flexibilidade foi crucial para um projeto de pesquisa que evoluiu iterativamente, com a descoberta de novas features e métricas ao longo do trabalho.

Os aprendizados dessa experiência destacam o potencial dessa stack tecnológica para projetos de processamento de linguagem natural. A ausência de PySpark não se mostrou uma limitação, demonstrando que soluções baseadas em Python puro podem ser perfeitamente adequadas para muitos cenários de análise textual. Para trabalhos futuros, recomenda-se explorar ainda mais as capacidades do MongoDB Atlas, como a implementação de índices de texto completo para consultas mais sofisticadas, e a automação de pipelines para atualização contínua das análises. Esta abordagem mostrou-se robusta o suficiente para servir como base para pesquisas mais avançadas na área de detecção de desinformação.

Os resultados sugerem que a combinação dessas características pode servir como base eficaz para sistemas de classificação automática, destacando-se especialmente o potencial da análise de trigramas e padrões de sentimento como features discriminativas. Para o público geral, estes achados oferecem indicadores práticos para identificação de conteúdo suspeito, como excesso de nomes de figuras públicas, tom emocional extremo e falta de fontes claramente atribuídas. Apesar das limitações inerentes ao escopo do estudo - como a necessidade de validação em diferentes contextos culturais e temporais - esta pesquisa fornece evidências robustas sobre as assinaturas linguísticas da desinformação, abrindo caminho tanto para o aprimoramento de ferramentas tecnológicas quanto para estratégias mais efetivas de alfabetização midiática. Como próximos passos, podemos ter a integração dessas descobertas em modelos preditivos e a criação de materiais educativos que traduzam esses padrões em orientações acessíveis para o público geral.

# 📚 Referências Bibliográficas Sugeridas

1. **MongoDB Atlas e Integração com Python (PyMongo)**  
   MongoDB, Inc. (2024). *A Guide to Connect Databricks and MongoDB Atlas using Python API*. CloudThat.  
   Disponível em: [https://www.cloudthat.com/resources/blog/a-guide-to-connect-databricks-and-mongodb-atlas-using-python-api](https://www.cloudthat.com/resources/blog/a-guide-to-connect-databricks-and-mongodb-atlas-using-python-api)  

2. **Databricks e Processamento de Dados com MongoDB Atlas**  
   Raisinghani, A. (2024). *Utilizing PySpark to Connect MongoDB Atlas with Azure Databricks*. MongoDB Developer Center.  
   Disponível em: [https://www.mongodb.com/developer/languages/python/atlas-databricks-pyspark-demo](https://www.mongodb.com/developer/languages/python/atlas-databricks-pyspark-demo)  

3. **Documentação Oficial do PyMongo**  
   MongoDB, Inc. (2025). *PyMongo Documentation*.  
   Disponível em: [https://pymongo.readthedocs.io](https://pymongo.readthedocs.io)  

4. **Documentação Oficial do Databricks**  
   Databricks, Inc. (2025). *Databricks Documentation*.  
   Disponível em: [https://docs.databricks.com](https://docs.databricks.com)  

