# TMP

## Практическая работа №1
```
@startuml "Практическая работа 1"
left to right direction
rectangle Касса {
Покупатель -- (Выбирает место)
Продавец -- (Проверяет наличие места)
(Проверяет наличие места) <. (Выбирает место)
(Покупатель) -- (Покупает товар)
(Оплачивает товар) <. (Покупает товар)
Покупатель -- (Забирает товар)
Банк -- (Проверяет наличие средств)
Банк -- (Подтверждает покупку)
(Подтверждает покупку) <. (Проверяет наличие средств)
(Проверяет наличие средств) <. (Оплачивает товар)
Продавец -- (Включает терминал)
(Оплачивает товар) <. (Включает терминал)
(Забирает товар) <. (Оплачивает товар)
(Покупает товар) <. (Проверяет наличие места)
Покупатель -- (Возвращает товар)
(Включает терминал) <. (Возвращает товар)
(Возвращает средства) <. (Включает терминал)
Банк -- (Возвращает средства)
(Забирает товар) <. (Подтверждает покупку)
}
@enduml
```
![Изображение](https://github.com/MariUt005/TMP/blob/405d48c8d501867228643389f4024e7c56295469/%D0%BF%D1%801.png)
