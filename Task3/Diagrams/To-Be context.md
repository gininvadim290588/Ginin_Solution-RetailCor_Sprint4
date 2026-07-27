@startuml RetailCore_Context_TOBE

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

title C4 Model - System Context Diagram (TO-BE)

LAYOUT_LEFT_RIGHT()
'========================
' Пользователи
'========================
Person( mcustomer,"Покупатель","Оформляет и оплачивает заказы")
Person( support,"Сотрудник службы поддержки","Контролирует обработку заказов")
Person( sales,"Сотрудник продаж","Управляет коммерческими предложениями")
Person( logistics,"Сотрудник логистики","Контролирует доставку")
Person( finance,"Финансовый специалист","Контролирует проведение платежей")
Person( analyst,"Бизнес-аналитик","Использует аналитические данные")

'========================
' Основная система
'========================

System(retailcore,"RetailCore Platform","Платформа управления заказами, оплатой, доставкой и интеграциями")

'========================
' Внешние системы
'========================

System_Ext(webshop,"Интернет-магазин","Основной канал оформления заказов")

System_Ext(marketplaces,"Маркетплейсы","Продажа товаров")

System_Ext(payment,"Платежные провайдеры","Обработка платежей")

System_Ext(delivery,"Службы доставки","Организация доставки")

System_Ext(erp,"ERP","Учет и корпоративные процессы")

System_Ext(bi,"BI / Аналитика","Отчетность")

'========================
' Пользователи
'========================

Rel(customer, webshop, "Оформляет заказ")

Rel(support, retailcore,"Просматривает статус\nобрабатывает исключения")

Rel(sales, retailcore,"Управляет заказами\nи коммерческими сценариями")

Rel(logistics, retailcore,"Контролирует доставку")

Rel(finance, retailcore,"Контролирует платежи")

Rel(analyst, bi,"Просматривает отчеты")

'========================
' Интеграции
'========================

Rel(webshop,retailcore,"REST API\nчерез Integration Layer")

Rel(marketplaces,retailcore,"REST API\nчерез Integration Layer")

Rel(retailcore,payment,"API")

Rel(retailcore,delivery,"API")

Rel(retailcore,erp,"REST API / События")

Rel(retailcore,bi,"События / ETL")

SHOW_LEGEND()

@enduml
