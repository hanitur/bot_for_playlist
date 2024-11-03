# Документация

<details>
  <summary>Код диаграммы</summary>
rtuml
actor Пользователь as act
participant "Telegram API" as tg
participant "Recommendation Service" as rs
participant "Yandex API" as ya
participant "Concert API" as ca
participant "AI API" as ai

act -> tg: Отправка сообщения с ссылкой на плейлист
activate tg
tg -> rs: Отправка сообщения со ссылкой на плейлист
deactivate tg
activate rs
rs -> ya: GET /playlist\nПолучить содержимое плейлиста
activate ya
ya --> rs: Ответ на запрос
deactivate ya
rs -> rs: Достать массив исполнителей
loop Повторить по всем уникальным исполнителям
rs -> ca: GET /concerts\nПолучить концерты по исполнителю
activate ca
ca --> rs: Ответ на запрос
deactivate ca
end
alt Не нашли концертов по исполнителям из плейлиста
rs -> ai: GET /answer\nПолучить список рекомендаций по похожим исполнителям
activate ai
ai --> rs: Ответ на запрос
deactivate ai
loop Повторить по всем уникальным исполнителям
rs -> ca: GET /concerts\nПолучить концерты по исполнителю
activate ca
ca --> rs: Ответ на запрос
deactivate ca
end
end
rs -> tg: POST /send_message\n Отправить сообщение со списком концертов пользователю
deactivate rs
activate tg
tg --> act: Сообщение со списком концертов
deactivate tg
@enduml
</details>
