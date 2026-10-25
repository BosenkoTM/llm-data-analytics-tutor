# EduTutor

**Интеллектуальная платформа сопровождения обучения в цифровой образовательной среде**

EduTutor — разрабатываемая образовательная система на основе больших языковых моделей и предметных баз знаний. Платформа объединяет поддержку самостоятельной работы обучающихся, управление учебными материалами и агентные сервисы для преподавателя.

Проект развивается в **Московском городском педагогическом университете (МГПУ), Институт цифрового образования**, при поддержке **Благотворительного фонда Владимира Потанина** — «LLM-ассистент для интеллектуальной поддержки обучения анализу данных в магистратуре».

[Образовательные сценарии](education/README.md) · [Архитектура](architecture/01_overview.md) · [Технологический стек](architecture/06_technology_stack.md) · [Компоненты](services/edututor/README.md)

## Технологический стек

**Интерфейс и образовательная среда**

[![Next.js](https://img.shields.io/badge/Next.js-111827?style=for-the-badge&logo=nextdotjs&logoColor=white)](https://nextjs.org/) [![CopilotKit](https://img.shields.io/badge/CopilotKit-1769D2?style=for-the-badge)](https://www.copilotkit.ai/) [![AG-UI](https://img.shields.io/badge/AG-UI-0068D2?style=for-the-badge)](https://docs.ag-ui.com/) [![Moodle 5](https://img.shields.io/badge/Moodle%205-F08A24?style=for-the-badge&logo=moodle&logoColor=white)](https://moodle.org/)

**Агенты и серверные сервисы**

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/) [![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/) [![Flask](https://img.shields.io/badge/Flask-3B5468?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com/) [![LangGraph](https://img.shields.io/badge/LangGraph-635BCE?style=for-the-badge)](https://www.langchain.com/langgraph) [![MCP](https://img.shields.io/badge/MCP-6750A4?style=for-the-badge)](https://modelcontextprotocol.io/)

**Модели, поиск и хранение данных**

[![Ollama](https://img.shields.io/badge/Ollama-33465E?style=for-the-badge)](https://ollama.com/) [![Qwen](https://img.shields.io/badge/Qwen-7B61C8?style=for-the-badge)](https://qwenlm.github.io/) ![RAG](https://img.shields.io/badge/RAG-008F86?style=for-the-badge) [![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/) [![pgvector](https://img.shields.io/badge/pgvector-008C9E?style=for-the-badge)](https://github.com/pgvector/pgvector) ![E5 embeddings](https://img.shields.io/badge/E5%20embeddings-008F86?style=for-the-badge)

[Назначение технологий и их взаимодействие](architecture/06_technology_stack.md)

## Концепция платформы

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

<sub>Редактируемая диаграмма: [`edututor_overview.mmd`](assets/edututor_overview.mmd)</sub>

**Основной принцип — поддержка самостоятельного решения учебной задачи.** Агент-тьютор помогает разобрать формулировку задания, объяснить методы, выявить ошибки кода и интерпретировать результаты. Ответы могут использовать материалы конкретной дисциплины из базы знаний. Преподаватель определяет критерии и сохраняет ответственность за итоговое оценивание.

## Образовательный охват

EduTutor проектируется как **расширяемая платформа для образовательных программ разных уровней и профилей**, а не как решение, ограниченное одной магистерской программой.

| Образовательный трек | Примеры учебных задач |
|---|---|
| **Бизнес-информатика и аналитика данных** | Python, SQL, сбор и консолидация данных, аналитические выводы |
| **Педагогическое образование** | Цифровые технологии обучения, анализ образовательных данных, методические сценарии |
| **Информатика и программная инженерия** | Алгоритмы, базы данных, работа с кодом, информационный поиск |
| **Робототехника и IoT** | Телеметрия, потоки данных, обработка сигналов, учебные эксперименты |
| **Машинное обучение и интеллектуальные системы** | Подготовка данных, построение моделей, анализ качества |
| **Информационная безопасность** | Разбор технических заданий, моделирование ситуаций, оценка решений |

В рамках **первоначального грантового сценария** прорабатываются дисциплины «Python для анализа данных» и «Программные средства сбора, консолидации и аналитики данных» для направления 38.04.05 «Бизнес-информатика». Другие направления обозначают область **масштабирования архитектуры и методических сценариев**, а не перечень уже завершённых внедрений.

[Образовательные треки и дисциплины](education/03_courses.md)

## Сценарий обучения

```mermaid
%%{init: {"theme": "base", "flowchart": {"htmlLabels": true, "nodeSpacing": 38, "rankSpacing": 52}, "themeVariables": {"fontFamily": "Arial, sans-serif", "fontSize": "15px", "background": "#ffffff", "primaryTextColor": "#122B5E", "lineColor": "#7692B1"}}}%%
flowchart TB
  A(["Обучающийся"]) --> B["Знакомство<br/>с заданием"]
  B --> C["Вопрос<br/>к тьютору"]
  C --> D["Самостоятельная<br/>работа"]
  D --> E["Проверка<br/>результата"]
  E --> F["Сдача<br/>в Moodle"]
  F --> G(["Обратная связь<br/>преподавателя"])
  G -. "доработка" .-> D
  class A,G person;
  class B,F lms;
  class C agent;
  class D,E ui;
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

<sub>Редактируемая диаграмма: [`learning_journey.mmd`](assets/learning_journey.mmd)</sub>

Взаимодействие начинается с задания в образовательной среде. Обучающийся выполняет работу самостоятельно и обращается к интеллектуальному помощнику за предметными пояснениями. Результат и итоговая обратная связь остаются частью учебного процесса, организованного преподавателем.

## Архитектура

```mermaid
%%{init: {"theme": "base", "flowchart": {"htmlLabels": true, "nodeSpacing": 38, "rankSpacing": 52}, "themeVariables": {"fontFamily": "Arial, sans-serif", "fontSize": "15px", "background": "#ffffff", "primaryTextColor": "#122B5E", "lineColor": "#7692B1"}}}%%
flowchart TB
  subgraph ST["01 · Учебный контур"]
    direction LR
    MO["Moodle 5"] -.-> WS["Workspace"] --> TT["Tutor"]
  end
  subgraph MAS["02 · Оркестрация"]
    direction LR
    OR["Orchestrator"] --> AG["Агенты<br/>a1 · a2 · a3"]
    AG --> MP["MCP<br/>stdio"]
  end
  subgraph TEACH["03 · Преподаватель"]
    direction LR
    PE(["Преподаватель"]) --> LD["EduRAG<br/>Loader"]
    PE -.-> AS["Assistant<br/>backend"]
    TI["Интерфейс<br/>/teacher — план"]
  end
  subgraph CORE["Знания и модель"]
    direction LR
    DB[("PostgreSQL<br/>pgvector")]
    LL["Ollama<br/>Qwen"]
  end
  TT --> DB
  TT --> LL
  LD --> DB
  OR -. "отдельный контур" .-> TT
  class MO lms;
  class WS ui;
  class TT,OR,AG,AS,LL agent;
  class MP infra;
  class PE person;
  class LD,DB data;
  class TI future;
  style ST fill:#F4F8FF,stroke:#A9C2E8,stroke-width:1px;
  style MAS fill:#F8F5FF,stroke:#C4B1E9,stroke-width:1px;
  style TEACH fill:#F4FBF9,stroke:#A2DCCA,stroke-width:1px;
  style CORE fill:#F7F9FC,stroke:#BAC9D9,stroke-width:1px;
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

<sub>Редактируемая диаграмма: [`platform_architecture.mmd`](assets/platform_architecture.mmd)</sub>

| Контур | Основные компоненты | Назначение |
|---|---|---|
| **Учебный** | Moodle 5, Student Workspace, Tutor, RAG | Контекстная поддержка обучающегося |
| **Многоагентный** | Orchestrator, специализированные агенты, MCP | Маршрутизация запросов и подключение инструментов |
| **Преподавательский** | EduRAG Loader, Assistant backend | Управление материалами и подготовка рекомендаций |
| **Инфраструктурный** | PostgreSQL/pgvector, Ollama/Qwen | Поиск знаний и локальный вывод модели |

Студенческий Tutor и Orchestrator представляют **разные контуры взаимодействия**. Отдельный интерфейс преподавателя `/teacher` и сквозной цикл согласования оценивания относятся к развитию платформы. Не все функции агентной оркестрации следует считать встроенными в каждый запрос студенческого интерфейса.

## Программные компоненты

| Компонент | Функции | Подробнее |
|---|---|---|
| **EduTutor** | Диалог, разбор задач, анализ кода, предметный поиск | [Агент-тьютор](services/edututor/README.md) |
| **Orchestrator** | Управление специализированными агентами | [Агенты и маршрутизация](architecture/02_agents.md) |
| **EduRAG Loader** | Дисциплины, темы, материалы и операции управления базой знаний | [Сервис загрузки](services/edurag_loader/README.md) |
| **Moodle `local_edututor`** | Передача учебного контекста в интерфейс помощника | [Интеграция Moodle](services/moodle_edututor/README.md) |
| **Assistant backend** | Подготовка предварительных замечаний преподавателю | [Сценарий преподавателя](education/02_teacher_path.md) |

## Разработка и развитие

В составе EduTutor реализованы студенческий интерфейс и агент-тьютор, серверные компоненты оркестрации, предметный поиск и EduRAG Loader с управлением учебной структурой. Интеграция всех компонентов в единые сквозные образовательные процессы развивается поэтапно.

Ближайшие архитектурные направления: расширение образовательных треков, повышение прослеживаемости ответов до исходных учебных материалов, совершенствование поиска и поддержка преподавательских сценариев с обязательным участием человека в принятии решений.

Публичная документация представляет **назначение продукта, его архитектуру и направления развития**. Она не является инструкцией доступа к рабочей инфраструктуре и не содержит закрытых конфигураций.

## Документация

| Область | Разделы |
|---|---|
| Образование | [Образовательные сценарии](education/README.md) · [Путь обучающегося](education/01_student_path.md) · [Роль преподавателя](education/02_teacher_path.md) · [Треки и дисциплины](education/03_courses.md) |
| Архитектура | [Обзор](architecture/01_overview.md) · [Агенты](architecture/02_agents.md) · [RAG](architecture/03_rag.md) · [Moodle](architecture/04_moodle.md) · [Принципы безопасности](architecture/05_security.md) · [Технологии](architecture/06_technology_stack.md) |
| Компоненты | [EduTutor](services/edututor/README.md) · [EduRAG Loader](services/edurag_loader/README.md) · [Плагин Moodle](services/moodle_edututor/README.md) |
| Исследовательские и квалификационные проекты | [Развитие сервиса поиска и работы с первоисточниками](tutor-loader-vkr/README.md) |
| Диаграммы | [Редактируемые исходники Mermaid](assets/README.md) |

---

**Автор проекта:** Босенко Т. М. · Московский городской педагогический университет.

**Поддержка проекта:** Благотворительный фонд Владимира Потанина, грант ГСГК-064/26.

Документация: [лицензия](LICENSE) · [сведения для цитирования](CITATION.cff). Публикация документации не означает открытого распространения исходных кодов внутренних сервисов.
