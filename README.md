# TMP

## Практическая работа №0
```
@startuml
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
![](пр0.png)


## Практическая работа №1
### Диаграмма вариантов использования
```
@startuml "Практическая работа 1"
left to right direction
title "Система продажи билетов на футбол"
actor Покупатель
actor Продавец
actor Банк

rectangle {
Покупатель -- (Выбрать место)
Покупатель -- (Забрать товар)
Покупатель -- (Оплатить товар)
Покупатель -- (Вернуть товар)
Продавец -- (Включить терминал)
Продавец -- (Проверить статус места)
Продавец -- (Изменить статус места)
Банк -- (Вернуть средства)
Банк -- (Подтвердить операцию)
Банк -- (Проверить наличие средств)
Банк -- (Списать средства)
(Выбрать место) ..> (Проверить статус места):<<include>>
(Проверить статус места) ..> (Включить терминал):<<include>>
(Включить терминал) ..> (Оплатить товар):<<include>>
(Оплатить товар) ..> (Проверить наличие средств):<<include>>
(Проверить наличие средств) ..> (Списать средства):<<include>>
(Списать средства) ..> (Подтвердить операцию):<<include>>
(Подтвердить операцию) ..> (Изменить статус места):<<include>>
(Подтвердить операцию) ..> (Забрать товар):<<include>>
(Вернуть товар) -- (Проверить статус места):<<include>>
(Включить терминал) ..> (Вернуть средства):<<include>>
(Вернуть средства) ..> (Подтвердить операцию):<<include>>
}
@enduml
```
![](пр1.png)

### Диаграмма классов
```
@startuml "Практическая работа 1"
class Покупатель {
+ ФИО
+ Деньги
Выбрать место()
Забрать товар()
Оплатить товар()
Вернуть место()
}

class Продавец {
+ ФИО
Включить терминал()
Проверить статус места()
Изменить статус места()
}

class Банк {
Вернуть средства()
Подтвердить покупку()
Проверить наличие средств()
Списать средства()
}

class Товар {
+ номер места
+ день и время
+ статус
}

class Билет {
+ номер места
+ день и время
+ статус
}

class Абонемент {
+ номер места
+ день и время
+ статус
}

Билет ..> Товар
Абонемент ..> Товар
Покупатель --> Товар:Выбирает
Продавец -- Товар:Проверяет статус
Продавец -- Банк:Проверять статус оплаты
Продавец --> Товар:Изменить статус
Покупатель --> Банк:Оплатить товар
Банк --> Покупатель:Вернуть деньги

@enduml
```
![](пр1_2.png)

## Практическая работа №2
### Диаграмма активностей
```
@startuml
|#pink|Покупатель|
start
if (Действие?) is (Купить) then
:Выбрать место;
|#lightblue|Продавец|
:Проверить статус места;
else (Вернуть)
endif

|#lightblue|Продавец|
:Проверить статус места;
:Включить терминал;

if (Действие?) is (Купить) then
|Банк|
:Проверить наличие средств;
:Списать деньги;
else (Вернуть)
|Банк|
:Вернуть средства;
endif
:Подтвердить операцию;

fork
|Продавец|
:Изменить статус места;
fork again
|Покупатель|
if (Действие?) is (Купить) then
:Забрать товар;
endif
endfork

stop
@enduml
```

![](пр2.png)

### Диаграмма последовательностей
```
@startuml
participant Покупатель as Foo
participant Продавец as Foo1
participant Терминал as Foo2
participant Банк as Foo3

Foo -> Foo1 : Выбрать место
Foo1 -> Foo2 : Включить терминал
Foo1 -> Foo : Разрешить оплату
Foo -> Foo2 : Приложить карту и ввести пинкод
Foo2 -> Foo3 : Оплатить
Foo3 -> Foo2 : Подтвердить оплату
Foo2 -> Foo1 : Сообщить о завершении оплаты
Foo1 -> Foo : Выдать билет
@enduml
```

![](пр2_1.png)

### Диаграмма развертывания
```
@startuml
database Билеты
node Покупатель
node Продавец
node Терминал
node Банк

Покупатель - Билеты: Выбирает
Продавец - Билеты: Продает
Продавец - Терминал: Включает
Покупатель - Терминал: Оплачивает
Терминал - Банк: Отправка данных об оплате
Банк - Терминал: Подтверждение оплаты
Терминал - Продавец: Подтвердить оплату
Продавец - Покупатель: Выдать билет
@enduml
```

![](пр2_2.png)

## Практическая работа №3
### Стратегия
```
class AuthMethod:
    def auth(self):
        ...

class Auth0(AuthMethod):
    def auth(self):
        print("Method 0")

class Auth1(AuthMethod):
    def auth(self):
        print("Method 1")

class Auth2(AuthMethod):
    def auth(self):
        print("Method2")

class AuthRun:
    def __init__(self, methodN):
        self.method = self.chooseMethod(methodN)
    def chooseMethod(self, N):
        self.method = AuthMethod()
        if N == 0:
            self.method = Auth0()
        if N == 1:
            self.method = Auth1()
        if N == 2:
            self.method = Auth2()
        return self.method.auth

a = AuthRun(0)
a.method()
```

### Шаблонный метод
```
class Algo:
    def step1(self):
        pass
    def step2(self):
        pass
    
class A(Algo):
    def step_1(self):
        print('Step A 1')
    def step_2(self):
        print('Step A 2')

class B(Algo):
    def step_1(self):
        print('Step B 1')
    def step_2(self):
        print('Step B 2')
    def something(self):
        print('do something else')
```

## Практическая работа 4

### Итератор
![image](https://github.com/MariUt005/TMP/assets/60814898/fd7aae82-829e-46c4-8941-8b459560bca4)
```
class Item:
    def __init__(self, number):
        self.number = number
    def __str__(self):
        return f"item #{self.number}"

class Iterator:
    def __init__(self, item):
        self._item = item
        self._index = 0
    def next(self):
        item = self._item[self._index]
        self._index += 1
        return item
    def has_next(self):
        return False if self._index >= len(self._item) else True

class Box:
    def __init__(self, amount_num: int = 10):
        self.num = [Item(it+1) for it in range(amount_num)]
        print(f"В коробке {amount_num} item")
    def amount_num(self):
        return len(self.num)
    def iterator(self):
        return Iterator(self.num)

item = Box(10)
iterator = item.iterator()
while iterator.has_next():
    item = iterator.next()
    print(str(item))
```

### Посетитель
![image](https://github.com/MariUt005/TMP/assets/60814898/3c56d307-bdf4-4fb9-9040-31cd78c0e395)

```
class A:
    def __init__(self):
        self.a = 'AAAA'
class B:
    def __init__(self):
        self.b = 'BBBB'

class C:
    def __init__(self):
        self.c = 'CCCC'

class Visitor:
    def method(self, item):
        if isinstance(item, A):
            self.methodA(item)
        elif isinstance(item, B):
            self.methodB(item)
        elif isinstance(item, C):
            self.methodC(item)
        else: print("Неизвестный класс")
    def methodA(self, item):
        print(item.a)
    def methodB(self, item):
        print(item.b)
    def methodC(self, item):
        print(item.c)

a = A()
b = B()
c = C()
v = Visitor()
v.method(a)
v.method(b)
v.method(c)
```

## Практическая работа 5

### Абстрактная фабрика
```
class Drink:
    def __init__(self, item):
        self.item = item
    def create(self): pass
class Food:
    def __init__(self, item):
        self.item = item
    def create(self): pass

class LunchDrink:
    def __init__(self):
        self.item = "Обед"
    def create(self):
        print(f'Создан напиток для приема: {self.item}')
class LunchFood:
    def __init__(self):
        self.item = "Обед"
    def create(self):
        print(f'Создана еда для приема: {self.item}')
class DinnerDrink:
    def __init__(self):
        self.item = "Ужин"
    def create(self):
        print(f'Создан напиток для приема: {self.item}')
class DinnerFood:
    def __init__(self):
        self.item = "Ужин"
    def create(self):
        print(f'Создана еда для приема: {self.item}')

class AbstractMeal:
    def getDrink(self): pass
    def getFood(self): pass

class DinnerFactory(AbstractMeal):
    def getDrink(self):
        return DinnerDrink()
    def getFood(self):
        return DinnerFood()
class LunchFactory(AbstractMeal):
    def getDrink(self):
        return LunchDrink()
    def getFood(self):
        return LunchFood()

class Application:
    def __init__(self, meal):
        self.meal = meal
    def create_gui(self):
        drink = self.meal.getDrink()
        food = self.meal.getFood()
        drink.create()
        food.create()

def create_factory(objectname: str):
    tabled = {
        "Обед": DinnerFactory,
        "Ужин": LunchFactory
    }
    return tabled[objectname]()

objectname = "Обед"
cr = create_factory(objectname)
app = Application(cr)
app.create_gui()

objectname = "Ужин"
cr = create_factory(objectname)
app = Application(cr)
app.create_gui()
```

### Строитель

```
class Product:
    bread = ["Толстое", "Тонкое"]
    meal = ['Ветчина', 'Пеперони']
    nam = ['Сыр', 'Огурцы']
    souse = ["Кетчуп", "Томатная паста"]

class pizza:
    def __init__(self, name):
        self.name = name
        self.meal = None
        self.topping = []
        self.souse = None
        self.botbread = None
    def printer(self):
        print(f'Название:{self.name}\n' \
              f'Мясо:{self.meal}\n' \
              f'Топинги:{[it for it in self.topping]}\n' \
              f'Соус:{self.souse}\n' \
              f'Тесто:{self.botbread}\n')

class Builder():
    def add_sauce(self) -> None: pass
    def add_meal(self) -> None: pass
    def add_topping(self) -> None: pass
    def prepare_botbread(self) -> None: pass
    def get_piz(self) -> pizza: pass

class Director:
    def __init__(self):
        self.builder = None
    def set_builder(self, builder: Builder):
        self.builder = builder
    def make_bur(self):
        if not self.builder:
            raise ValueError("Builder didn't set")
        self.builder.add_sauce()
        self.builder.add_meal()
        self.builder.add_topping()
        self.builder.prepare_botbread()

class RichPiz(Builder):
    def __init__(self):
        self.piz = pizza("Богатая")
    def add_sauce(self) -> None:
        self.piz.souse = Product.souse[0]
    def add_meal(self) -> None:
        self.piz.meal = Product.meal[0]
    def add_topping(self) -> None:
        self.piz.topping.append(Product.nam[0])
        self.piz.topping.append(Product.nam[1])
    def prepare_botbread(self) -> None:
        self.piz.botbread = Product.bread[0]
    def get_piz(self) -> pizza:
        return self.piz

class DefaultPiz(Builder):
    def __init__(self):
        self.piz = pizza("Обычная")
    def add_sauce(self) -> None:
        self.piz.souse = Product.souse[1]
    def add_meal(self) -> None:
        self.piz.meal = Product.meal[1]
    def add_topping(self) -> None:
        self.piz.topping.append(Product.nam[0])
    def prepare_botbread(self) -> None:
        self.piz.botbread = Product.bread[1]
    def get_piz(self) -> pizza:
        return self.piz

director = Director()
print("Богатая-1, Обычная-2")
a=int(input())
if a==1:
    builder = RichPiz()
else:
    builder = DefaultPiz()
director.set_builder(builder)
director.make_bur()
pizza = builder.get_piz()
pizza.printer()
```

### Адаптер
```
class intNum:
    def geti(self):
        return 123456

class strNum:
    def gets(self):
        return "11111"

class Adapter(intNum,strNum):
    def get1(self):
        return str(self.geti())
    def get2(self):
        return int(self.gets())

work = Adapter()
intwork=intNum()
strwork=strNum()
#print("Итог:" + intwork.geti()) Выдаст ошибку
#print(intwork.geti()+strwork.gets()) Выдаст ошибку
print("Итог:" + work.get1())
print(work.get2()+intwork.geti())
```

### Посредник
```
class User():
    def __init__(self, med, name, group):
        self.mediator = med
        self.name = name
        self.group = group
    def send(self, msg): pass
    def send1(self, msg): pass
    def send2(self, msg): pass
    def receive(self, msg): pass

class ChatMediator:
    def __init__(self):
        self.users = []
    def add_user(self, user):
        self.users.append(user)
    def send_message(self, msg, user):
        for u in self.users:
            if u != user:
                u.receive(msg)
    def send_messageM(self,msg,user):
        for u in self.users:
            if u != user and u.group == "Biso-01":
                u.receive(msg)
    def send_messageW(self,msg,user):
        for u in self.users:
            if u != user and u.group == "Biso-02":
                u.receive(msg)

class ConcreteUser(User):
    def send(self, msg):
        print(self.name + ": Отправил сообщение: " + msg)
        self.mediator.send_message(msg, self)
    def send1(self, msg):
        print(self.name + ": Отправил БИСО-01: " + msg)
        self.mediator.send_messageM(msg, self)
    def send2(self, msg):
        print(self.name + ": Отправил БИСО-02: " + msg)
        self.mediator.send_messageW(msg, self)
    def receive(self, msg):
        print(self.name + ": Получено сообщение: " + msg)

def printl():
    print("-" * 50)

mediator = ChatMediator()
user1 = ConcreteUser(mediator, "Мария", "Biso-01")
user2 = ConcreteUser(mediator, "Иван", "Biso-01")
user3 = ConcreteUser(mediator, "Руслан", "Biso-02")
user4 = ConcreteUser(mediator, "Илья", "Biso-02")

mediator.add_user(user1)
mediator.add_user(user2)
mediator.add_user(user3)
mediator.add_user(user4)

user1.send("Привет всем")
printl()
user1.send1("Привет БИСО-01")
printl()
user1.send2("Привет БИСО-02")
printl()
```

## Практическая работа 5

### Инверсия управления

main.py:
```
from hello import *
print("main.py:")
p.append("Маша")
p.append("Андрей")
n.append(5)
n.append(10)
hello()
calc()
```
hello.py:
```
print("hello.py")
p = []
n=[]
def hello():
    for person in p:
        print(f"Привет, {person}.")
def calc(sum=0):
    for num in n:
        sum=num+sum
    print(sum)
p.append("Иван")
p.append("Артем")
hello()
n.append(4)
n.append(2)
calc()
```

### Заместитель (proxy)

```
from abc import ABC
class PasswordItem:
    def __init__(self, password):
        self.password = password
    def get(self):
        return self.password
class Target(ABC):
    def ex(self) -> None:
        pass
class Real(Target):
    def ex(self) -> None:
        print("Настоящая часть кода запущена...\n5*5 =", 5*5)
class Proxy(Target):
    def __init__(self, real_target: Real) -> None:
        self._real_target = real_target
    def ex(self) -> None:
        if self.access():
            self._real_target.ex()
    def access(self) -> bool:
        realpassword = 12345
        print("Proxy: Проверка доступа")
        if realpassword == password.get():
            return True
        else:
            print("В доступе отказано")
            return False
def client(target: Target) -> None:
    target.ex()
if __name__ == "__main__":
    password = PasswordItem(111111)
    print(password.get())
    print("Запуск напрямую, без Proxy:")
    real_target = Real()
    client(real_target)
    print("\nЗапуск с Proxy и неправильным паролем:")
    proxy = Proxy(real_target)
    client(proxy)
    password = PasswordItem(12345)
    print("\nЗапуск с Proxy и правильным паролем:")
    proxy = Proxy(real_target)
    client(proxy)
```

### Компоновщик (дерево)

```
class Leaf:
    def __init__(self, number, *args):
        self.number = number
        self.position = args[0]


    def show(self):
        print("\t", end="")
        print(self.position)

    def showNum(self):
        print("\t", end="")
        print(self.number)


class Element:
    def __init__(self, number, *args):
        self.number = number
        self.position = args[0]
        self.children = []

    def add(self, child):
        self.children.append(child)

    def remove(self, child):
        self.children.remove(child)

    def show(self):
        print(self.position)
        for child in self.children:
            print("\t", end="")
            child.show()

    def showNum(self):
        print(self.number)
        for child in self.children:
            print("\t", end="")
            child.showNum()



if __name__ == "__main__":
    Hi = Element("A1", "Ректор")
    Item1 = Element("B1", "Директор института 1")
    Item2 = Element("B2", "Директор института 2")
    Item3 = Element("B3", "Директор института 3")
    Item11 = Leaf("C1", "Заведующий кафедрой 11")
    Item12 = Leaf("C2", "Заведующий кафедрой 12")
    Item21 = Leaf("C3", "Заведующий кафедрой 21")
    Item22 = Leaf("C4", "Заведующий кафедрой 22")
    Item31 = Leaf("C5", "Заведующий кафедрой 31")
    Item32 = Leaf("C6", "Заведующий кафедрой 32")
    Item1.add(Item11)
    Item1.add(Item12)
    Item2.add(Item21)
    Item2.add(Item22)
    Item3.add(Item31)
    Item3.add(Item32)

    Hi.add(Item1)
    Hi.add(Item2)
    Hi.add(Item3)
    Hi.show()
    print("Номера:")
    Hi.showNum()
```
