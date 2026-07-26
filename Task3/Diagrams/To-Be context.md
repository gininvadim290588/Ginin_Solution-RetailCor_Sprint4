@startuml RetailCore_Target_Context

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

title C4 Model - System Context Diagram (TO-BE)

LAYOUT_LEFT_RIGHT()

Person(customer,"Покупатель")
Person(support,"Служба поддержки")
Person(admin,"Бизнес-пользователь")

System(retailcore,"RetailCore Platform","Платформа управления заказами")

System_Ext(shop,"Интернет-магазин")
System_Ext(mp,"Маркетплейсы")
System_Ext(payment,"Платежные провайдеры")
System_Ext(delivery,"Службы доставки")
System_Ext(erp,"ERP")
System_Ext(bi,"BI")

Rel(customer,shop,"Оформляет заказ")

Rel(shop,retailcore,"REST API")

Rel(admin,retailcore,"Работа с заказами")

Rel(support,retailcore,"Поддержка заказов")

Rel(retailcore,payment,"API")

Rel(retailcore,delivery,"API")

Rel(retailcore,erp,"Интеграция")

Rel(retailcore,bi,"Передача событий и данных")

SHOW_LEGEND()

@endumlf
