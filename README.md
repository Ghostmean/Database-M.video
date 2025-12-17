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