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

O objetivo deste projeto é desenvolver um pipeline completo para detecção automática de notícias falsas, com base na análise de padrões linguísticos em textos jornalísticos. As metas específicas incluem:

- Armazenar de forma flexível e escalável um volume significativo de notícias reais e falsas;
- Processar os dados textuais em larga escala utilizando técnicas de NLP (*Natural Language Processing*);
- Treinar e avaliar um modelo de *machine learning* para classificar automaticamente notícias como verdadeiras ou falsas;
- Avaliar a viabilidade da arquitetura proposta no contexto de aplicações reais que demandam escalabilidade, performance e robustez;
- Identificar métricas linguísticas complementares, como as palavras mais frequentes em textos de *fake news*, fornecendo análises exploratórias sobre padrões de vocabulário e estilo.

## Tecnologias Escolhidas

### MongoDB Atlas

O banco de dados NoSQL MongoDB Atlas foi escolhido por sua adequação ao armazenamento de dados semi-estruturados. A estrutura de documentos JSON facilita a modelagem de notícias jornalísticas, e a escalabilidade nativa em nuvem oferece flexibilidade para manipular grandes volumes de dados. A busca textual integrada e a fácil integração com outras ferramentas tornam o MongoDB a escolha ideal para este projeto.

### Apache Spark (PySpark)

A análise e classificação de textos em larga escala requerem alto desempenho e capacidade de paralelismo. O Apache Spark, com seu módulo PySpark, oferece uma arquitetura distribuída que atende perfeitamente a esses requisitos. Com bibliotecas específicas para NLP e *machine learning*, como Spark NLP e MLlib, o Spark permite o pré-processamento, vetorização e classificação eficiente dos textos.

### Integração MongoDB + Spark

A integração entre MongoDB Atlas e Apache Spark é um dos principais pilares da arquitetura. O uso do conector oficial permite a leitura e escrita dos dados de forma distribuída, mantendo alta performance e reduzindo a latência entre armazenamento e processamento.

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

## Resultados Esperados

O projeto entregará um pipeline funcional de ponta a ponta para a detecção automática de *fake news*, desde a ingestão até a classificação. O modelo treinado será avaliado por métricas como **acurácia**, **precisão** e **recall**, permitindo uma análise quantitativa de desempenho. A integração entre MongoDB Atlas e Spark será demonstrada em um cenário real, evidenciando a eficácia da arquitetura distribuída.

Além disso, espera-se produzir uma análise textual qualitativa identificando as **palavras mais recorrentes em notícias falsas**, fornecendo indícios linguísticos úteis para a compreensão e identificação desses textos.

---
