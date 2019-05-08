Despliegue de Grafana en host Bastion (+InfluxDB + CollectD + Plugins)
========================================



Descripcion
--------------------------------

* Este documento explica el proceso para ejecutar la instalación completa de un sistema de monitorización y alarmado basados en Grafana.
* Empleamos CollectD desplegado en todos los nodos para la recolección de metricas
* Las metricas son almacenadas en una base de datos de series en el bastion (InfluxDB)

(ver [TODO.txt](https://github.com/antoniohernan/deploy_monitor/blob/master/TODO.txt) )



Pre-requisitos
--------------------------------

El sistema operativo del nodo bastion debe ser **CentOS/7**

En este nodo bastion debemos instalar el repositorio EPEL y Ansible (ansible 2.7.10 o posterior)

```bash
[root@BASTION ~]# yum -y install epel-release
[root@BASTION ~]# yum -y install ansible
```

Debemos generar y distribuir la clave SSH en todos los nodos

```bash
[root@BASTION ~]# ssh-keygen -t rsa
[root@BASTION ~]# ssh-copy-id -i /root/.ssh/id_rsa root@<NODO>
```

Una vez configurado el inventario de hosts debemos probar la conexión con todos ellos

```bash
[root@BASTION ansible]# ansible all -i ./inventories/hosts.yml -m ping -o
BASTION | SUCCESS => {"changed": false, "ping": "pong"}
PROWEB02 | SUCCESS => {"changed": false, "ping": "pong"}
PROAPP02 | SUCCESS => {"changed": false, "ping": "pong"}
PROWEB01 | SUCCESS => {"changed": false, "ping": "pong"}
PROAPP01 | SUCCESS => {"changed": false, "ping": "pong"}
PROMSQL01 | SUCCESS => {"changed": false, "ping": "pong"}
```



## Diagrama

![alt text](https://github.com/antoniohernan/deploy_monitor/blob/master/Deploy_Monitor.png)



Detalles
--------
Inventario:

Debemos indicar los interfases de red que generarán métricas en InfluxDB para todos los hosts.

```yaml
[bastion]
BASTION ansible_host=192.168.0.99 host_name=BASTION net_iface=eth1
```



Descarga de este proyecto
-----------------------------------------

```bash
git clone https://github.com/antoniohernan/deploy_monitor.git
```


Despliegue
-----------------------------------------
```bash
ansible-playbook -i ./inventories/hosts.yml ./install_monitoring.yml
```



Acceso a consola de Grafana
-----------------------------------------
http://bastionhostip:3000

Usuario : admin

Clave : admin!

