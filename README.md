**Задание 1**

![Image alt](https://github.com/mezhibo/Policing-shaping/blob/45ccc0eb0ae3ddac0f245a8a817f5ab5cb5c5ed2/IMG/1.png)

В указанной топологии ограничить входящую и исходящую скорость всего IPv4 трафика client1. Для ограничения использовать полисер с моделью single rate Two Color. Полисер должен обеспечивать постоянную полосу в 10Мбит/c и позволять амортизировать всплески, превышающие допустимую полосу на 6 Мбит на протяжении 1 секунды.

Отправьте полный список конфигураций: class-map, policy-map service-policy. Если используете симуляцию, то прислать скриншот, где видно счетчики полисера


**Решение 1**

В симуляторе EVE-NG создана следующая топология:

![Image alt](https://github.com/mezhibo/Policing-shaping/blob/45ccc0eb0ae3ddac0f245a8a817f5ab5cb5c5ed2/IMG/2.png)


Для настройки полисера на маршрутизаторе R1 выполнены следующие команды:

```
-- ACL для классификации трафика
R1(config)#ip access-list extended ip4-traff   
R1(config-ext-nacl)#permit ip host 192.168.200.200 any
R1(config-ext-nacl)#permit ip any host 192.168.200.200

-- Классификация трафика
R1(config)#class-map match-any ip4-traff
R1(config-cmap)#match access-group name ip4-traff

-- Политика QoS
R1(config)#policy-map ip4-traff 
R1(config-pmap)#class ip4-traff
R1(config-pmap-c)#police cir 10m bc 750000 conform-action transmit exceed-action drop

-- Применение политики
R1(config)#int e0/0
R1(config-if)#service-policy input ip4-traff
R1(config-if)#service-policy output ip4-traff

```


Необходимо учесть, что cbs указывается в битах, а 6 Мбит в задании - это 750000 бит


Итог выполнения задания:

![Image alt](https://github.com/mezhibo/Policing-shaping/blob/45ccc0eb0ae3ddac0f245a8a817f5ab5cb5c5ed2/IMG/3.png)


[Конфигурация маршрутизатора R1](https://github.com/mezhibo/Policing-shaping/blob/6e8d0918c842e3d5422e6e9232b0aa8b89a979ab/IMG/router1.txt)
