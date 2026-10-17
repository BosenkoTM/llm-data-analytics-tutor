# Агенты и интерфейсы EduTutor

[← Архитектура](01_overview.md) · [Сервис EduTutor](../services/edututor/README.md)

В продукте разделены **два образовательных сценария**: сопровождение студента и помощь преподавателю в проверке. Для организации их действий используется LangGraph; интерфейс построен на технологиях CopilotKit / AG-UI, инструменты подключаются через MCP.

## Два сценария использования

```mermaid
flowchart TB
    S["Магистрант"] --> U["EduTutor UI"]
    U --> T["Агент-Тьютор"]
    T --> R["Найти материалы<br/>нужной дисциплины"]
    R --> H["Объяснение и подсказки<br/>по самостоятельной работе"]
    P["Преподаватель"] --> A["Агент-Ассистент"]
    X["Представленная работа<br/>и критерии"] --> A
    A --> C["Предварительные замечания"]
    C --> P
    P --> F["Окончательная оценка"]
    classDef student fill:#E9EDFF,stroke:#5666B9,color:#23315D,stroke-width:1.6px;
    classDef teacher fill:#E2F6F2,stroke:#14827A,color:#14524F,stroke-width:1.6px;
    classDef learning fill:#E9F2FF,stroke:#3575B9,color:#173F68,stroke-width:1.6px;
    classDef moodle fill:#FFF1D8,stroke:#BC7B18,color:#664510,stroke-width:1.6px;
    classDef ai fill:#F0E9FF,stroke:#8356BD,color:#4B2B73,stroke-width:1.6px;
    classDef knowledge fill:#E4F5E9,stroke:#288456,color:#154A32,stroke-width:1.6px;
    classDef tech fill:#EDF1F5,stroke:#64768A,color:#35465C,stroke-width:1.5px;
    classDef guard fill:#FFF0EA,stroke:#C2644D,color:#713A2E,stroke-width:1.6px;
    class S student;
    class U,R learning;
    class T,A ai;
    class H knowledge;
    class P,F teacher;
    class X tech;
    class C moodle;
```

### Агент-Тьютор

Объясняет термины и задания, помогает находить причины ошибок и понимать результаты анализа. Подсказки должны соответствовать учебным материалам, доступным студенту; закрытые эталоны решений в ответах недопустимы.

### Агент-Ассистент

Готовит преподавателю замечания по работе и критериям. Это **предварительный разбор**, а не автономное выставление оценок. Преподаватель проверяет и при необходимости корректирует результат.

### Технологический слой

**LangGraph** организует последовательность действий агентов, **MCP** обеспечивает доступ к разрешённым инструментам (поиск знаний, проверка кода, журнал действий), а **Qwen через Ollama** формирует текстовые пояснения. Состав доступных инструментов зависит от рабочего контура и прав пользователя.

[Как материалы попадают в ответы →](03_rag.md)
