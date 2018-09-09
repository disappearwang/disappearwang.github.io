---
layout: post
title:  "Architecture Applicatif"
date:   2018-08-24 16:00:00 +0200
categories: Architect
---
le F5, c'est le firewall
c'est lui qui porte l'ip publique apache
le client tape sur l'url, l'url est traduite en IP publique, il atterit sur le F5
ensuite le F5 a des pools de serveurs apache
une IP Publique=n serveurs apache
c'est comme ca que j'ai monté le domaine kate pour des clients GDS pour WDI
(KATE = solution interne qui permet d'aiguiller les flux apache vers différents datacenter, Lille et SDN sur mon schema)
ce sont les pools apache, avec le ip privée (une ip privée par apache)
et après l'apache envoie sur l'haproxu
haproxy (load balancer)
et après on arrive (enfin) sur les tomcat, ou les jar pour TAC
les 2 apache pointent sur 1 HAP
techniquement ils pointent sur une VIP Haproxy : une virtual IP
la VIP haproxy connait 2 IP haproxy : un master, qui marche toujours et qui répond, et un slave, qui prend le relais si le master tombe
c'est une ligne de vie qui est testée : en gros un pingvoila c'est le keepalive qui surveille la VIP et qui teste les IP des 2 haproxy
des qu'un ne répond plus, il bascule sur l'autre.
tout est doublé en prod
les apache, les HAP, les serveurs applis
