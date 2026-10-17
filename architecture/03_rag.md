# База знаний и поиск RAG

[← Архитектура](01_overview.md) · [EduRAG Loader](../services/edurag_loader/README.md)

EduRAG Loader создаёт базу знаний на основе материалов преподавателя. EduTutor извлекает относящиеся к вопросу фрагменты и использует их как контекст для локальной языковой модели.

## Подготовка материалов

```mermaid
flowchart LR
    A["Дисциплина<br/>тема · занятие"] --> B["Загрузка<br/>HTML · Notebook · документы"]
    B --> C["Извлечение<br/>и разбиение"]
    C --> D["Предпросмотр<br/>преподавателем"]
    D --> E["E5-large<br/>эмбеддинги"]
    E --> F[("PostgreSQL + pgvector<br/>фрагменты · связи · версии")]
    classDef student fill:#E9EDFF,stroke:#5666B9,color:#23315D,stroke-width:1.6px;
    classDef teacher fill:#E2F6F2,stroke:#14827A,color:#14524F,stroke-width:1.6px;
    classDef learning fill:#E9F2FF,stroke:#3575B9,color:#173F68,stroke-width:1.6px;
    classDef moodle fill:#FFF1D8,stroke:#BC7B18,color:#664510,stroke-width:1.6px;
    classDef ai fill:#F0E9FF,stroke:#8356BD,color:#4B2B73,stroke-width:1.6px;
    classDef knowledge fill:#E4F5E9,stroke:#288456,color:#154A32,stroke-width:1.6px;
    classDef tech fill:#EDF1F5,stroke:#64768A,color:#35465C,stroke-width:1.5px;
    classDef guard fill:#FFF0EA,stroke:#C2644D,color:#713A2E,stroke-width:1.6px;
    class A learning;
    class B,C tech;
    class D teacher;
    class E ai;
    class F knowledge;
```

Для хранения контекста используется ключ дисциплины `rag_key`. Учитываются учебная иерархия, принадлежность фрагментов документам и активности, а также связи дочерних фрагментов с исходными разделами (Parent–Child).

## Получение контекста для ответа

```mermaid
flowchart TB
    Q["Вопрос студента"] --> X["Выбор дисциплины<br/>по учебному контексту"]
    X --> S["Поиск релевантных фрагментов"]
    S --> C["Отбор доступных источников<br/>и восстановление контекста"]
    C --> M["Qwen / Ollama<br/>формирование ответа"]
    M --> A["Пояснение студенту"]
    classDef student fill:#E9EDFF,stroke:#5666B9,color:#23315D,stroke-width:1.6px;
    classDef teacher fill:#E2F6F2,stroke:#14827A,color:#14524F,stroke-width:1.6px;
    classDef learning fill:#E9F2FF,stroke:#3575B9,color:#173F68,stroke-width:1.6px;
    classDef moodle fill:#FFF1D8,stroke:#BC7B18,color:#664510,stroke-width:1.6px;
    classDef ai fill:#F0E9FF,stroke:#8356BD,color:#4B2B73,stroke-width:1.6px;
    classDef knowledge fill:#E4F5E9,stroke:#288456,color:#154A32,stroke-width:1.6px;
    classDef tech fill:#EDF1F5,stroke:#64768A,color:#35465C,stroke-width:1.5px;
    classDef guard fill:#FFF0EA,stroke:#C2644D,color:#713A2E,stroke-width:1.6px;
    class Q student;
    class X,S learning;
    class C knowledge;
    class M ai;
    class A moodle;
```

### Поиск в EduRAG Loader и EduTutor

В диагностическом поиске Loader реализован `hybrid_v1`: предварительный векторный поиск, после которого кандидаты ранжируются по семантическому, лексическому и структурному соответствию. **Это функция Loader**, а не подтверждённая функция действующего EduTutor.

В EduTutor ранее применялся отдельный семантический поиск. Совместное использование `hybrid_v1`, точная привязка задания и отображение источников в ответе — следующие задачи интеграции.

**Ограничение:** релевантность фрагмента сама по себе ещё не гарантирует полноту или безошибочность итогового ответа. Поддержка должна опираться на доступный студенту материал и не раскрывать преподавательские решения.
