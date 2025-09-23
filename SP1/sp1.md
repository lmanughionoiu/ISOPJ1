---
layout: default
title: "Sprint 1: Instal·lació, Configuració Inicial i Programari de Base"
---

## Virtualització i instal·lació del SO Ubuntu
Per iniciar a instal·lar el sistema operatiu que volem a una màquina virtual, hem de buscar els requisits mínims per tindre un bon funcionament de la maquina.
En aquest cas, es fará servir el SO Uunutu 22.04.3, amb uns requisits minims de:
  CPU: 2 vCPUs
  RAM: 2 GB
  Almacenament: 25 GB

Una vegada tenim la nostra imatge del SO i sabem els requisits mínims, podem començar a instalar la nostra imatge virtual. 
Per a fer aquesta virtualització es fará servir VirtualBox.

Primerament, amb el VirtualBox iniciat, tenim una interficie bastant intuitiva per a poder començar a instalar la nostra imatge.

A partir d'aquí començarem a seguir els següents passos:

- Farem clic a la part superior, on hi ha una icona que fica "Nova".

<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/4c877c90-03d6-48f5-b8ce-69a0063a5b9b" />
- Una vegada donat, li fiquem el nom que volem tenir a la nostra máquina virtual i elegirem l'imatge ISO del nostre SO on la tinguessim descarregada.
- Per a poder fer l'instalació es recomanable activar l'opció de saltar la instalació desatenguda, ja que així podrem elegir natros la configuració que volem al SO i les particions del disc dur. Aixi que li donem a la casella de "Skip Unattended Installation".



<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/e5afe89c-75f5-4b23-a224-472dbee1047c" />
· Al seguir endavant, haurém de elegir la cantitat de recursos que li volem donar al hardware. Com abans he dit, necessitarem saber els requisits minims del SO. En el meu cas, per a fer un ús comode i fluit, fare servir 4GB de RAM i 4 vCPUs. 

<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/634c88da-c290-4092-9d06-7d90d3bdf5ad" />
· Seguint amb la configuració, en el meu cas faré servir 80 GB de almacenament virtual, ja que així podré dedicar-li 40 GB a Ubuntu i 40 GB lliures.

· Per a poder fer servir la xarxa, haurem d'entrar a "Paràmetres -> Xarxa", i cambiarem el que ja surt per defecte, "NAT" a "Xarxa NAT". Aquesta decisió l'explicaré a l'apartat "Configuració de la xarxa".

## Llicenciament
## Gestors d'arrencada per a instal·lacions DUALS
## Punts de restauració
## Configuració de la xarxa
## Comandes generals i instal·lacions
