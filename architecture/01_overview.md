# 01. Обзор архитектуры

## Назначение системы

`potanin_assistant` — мультиагентный LLM-ассистент для интеллектуальной поддержки
обучения анализу данных в магистратуре МГПУ (направление 38.04.05 «Бизнес-информатика»,
дисциплины «Python для анализа данных» и «Программные средства сбора, консолидации и
аналитики данных»). Система переводит практикумы от схемы «задание → выполнение → ручная
проверка» к адаптивной среде с интеллектуальной поддержкой в реальном времени для двух
категорий пользователей — студентов и преподавателей.

> Настоящий документ описывает архитектуру на уровне компонентов и контрактов.
> Исходный код бизнес-логики, системные промпты-ядра, датасеты и веса моделей не публикуются.

## Две роли агентов

Система строится вокруг двух интеллектуальных агентов поверх общего слоя инструментов и
локальной языковой модели.

- **Агент-Тьютор** — для студента. Объясняет материал по верифицированной базе знаний курса,
  разбирает ошибки, в «режиме агента» выполняет пре-валидацию присланного студентом кода
  (Python/SQL) в изолированной песочнице. **Оценок не выставляет**; помогает наводящими
  вопросами, не выдавая готовое решение целиком (философия «duck debugging»).
- **Агент-Ассистент** — для преподавателя. Принимает присланные работы, прогоняет их в
  песочнице и формирует **предварительную** оценку по рубрике с пояснением по критериям.

## Принцип Human-in-the-loop

Финальная оценка всегда остаётся за преподавателем. Human-in-the-loop реализован как
**управляемое состояние графа** (а не как текстовая инструкция в промпте): предварительная
оценка фиксируется в статусе «черновик» (`draft`), граф Ассистента приостанавливается и ждёт
явного решения преподавателя, и только после подтверждения оценка переходит в статус
`approved`. Это гарантирует, что автоматическая эвристика не может самостоятельно выставить
итоговый балл.

## Ключевой стек

| Слой | Технология (обобщённо) |
|---|---|
| Оркестрация агентов | LangGraph (графы состояний) |
| Взаимодействие «агент ↔ инструменты/данные» | Model Context Protocol (MCP), JSON-RPC 2.0 |
| База знаний / RAG | pgvector поверх PostgreSQL |
| Языковая модель | локальная Qwen через Ollama (приватность внутри контура вуза), облачный API — резерв |
| Веб-интерфейс | CopilotKit / AG-UI (чат, генеративный UI, HITL) |
| Бэкенды | FastAPI |
| Журнал верифицируемой оркестрации | `λ(d)` (`lambda_trace_log`) |

## Контекстная диаграмма

```mermaid
flowchart TD
    Student(["Студент<br/>(магистратура)"])
    Teacher(["Преподаватель"])

    subgraph SYS["LLM-ассистент potanin_assistant"]
        direction TB
        UI["Веб-интерфейс<br/>CopilotKit / AG-UI"]
        Backends["Бэкенды-агенты<br/>Тьютор + Ассистент (FastAPI)"]
        MCPLayer["Слой MCP<br/>tools / resources / prompts"]
        UI --> Backends --> MCPLayer
    end

    RAG[("RAG-база знаний<br/>pgvector / PostgreSQL")]
    LLM["Локальная LLM<br/>Qwen через Ollama"]
    Trace[("Журнал оркестрации<br/>λ(d) · lambda_trace_log")]

    Student -->|вопросы, код| UI
    Teacher -->|проверка работ, HITL| UI

    MCPLayer -->|retrieval| RAG
    MCPLayer -->|инференс| LLM
    Backends -.->|write_trace| Trace

    classDef actor fill:#EEEDFE,stroke:#534AB7,color:#26215C;
    classDef store fill:#E6F1FB,stroke:#185FA5,color:#042C53;
    classDef infra fill:#F1EFE8,stroke:#5F5E5A,color:#2C2C2A;
    class Student,Teacher actor;
    class RAG,Trace store;
    class LLM infra;
```

## Слоевая структура (компоненты)

Ключевая идея архитектуры: **CopilotKit / AG-UI стоит _над_ MCP-агентами, а не вместо них**.
MCP — это слой «агент ↔ инструменты/данные» (ядро); AG-UI — слой «агент ↔ интерфейс»,
который лишь отображает результат работы MCP-агентов.

```mermaid
flowchart TB
    UI["copilot_ui<br/>Next.js + CopilotKit · чат, генеративный UI, HITL"]

    subgraph AGENTS["Агенты-оркестраторы (LangGraph, MCP-клиенты)"]
        direction LR
        TUT["agent_tutor<br/>граф Тьютора"]
        ASS["agent_assistant<br/>граф Ассистента"]
    end

    subgraph MCP["Слой MCP (JSON-RPC 2.0)"]
        direction TB
        TOOLS["tools<br/>validate_code · generate_task · grade_submission · write_trace"]
        RES["resources<br/>materials · rpd · rubric"]
        PR["prompts<br/>tutor_system · assistant_system"]
        SBX["sandbox<br/>изоляция исполнения Python/SQL"]
    end

    SHARED["shared<br/>конфиг · схема БД · префлайт · RAG-утилиты"]
    RAGDB[("RAG-хранилище<br/>pgvector / PostgreSQL")]
    LLM["Локальная LLM<br/>Qwen / Ollama"]

    UI -->|AG-UI| TUT
    UI -->|AG-UI| ASS
    TUT -->|MCP| MCP
    ASS -->|MCP| MCP
    TOOLS --> SBX
    RES --> RAGDB
    TUT --> LLM
    ASS --> LLM
    AGENTS -.-> SHARED
    MCP -.-> SHARED

    classDef ui   fill:#EEEDFE,stroke:#534AB7,color:#26215C;
    classDef ag   fill:#E1F5EE,stroke:#0F6E56,color:#04342C;
    classDef mcp  fill:#FAEEDA,stroke:#854F0B,color:#412402;
    classDef infra fill:#F1EFE8,stroke:#5F5E5A,color:#2C2C2A;
    class UI ui;
    class TUT,ASS ag;
    class TOOLS,RES,PR,SBX mcp;
    class SHARED,RAGDB,LLM infra;
```

Подробное описание модулей — в [02_components.md](02_components.md).
