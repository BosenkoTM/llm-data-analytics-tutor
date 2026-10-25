# EduRAG Loader — управление учебной базой знаний

[← EduTutor](../../README.md) · [Предметный RAG](../../architecture/03_rag.md)

EduRAG Loader — компонент для организации предметных материалов образовательных программ. Он позволяет преподавателю управлять структурой дисциплин, тем и учебных документов и подготавливать содержание к использованию в поисковых сценариях EduTutor.

## Информационный поток

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

<sub>Редактируемая диаграмма: [`rag_pipeline.mmd`](../../assets/rag_pipeline.mmd)</sub>

## Возможности

| Направление | Назначение |
|---|---|
| Каталог | Создание и изменение дисциплин и тем |
| Учебные материалы | Загрузка, просмотр и управление документами |
| Обработка | Выделение текста, фрагментация, формирование эмбеддингов |
| Поиск | Работа с предметными индексами и диагностикой релевантности |
| Управление | Исправление и удаление отдельных объектов каталога |

В приложении предусмотрен рабочий режим управления материалами; отдельный режим просмотра используется для диагностики. **Загрузка документа и завершение его индексации — разные стадии**, поэтому наличие записи о загрузке само по себе не означает готовность материала к поиску.

## Технологии

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/) [![Flask](https://img.shields.io/badge/Flask-3B5468?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com/) [![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/) [![pgvector](https://img.shields.io/badge/pgvector-008C9E?style=for-the-badge)](https://github.com/pgvector/pgvector) ![RAG](https://img.shields.io/badge/RAG-008F86?style=for-the-badge) ![E5 embeddings](https://img.shields.io/badge/E5%20embeddings-008F86?style=for-the-badge)

[Модель учебного контента](../../architecture/03_rag.md) · [Роль преподавателя](../../education/02_teacher_path.md)
