# Интеллектуальные агенты и оркестрация

[← Архитектура](01_overview.md) · [EduTutor](../services/edututor/README.md)

Агентная архитектура EduTutor предусматривает распределение задач между специализированными компонентами. **Агент-тьютор** ориентирован на взаимодействие с обучающимся; **Агент-ассистент** — на поддержку преподавателя; **Orchestrator** — на управление маршрутизацией в отдельном многоагентном контуре.

## Логика маршрутизации

```mermaid
%%{init: {"theme": "base", "flowchart": {"htmlLabels": true, "nodeSpacing": 38, "rankSpacing": 52}, "themeVariables": {"fontFamily": "Arial, sans-serif", "fontSize": "15px", "background": "#ffffff", "primaryTextColor": "#122B5E", "lineColor": "#7692B1"}}}%%
flowchart TB
  Q(["Запрос и<br/>контекст"]) --> OR["Orchestrator"]
  OR --> ROUTE{"Выбор<br/>маршрута"}
  subgraph AGENTS["Агенты"]
    direction LR
    T["a1 · Tutor"]
    V["a2 · Проверка<br/>кода"]
    P["a3 · План<br/>обучения"]
  end
  ROUTE -->|a1| T
  ROUTE -->|a2| V
  ROUTE -->|a3| P
  ROUTE -.-> CL["Уточнение<br/>контекста"]
  ROUTE -.-> SF["Безопасный<br/>отказ"]
  V --> MCP["Инструмент<br/>MCP"]
  T --> OUT["Ответ и<br/>трассировка"]
  MCP --> OUT
  P --> OUT
  CL --> OUT
  SF --> OUT
  class Q person;
  class OR,ROUTE,T,V,P,OUT agent;
  class MCP infra;
  class CL ui;
  class SF guard;
  style AGENTS fill:#F8F5FE,stroke:#C4B1E9,stroke-width:1px;
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

<sub>Редактируемая диаграмма: [`agent_orchestration.mmd`](../assets/agent_orchestration.mmd)</sub>

| Компонент | Область применения |
|---|---|
| **a1 — Tutor** | Предметные пояснения и поддержка учебных действий |
| **a2 — проверка кода** | Валидация учебных решений средствами инструментов MCP |
| **a3 — планирование** | Построение последовательности действий; подключение РПД отложено |
| **Уточнение и безопасный отказ** | Обработка недостаточного контекста и ограничений маршрута |

## Научная концепция

Исследовательская модель использует граф состояний **M**, функцию выбора маршрута **τ**, каталог компетенций **Θ**, переходы **δ** и трассировку действий **λ**. Эти понятия описывают способ организации и анализа управляемой агентной системы; наличие соответствующих компонентов не равнозначно завершённой проверке всех свойств модели.

## Роль преподавателя

Инструменты могут формировать предварительные замечания, предложения по доработке и объяснения. **Утверждение оценки остаётся за преподавателем**. Отдельный пользовательский интерфейс преподавателя и сквозной Human-in-the-loop workflow являются направлениями развития.

## Технологическая основа

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/) [![LangGraph](https://img.shields.io/badge/LangGraph-635BCE?style=for-the-badge)](https://www.langchain.com/langgraph) [![MCP](https://img.shields.io/badge/MCP-6750A4?style=for-the-badge)](https://modelcontextprotocol.io/) [![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/) [![Ollama](https://img.shields.io/badge/Ollama-33465E?style=for-the-badge)](https://ollama.com/)

[Технологический стек](06_technology_stack.md) · [Поиск по учебным материалам](03_rag.md)
