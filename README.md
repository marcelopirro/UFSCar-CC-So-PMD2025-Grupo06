**Autores:**  
- Marcelo Pirro – 800510  
- Guilherme Campos Bortoletto – 801477  

**Projeto:**  
**Detecção de Fake News com Processamento em Larga Escala Utilizando MongoDB Atlas e Apache Spark**  

**Ano:** 2025  

---

## Introdução

Com o avanço das tecnologias de informação e o crescente consumo de conteúdo digital, a disseminação de notícias falsas – as chamadas *fake news* – tornou-se um dos maiores desafios da sociedade contemporânea. Esses conteúdos enganosos podem influenciar opiniões públicas, comprometer processos democráticos e gerar consequências sociais e econômicas significativas. Diante desse cenário, surge a necessidade de soluções tecnológicas capazes de identificar e mitigar a propagação de informações falsas de forma automatizada, rápida e escalável.

## Objetivo

 Nosso objetivo é desenvolver um pipeline completo de detecção de notícias falsas (fake news) por meio da análise de padrões linguísticos em textos jornalísticos. Buscamos demonstrar a aplicação de tecnologias NoSQL e frameworks de processamento distribuído para

- Armazenar de forma flexível e escalável um volume significativo de notícias reais e falsas;
- Processar os dados textuais em larga escala utilizando técnicas de NLP (*Natural Language Processing*);
- Treinar e avaliar um modelo de *machine learning* para classificar automaticamente notícias como verdadeiras ou falsas;
- Avaliar a viabilidade da arquitetura proposta no contexto de aplicações reais que demandam escalabilidade, performance e robustez;
- Identificar métricas linguísticas complementares, como as palavras mais frequentes em textos de *fake news*, fornecendo análises exploratórias sobre padrões de vocabulário e estilo.

## Tecnologias Escolhidas

### MongoDB Atlas

A escolha de um banco de dados NoSQL como o **MongoDB** é mais adequada para este projeto porque lidamos com dados textuais e semi-estruturados (notícias) que variam em formato e conteúdo.

A natureza dos dados tratados no projeto — notícias jornalísticas — envolve estruturas semi-estruturadas, com campos como **título**, **categoria**, **conteúdo textual**, **data** e possíveis **metadados futuros**. O **MongoDB Atlas** resolve esse problema ao utilizar um modelo de documentos **JSON**, que se adapta perfeitamente a esse tipo de dado textual. Além disso, trata-se de uma solução **gerenciada na nuvem**, com fácil escalabilidade horizontal.

Diferente dos bancos relacionais, que exigem esquemas rígidos e múltiplos *joins*, o MongoDB oferece uma **estrutura flexível**, possui **indexação** e **busca textual nativa**, e se integra diretamente ao **Apache Spark**, permitindo o **processamento distribuído eficiente** dos dados — algo essencial para o volume e complexidade das análises propostas.

### Apache Spark (PySpark)

A etapa de análise e classificação de *fake news* exige o processamento de grandes volumes de texto, tornando essencial o uso de uma ferramenta que suporte dados em escala. O **Apache Spark**, com seu módulo **PySpark**, é ideal para esse cenário, pois oferece uma **arquitetura distribuída** que permite escalabilidade horizontal do processamento, garantindo eficiência mesmo diante de grandes conjuntos de dados.

O Spark conta com bibliotecas integradas como o **Spark MLlib**, voltado para *machine learning*, e o **Spark NLP**, utilizado para tratamento linguístico avançado. Além disso, sua **integração nativa com fontes de dados como o MongoDB** elimina etapas intermediárias de ETL, reduzindo a duplicação de dados e acelerando o fluxo de processamento.

Outro diferencial do Spark é o uso de **DataFrames otimizados** e a **execução preguiçosa**, que contribuem para uma manipulação de dados mais eficiente, impactando diretamente na performance da aplicação.

### Integração MongoDB + Spark

A integração entre **MongoDB Atlas** e **Apache Spark** é um dos maiores diferenciais da arquitetura proposta, pois combina um armazenamento altamente flexível com um processamento intensivo de dados de forma eficiente.

O conector oficial entre essas duas tecnologias permite a **leitura e escrita direta dos dados**, o que possibilita que o processamento ocorra no próprio local onde os dados estão armazenados. Isso reduz significativamente a latência e evita sobrecargas desnecessárias.

Essa integração garante uma **solução coesa e escalável**, capaz de lidar com a armazenagem de dados textuais não estruturados, além de permitir sua análise em larga escala. As tecnologias foram escolhidas por oferecerem **alto desempenho**, **fácil integração**, **escalabilidade horizontal** e **alinhamento total com os objetivos acadêmicos e técnicos da disciplina**.

## Fonte de Dados

- Dataset: Fake and Real News Dataset – Kaggle  
- Link: [https://www.kaggle.com/datasets/clmentbisaillon/fake-and-real-news-dataset](https://www.kaggle.com/datasets/clmentbisaillon/fake-and-real-news-dataset)  
- Conteúdo: Cerca de 44.000 artigos de notícias, divididos nos arquivos `Fake.csv` e `True.csv`.  
- Atributos: título, conteúdo, categoria, data de publicação.

## Fluxo do Projeto

1. **Obtenção dos Dados** – download dos arquivos CSV do Kaggle.  
2. **Pré-processamento Inicial** – limpeza e padronização em Python.  
3. **Inserção no MongoDB Atlas** – cada notícia é armazenada como documento JSON.  
4. **Leitura com Apache Spark** – via MongoDB Spark Connector.  
5. **Pré-processamento com PySpark** – tokenização, vetorização e cálculo do TF-IDF.  
6. **Treinamento de Modelos** – aplicação de algoritmos como Naive Bayes e Regressão Logística.  
7. **Análise Textual Exploratória** – identificação de padrões linguísticos e vocabulário frequente em fake news.  

### Pipeline de Análise e Classificação de Notícias

Como mencionado anteriormente, o projeto tem como objetivo **analisar e classificar notícias como verdadeiras ou falsas** a partir de um conjunto de dados obtido da plataforma Kaggle, composto pelos arquivos `Fake.csv` e `True.csv`. Esses dados representam textos jornalísticos de fontes confiáveis e não confiáveis, contendo colunas como **título**, **autor**, **data** e **conteúdo da notícia**.

A primeira etapa do fluxo consiste na **obtenção dos dados em formato CSV**. Em seguida, realiza-se um **pré-processamento inicial com Python**, que inclui limpeza básica, como a **remoção de valores nulos**, a **unificação dos dois arquivos** e a **inserção de um rótulo binário** indicando se a notícia é verdadeira ou falsa.

Após essa preparação, os dados são inseridos no **MongoDB Atlas**, onde são armazenados como documentos **JSON**, garantindo uma estrutura flexível e compatível com dados semi-estruturados.

Na sequência, o processamento passa para o **Apache Spark**, que lê os dados diretamente do MongoDB através do **MongoDB Spark Connector**. Em seguida, com o **PySpark**, realiza-se o **pré-processamento textual**, que inclui etapas como **tokenização**, **vetorização** e o **cálculo do TF-IDF (Term Frequency-Inverse Document Frequency)**, convertendo os textos em representações numéricas.

Com esses vetores prontos, inicia-se o **treinamento de modelos de *machine learning*** utilizando o **Spark MLlib**. Algoritmos como **Naive Bayes** e **Regressão Logística** são aplicados para ensinar o modelo a distinguir notícias falsas das verdadeiras. Todo o processo é realizado em um **ambiente distribuído**, aproveitando a **escalabilidade e desempenho do Apache Spark**.


## Resultados Esperados

O projeto entregará um pipeline funcional de ponta a ponta para a detecção automática de *fake news*, desde a ingestão até a classificação. O modelo treinado será avaliado por métricas como **acurácia**, **precisão** e **recall**, permitindo uma análise quantitativa de desempenho. A integração entre MongoDB Atlas e Spark será demonstrada em um cenário real, evidenciando a eficácia da arquitetura distribuída.

Além disso, espera-se produzir uma análise textual qualitativa identificando as **palavras mais recorrentes em notícias falsas**, fornecendo indícios linguísticos úteis para a compreensão e identificação desses textos.

---
