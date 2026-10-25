# Интеграция с Moodle 5

[← Архитектура](01_overview.md) · [Учебный маршрут](../education/01_student_path.md)

Moodle остаётся базовой системой организации учебного процесса: в LMS размещены дисциплины, задания, сроки, отправленные работы и оценки. EduTutor подключается как **интеллектуальное сопровождение**, не заменяя эти функции.

**Цветная схема связи Moodle и EduTutor**

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

<sub>Исходник схемы: [`moodle_context.mmd`](../assets/moodle_context.mmd)</sub>

## Путь пользователя

1. Магистрант открывает учебное задание в Moodle.
2. Плагин `local_edututor` предусматривает передачу подписанного контекста курса и задания.
3. Student Workspace показывает интерфейс диалога с Тьютором.
4. Tutor помогает разобраться в условии и материалах дисциплины.
5. Работа направляется на оценивание средствами Moodle и преподавателя.

## Контекст обучения

Особенно важны устойчивые идентификаторы **курса, учебной активности и задания**. Они позволяют связывать вопрос студента с подходящими материалами RAG. Уточнение индивидуального варианта и исходного документа является направлением дальнейшей интеграции.

Плагин реализует механизм подписанного запуска, однако это не следует трактовать как утверждение, что **любой пользовательский маршрут во всех окружениях уже полностью интегрирован**.

## Разделение ответственности

| Moodle | EduTutor |
|---|---|
| Выдача и сдача задания | Предметная помощь при его выполнении |
| Учёт обучающихся и работ | Использование контекста курса |
| Итоговые оценки преподавателя | Пояснения, черновые замечания, поиск материалов |

Автоматическая публикация оценок из Агента-Ассистента в журнал Moodle здесь не заявляется.

[Плагин Moodle](../services/moodle_edututor/README.md) · [Контроль информации](05_security.md)
