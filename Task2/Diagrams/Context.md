@startuml RetailCore_Context

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

title C4 Model - System Context Diagram (AS-IS)
title RetailCore Platform

LAYOUT_LEFT_RIGHT()

'========================
' Пользователи
'========================

Person(customer, "Покупатель","Оформляет и оплачивает заказы")

Person(support, "Сотрудник службы поддержки","Просматривает и сопровождает заказы")

Person(sales, "Сотрудник продаж","Управляет продажами и коммерческими предложениями")

Person(logistics, "Сотрудник логистики","Контролирует выполнение доставки")

Person(finance, "Финансовый специалист","Контролирует проведение платежей")

Person(analyst, "Бизнес-аналитик","Использует данные платформы для анализа")

'========================
' Основная система
'========================

System(retailcore, "RetailCore","Платформа управления заказами, оплатой, доставкой и интеграциями")

'========================
' Внешние системы
'========================

System_Ext(webshop, "Интернет-магазин","Основной канал оформления заказов")

System_Ext(marketplaces, "Маркетплейсы","Получение и передача заказов")

System_Ext(payment, "Платежные провайдеры","Обработка платежей")

System_Ext(delivery, "Службы доставки","Доставка заказов")

System_Ext(erp, "ERP","Учет и корпоративные процессы")

System_Ext(bi, "BI / Аналитика","Отчетность и аналитика")

'========================
' Связи пользователей
'========================

Rel(customer, webshop, "Оформляет заказ")
Rel(support, retailcore, "Просматривает и изменяет информацию о заказах")
Rel(sales, retailcore, "Управляет коммерческими сценариями")
Rel(logistics, retailcore, "Контролирует доставку")
Rel(finance, retailcore, "Контролирует оплату")
Rel(analyst, bi, "Просматривает отчеты")

'========================
' Интеграции
'========================
Rel(webshop, retailcore, "Создание и обработка заказов", "HTTPS / REST")

Rel(marketplaces, retailcore,"Передача заказов и получение статусов","API / Batch")

Rel(retailcore, payment,"Авторизация и проведение платежей","REST")

Rel(retailcore, delivery,"Расчет стоимости доставки\Создание отправлений\ Получение статусов","REST")

Rel(retailcore, erp,"Передача информации о заказах","Интеграция")

Rel(retailcore, bi,"Передача данных для отчетности","ETL / Batch")

SHOW_LEGEND()

@enduml

