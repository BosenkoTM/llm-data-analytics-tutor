# Предметная база знаний и поиск RAG

[← Архитектура](01_overview.md) · [EduRAG Loader](../services/edurag_loader/README.md)

Retrieval-Augmented Generation (RAG) используется для включения в ответ материалов соответствующей дисциплины. Это позволяет связывать пояснения с учебным контекстом: темой занятия, заданием, терминологией и методическими требованиями преподавателя.

## Подготовка материалов и формирование ответа

```mermaid
%%{init: {"theme": "base", "flowchart": {"htmlLabels": true, "nodeSpacing": 38, "rankSpacing": 52}, "themeVariables": {"fontFamily": "Arial, sans-serif", "fontSize": "15px", "background": "#ffffff", "primaryTextColor": "#122B5E", "lineColor": "#7692B1"}}}%%
flowchart TB
  subgraph PREP["Подготовка материалов"]
    direction LR
    P(["Преподаватель"]) --> C["Дисциплина<br/>и тема"]
    C --> U["Загрузка<br/>документа"]
    U --> X["Обработка<br/>и разбиение"]
    X --> E["Эмбеддинги<br/>и индекс"]
  end
  E --> DB[("PostgreSQL<br/>pgvector")]
  subgraph USE["Поддержка обучающегося"]
    direction LR
    Q(["Вопрос<br/>обучающегося"]) --> RET["Поиск<br/>по материалам"]
    RET --> T["Tutor + LLM"]
    T --> A(["Предметное<br/>объяснение"])
  end
  DB --> RET
  class P,Q,A person;
  class C,U,X,E,DB,RET data;
  class T agent;
  style PREP fill:#F4FBF9,stroke:#A2DCCA,stroke-width:1px;
  style USE fill:#F5F8FF,stroke:#B4C8E9,stroke-width:1px;
classDef person fill:#E8EEFC,stroke:#345FA8,color:#122B5E,stroke-width:1.7px;
classDef lms fill:#FFF1DA,stroke:#C5852D,color:#66400B,stroke-width:1.7px;
classDef ui fill:#E8F3FE,stroke:#1769D2,color:#123F7A,stroke-width:1.7px;
classDef agent fill:#F0E9FC,stroke:#7959C7,color:#443076,stroke-width:1.7px;
classDef data fill:#E1F5F0,stroke:#008F86,color:#075950,stroke-width:1.7px;
classDef infra fill:#EFF2F7,stroke:#6B7F95,color:#33465E,stroke-width:1.7px;
classDef future fill:#F8F6FC,stroke:#9A88B9,color:#605579,stroke-width:1.5px,stroke-dasharray:4 4;
classDef guard fill:#FFF0EB,stroke:#D36E56,color:#813C2C,stroke-width:1.7px;
linkStyle default stroke:#7692B1,stroke-width:1.4px;
```

<sub>Редактируемая диаграмма: [`rag_pipeline.mmd`](../assets/rag_pipeline.mmd)</sub>

**Материалы дисциплины** организуются в каталоге, обрабатываются и подготавливаются для поиска. **Запрос обучающегося** сопоставляется с доступным предметным контекстом, после чего языковая модель формирует пояснение.

Важно различать загрузку файла и появление его фрагментов в поисковом индексе. Подготовка, подтверждение и индексация — разные операции; их согласованность в полном цикле продолжает развиваться.

## Информационная модель

```mermaid
%%{init: {"theme": "base", "flowchart": {"htmlLabels": true, "nodeSpacing": 38, "rankSpacing": 52}, "themeVariables": {"fontFamily": "Arial, sans-serif", "fontSize": "15px", "background": "#ffffff", "primaryTextColor": "#122B5E", "lineColor": "#7692B1"}}}%%
flowchart TB
  C["Дисциплина"] --> T["Тема"]
  T --> L["Занятие"]
  L --> D["Учебный<br/>материал"]
  D --> R[("Предметная<br/>база RAG")]
  T -.-> A["Лабораторная<br/>работа"]
  T -.-> P["Практическая<br/>работа"]
  A --> D
  P --> D
  class C,T,D,R data;
  class L,A,P ui;
classDef person fill:#E8EEFC,stroke:#345FA8,color:#122B5E,stroke-width:1.7px;
classDef lms fill:#FFF1DA,stroke:#C5852D,color:#66400B,stroke-width:1.7px;
classDef ui fill:#E8F3FE,stroke:#1769D2,color:#123F7A,stroke-width:1.7px;
classDef agent fill:#F0E9FC,stroke:#7959C7,color:#443076,stroke-width:1.7px;
classDef data fill:#E1F5F0,stroke:#008F86,color:#075950,stroke-width:1.7px;
classDef infra fill:#EFF2F7,stroke:#6B7F95,color:#33465E,stroke-width:1.7px;
classDef future fill:#F8F6FC,stroke:#9A88B9,color:#605579,stroke-width:1.5px,stroke-dasharray:4 4;
classDef guard fill:#FFF0EB,stroke:#D36E56,color:#813C2C,stroke-width:1.7px;
linkStyle default stroke:#7692B1,stroke-width:1.4px;
```

<sub>Редактируемая диаграмма: [`course_structure.mmd`](../assets/course_structure.mmd)</sub>

| Элемент | Назначение |
|---|---|
| **Дисциплина** | Область учебных знаний |
| **Тема** | Логический раздел содержания |
| **Занятие** | Форма учебной деятельности |
| **Материал** | Документ, Notebook, условие работы, критерии, пример или методический текст |
| **Поисковый индекс** | Фрагменты, метаданные и векторные представления |

## Технологии

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/) [![pgvector](https://img.shields.io/badge/pgvector-008C9E?style=for-the-badge)](https://github.com/pgvector/pgvector) ![RAG](https://img.shields.io/badge/RAG-008F86?style=for-the-badge) ![E5 embeddings](https://img.shields.io/badge/E5%20embeddings-008F86?style=for-the-badge) [![Ollama](https://img.shields.io/badge/Ollama-33465E?style=for-the-badge)](https://ollama.com/) [![Qwen](https://img.shields.io/badge/Qwen-7B61C8?style=for-the-badge)](https://qwenlm.github.io/)

В качестве основы предметного поиска используются PostgreSQL и pgvector; эмбеддинги формируются средствами соответствующей модели, а генерация пояснений выполняется локальной LLM. Структурно-семантические методы поиска развиваются в рамках согласования Loader и Tutor.

[Управление материалами](../services/edurag_loader/README.md) · [Образовательные треки](../education/03_courses.md)
