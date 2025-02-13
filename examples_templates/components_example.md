# Компонентная архитектура
<!-- Состав и взаимосвязи компонентов системы между собой и внешними системами с указанием протоколов, ключевые технологии, используемые для реализации компонентов.
Диаграмма контейнеров C4 и текстовое описание. 
-->
## Компонентная диаграмма

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(admin, "Администратор")
Person(moderator, "Модератор")
Person(user, "Пользователь")

System_Ext(web_site, "Клиентский веб-сайт", "HTML, CSS, JavaScript, React", "Веб-интерфейс")

System_Boundary(conference_site, "Сайт блогов") {
   'Container(web_site, "Клиентский веб-сайт", ")
   Container(client_service, "Сервис авторизации", "C++", "Сервис управления пользователями", $tags = "microService")    
   Container(post_service, "Сервис постов", "C++", "Сервис управления блогами", $tags = "microService") 
   Container(blog_service, "Сервис блогов", "C++", "Сервис управления постами", $tags = "microService")   
   ContainerDb(db, "База данных", "MySQL", "Хранение данных о блогах, постах и пользователях", $tags = "storage")
   
}

Rel(admin, web_site, "Просмотр, добавление и редактирование информации о пользователях, конференциях и докладах")
Rel(moderator, web_site, "Модерация контента и пользователей")
Rel(user, web_site, "Регистрация, просмотр информации о конференциях и докладах и запись на них")

Rel(web_site, client_service, "Работа с пользователями", "localhost/person")
Rel(client_service, db, "INSERT/SELECT/UPDATE", "SQL")

Rel(web_site, post_service, "Работа с постами", "localhost/pres")
Rel(post_service, db, "INSERT/SELECT/UPDATE", "SQL")

Rel(web_site, blog_service, "Работа с блогами", "localhost/conf")
Rel(blog_service, db, "INSERT/SELECT/UPDATE", "SQL")

@enduml
```
## Список компонентов  

### Сервис авторизации
**API**:
-	Создание нового пользователя
      - входные параметры: login, пароль, имя, фамилия, email, обращение (г-н/г-жа)
      - выходные параметры: отсутствуют
-	Поиск пользователя по логину
     - входные параметры:  login
     - выходные параметры: имя, фамилия, email, обращение (г-н/г-жа)
-	Поиск пользователя по маске имени и фамилии
     - входные параметры: маска фамилии, маска имени
     - выходные параметры: login, имя, фамилия, email, обращение (г-н/г-жа)

### Сервис блогов
**API**:
- Создание блога
  - Входные параметры: название блога, категория, аннотация, автор и дата чоздания
  - Выходыне параметры: млентификатор блога
- Получение списка всех блогов
  - Входные параметры: отсутствуют
  - Выходные параметры: массив с блогов, где для каждого указаны его идентификатор, название, категория, аннотация, автор и дата написания

### Сервис постов
**API**:
- Создание поста
  - Входные параметры: заголовок поста, автор, блог, содержания поста, дата создания
  - Выходные параметры: идентификатор поста
- Получение списка постов в блоге
  - Входные параметры: блог
  - Выходные параметры: массив с постами (идентификатор, заголовок поста, автор, блог, содержания поста, дата создания)
- Получение поста
  - Входнае параметры: идентификатор поста
  - Выходные парамтеры: заголовок поста, автор, блог, содержания поста, дата создани
- Изменение поста
  - Входные параметры: идентификатор поста, заголовок поста, автор, блог, содержания поста, дата создания
  - Выходные параметры: отсутствуют


### Модель данных
```puml
@startuml

class Blog {
  id
  name
  type
  description
  date
  author_id
}

class User {
  id
  login
  first_name
  last_name
  email
  title
}

class Topic {
  id
  title
  author_id
  blog_id
  body
  change_date
}

User <- Blog
Blog <- Topic

@enduml
```