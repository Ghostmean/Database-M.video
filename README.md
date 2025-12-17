**Лабораторные работы по БД**

Перечень [лабораторные работы](https://edu.irnok.net/lib/exe/fetch.php?media=db:%D0%B2%D0%B0%D1%80%D0%B8%D0%B0%D0%BD%D1%82%D1%8B_%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B9_%D0%BF%D0%BE_%D1%83%D0%B4.pdf)

Telegram: [@your_username]

# Постановка задачи (Ваш вариант)

**Магазин электронной техники (подобный M.Video)**

*Сущности:* Клиенты (ID, ФИО, контактные данные), Товары (название, описание, цена, количество на складе), Заказы (номер, дата, статус), Состав заказа (товар, количество, цена на момент покупки).

*Процессы:* Клиенты оформляют заказы на товары. В заказ может входить несколько товаров в разном количестве. Фиксируется статус заказа (Новый, Подтвержден, Отправлен, Доставлен, Отменен) и цена товара на момент покупки.

*Выходные документы:*
  - Для заданного клиента вывести список всех его заказов с общей суммой каждого заказа, отсортированный по дате (сначала новые).
  - Выдать топ-5 самых продаваемых товаров за указанный период, отсортированный по количеству проданных единиц (по убыванию).

# Лабораторная работа 1 (Проектирование логической и физической модели БД)

## ER-диаграмма

```mermaid
erDiagram
    CLIENTS ||--o{ ORDERS : "places"
    ORDERS ||--|{ ORDER_ITEMS : "contains"
    ORDER_ITEMS }o--|| PRODUCTS : "refers_to"

    CLIENTS {
        integer client_id PK "SERIAL"
        varchar first_name
        varchar last_name
        varchar email
        varchar phone
        date registration_date
    }

    PRODUCTS {
        integer product_id PK "SERIAL"
        varchar product_name
        text description
        numeric price
        integer stock_quantity
    }

    ORDERS {
        integer order_id PK "SERIAL"
        integer client_id FK
        timestamp order_date
        varchar status
    }

    ORDER_ITEMS {
        integer order_id PK,FK
        integer product_id PK,FK
        integer quantity
        numeric price_at_time
    }

-- Создаем схему с вашей фамилией
CREATE SCHEMA Ivanov; -- Замените Ivanov на вашу фамилию

-- Устанавливаем эту схему по умолчанию для текущей сессии
SET search_path TO Ivanov;

-- 1. Таблица 'clients' (Клиенты)
CREATE TABLE clients (
    client_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20) NULL,
    registration_date DATE NOT NULL DEFAULT CURRENT_DATE
);

-- 2. Таблица 'products' (Товары)
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    description TEXT NULL,
    price NUMERIC(10, 2) NOT NULL CHECK (price >= 0),
    stock_quantity INTEGER NOT NULL DEFAULT 0 CHECK (stock_quantity >= 0)
);

-- 3. Таблица 'orders' (Заказы)
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    client_id INTEGER NOT NULL REFERENCES clients(client_id),
    order_date TIMESTAMP NOT NULL DEFAULT NOW(),
    status VARCHAR(20) NOT NULL DEFAULT 'Новый' CHECK (status IN ('Новый', 'Подтвержден', 'Отправлен', 'Доставлен', 'Отменен'))
);

-- 4. Таблица 'order_items' (Состав заказа)
CREATE TABLE order_items (
    order_id INTEGER NOT NULL REFERENCES orders(order_id),
    product_id INTEGER NOT NULL REFERENCES products(product_id),
    quantity INTEGER NOT NULL CHECK (quantity > 0),
    price_at_time NUMERIC(10, 2) NOT NULL CHECK (price_at_time >= 0),
    PRIMARY KEY (order_id, product_id) -- Составной первичный ключ
);