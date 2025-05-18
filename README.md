# Технологии разработки приложений для мобильных платформ. Подюков Илья. ФИТ-2-2024 НМ. ЛР-2

## Введение

Копируется программа из прошлой лабораторной работы, и дополнительно к предыдущим зависимостям добавляется `expo-sqlite`

## Документация схемы БД

##### Обзор базы данных

Приложение использует SQLite базу данных с именем `markers.db`, которая содержит две таблицы:

1. markers - для хранения информации о маркерах на карте
2. marker_images - для хранения изображений, связанных с маркерами

#### Таблица markers

##### Структура таблицы

| Поле       | Тип данных | Описание                         | Ограничения               |
| ---------- | ---------- | -------------------------------- | ------------------------- |
| id         | TEXT       | Уникальный идентификатор маркера | PRIMARY KEY, NOT NULL     |
| latitude   | REAL       | Географическая широта маркера    | NOT NULL                  |
| longitude  | REAL       | Географическая долгота маркера   | NOT NULL                  |
| created_at | DATETIME   | Дата и время создания маркера    | DEFAULT CURRENT_TIMESTAMP |

##### SQL-запрос создания таблицы

```
CREATE TABLE IF NOT EXISTS markers (
  id TEXT PRIMARY KEY NOT NULL,
  latitude REAL NOT NULL,
  longitude REAL NOT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

#### Таблица marker_images

### Структура таблицы

| Поле       | Тип данных | Описание                             | Ограничения               |
| ---------- | ---------- | ------------------------------------ | ------------------------- |
| id         | TEXT       | Уникальный идентификатор изображения | PRIMARY KEY, NOT NULL     |
| marker_id  | TEXT       | Идентификатор связанного маркера     | NOT NULL, FOREIGN KEY     |
| uri        | TEXT       | URI или путь к файлу изображения     | NOT NULL                  |
| created_at | DATETIME   | Дата и время добавления изображения  | DEFAULT CURRENT_TIMESTAMP |

##### SQL-запрос создания таблицы

```
CREATE TABLE IF NOT EXISTS marker_images (
  id TEXT PRIMARY KEY NOT NULL,
  marker_id TEXT NOT NULL,
  uri TEXT NOT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (marker_id) REFERENCES markers (id) ON DELETE CASCADE
);
```

#### Связи между таблицами

- Таблица `marker_images` связана с таблицей `markers` через поле `marker_id`
- Установлено ограничение `ON DELETE CASCADE`, что означает автоматическое удаление всех изображений маркера при удалении самого маркера
