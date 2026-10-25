# Сценарий обучающегося

[← Образовательные сценарии](README.md) · [Tutor](../services/edututor/README.md)

EduTutor сопровождает обучающегося во время выполнения задания: помогает разобраться в постановке задачи, подобрать метод решения, найти ошибку и объяснить полученный результат. Сценарий применим к различным образовательным программам и уровням подготовки.

## Маршрут работы

```mermaid
%%{init: {"theme": "base", "flowchart": {"htmlLabels": true, "nodeSpacing": 38, "rankSpacing": 52}, "themeVariables": {"fontFamily": "Arial, sans-serif", "fontSize": "15px", "background": "#ffffff", "primaryTextColor": "#122B5E", "lineColor": "#7692B1"}}}%%
flowchart TB
  A(["Обучающийся"]) --> B["Знакомство<br/>с заданием"]
  B --> C["Вопрос<br/>к тьютору"]
  C --> D["Самостоятельная<br/>работа"]
  D --> E["Проверка<br/>результата"]
  E --> F["Сдача<br/>в Moodle"]
  F --> G(["Обратная связь<br/>преподавателя"])
  G -. "доработка" .-> D
  class A,G person;
  class B,F lms;
  class C agent;
  class D,E ui;
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

<sub>Редактируемая диаграмма: [`learning_journey.mmd`](../assets/learning_journey.mmd)</sub>

| Этап | Действия обучающегося | Роль EduTutor |
|---|---|---|
| Постановка задачи | Изучает условие и критерии | Уточняет термины и ограничения |
| Планирование | Выбирает способ решения | Объясняет методы и возможные шаги |
| Выполнение | Работает с данными или кодом | Помогает разбирать ошибки и понятия |
| Анализ | Проверяет выводы и оформление | Даёт пояснения с опорой на доступный контекст |
| Завершение | Передаёт результат преподавателю | Не подменяет итоговую оценку |

## Примеры по образовательным трекам

В бизнес-информатике это может быть анализ наборов данных, в педагогическом образовании — изучение цифровых учебных практик, в робототехнике — обработка телеметрии, в программировании — разбор алгоритма, в информационной безопасности — анализ учебного технического случая.

**Примеры иллюстрируют масштабируемость сценария**, а не утверждают, что каждый из них уже внедрён в учебный процесс.

[Образовательные треки](03_courses.md) · [Интеграция с Moodle](../architecture/04_moodle.md)
