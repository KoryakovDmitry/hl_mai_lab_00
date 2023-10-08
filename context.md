# Контекст решения

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

Person(user, "Пользователь")
System(service, "Сервис заказа услуг", "API для работы с пользователями, услугами и заказами")
System(database, "База данных", "Хранение данных о пользователях, услугах и заказах")

Rel(user, service, "Работа с пользователями, услугами и заказами")
Rel(service, database, "INSERT/SELECT/UPDATE", "SQL")

@enduml
```
