@startuml RetailCore_TOBE_Container

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

title C4 Container Diagram - RetailCore (TO-BE Transition Architecture)

LAYOUT_LEFT_RIGHT()

'========================
' Пользователи
'========================

Person(customer, "Покупатель")
Person(support, "Сотрудник поддержки")
Person(admin, "Бизнес-пользователь")

'========================
' Внешние системы
'========================

System_Ext(shop, "Интернет-магазин")
System_Ext(mp, "Маркетплейсы")
System_Ext(paymentProvider, "Платежные провайдеры")
System_Ext(deliveryProvider, "Службы доставки")
System_Ext(erp, "ERP")
System_Ext(bi, "BI / Аналитика")

'========================
' RetailCore
'========================

System_Boundary(retailcore, "RetailCore Platform") {

    Container(api, "API Gateway / Integration Layer", "Java / Spring Boot", "Единая точка входа для внутренних и внешних интеграций")

    Container(status, "Order Status Service", "Java", "Управление жизненным циклом заказа")

    Container(notification, "Notification Service","Java", "Отправка уведомлений клиентам и внутренним пользователям")

    Container(analytics, "Analytics Service","Python","Подготовка данных для аналитики")

    ContainerDb(statusdb, "Status DB","PostgreSQL","Хранение состояния заказов")

    ContainerQueue(events, "Event Bus","Kafka","Обмен событиями между сервисами")

    Container_Boundary(monolith, "Сохраняемый монолит") {

        Container(checkout,"Checkout","Java","Оформление заказа")

        Container(order,"Order Processing","Java","Основная обработка заказов")

        Container(pricing,"Pricing & Promo","Java","Расчет стоимости и применение промо")

        Container(payment,"Payment Orchestration","Java", "Оркестрация платежей")

        Container(delivery,"Delivery Routing", "Java","Маршрутизация доставки")

        ContainerDb(oracle, "Oracle Database", "Oracle","Основная операционная база данных (сохраняется на переходном этапе)")
    }

}

'========================
' Пользователи
'========================

Rel(customer, shop, "Оформляет заказ")

Rel(support, api, "Работа с заказами")

Rel(admin, api, "Управление заказами")

'========================
' Внешние интеграции
'========================

Rel(shop, api, "REST API")

Rel(mp, api, "REST / Batch")

Rel(api, checkout, "REST")

Rel(api, status, "REST")

Rel(api, analytics, "REST")

'========================
' Монолит
'========================

Rel(checkout, pricing, "Получение стоимости")

Rel(checkout, order, "Создание заказа")

Rel(order, payment, "Инициирование оплаты")

Rel(order, delivery, "Создание доставки")

Rel(order, oracle, "Чтение/запись")

Rel(pricing, oracle, "Чтение")

Rel(payment, oracle, "Чтение/запись")

Rel(delivery, oracle, "Чтение/запись")

'========================
' Новые сервисы
'========================

Rel(order, events, "Публикует события")

Rel(events, status, "OrderCreated\nOrderUpdated")

Rel(events, notification, "OrderStatusChanged")

Rel(events, analytics, "Бизнес-события")

Rel(status, statusdb, "CRUD")

Rel(status, oracle,"Временная синхронизация\n(переходный этап)","SQL/API")

'========================
' Внешние системы
'========================

Rel(payment, paymentProvider,"Проведение платежей","REST")

Rel(delivery, deliveryProvider,"Создание доставки","REST")

Rel(analytics, bi,"Передача данных","API")

Rel(order, erp,"Интеграция","REST")

SHOW_LEGEND()

@enduml
