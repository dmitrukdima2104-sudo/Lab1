# Лабораторна робота 1. Робота з СУБД PostgreSQL та основи SQL

## Загальна інформація

**Здобувач освіти:** [Дмитрук Дмитро Дмитрович]
**Група:** [ІПЗ-31]
**Обраний рівень складності:** [2]

## Виконання завдань та код запитів

### 1. Список таблиць бази даних
Спочатку перевіримо, які таблиці є в нашій базі даних:
```sql
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'public'
ORDER BY table_name;

Результат: У базі даних створено 12 основних таблиць: categories, customer_orders_summary, customers, employee_performance, employees, monthly_sales_report
order_items, orders, product_sales_summary, products, regions, suppliers.

Скріншот:![Список таблиць](screenshots/sql%20skrin1.png)

2. Базові запити, фільтрація та сортування (Рівень 1)
У цьому блоці ми робимо прості вибірки, сортування та обмежуємо кількість рядків.

```sql
-- Отримати всіх клієнтів
SELECT * FROM customers;

-- Вивести назви товарів і їхні ціни
SELECT product_name, unit_price FROM products;

-- Фільтрація співробітників за посадою (відділ продажів)
SELECT * FROM employees WHERE title ILIKE '%продаж%';

-- Замовлення зі статусом delivered
SELECT * FROM orders WHERE order_status = 'delivered';

-- Сортування товарів за зростанням ціни та обмеження ліміту (топ-10 найдорожчих)
SELECT * FROM products ORDER BY unit_price DESC LIMIT 10;
```

Результат: Отримано 15 записів клієнтів, включаючи як фізичних осіб, так і юридичні особи з різних міст України.

Скріншот

3. Пошук за зразком (LIKE / ILIKE)
Використовується для пошуку підрядків (наприклад, за частиною імені чи назви товару).

SQL
-- Клієнти, чиї імена починаються на "Іван"
SELECT * FROM customers WHERE contact_name LIKE 'Іван%';

-- Товари, в назві яких є слово "phone" або "телефон"
SELECT * FROM products WHERE product_name ILIKE '%phone%' OR product_name ILIKE '%телефон%';

-- Самостійні приклади:
SELECT * FROM customers WHERE email LIKE '%@gmail.com';
SELECT * FROM products WHERE product_name ILIKE '%чохол%';
SELECT * FROM employees WHERE last_name LIKE '%ук';
Скріншот результату:

4. Логічні оператори (AND, OR, NOT)
Дозволяють комбінувати кілька умов одночасно.

SQL
-- Товари у ціновому діапазоні від 15000 до 50000 грн
SELECT * FROM products WHERE unit_price > 15000 AND unit_price < 50000;

-- Активні товари (є на складі і не зняті з виробництва)
SELECT * FROM products WHERE units_in_stock > 0 AND NOT discontinued;

-- Клієнти не з Києва, у яких вказано номер телефону
SELECT * FROM customers WHERE NOT city = 'Київ' AND phone IS NOT NULL;
Скріншот результату:

5. Оператори IN, BETWEEN, IS NULL
Спеціальні оператори для перевірки списків, інтервалів дат/цін та пустих полів (NULL).

SQL
-- Клієнти з міст Київ, Харків, Одеса, Дніпро (оператор IN)
SELECT * FROM customers WHERE city IN ('Київ', 'Харків', 'Одеса', 'Дніпро');

-- Замовлення за перший квартал 2024 року (оператор BETWEEN)
SELECT * FROM orders WHERE order_date BETWEEN '2024-01-01' AND '2024-03-31';

-- Клієнти без вказаної компанії — фізичні особи (оператор IS NULL)
SELECT * FROM customers WHERE company_name IS NULL;
Скріншот результату:

6. Складне сортування та пагінація (OFFSET / LIMIT)
Використовується для сортування за кількома колонкам та поділу великої кількості даних на сторінки.

SQL
-- Складне сортування за категорією (зростання) та ціною (спадання)
SELECT * FROM products ORDER BY category_id ASC, unit_price DESC;

-- Пагінація: виведення другої сторінки товарів (по 10 штук на сторінку, пропускаємо перші 10)
SELECT * FROM products ORDER BY product_name LIMIT 10 OFFSET 10;
Скріншот результату:
## Висновки

**Самооцінка**: [4]

**Обґрунтування**: Під час виконання лабораторної роботи було повністю засвоєно принципи роботи з реляційними базами даних у середовищі PostgreSQL (Supabase). 
Успішно створено та виконано запити різного рівня складності: від базових вибірок (SELECT, WHERE, ORDER BY) до розширеної фільтрації за допомогою шаблонів (LIKE), 
логічних операторів (AND, OR, NOT), діапазонів (BETWEEN, IN) та пагінації (LIMIT, OFFSET). Усі результати перевірено та задокументовано за допомогою скріншотів.
