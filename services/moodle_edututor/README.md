# Moodle `local_edututor` — связь задания и помощника

[← На главную](../../README.md) · [Интеграция Moodle](../../architecture/04_moodle.md)

Плагин **`local_edututor`** предназначен для перехода из Moodle 5 к EduTutor в контексте конкретного курса и учебного задания.

**Цветная схема интеграции Moodle**

```mermaid
flowchart LR
    S(["Магистрант"]) --> M["Moodle 5<br/>курс и задание"]
    M --> PL["Плагин<br/>local_edututor"]
    PL -. "подписанный контекст" .-> W["Student Workspace"]
    W --> T["Агент-Тьютор"]
    T --> C["Пояснения<br/>по учебному контексту"]
    S --> SUB["Сдача работы<br/>в Moodle"]
    SUB --> P(["Преподаватель<br/>выставляет оценку"])
    class S,P person;
    class M,PL,SUB lms;
    class W,C ui;
    class T agent;

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

<sub>Исходник схемы: [`moodle_context.mmd`](../../assets/moodle_context.mmd)</sub>

## Что делает интеграция

- Предусматривает подписанный контекст запуска для связи LMS с интерфейсом помощника.
- Позволяет учитывать учебный курс и задание при обращении к EduTutor.
- Сохраняет Moodle основным инструментом выдачи, сдачи и итогового оценивания работ.

## Что остаётся в развитии

Более точное сопоставление индивидуального варианта, версии исходного материала и учебной активности требует согласованной работы Moodle, Tutor и EduRAG Loader. Полный преподавательский интерфейс и автоматическая публикация оценки не относятся к действующим возможностям плагина.

[Образовательный маршрут](../../education/01_student_path.md) · [Архитектура](../../architecture/01_overview.md)
