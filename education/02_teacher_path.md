# Сценарий преподавателя

[← Образовательные сценарии](README.md) · [EduRAG Loader](../services/edurag_loader/README.md)

Преподаватель задаёт учебную структуру дисциплины, готовит материалы и критерии оценивания. EduTutor предоставляет инструменты для организации базы знаний и предусматривает интеллектуальную поддержку подготовки обратной связи.

## Организация учебного процесса

```mermaid
%%{init: {"theme": "base", "flowchart": {"htmlLabels": true, "nodeSpacing": 38, "rankSpacing": 52}, "themeVariables": {"fontFamily": "Arial, sans-serif", "fontSize": "15px", "background": "#ffffff", "primaryTextColor": "#122B5E", "lineColor": "#7692B1"}}}%%
flowchart TB
  P(["Преподаватель"]) --> LO["EduRAG<br/>Loader"]
  LO --> C["Дисциплины<br/>и темы"]
  C --> D["Материалы<br/>и документы"]
  D --> KB[("База<br/>знаний")]
  S(["Работа<br/>обучающегося"]) -.-> AS["Assistant<br/>backend"]
  AS --> R["Черновые<br/>замечания"]
  R -.-> H{"Решение<br/>преподавателя"}
  H --> FB["Обратная<br/>связь"]
  TI["/teacher<br/>в разработке"] -.-> AS
  class P,S person;
  class LO,C,D,KB data;
  class AS,R agent;
  class H guard;
  class FB lms;
  class TI future;
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

<sub>Редактируемая диаграмма: [`teacher_workflow.mmd`](../assets/teacher_workflow.mmd)</sub>

| Этап | Роль преподавателя | Поддержка системы |
|---|---|---|
| Подготовка дисциплины | Формирует темы и учебные активности | EduRAG Loader организует каталог |
| Публикация материалов | Загружает документы и задания | Обработка материалов для предметного поиска |
| Сопровождение | Анализирует типичные затруднения | Tutor поясняет учебные вопросы |
| Проверка работ | Оценивает результат по критериям | Assistant backend может подготовить черновые замечания |
| Итоговое решение | Утверждает обратную связь и оценку | Moodle остаётся системой учёта результатов |

Существование Assistant backend не означает завершения отдельного интерфейса преподавателя или автоматического присвоения оценок. **Human-in-the-loop — архитектурный принцип сохранения человеческого решения**, который развивается последовательно.

[Схема ответственности](../architecture/05_security.md) · [Структура базы знаний](../architecture/03_rag.md)
