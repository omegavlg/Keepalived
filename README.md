# Домашнее задание к занятию "`Disaster recovery и Keepalived`" - `Дедюрин Денис`

---

## Задание 1

* Дана схема для Cisco Packet Tracer, рассматриваемая в лекции.
* На данной схеме уже настроено отслеживание интерфейсов маршрутизаторов Gi0/1 (для нулевой группы)
* Необходимо аналогично настроить отслеживание состояния интерфейсов Gi0/0 (для первой группы).
* Для проверки корректности настройки, разорвите один из кабелей между одним из маршрутизаторов и Switch0 и запустите ping между PC0 и Server0.
* На проверку отправьте получившуюся схему в формате pkt и скриншот, где виден процесс настройки маршрутизатора.


`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

Router1
```
Router0> enable
Router0# configure terminal
Router0(config)# interface gigabitEthernet 0/0
Router0(config-if)# standby version 2
Router0(config-if)# standby 0 ip 192.168.0.1
Router0(config-if)# standby 0 priority 105
Router0(config-if)# standby 0 preempt
Router0(config-if)# standby 0 track gigabitEthernet 0/1
Router0(config-if)# exit
Router0(config)# interface gigabitEthernet 0/1
Router0(config-if)# standby version 2
Router0(config-if)# standby 1 ip 192.168.1.1
Router0(config-if)# standby 1 priority 100
Router0(config-if)# standby 1 preempt
Router0(config-if)# standby 1 track gigabitEthernet 0/0
Router0(config-if)# exit
Router0(config)# exit
Router0# write memory
```

Router2
```
Router1> enable
Router1# configure terminal
Router1(config)# interface gigabitEthernet 0/0
Router1(config-if)# standby version 2
Router1(config-if)# standby 0 ip 192.168.0.1
Router1(config-if)# standby 0 priority 105
Router1(config-if)# standby 0 preempt
Router1(config-if)# standby 0 track gigabitEthernet 0/1
Router1(config-if)# exit
Router1(config)# interface gigabitEthernet 0/1
Router1(config-if)# standby version 2
Router1(config-if)# standby 1 ip 192.168.1.1
Router1(config-if)# standby 1 priority 100
Router1(config-if)# standby 1 preempt
Router1(config-if)# standby 1 track gigabitEthernet 0/0
Router1(config-if)# exit
Router1(config)# exit
Router1# write memory
```

![pkt](https://github.com/omegavlg/Keepalived/blob/main/hsrp_advanced_dedyurin.pkt)

---

## Задание 2
* Запустите две виртуальные машины Linux, установите и настройте сервис Keepalived как в лекции, используя пример конфигурационного файла.
* Настройте любой веб-сервер (например, nginx или simple python server) на двух виртуальных машинах
* Напишите Bash-скрипт, который будет проверять доступность порта данного веб-сервера и существование файла index.html в root-директории данного веб-сервера.
* Настройте Keepalived так, чтобы он запускал данный скрипт каждые 3 секунды и переносил виртуальный IP на другой сервер, если bash-скрипт завершался с кодом, отличным от нуля (то есть порт веб-сервера был недоступен или отсутствовал index.html). Используйте для этого секцию vrrp_script
* На проверку отправьте получившейся bash-скрипт и конфигурационный файл keepalived, а также скриншот с демонстрацией переезда плавающего ip на другой сервер в случае недоступности порта или файла index.html
```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота 2](ссылка на скриншот 2)`


---

## Задание 3

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`

## Задание 4

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`
