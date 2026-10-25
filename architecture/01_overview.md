# Архитектура EduTutor

[← На главную](../README.md) · [Агенты](02_agents.md) · [RAG](03_rag.md)

EduTutor соединяет **образовательный процесс**, **предметный информационный контекст** и **интеллектуальные сервисы**. Архитектура организована так, чтобы преподаватель отвечал за содержание курса, LMS — за учебные активности, а помощник — за сопровождение работы с данными.

**Архитектура трёх контуров**

```mermaid
flowchart TB
    subgraph ST["STUDENT · Учебный контур"]
        direction LR
        M["Moodle 5"] -. "контекст задания" .-> W["Workspace"]
        W --> T["Tutor"]
    end
    subgraph ML["MAS · Контур оркестрации"]
        direction LR
        OR["Orchestrator"] --> A1["a1 · Tutor"]
        OR --> A2["a2 · Валидация"]
        OR --> A3["a3 · Планирование"]
        A2 --> MCP["Инструменты MCP<br/>stdio"]
    end
    subgraph TR["TEACHER · Контур преподавателя"]
        direction LR
        P["Преподаватель"] --> L["EduRAG Loader"]
        P -. "запрос на анализ" .-> AS["Assistant backend"]
        AS -. "утверждение человеком" .-> P
        FUT["Интерфейс /teacher<br/>разрабатывается"]
    end
    subgraph DS["Данные и модель"]
        direction LR
        KB[("Учебная база знаний<br/>PostgreSQL / pgvector")]
        OLL["Qwen / Ollama"]
    end
    T --> KB
    T --> OLL
    L --> KB
    A1 --> KB
    A3 -. "контент РПД отложен" .-> FUT
    class M lms;
    class W ui;
    class T,OR,A1,A2,A3,AS agent;
    class MCP,OLL infra;
    class P person;
    class L,KB data;
    class FUT future;
style ST fill:#F4F8FF,stroke:#A2BCEE,stroke-width:1px;
style ML fill:#F7F4FF,stroke:#B9A4E7,stroke-width:1px;
style TR fill:#F3FAF8,stroke:#A2D8C8,stroke-width:1px;
style DS fill:#F6F8FA,stroke:#BBC7D3,stroke-width:1px;
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

<sub>Исходник схемы: [`platform_architecture.mmd`](../assets/platform_architecture.mmd)</sub>

## Три контура

### 01 · STUDENT — обучение

`Moodle 5 → Student Workspace → Агент-Тьютор → RAG / Ollama`

Магистрант открывает задание в Moodle, задаёт вопросы и получает пояснения по предмету, условиям работы и ошибкам кода. Tutor использует локальную языковую модель и доступный RAG-контекст.

### 02 · MAS — интеллектуальная оркестрация

`Orchestrator → специализированные агенты → MCP-инструменты`

Контроллер распределяет запросы по компетенциям и маршрутам, а специализированные агенты выполняют отдельные задачи. Технологии LangGraph и MCP используются для исследования управляемости и проверяемости многоагентных процессов. Это **отдельный контур**, который не следует отождествлять со всеми запросами студенческого Tutor.

### 03 · TEACHER — подготовка содержания и обратная связь

`Преподаватель → EduRAG Loader → база знаний`  
`Работа студента → Assistant backend → предварительные замечания → преподаватель`

EduRAG Loader организует учебный контент. Backend Агента-Ассистента предназначен для предварительного анализа, но отдельный интерфейс `/teacher` и полноценный цикл утверждения оценки относятся к дальнейшему развитию.

## Основные технологические компоненты

| Уровень | Компоненты | Роль |
|---|---|---|
| Образовательный | Moodle 5, учебные задания | Курсы, варианты, сдача работы |
| Представление | Next.js, CopilotKit / AG-UI | Пользовательский диалог |
| Агентный | Tutor, Orchestrator, Assistant, LangGraph | Пояснения, маршрутизация, черновая обратная связь |
| Инструментальный | MCP stdio | Вызов специализированных функций |
| Информационный | EduRAG Loader, PostgreSQL, pgvector | Подготовка и поиск материала |
| Генеративный | Ollama, Qwen | Формирование предметных пояснений |

## Ответственность компонентов

**Moodle** остаётся системой учебных заданий. **EduRAG Loader** организует содержание дисциплин. **Tutor** сопровождает студента. **Orchestrator** исследует управляемую координацию агентов. **Преподаватель** утверждает образовательные решения.

[Агенты и управление](02_agents.md) · [Поиск материалов](03_rag.md) · [Moodle](04_moodle.md) · [Ответственность и данные](05_security.md)
