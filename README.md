# Лабораторная работа. Базовая настройка коммутатора

## Задачи
## Часть 1. Проверка конфигурации коммутатора по умолчанию

## Часть 2. Создание сети и настройка основных параметров устройства

- Настройте базовые параметры коммутатора.
- Настройте IP-адрес для ПК.

## Часть 3. Проверка сетевых подключений

- Отобразите конфигурацию устройства.
- Протестируйте сквозное соединение, отправив эхо-запрос.
- Протестируйте возможности удаленного управления с помощью Telnet


### Часть 1. Проверка конфигурации коммутатора по умолчанию

1. Изучите текущий файл running configuration.
   - Сколько интерфейсов FastEthernet имеется на коммутаторе 2960? *Ответ: 24*
   - Сколько интерфейсов Gigabit Ethernet имеется на коммутаторе 2960? *Ответ: 2*
   - Каков диапазон значений, отображаемых в vty-линиях? *Ответ: 0-15*
   - Назначен ли IP-адрес сети VLAN 1? *Ответ: нет*
   - Какой MAC-адрес имеет SVI? *Ответ: ввел команду `show mac address-table`, он не показал MAC*
   - Данный интерфейс включен? *Ответ: да*
   - Под управлением какой версии ОС Cisco IOS работает коммутатор? *Ответ: Version 15.0(2)SE4*
   - Как называется файл образа системы? *Ответ: C2960-LANBASEK9-M?*
   - Изучите свойства по умолчанию интерфейса FastEthernet, который используется компьютером PC-A.
     - Интерфейс включен или выключен? *Ответ: да*
     - Что нужно сделать, чтобы включить интерфейс? *Ответ: зайти на него через глоб конф и ввести `no shutdown`*
     - Какой MAC-адрес у интерфейса? *Ответ: ?*
     - Какие настройки скорости и дуплекса заданы в интерфейсе? *Ответ: ?наверное fulldupleks*

### Часть 2. Создание сети и настройка основных параметров устройства

- Настройка базовых параметров коммутатора:
  - Время: `clock set 17:33:00 27 march 2024`
  - Имя: `hostname NI01`
  - Пароль для пользовательского режима:
    ```
    NI01(config)# line con
    NI01(config)# line console 0
    NI01(config-line)# password cisco
    NI01(config-line)# login
    ```
  - Пароль для привилегированного режима:
    ```
    NI01(config)# enable password cisco
    NI01(config)# enable secret class
    ```
  - Сделать конфиг:
    ```
    NI01# copy running-config startup
    NI01# copy running-config startup-config
    ```
  - Баннер:
    ```
    SW01Niko(config)# banner m
    SW01Niko(config)# banner motd $
    Enter TEXT message. End with the character '$'.
    -------------------------------------------------------------------HEllo---------------------------
    ```
  - Дать IP и маску сетевой карте и включить порт:
    ```
    SW01Niko(config)# interface vl
    SW01Niko(config)# interface vlan 1
    SW01Niko(config-if)# ip add
    SW01Niko(config-if)# ip address 192.168.1.1 255.255.255.0
    SW01Niko(config-if)# no sh
    SW01Niko(config-if)# no shutdown
    ```
  - Дать IP адрес и маску PC0: `192.168.1.2 255.255.255.0`

### Часть 3. Проверка сетевых подключений

- Тестирование сквозного соединения, отправив эхо-запрос:
C:> ping 192.168.1.1
```
Pinging 192.168.1.1 with 32 bytes of data:
Request timed out.
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Ping statistics for 192.168.1.1:
Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
Minimum = 0ms, Maximum = 0ms, Average = 0ms
```

- Пароль на vty линию и доступ по Telnet:
```
SW01Niko(config)# line vty ?
<0-15> First Line number
SW01Niko(config)# line vty 0 4
SW01Niko(config-line)# pass
SW01Niko(config-line)# password class
SW01Niko(config-line)# login
SW01Niko(config-line)# transport in
SW01Niko(config-line)# transport input telnet
```


- Отобразить конфигурацию коммутатора:
```
SW01Niko# show run
Building configuration...
Current configuration : 1242 bytes
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
hostname SW01Niko
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
spanning-tree mode pvst
spanning-tree extend system-id
interface FastEthernet0/1
interface FastEthernet0/2
...
interface Vlan1
ip address 192.168.1.1 255.255.255.0
banner motd ^CHELLO^C
line con 0
password 7 0822455D0A16
login
line vty 0 4
password 7 0822404F1A0A
login
transport input telnet
line vty
```













