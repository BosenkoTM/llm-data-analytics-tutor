# EduTutor — интеллектуальная поддержка обучающегося

[← EduTutor](../../README.md) · [Архитектура](../../architecture/01_overview.md)

Агент-тьютор предназначен для предметных пояснений во время самостоятельной учебной деятельности. Его архитектура соединяет пользовательский интерфейс, серверную обработку, поиск по материалам дисциплины и локальную языковую модель.

## Взаимодействие компонентов

```mermaid
%%{init: {"theme": "base", "flowchart": {"htmlLabels": true, "nodeSpacing": 38, "rankSpacing": 52}, "themeVariables": {"fontFamily": "Arial, sans-serif", "fontSize": "15px", "background": "#ffffff", "primaryTextColor": "#122B5E", "lineColor": "#7692B1"}}}%%
flowchart TB
  subgraph STUD["Сопровождение обучения"]
    direction LR
    S(["Обучающийся"]) --> MD["Задание<br/>в Moodle 5"]
    MD -.-> UI["Student<br/>Workspace"]
    UI --> T["Агент-<br/>тьютор"]
  end
  subgraph KNOW["Знания и генерация"]
    direction LR
    R["Предметный<br/>поиск RAG"] --> PG[("PostgreSQL<br/>pgvector")]
    O["Локальная<br/>LLM"]
  end
  PR(["Преподаватель"]) --> LO["EduRAG<br/>Loader"]
  LO --> PG
  T --> R
  T --> O
  PR -. "итоговая оценка" .-> MD
  class S,PR person;
  class MD lms;
  class UI ui;
  class T,O agent;
  class LO,R,PG data;
  style STUD fill:#F5F8FF,stroke:#A9BDE9,stroke-width:1px;
  style KNOW fill:#F2FBF8,stroke:#9ED9CA,stroke-width:1px;
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

<sub>Редактируемая диаграмма: [`edututor_overview.mmd`](../../assets/edututor_overview.mmd)</sub>

## Основные задачи

- Объяснение формулировок учебного задания и терминологии дисциплины.
- Поддержка работы с Python, SQL, алгоритмами и данными в предусмотренных сценариях.
- Анализ типичных ошибок и пояснение подходов к их исправлению.
- Использование предметного контекста RAG при формировании ответа.
- Содействие самостоятельной интерпретации и проверке результатов.

Тьютор является **помощником в обучении**, а не системой автоматического выставления оценок. Связь с заданиями Moodle, конкретными вариантами и первоисточниками развивается поэтапно.

## Программная основа

[![Next.js](https://img.shields.io/badge/Next.js-111827?style=for-the-badge&logo=nextdotjs&logoColor=white)](https://nextjs.org/) [![CopilotKit](https://img.shields.io/badge/CopilotKit-1769D2?style=for-the-badge)](https://www.copilotkit.ai/) [![AG-UI](https://img.shields.io/badge/AG-UI-0068D2?style=for-the-badge)](https://docs.ag-ui.com/) [![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/) [![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/) [![Ollama](https://img.shields.io/badge/Ollama-33465E?style=for-the-badge)](https://ollama.com/) [![Qwen](https://img.shields.io/badge/Qwen-7B61C8?style=for-the-badge)](https://qwenlm.github.io/) ![RAG](https://img.shields.io/badge/RAG-008F86?style=for-the-badge)

[Образовательный маршрут](../../education/01_student_path.md) · [Агенты](../../architecture/02_agents.md) · [Технологии](../../architecture/06_technology_stack.md)
