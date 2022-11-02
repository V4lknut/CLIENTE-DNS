# CLIENTE DNS


## **1. Revise el fichero /etc/host.conf de su estación. ¿Cómo está configurado el equipo? ¿Qué sentido tiene la sentencia order?**

```
smx2a@ada-119:~$ cat /etc/host.conf
# The "order" line is only used by old versions of the C library.
order hosts,bind
multi on
```
### Este equipo está configurado de tal manera que cuando quiera obtener la IP de un nombre de dominio primero consultará el fichero **``/etc/host``** y si no lo encuentra en ese fichero preguntará al **DNS** (bind).  


&nbsp;

## **2. Revise el fichero /etc/resolv.conf de su estación. ¿Qué servidor DNS se utiliza? ¿Cuál es el dominio de búsqueda por defecto? ¿Está ejecutando systemd-resolved?**

```
smx2a@ada-119:~$ cat /etc/resolv.conf
# This is /run/systemd/resolve/stub-resolv.conf managed by man:systemd-resolved(8).
# Do not edit.
#
# This file might be symlinked as /etc/resolv.conf. If you're looking at
# /etc/resolv.conf and seeing this text, you have followed the symlink.
#
# This is a dynamic resolv.conf file for connecting local clients to the
# internal DNS stub resolver of systemd-resolved. This file lists all
# configured search domains.
#
# Run "resolvectl status" to see details about the uplink DNS servers
# currently in use.
#
# Third party programs should typically not access this file directly, but only
# through the symlink at /etc/resolv.conf. To manage man:resolv.conf(5) in a
# different way, replace this symlink by a static file or a different symlink.
#
# See man:systemd-resolved.service(8) for details about the supported modes of
# operation for /etc/resolv.conf.

nameserver 127.0.0.53
options edns0 trust-ad
search ada.elpuig.xeill.net
```
### Como podemos observar en el fichero **``/etc/resolv.conf``**, el servidor DNS que estamos utilizando está escrito en la línea que comienza por **nameserver** y es el que tiene la IP **127.0.0.53**. El número 53 de la IP hace referencia al puerto que utiliza el servicio DNS, que efectivamente es el 53.
### El dominio de búsqueda por defecto es **ada.elpuig.xeill.net**, y lo podemos encontrar en la línea **search**.
### Sí que está ejecutando **systemd-resolved** ya que el fichero está generado por este servicio. Lo podemos observar en la primera línea del fichero: ***``This is /run/systemd/resolve/stub-resolv.conf managed by man:systemd-resolved(8).``***  


&nbsp;

## **3. Utilice el comando netstat para comprobar si el puerto 53 TCP y UDP está abierto por alguna herramienta.** 

```
root@luna:/home/usuario# netstat -putan | grep ":53\ "
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      624/systemd-resolve 
udp        0      0 127.0.0.53:53           0.0.0.0:*                           624/systemd-resolve 
root@luna:/home/usuario#
```
```
smx2a@ada-119:~$ netstat -putan | grep ":53\ "
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        0      0 172.16.0.1:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 192.168.122.1:53        0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
udp        0      0 172.16.0.1:53           0.0.0.0:*                           -                   
udp        0      0 192.168.122.1:53        0.0.0.0:*                           -                   
udp        0      0 127.0.0.53:53           0.0.0.0:*                           -                   
```
### Los puertos 53 TCP y UDP del DNS están **abiertos**, las otras IP son para máquinas virtuales y contenedores. 


&nbsp;

## **4. Utilice el comando resolvectl dns para obtener los servidores DNS que utiliza la máquina. ¿Qué servidores son? ¿De dónde los ha obtenido la máquina? ¿Son servidores recursivos o no recursivos?**

```
smx2a@ada-119:~$ resolvectl dns
Global:
Link 2 (enp2s0): 192.168.12.10
Link 3 (virbr0):
Link 4 (lxdbr0):
```
### El servidor que utiliza la máquina es el que tiene la IP **192.168.12.10**. Este servidor lo ha obtenido del router Cirdan, que es el que se encarga de dar las IP tanto de las máquinas como la del DNS, como la del router. Es un servidor recursivo  


&nbsp;

## **5. Utilice el comando resolvectl status. ¿Qué información muestra?**







&nbsp;

## **6. ¿Qué comando muestra información sobre la cache de systemd-resolved? ¿Qué porcentaje de aciertos ha obtenido su máquina?**


















