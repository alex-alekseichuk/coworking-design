# coworking-design

Architectural design of a coworking network management system.

2025-05-20

<!-- TOC -->

1. [Глоссарий](#глоссарий)
2. [Use cases](#use-cases)
3. [Бизнес объекты](#бизнес-объекты)

<!-- /TOC -->

<a id="markdown-глоссарий" name="глоссарий"></a>

## Глоссарий

- *Менеджер* управляет коворкингами, их администраторами и пользователями (online)
- *Администратор* коворкинга offline, на связи online
- *Пользователь* имеет аккаунт, offline, online (web site, mobile app)

- Коворкинг
- Схема комнат и рабочих мест в коворкинге
- Комната в коворкинге
  - Типы комнат: working, переговорка, common
- Место в комнате (стол, с компьютером, с канц. товарами)
- Расписание работы коворкинга для пользователей
  - C 7:00 до 10:00
  - Круглосуточно
  - Только по рабочим дням
  - Каждый день
- Цены коворкинга
- Скидки, акции
- Features коворкинга
  - Бытовые аксессуары коворкинга
  - Микроволновка, Чайник, 
  - Доставка еды через сторонние системы
  - Столовая
  - Принтер

- Расписание работы администраторов
- Смена администратора
  - как происходит пересменка?

- Бронь пользователя на место в коворкинге
  - фиксированное место или на-свободное
  - срок, по часам, по дням, по месяцам
- Бронь пользователя на комнату



<a id="markdown-use-cases" name="use-cases"></a>

## Use cases

```puml
@startuml
left to right direction
actor "Пользователь" as user

rectangle online {
  usecase "Бронирует место" as uc_book_online
  usecase "Чат с администратором" as uc_chat
}

rectangle offline {
  usecase "Бронирует место " as uc_book_offline
  usecase "Check-in на входе" as uc_checkin
  usecase "Check-out на выходе" as uc_checkout
}

:user: -- (uc_book_online)
:user: -- (uc_chat)
:user: -- (uc_book_offline)
:user: -- (uc_checkin)
:user: -- (uc_checkout)
@enduml
```

```puml
@startuml
left to right direction
actor "Администратор" as admin

rectangle online {
  usecase "Отвечает пользователю в омниканальном чате" as uc_chat
}

rectangle offline {
  usecase "регистрирует пользователя" as uc_reg_user
  usecase "идентифицирует пользователя" as uc_id_user
  usecase "'Выгоняет' пользователя, если уже пора" as uc_kick
}

rectangle coworking {
  usecase "принимает коворкинг" as uc_checkin
  usecase "сдает коворкинг" as uc_checkout
  usecase "контролирует уборку/сохранность" as uc_clean
}

:admin: -- (uc_chat)
:admin: -- (uc_reg_user)
:admin: -- (uc_id_user)
:admin: -- (uc_kick)
:admin: -- (uc_checkin)
:admin: -- (uc_checkout)
:admin: -- (uc_clean)
@enduml
```

```puml
@startuml
left to right direction
actor "Manager" as manager

usecase "Управление коворкингом" as uc_handle_coworking
usecase "Управление персоналом" as uc_handle_admin
usecase "Управление пользователями" as uc_handle_user
usecase "Смотрит отчеты, статистику" as uc_reports

usecase "Регистрирует коворкинг, расписание" as uc_add_coworking
usecase "Составляет схему комнат и мест" as uc_schema

usecase "Добавляет нового администратора" as uc_add_admin
usecase "Составляет расписание работы администраторов" as uc_schedule_admin
usecase "Общается с администратором" as uc_chat_admin

usecase "Регистрирует пользователя" as uc_reg_user
usecase "Общается с пользователем" as uc_chat_user

:manager: -- (uc_handle_coworking)
:manager: -- (uc_handle_admin)
:manager: -- (uc_handle_user)
:manager: -- (uc_reports)

(uc_handle_coworking) --> (uc_add_coworking)
(uc_handle_coworking) --> (uc_schema)

(uc_handle_admin) --> (uc_add_admin)
(uc_handle_admin) --> (uc_schedule_admin)
(uc_handle_admin) --> (uc_chat_admin)

(uc_handle_user) --> (uc_reg_user)
(uc_handle_user) --> (uc_chat_user)

@enduml
```

<a id="markdown-бизнес-объекты" name="бизнес-объекты"></a>

## Бизнес объекты

```puml
@startuml
entity Coworking {
  * id
  --
  * address
  * рассписание
  * {field} цены, акции
  * схема
  * {field} admin_id (current admin)
}

entity Room {
  * id
  --
  * {field} type
}

entity Place {
  * id
  --
}

entity User {
  * id
  --
  * name
  * email
  * phone
  * balance
}

entity Booking {
  * id
  --
  * {field} type
  * period
  * balance
}

entity Account {
  * id
  --
  * email
  * password
  * role (manager, admin)
}

Coworking *-- Room
Room *-- Place
Booking --> User
Booking --> Place
Booking --> Room

@enduml
```
