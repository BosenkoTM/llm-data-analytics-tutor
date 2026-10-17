# Доступ к учебной информации и контроль качества

[← Архитектура](01_overview.md) · [Образовательный маршрут](../education/01_student_path.md)

При работе со студенческими заданиями необходимо разделять **доступные материалы**, **закрытые преподавательские эталоны** и **данные обучающихся**. Подпись запуска Moodle подтверждает происхождение контекста, но не заменяет проверку прав на конкретные документы.

## Границы доступа

```mermaid
flowchart TB
    M["Подписанный запуск<br/>Moodle"] --> V["Проверка пользователя<br/>и его роли"]
    V --> P{"Разрешён доступ<br/>к материалу?"}
    P -- "Да" --> K["Фрагменты учебного курса<br/>для этой роли"]
    K --> T["Тьютор / Ассистент<br/>ответ по доступным источникам"]
    P -- "Нет" --> D["Источник не передаётся<br/>в контекст LLM"]
    T --> H["Преподаватель решает<br/>вопрос оценивания"]
    classDef student fill:#E9EDFF,stroke:#5666B9,color:#23315D,stroke-width:1.6px;
    classDef teacher fill:#E2F6F2,stroke:#14827A,color:#14524F,stroke-width:1.6px;
    classDef learning fill:#E9F2FF,stroke:#3575B9,color:#173F68,stroke-width:1.6px;
    classDef moodle fill:#FFF1D8,stroke:#BC7B18,color:#664510,stroke-width:1.6px;
    classDef ai fill:#F0E9FF,stroke:#8356BD,color:#4B2B73,stroke-width:1.6px;
    classDef knowledge fill:#E4F5E9,stroke:#288456,color:#154A32,stroke-width:1.6px;
    classDef tech fill:#EDF1F5,stroke:#64768A,color:#35465C,stroke-width:1.5px;
    classDef guard fill:#FFF0EA,stroke:#C2644D,color:#713A2E,stroke-width:1.6px;
    class M moodle;
    class V,P tech;
    class K knowledge;
    class T ai;
    class D guard;
    class H teacher;
```

Принципы продукта: не публиковать закрытые решения студентов и преподавателей; ограничивать поиск по полномочиям; не записывать оценки без явного решения преподавателя; сохранять сведения об использованных источниках и действиях для диагностики спорных случаев.

В архитектуре предусмотрена прослеживаемость действий агента. **Наличие схемы контроля не означает, что завершена сквозная проверка разграничения доступа во всех сервисах.**
