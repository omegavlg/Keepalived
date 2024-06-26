# Домашнее задание к занятию "`Disaster recovery и Keepalived`" - `Дедюрин Денис`

---

## Задание 1

* Дана схема для Cisco Packet Tracer, рассматриваемая в лекции.
* На данной схеме уже настроено отслеживание интерфейсов маршрутизаторов Gi0/1 (для нулевой группы)
* Необходимо аналогично настроить отслеживание состояния интерфейсов Gi0/0 (для первой группы).
* Для проверки корректности настройки, разорвите один из кабелей между одним из маршрутизаторов и Switch0 и запустите ping между PC0 и Server0.
* На проверку отправьте получившуюся схему в формате pkt и скриншот, где виден процесс настройки маршрутизатора.

Скриншот вывода команд show standby, демонстрирующий, что для интерфейсов g0/0 и g0/1 настроен HSRP с параметром preempt, который позволяет маршрутизатору стать активным, как только его приоритет становится выше, чем у партнера.
<img src = "img/cpt01.png" width = 100%>

Также настроена функция standby track, которая позволяет отслеживать состояние интерфейса и изменять его приоритет в зависимости от состояния других интерфейсов.

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

[pkt](https://github.com/omegavlg/Keepalived/blob/main/hsrp_advanced_dedyurin.pkt)

---

## Задание 2
* Запустите две виртуальные машины Linux, установите и настройте сервис Keepalived как в лекции, используя пример конфигурационного файла.
* Настройте любой веб-сервер (например, nginx или simple python server) на двух виртуальных машинах
* Напишите Bash-скрипт, который будет проверять доступность порта данного веб-сервера и существование файла index.html в root-директории данного веб-сервера.
* Настройте Keepalived так, чтобы он запускал данный скрипт каждые 3 секунды и переносил виртуальный IP на другой сервер, если bash-скрипт завершался с кодом, отличным от нуля (то есть порт веб-сервера был недоступен или отсутствовал index.html). Используйте для этого секцию vrrp_script
* На проверку отправьте получившейся bash-скрипт и конфигурационный файл keepalived, а также скриншот с демонстрацией переезда плавающего ip на другой сервер в случае недоступности порта или файла index.html

#### Скрипт check_web.sh
Выполняет проверку на открытый порт 80 ИЛИ присутствие файла index.html в директории /usr/share/nginx/html/, если хотя бы одно из этих условий не будет выполнено, тогда скрипт завершится с кодом ошибки "1", и передаст управление другой ноде.

```
#!/bin/bash

echo "Script started at $(date)" >> /tmp/check_web.log
exec 3<> /dev/tcp/127.0.0.1/80
PORT_STATUS=$?
exec 3>&-

INDEX_FILE="/usr/share/nginx/html/index.html"
if [ -f "$INDEX_FILE" ]; then
  FILE_STATUS=0
else
  FILE_STATUS=1
fi

echo "$PORT_STATUS"
echo "$FILE_STATUS"

if [[ $PORT_STATUS -eq 1 || $FILE_STATUS -eq 1 ]]; then
  echo "1" > /tmp/track_file
  exit 1
else
  echo "0" > /tmp/track_file
  exit 0
fi
echo "Script finished with exit code $?" >> /tmp/check_web.log
```

#### Скрипт keepalived.conf

```
global_defs {
        script_user root
        enable_script_security
        }

vrrp_script check_web {
        script "/etc/keepalived/check_web.sh"
        interval 3
        }

vrrp_instance VI_1 {
        state MASTER
        interface enp0s8
        virtual_router_id 111
        priority 255
        advert_int 1
        }

        virtual_ipaddress {
                192.168.1.111/24
        }

        track_script {
                check_web
        }
}
```
