# Архитектура платформы

[← EduTutor](../README.md) · [Технологии](06_technology_stack.md) · [Агенты](02_agents.md)

EduTutor объединяет образовательную среду, интерфейсы взаимодействия, агентные сервисы и предметную базу знаний. Каждый уровень имеет собственную область ответственности; развитие платформы предусматривает независимое расширение программных компонентов и учебных сценариев.

## Основные контуры

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

<sub>Редактируемая диаграмма: [`platform_architecture.mmd`](../assets/platform_architecture.mmd)</sub>

### Учебный контур

**Moodle 5 → Student Workspace → Агент-тьютор → предметный RAG / LLM.** Обучающийся получает пояснения в контексте дисциплины, выполняет задания и передаёт результат через принятую в образовательной организации систему обучения. Преподаватель оценивает результат.

### Контур агентной оркестрации

**Orchestrator → специализированные агенты → инструменты MCP.** Маршрутизатор определяет способ обработки запроса с учётом компетенций и предусмотренных управляющих переходов. Этот исследовательский контур архитектурно отделён от штатного запроса Tutor.

### Контур преподавателя

**EduRAG Loader → база знаний** обеспечивает управление структурой учебных материалов. **Assistant backend** ориентирован на подготовку предварительной обратной связи. Полноценный интерфейс `/teacher` и завершённый процесс принятия решений преподавателем развиваются отдельно.

## Технологические уровни

| Уровень | Решения | Назначение |
|---|---|---|
| Образовательная среда | Moodle 5, учебные активности | Задания, взаимодействие и оценивание |
| Интерфейсы | Next.js, CopilotKit, AG-UI | Учебный диалог и взаимодействие с сервисами |
| Агентная логика | Python, FastAPI, LangGraph, MCP | Обработка запросов и координация инструментов |
| Учебные материалы | EduRAG Loader, Flask | Каталог и управление содержанием дисциплин |
| Поиск и хранилище | PostgreSQL, pgvector, RAG | Извлечение контекста для ответа |
| Генеративная модель | Ollama, Qwen; модель эмбеддингов E5 | Локальное формирование ответа и представлений текста |

## Архитектурные принципы

- **Модульность:** образовательный контур и исследовательская оркестрация развиваются независимо.
- **Контекстность:** содержательное основание ответа задают материалы курса и учебное задание.
- **Прослеживаемость:** развитие системы ориентировано на связь найденного фрагмента с первоисточником.
- **Контроль преподавателя:** интеллектуальная рекомендация не заменяет итоговое оценивание.
- **Расширяемость:** образовательный профиль задаётся материалами, заданиями и правилами работы, а не жёстко зашивается в архитектуру.

[Агенты](02_agents.md) · [База знаний](03_rag.md) · [Границы ответственности](05_security.md)
