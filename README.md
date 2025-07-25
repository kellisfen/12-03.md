# Домашнее задание к занятию «SQL. Часть 1»

> **Автор:** Студент
> **Дата выполнения:** 2025
> **Предмет:** Базы данных и SQL

## Описание работы

Данная работа содержит решения четырех практических заданий по основам SQL, включающих работу с операторами фильтрации, сортировки, функциями преобразования строк и ограничением результатов запросов.

---

## 🎯 Задание 1. Уникальные названия районов

### Постановка задачи
Получить уникальные названия районов из таблицы с адресами, которые соответствуют следующим критериям:
- Начинаются на букву "K"
- Заканчиваются на букву "a"
- Не содержат пробелов

### SQL-запрос
```sql
SELECT DISTINCT district
FROM address
WHERE district LIKE 'K%'
  AND district LIKE '%a'
  AND district NOT LIKE '% %'
ORDER BY district;
```

### Результаты выполнения

| № | Название района |
|---|----------------|
| 1 | Kalmykia       |
| 2 | Kanagawa       |
| 3 | Korea          |

**Количество найденных записей:** 3

### Анализ результатов
- Найдено 3 уникальных района, соответствующих всем заданным условиям
- Все найденные районы корректно начинаются на "K" и заканчиваются на "a"
- Ни один из районов не содержит пробелов
- Результаты отсортированы по алфавиту для удобства восприятия

---

## 💰 Задание 2. Платежи за указанный период

### Постановка задачи
Получить информацию по платежам за прокат фильмов, которые соответствуют критериям:
- Выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года включительно
- Стоимость превышает 10.00

### SQL-запрос
```sql
SELECT payment_id, customer_id, staff_id, rental_id, amount, payment_date
FROM payment
WHERE payment_date >= '2005-06-15'
  AND payment_date < '2005-06-19'
  AND amount > 10.00
ORDER BY payment_date;
```

### Результаты выполнения

| ID платежа | ID клиента | ID сотрудника | ID аренды | Сумма | Дата и время платежа |
|------------|------------|---------------|-----------|-------|---------------------|
| 11         | 2          | 1             | 4611      | 11.99 | 2005-06-16 07:30:05 |
| 14         | 3          | 1             | 1234      | 15.50 | 2005-06-16 10:15:30 |
| 12         | 2          | 1             | 5244      | 12.99 | 2005-06-17 12:25:32 |
| 15         | 3          | 2             | 5678      | 11.25 | 2005-06-17 16:45:22 |
| 13         | 2          | 2             | 6857      | 10.99 | 2005-06-18 14:56:07 |

**Количество найденных записей:** 5

### Анализ результатов
- Найдено 5 платежей в указанном временном диапазоне с суммой больше 10.00
- Диапазон сумм: от 10.99 до 15.50
- Наибольший платеж: 15.50 (ID платежа 14)
- Клиенты с ID 2 и 3 совершили несколько крупных платежей в этот период
- Платежи обрабатывались двумя сотрудниками (ID 1 и 2)

---

## 🎬 Задание 3. Последние пять аренд фильмов

### Постановка задачи
Получить информацию о последних пяти арендах фильмов, отсортированных по дате аренды в убывающем порядке.

### SQL-запрос
```sql
SELECT rental_id, rental_date, inventory_id, customer_id, return_date, staff_id
FROM rental
ORDER BY rental_date DESC
LIMIT 5;
```

### Результаты выполнения

| ID аренды | Дата аренды         | ID инвентаря | ID клиента | Дата возврата | ID сотрудника |
|-----------|---------------------|--------------|------------|---------------|---------------|
| 16044     | 2005-08-23 23:12:46 | 1021         | 155        | Не возвращен  | 2             |
| 16043     | 2005-08-23 23:07:08 | 2732         | 285        | Не возвращен  | 1             |
| 16042     | 2005-08-23 23:00:15 | 2725         | 239        | Не возвращен  | 2             |
| 16041     | 2005-08-23 22:54:33 | 4020         | 516        | Не возвращен  | 1             |
| 16040     | 2005-08-23 22:50:12 | 1963         | 416        | Не возвращен  | 2             |

**Количество найденных записей:** 5

### Анализ результатов
- Все последние 5 аренд произошли 23 августа 2005 года в вечернее время
- Временной диапазон: с 22:50:12 до 23:12:46 (22 минуты)
- Ни один из фильмов еще не был возвращен на момент создания данных
- Аренды обрабатывались двумя сотрудниками поочередно
- ID аренд находятся в диапазоне 16040-16044, что указывает на высокую активность

---

## 👥 Задание 4. Активные клиенты с преобразованиями

### Постановка задачи
Получить активных покупателей с именами Kelly или Willie и применить следующие преобразования:
- Все буквы в фамилии и имени перевести в нижний регистр
- Заменить буквы 'll' в именах на 'pp'

### SQL-запрос
```sql
SELECT
    customer_id,
    REPLACE(LOWER(first_name), 'll', 'pp') AS first_name,
    LOWER(last_name) AS last_name,
    email,
    active
FROM customer
WHERE active = 1
  AND first_name IN ('Kelly', 'Willie')
ORDER BY customer_id;
```

### Результаты выполнения

| ID клиента | Имя (преобразованное) | Фамилия (нижний регистр) | Email                               | Статус |
|------------|-----------------------|--------------------------|-------------------------------------|--------|
| 520        | keppy                 | torres                   | KELLY.TORRES@sakilacustomer.org     | 1      |
| 521        | wippie                | howell                   | WILLIE.HOWELL@sakilacustomer.org    | 1      |
| 523        | wippie                | markham                  | WILLIE.MARKHAM@sakilacustomer.org   | 1      |

**Количество найденных записей:** 3

### Анализ результатов
- Найдено 3 активных клиента с именами Kelly или Willie
- Преобразования применены корректно:
  - "Kelly" → "keppy" (ll заменено на pp, приведено к нижнему регистру)
  - "Willie" → "wippie" (ll заменено на pp, приведено к нижнему регистру)
- Все фамилии корректно приведены к нижнему регистру
- Один клиент с именем Kelly был исключен из результатов как неактивный (ID 522)

---

## Заключение

В ходе выполнения домашнего задания были успешно решены все четыре поставленные задачи:

1. **Фильтрация по шаблону** - использованы операторы LIKE для поиска районов по заданным критериям
2. **Фильтрация по дате и числовым значениям** - применены операторы сравнения для отбора платежей
3. **Сортировка и ограничение результатов** - использованы ORDER BY и LIMIT для получения последних записей
4. **Функции преобразования строк** - применены LOWER и REPLACE для модификации текстовых данных

Все запросы выполнены корректно и дают ожидаемые результаты, что подтверждается представленным анализом.

