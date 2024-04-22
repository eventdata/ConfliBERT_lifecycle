# ConfliBERT lifecycle

## Table of contents
1. [Authors](#authors)
2. [Introduction](#introduction)
3. [Structure](#structure)
4. [Who benefits from this project?](#audience)
5. [Information style](#information)
6. [Funding](#funding)

---

## Authors <a name="authors"></a>

Patrick T. Brandt, Vito D'Orazio, Latifur Khan, and Javier Osorio

Yibo Hu, Shreyas Meher, Marcus Sianan, Sultan Alsarra, Wooseong Yang, Amber Converse, XXX we need all the names XXX.


---

## Introduction <a name="introduction"></a>

This repository documents the process and technical elements related to developing ConfliBERT models, a set of domain-specific Large Language Model (LLM) specialized on political conflict and violence in different languages. 

Thanks to the support of the National Science Foundation, our research team developed ConfliBERT ([Hu et al. 2022](https://aclanthology.org/2022.naacl-main.400/)), a domain-specific LLM specially designed to process a variety of Natural Language Procesing (NLP) tasks related to politics and violence in English text. ConfliBERT outperforms all other LLMs in when conducting conflict specific NLP tasks and has been validated as the state of the art in Machine Learning (ML) developments by independent teams of researchers ([Häffner et al. 2023](https://www.cambridge.org/core/journals/political-analysis/article/introducing-an-interpretable-deep-learning-approach-to-domainspecific-dictionary-creation-a-use-case-for-conflict-prediction/BB6AD7222954A1779D97AB319621DC7E)).

Given that conflict scholars often focus on analyzing conflict processes in foreign locations, the team leveraged on ConfliBERT's outstanding performance in English to advance multilingual versions of ConfliBERT in other languages including Spanish ([Yang et al. 2023](https://ieeexplore.ieee.org/document/10409883)) and Arabic ([Alsarra et al. 2023](https://aclanthology.org/2023.ranlp-1.11/#:~:text=2023.-,ConfliBERT%2DArabic%3A%20A%20Pre%2Dtrained%20Arabic%20Language%20Model%20for,%E2%80%93108%2C%20Varna%2C%20Bulgaria.)). This multilingual approach provides tools for researchers to analyze conflict dynamics in other countries in their native languages. Based on these developments, the research team is currently focused on refining and expanding the multilingual family of ConfliBERT models to additional languages.

---

## Structure <a name="structure"></a>

Based on our ConfliBERT models, this repository provides an intuitive explanation and more technical documentation on the different aspects related to creating domain-specific LLMs. To do so, this repository covers the distinct stages in the lifecycle of LLM development.

The content includes distinct folders corresponding to each of the main stages of LLM development:

1. **Scraping**:
   * Describes the criteria considered for gathering domain-specific content.
   * Presents a detailed list of the information sources used to build the corpora used for each ConfliBERT model.
   * This folder includes the scripts used for scraping the different websites.
2. **Corpus**:
   * Describes the methodology implemented to ensure the domain validity of the text used to conform the corpora in each language. This incudes a detailed description of the words used to filter out false positives, and the process of shaping domain-specific text tailored to ConfliBERT.
   * Due to copyright restrictions, we cannot make publicly available the entire corpora used to develop the different ConfliBERT models.
3. **LLM Development**:
   * Provides a technical description onf the LLM design and architecture including: environment setup, script development, and model training.
4. **Training Datasets**:
   * Describes the different applications developed by this research team to test ConfliBERT's performance in a variety of NLP tasks.
5. **Machine translation**:
   * Presents the methodology used to generate machine translations to compare the performance of native language ConfliBERT models in their original languages and ConfliBERT EN in machine-generated corpora. 
6. **Applications**:
   * Documents additional ConfliBERT applications using datasets developed by other research teams.

For stages 1-3, the repository includes specific docummentation for the different languages included in the ConfliBERT family:

* AR - Arabic - Done and published ([Alsarra et al. 2023](https://aclanthology.org/2023.ranlp-1.11/#:~:text=2023.-,ConfliBERT%2DArabic%3A%20A%20Pre%2Dtrained%20Arabic%20Language%20Model%20for,%E2%80%93108%2C%20Varna%2C%20Bulgaria.))
* **EN - English**: Completed and published ([Hu et al. 2022](https://aclanthology.org/2022.naacl-main.400/))
* **FR - French**: In progress
* **HI - Hindi**: In progress
* **RU - Russian**: In progress
* **SP - Spanish**: Completed and published ([Yang et al. 2023](https://ieeexplore.ieee.org/document/10409883)). Update in progress.

---

## Who benefits from this project? <a name="audience"></a>

The information in this repository aims to help different types of users with distinct levels of technical sophistication. 

Users with basic expertise in computational social sciences interested in using ConflBERT for their own research projects will find intuitive and easy to use tools, examples, and recomendations in this repository. This type of users include graduate students in the social sciences, practitoners, security analysits, and Assistant Professors. 

In addition, the repository provides more technical information for advanced computer science users interested in developing or enhancing their own domain-specific LLMs. This includes graduate students in computer sciences, developers, senior analysts and researchers, as well as Associate and Full Professors.



---

## Information style <a name="information"></a>

The content of this repository is structured in two levels:

1. Substantive information.
   * This first level aims to present information in an accessible and intuitive way to enable researchers use the tools developed in this project.
   * The information includes clear descriptions, explanations, examples, and tutorials so that users understand the basic intuition behind ConfliBERT and easily apply  these tools to advance their own research.  
2. Technical details.
   * The second level offers more detailed technical information at the different stages of the LLM development lifecycle for developers or advanced researchers.

---

## Funding  <a name="funding"></a>

This research project is possible thanks to the generous support of the National Science Foundation through a variety of reserach grants (XXXXXX).


