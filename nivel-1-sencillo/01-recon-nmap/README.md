# 🎯 Objetivos
- Descubrir la IP de la máquina objetivo en el segmento de red local.
- Realizar un escaneo de puertos completo para mapear la superficie de ataque.
- Identificar versiones de software y scripts básico de auditoría (NSE).

---

### Características

**Nivel:** 1 - Sencillo  

**Entorno:** VMware Lab  

**Máquina Atacante:** Kali Linux 

**Máquina Objetivo:** Metasploitable 2  

**IP Objetivo:** `192.168.192.128`  


---
<br></br>

## 🛠️ Preparación del Lab


Primeramente necesitaremos conocer las direcciones IP de nuestras VMs, tanto de la VM víctima como la del atacante.

Para poder identificar las IP´s haremos lo siguiente:

### 1. Entramos en la VM de Kali y ejecutamos el siguiente comando:

```
ip a
```

- Este nos mostrara de forma rápida y detallada la configuración de todas las redes y direcciones IP asignadas a las interfaces del equipo.

### Resultado

<img src="./assets/IPs.png" alt="">

En este caso lla eth0 es la del adaptador Host-Only y tiene la siguiente dirección IP: **192.168.192.129/24**

<br></br>
### 2. Continuamos con el mismo proceso, pero en la VM de Metasploitable2

```
ip a
```

### Resultado

<img src="./assets/metasploit2.png" alt="">

También esta en el adaptador Host-Only lo cual es importante de cumplir, y tiene la siguiente dirección IP: **192.168.192.128/24**


Teniendo esta información ya podemos continuar con los ejercicios


<br></br>


## 🔎 Descubrimiento de Hosts

Para realizar un descubrimiento de Hosts en la red local realizamos lo siguiente:

```
sudo netdiscover -r 192.168.192.128/24

O también utilizando Nmap:

sudo nmap -sn 192.168.192.128/24
```

<br></br>


## 🔎🚀 Escaneo Rápido de los principales Puertos TCP

Realizar un escaneo de los puertos TCP es una buena manera de saber que servicios TCP estan abiertos, lo cual nos da una buena idea de como proceder

```
sudo nmap -sS -F 192.168.192.128
```

### Resultado

<img src="./assets/tcp" alt="">

Podemos apreciar un total de 18 de los principales puertos TCP abiertos


<br></br>


## 🔎 Escaneo Completo con Detección de Servicios y Versiones

Hacer un escaneo completo es muy útil porque esto nos da muchisima información muy valiosa a la hora de seguir con una posterior explotación

Escanearemos los 65,5355 puertos TCP, detectando versiones de servicios, además de identificar el Sistema Operativo

```
sudo nmap -sCV -p- 192.168.192.128 -oA scan_metasploitable
```

#### 🔧 Modificadores Utilizados

* -sC: ejecuta scripts por defecto de Nmap NSE
* -sV: detecta las versiones de los servicios
* -p-: escanea todos los puertos
* -oA scan_metasploitable: guarda los resultados en 3 formatos (.nmap, .gnmap, .xml)


### Resultado

```
Not shown: 65506 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
21/tcp    open  ftp         vsftpd 2.3.4 (Anonymous FTP allowed)
22/tcp    open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1
23/tcp    open  telnet      Linux telnetd
25/tcp    open  smtp        Postfix smtpd
53/tcp    open  domain      ISC BIND 9.4.2
80/tcp    open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp   open  rpcbind     2 (RPC #100000)
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X
445/tcp   open  netbios-ssn Samba smbd 3.0.20-Debian
512/tcp   open  exec        netkit-rsh rexecd
513/tcp   open  login?
514/tcp   open  shell       Netkit rshd
1099/tcp  open  java-rmi    GNU Classpath grmiregistry
1524/tcp  open  bindshell   Metasploitable root shell
2049/tcp  open  nfs         2-4 (RPC #100003)
2121/tcp  open  ftp         ProFTPD 1.3.1
3306/tcp  open  mysql       MySQL 5.0.51a-3ubuntu5
3632/tcp  open  distccd     distccd v1 ((GNU) 4.2.4)
5432/tcp  open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp  open  vnc         VNC (protocol 3.3)
6000/tcp  open  X11         (access denied)
6667/tcp  open  irc         UnrealIRCd
8180/tcp  open  http        Apache Tomcat/Coyote JSP engine 1.1
8787/tcp  open  drb         Ruby DRb RMI
MAC Address: 00:0C:29:E4:CC:65 (VMware)
Service Info: Host: metasploitable.localdomain; OS: Linux

```


<br></br>


## 📚 Lecciones Aprendidas

* El parámetro **-p-** es fundamental para no omitir servicios expuestos en puertos no estándar
* El uso de **-oA** permite almacenar evidencias en formatos analizables para posteriores reportes e informes
* Metasploitable2 expone una gran variedad de servicios obsoletos y vulnerables por diseño


