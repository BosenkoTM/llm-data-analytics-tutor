# Архитектура системы

[← Главная](../README.md) · [Образовательный маршрут](../education/01_student_path.md)

Архитектура следует учебному процессу: **преподаватель задаёт содержание**, **Moodle предоставляет контекст занятия**, **EduTutor помогает обучающемуся**, а **человек сохраняет контроль над оцениванием**.

## Архитектура продукта

```mermaid
flowchart TB
    subgraph EDU["Образовательный уровень"]
       T["Преподаватель"]
       S["Магистрант"]
       M["Moodle 5<br/>курсы и задания"]
       S --> M
       T --> M
    end
    subgraph APP["Интеллектуальные сервисы"]
       L["EduRAG Loader<br/>подготовка материалов"]
       UI["EduTutor UI<br/>диалог и обратная связь"]
       A["LangGraph<br/>Тьютор и Ассистент"]
       MCP["MCP<br/>инструменты"]
       UI --> A --> MCP
    end
    subgraph DATA["Локальная инфраструктура"]
       KB[("PostgreSQL<br/>pgvector · база знаний")]
       E["E5-large<br/>эмбеддинги"]
       Q["Qwen / Ollama<br/>формирование ответов"]
    end
    T --> L
    M -->|"подписанный запуск"| UI
    L --> E --> KB
    MCP --> KB
    MCP --> Q
    Q --> UI
    A -. "черновик замечаний" .-> T
    classDef student fill:#E9EDFF,stroke:#5666B9,color:#23315D,stroke-width:1.6px;
    classDef teacher fill:#E2F6F2,stroke:#14827A,color:#14524F,stroke-width:1.6px;
    classDef learning fill:#E9F2FF,stroke:#3575B9,color:#173F68,stroke-width:1.6px;
    classDef moodle fill:#FFF1D8,stroke:#BC7B18,color:#664510,stroke-width:1.6px;
    classDef ai fill:#F0E9FF,stroke:#8356BD,color:#4B2B73,stroke-width:1.6px;
    classDef knowledge fill:#E4F5E9,stroke:#288456,color:#154A32,stroke-width:1.6px;
    classDef tech fill:#EDF1F5,stroke:#64768A,color:#35465C,stroke-width:1.5px;
    classDef guard fill:#FFF0EA,stroke:#C2644D,color:#713A2E,stroke-width:1.6px;
    class S student;
    class T teacher;
    class M moodle;
    class L,UI learning;
    class A,MCP,Q ai;
    class KB knowledge;
    class E tech;
```

## Границы ответственности

- **Moodle** хранит учебные задания и запускает помощника, но не генерирует ответы.
- **EduRAG Loader** принимает проверенные преподавателем материалы, публикует фрагменты и эмбеддинги.
- **EduTutor** использует найденные материалы в диалоге и для предварительной проверки работ.
- **PostgreSQL/pgvector** хранит учебный индекс; **Qwen/Ollama** формирует пояснение на основе переданного контекста.
- **Преподаватель** отвечает за критерии и итоговую оценку.

## Подробные схемы

[Агенты и интерфейсы](02_agents.md) · [База знаний и RAG](03_rag.md) · [Интеграция Moodle](04_moodle.md) · [Защита данных](05_security.md)
