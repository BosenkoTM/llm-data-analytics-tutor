# EduTutor — интеллектуальный помощник

[← На главную](../../README.md) · [Общая архитектура](../../architecture/01_overview.md)

**EduTutor** помогает магистранту изучать анализ данных на практике. Его основа — Student Workspace, Агент-Тьютор, поиск RAG и локальная языковая модель.

**Цветная архитектура EduTutor**

```mermaid
flowchart LR
    S(["Магистрант"]) --> M["Задание в Moodle 5"]
    M -. "контекст задания" .-> W["Student Workspace"]
    W --> T["Агент-Тьютор"]
    T --> R["Предметный RAG"]
    T --> O["Локальная LLM"]
    R --> K[("База знаний<br/>PostgreSQL / pgvector")]
    P(["Преподаватель"]) --> L["EduRAG Loader"]
    L --> K
    P -. "контроль итоговой оценки" .-> M
    class S,P person;
    class M lms;
    class W ui;
    class T,O agent;
    class R,L,K data;

classDef person fill:#E5ECFD,stroke:#4164B9,color:#142C59,stroke-width:2px;
classDef lms fill:#FFF1D6,stroke:#D28A2B,color:#66440C,stroke-width:1.8px;
classDef ui fill:#E4F1FE,stroke:#2774C7,color:#153D6A,stroke-width:1.8px;
classDef agent fill:#EDE7FD,stroke:#7960C2,color:#3D2D73,stroke-width:1.8px;
classDef data fill:#E1F5EF,stroke:#268A6D,color:#155C4B,stroke-width:1.8px;
classDef infra fill:#EBEFF5,stroke:#6F8198,color:#35445A,stroke-width:1.8px;
classDef guard fill:#FFEBE8,stroke:#C76650,color:#7B3429,stroke-width:1.8px;
classDef future fill:#F4F2FB,stroke:#9D95BD,color:#55506E,stroke-width:1.5px,stroke-dasharray:5 4;
linkStyle default stroke:#63809B,stroke-width:1.8px;
```

<sub>Исходник схемы: [`edututor_overview.mmd`](../../assets/edututor_overview.mmd)</sub>

## Агент-Тьютор: сопровождение студента

- Разъяснение формулировок учебного задания и технических терминов.
- Пояснение методов обработки, анализа и консолидации данных.
- Помощь в чтении Python-кода и поиске причин ошибок.
- Использование учебного материала дисциплины для предметного контекста.
- Поддержка интерпретации результатов без подмены самостоятельной работы.

## Интеллектуальные компоненты

| Компонент | Назначение |
|---|---|
| **Workspace** | Пользовательский интерфейс и учебный диалог |
| **Tutor** | Объяснения и предметный поиск |
| **Orchestrator** | Маршрутизация задач исследовательского многоагентного контура |
| **Assistant backend** | Черновые замечания по работе для преподавателя |
| **MCP** | Доступ к специализированным инструментам |
| **Ollama / Qwen** | Генерация ответов локальной языковой моделью |

## Актуальное направление развития

Приоритетом остаётся согласование задания Moodle, учебной активности EduRAG Loader и найденного первоисточника. Преподавательский интерфейс и полный цикл Human-in-the-loop рассматриваются отдельно от действующего студенческого сервиса.

**Итоговую оценку выставляет преподаватель.**

[Агенты](../../architecture/02_agents.md) · [RAG](../../architecture/03_rag.md) · [Студент](../../education/01_student_path.md)
