# Технологический стек

[← EduTutor](../README.md) · [Архитектура](01_overview.md)

Компоненты EduTutor объединяют веб-интерфейс, серверную обработку запросов, интеллектуальную оркестрацию, поисковый слой и образовательную платформу. Приведённый ниже стек описывает **используемые технологии**, а не набор обязательных компонентов для каждой учебной активности.

## Пользовательские интерфейсы и LMS

[![Next.js](https://img.shields.io/badge/Next.js-111827?style=for-the-badge&logo=nextdotjs&logoColor=white)](https://nextjs.org/) [![CopilotKit](https://img.shields.io/badge/CopilotKit-1769D2?style=for-the-badge)](https://www.copilotkit.ai/) [![AG-UI](https://img.shields.io/badge/AG-UI-0068D2?style=for-the-badge)](https://docs.ag-ui.com/) [![Moodle 5](https://img.shields.io/badge/Moodle%205-F08A24?style=for-the-badge&logo=moodle&logoColor=white)](https://moodle.org/)

| Технология | Роль |
|---|---|
| Next.js | Пользовательский интерфейс Workspace |
| CopilotKit / AG-UI | Взаимодействие интерфейса с агентными сервисами |
| Moodle 5 | Дисциплины, задания и результаты учебной работы |

## Серверные сервисы и интеллектуальная логика

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/) [![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/) [![Flask](https://img.shields.io/badge/Flask-3B5468?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com/) [![LangGraph](https://img.shields.io/badge/LangGraph-635BCE?style=for-the-badge)](https://www.langchain.com/langgraph) [![MCP](https://img.shields.io/badge/MCP-6750A4?style=for-the-badge)](https://modelcontextprotocol.io/)

| Технология | Роль |
|---|---|
| Python, FastAPI | Серверные интерфейсы Tutor и агентных компонентов |
| Flask | Веб-приложение EduRAG Loader |
| LangGraph | Графовое управление специализированными агентами |
| MCP (stdio) | Подключение инструментов к агентному контуру |

## Данные, поиск и локальные модели

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/) [![pgvector](https://img.shields.io/badge/pgvector-008C9E?style=for-the-badge)](https://github.com/pgvector/pgvector) ![RAG](https://img.shields.io/badge/RAG-008F86?style=for-the-badge) ![E5 embeddings](https://img.shields.io/badge/E5%20embeddings-008F86?style=for-the-badge) [![Ollama](https://img.shields.io/badge/Ollama-33465E?style=for-the-badge)](https://ollama.com/) [![Qwen](https://img.shields.io/badge/Qwen-7B61C8?style=for-the-badge)](https://qwenlm.github.io/)

| Технология | Роль |
|---|---|
| PostgreSQL / pgvector | Хранение материалов и векторный поиск |
| RAG / E5 | Извлечение контекста и формирование эмбеддингов |
| Ollama / Qwen | Локальная генерация текста |

## Принцип независимости образовательных треков

Выбор направления подготовки определяет содержание дисциплин, тип учебных заданий, набор материалов и критерии оценки. **Архитектура платформы не ограничивается одной образовательной программой:** подключение новых треков предполагает настройку предметной базы знаний и педагогических сценариев, а не обязательную замену базовых сервисов.

[Образовательные направления](../education/03_courses.md) · [Интеллектуальные агенты](02_agents.md)
