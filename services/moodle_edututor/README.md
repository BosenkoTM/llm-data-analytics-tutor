# Moodle `local_edututor` — интеграция учебного задания

[← EduTutor](../../README.md) · [Архитектура интеграции](../../architecture/04_moodle.md)

Интеграционный компонент `local_edututor` предназначен для перехода из Moodle 5 к интеллектуальному помощнику с учётом контекста курса и учебного задания. Moodle остаётся основной системой организации учебной активности и фиксации результатов.

## Схема взаимодействия

```mermaid
%%{init: {"theme": "base", "flowchart": {"htmlLabels": true, "nodeSpacing": 38, "rankSpacing": 52}, "themeVariables": {"fontFamily": "Arial, sans-serif", "fontSize": "15px", "background": "#ffffff", "primaryTextColor": "#122B5E", "lineColor": "#7692B1"}}}%%
flowchart TB
  S(["Обучающийся"]) --> M["Moodle 5<br/>курс и задание"]
  M --> PL["Плагин<br/>local_edututor"]
  PL -. "контекст" .-> WS["Student<br/>Workspace"]
  WS --> T["Tutor"]
  T --> HELP["Пояснения<br/>к заданию"]
  S --> SUB["Сдача<br/>работы"]
  SUB --> M
  M --> P(["Оценивание<br/>преподавателем"])
  class S,P person;
  class M,PL,SUB lms;
  class WS,HELP ui;
  class T agent;
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

<sub>Редактируемая диаграмма: [`moodle_context.mmd`](../../assets/moodle_context.mmd)</sub>

## Назначение

- Передача необходимых сведений о курсе и задании в интерфейс EduTutor.
- Использование подписанного контекста для обращения к интеллектуальному помощнику.
- Сохранение процессов сдачи работ и итогового оценивания в Moodle.
- Подготовка к более точному сопоставлению учебных материалов, индивидуальных вариантов и исходных документов.

Функциональность преподавательского интерфейса и автоматической публикации оценок не относится к подтверждённым возможностям текущего плагина. Сквозная интеграция разворачивается последовательно и зависит от конфигурации образовательной среды.

## Технологии

[![Moodle 5](https://img.shields.io/badge/Moodle%205-F08A24?style=for-the-badge&logo=moodle&logoColor=white)](https://moodle.org/) [![Next.js](https://img.shields.io/badge/Next.js-111827?style=for-the-badge&logo=nextdotjs&logoColor=white)](https://nextjs.org/) [![AG-UI](https://img.shields.io/badge/AG-UI-0068D2?style=for-the-badge)](https://docs.ag-ui.com/)

[Образовательные сценарии](../../education/README.md) · [Ответственность компонентов](../../architecture/05_security.md)
