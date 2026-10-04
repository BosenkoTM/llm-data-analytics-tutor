# 5. Код программы

Ниже — **иллюстративные фрагменты** (псевдокод / упрощённые заготовки), показывающие роль
трёх слоёв архитектуры. Они **не являются** полным исходным кодом закрытой реализации и
намеренно не содержат рабочих констант, SQL DDL, точных имён внутренних модулей и алгоритмов
дедупликации.

## 5.1. Backend: приём данных

```python
# Иллюстрация: маршрут приёма учебной загрузки (упрощённо)
@app.post("/ingest")
def accept_materials():
    """Принять файлы, различить предпросмотр и подтверждение записи."""
    if not storage_ready():
        return error("хранилище недоступно")

    mode = request.form.get("mode")  # preview | confirm | cancel
    if mode == "confirm":
        draft = take_preview_session(request)
        return commit_to_knowledge_store(draft)  # запись только здесь

    documents = prepare_from_uploads(request.files)
    if mode == "preview":
        return show_preview(store_draft(documents))  # без INSERT

    return error("неизвестный режим")
```

## 5.2. Python-обработка: разбиение текста

```python
def split_learning_text(text: str) -> list[str]:
    """
    Учебный пайплайн: разрезать материал на фрагменты для эмбеддингов.
    Детали границ, overlap и защиты code fences — в закрытой реализации.
    """
    text = normalize(text)
    if not text:
        return []
    return chunk_with_structure_awareness(text)  # black-box в публичной доке
```

## 5.3. Взаимодействие с БД

```python
def persist_fragments(course_id: str, topic: str, fragments: list[str]) -> int:
    """Векторизовать фрагменты и записать в хранилище знаний одной транзакцией."""
    vectors = embed_locally(fragments)  # модель и размерность — закрытый контур
    with db_transaction() as cur:
        for fragment, vector in zip(fragments, vectors):
            cur.execute(
                "INSERT INTO <knowledge_table> (course, topic, body, embedding) "
                "VALUES (%s, %s, %s, %s)",
                (course_id, topic, fragment, vector),
            )
    return len(fragments)
```

## Связь «три функции → архитектура»

| Функция (логически) | Слой | Дисциплинарный акцент |
|---|---|---|
| Приём / режимы preview–confirm | Сбор · UI → сервер | ПС сбора и консолидации |
| Разбиение учебного текста | Python-пайплайн | Python для анализа данных |
| Транзакционная запись векторов | DWH | Консолидация + подготовка к RAG |
