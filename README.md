**Autores:**  
- Marcelo Pirro – 800510  
- Guilherme Campos Bortoletto – 801477  

**Projeto:**  
**Detecção de Fake News com Processamento em Larga Escala Utilizando MongoDB Atlas e Apache Spark**  

**Ano:** 2025  

---

## Resumo

O projeto tem como foco o desenvolvimento de um pipeline completo para a detecção automática de fake news, integrando tecnologias de armazenamento e processamento em larga escala. Utilizando o MongoDB Atlas como banco de dados NoSQL e o Apache Spark (PySpark) como plataforma de processamento distribuído, o sistema é capaz de armazenar, transformar e analisar milhares de notícias jornalísticas.

A solução trabalha com um conjunto de dados obtido do Kaggle, contendo aproximadamente 44 mil notícias reais e falsas. Após o pré-processamento dos dados em Python, os registros são inseridos no MongoDB como documentos JSON. A partir daí, o Spark realiza operações de limpeza, tokenização, vetorização e transformação com TF-IDF. Com os dados preparados, são treinados modelos de machine learning para classificar os textos como verdadeiros ou falsos.

Além da classificação automática, o projeto realiza uma análise exploratória linguística, identificando as palavras mais frequentes em conteúdos falsos, revelando padrões de escrita e vocabulário característicos. O pipeline completo é escalável, eficiente e compatível com aplicações reais, demonstrando a viabilidade técnica da integração entre MongoDB e Spark.

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


![Obtenção dos Dados](https://github.com/user-attachments/assets/32eb0e40-87ff-4f57-a9ec-25a35363f6bb)




## Resultados Esperados

O projeto entregará um pipeline funcional de ponta a ponta para a detecção automática de *fake news*, desde a ingestão até a classificação. O modelo treinado será avaliado por métricas como **acurácia**, **precisão** e **recall**, permitindo uma análise quantitativa de desempenho. A integração entre MongoDB Atlas e Spark será demonstrada em um cenário real, evidenciando a eficácia da arquitetura distribuída.

Além disso, espera-se produzir uma análise textual qualitativa identificando as **palavras mais recorrentes em notícias falsas**, fornecendo indícios linguísticos úteis para a compreensão e identificação desses textos.

---
