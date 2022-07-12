# Домашнее задание к занятию "3.7. Компьютерные сети, лекция 2"

1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?

`В Ubuntu можно проверить с помощью утилиты ifconfig (которая идет в составе пакета net-tools) , либо с помощью команды ls /sys/class/net`
`В Windows можно использовать команду ipconfig /all`

![image](https://user-images.githubusercontent.com/106814458/178582172-9a25693a-d364-4b50-908c-6624e241b3de.png)

2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?

`Link Layer Discovery Protocol (LLDP) — протокол канального уровня, позволяющий сетевому оборудованию оповещать оборудование, работающее в локальной сети, о своём существовании и передавать ему свои характеристики, а также получать от него аналогичные сведения. Описание протокола приводится в стандарте IEEE 802.1AB-2009`

`Существует пакет lldpd и команда lldpctl`

3. Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.

`Vlan - виртуальное разделение коммутатора. Существует пакет vlan.После его установки с помощью утилиты vconfig мы можем добавлять или наоборот удалять VLAN'ы.Для примера выполним команду sudo vconfig add eth0 300 после используем команду vconfig -a и видим созданный нами VLAN`

![image](https://user-images.githubusercontent.com/106814458/178595003-27264073-c49a-4d02-8f17-623e99389210.png)

4. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.

`LAG - агрегация портов , бывают двух типов : статический и динамический (LACP протокол) в Linux называется Bonding ,это механизм, используемый Linux-серверами и предполагающий связь нескольких физических интерфейсов в один виртуальный, что позволяет обеспечить большую пропускную способность или отказоустойчивость в случае повреждения кабеля`
`Опции для балансировки нагрузки можем видеть в следующей таблице:`

![image](https://user-images.githubusercontent.com/106814458/178599447-a3e9daf8-0297-4a0d-86f5-6b0ed98e6fe5.png)

`Для конфигурации необходимо в файле /etc/network/interfaces выполнить следующие изменения :`

```
$ sudo nano /etc/network/interfaces

# The primary network interface

auto bond0
iface bond0 inet static
    address 192.168.1.150
    netmask 255.255.255.0    
    gateway 192.168.1.1
    dns-nameservers 192.168.1.1 8.8.8.8
    dns-search domain.local
        slaves eth0 eth1
        bond_mode 0
        bond-miimon 100
        bond_downdelay 200
        bound_updelay 200
```
  
`где bond_mode 0 - как раз таки опция для включения режима balance-rr`     

5. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.

6. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.

7. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?

