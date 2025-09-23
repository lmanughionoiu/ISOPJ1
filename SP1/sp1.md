---
layout: default
title: "Sprint 1: Instal·lació, Configuració Inicial i Programari de Base"
---

## Virtualització i instal·lació del SO Ubuntu
Per iniciar a instal·lar el sistema operatiu que volem a una màquina virtual, hem de buscar els requisits mínims per tindre un bon funcionament de la màquina.

En aquest cas, es farà servir el SO Ubuntu 22.04.3, amb uns requisits mínims de:
  CPU: 2 vCPUs
  RAM: 2 GB
  Almacenament: 25 GB

Una vegada tenim la nostra imatge del SO i sabem els requisits mínims, podem començar a instal·lar la nostra imatge virtual. 
Per a fer aquesta virtualització es farà servir VirtualBox.

Primerament, amb el VirtualBox iniciat, tenim una interfície bastant intuïtiva per a poder començar a instal·lar la nostra imatge.

A partir d'aquí començarem a seguir els següents passos:

- Farem clic a la part superior, on hi ha una icona que fica "Nova".

<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/4c877c90-03d6-48f5-b8ce-69a0063a5b9b" />

- Una vegada donat, li fiquem el nom que volem tenir a la nostra màquina virtual i elegirem la imatge ISO del nostre SO on la tinguéssim descarregada.
  
- Per a poder fer la instal·lació és recomanable activar l'opció de saltar la instal·lació desavenguda, ja que així podrem elegir nosaltres la configuració que volem al SO i les particions del disc dur. Així que li donem a la casella de "Skip Unattended Installation".

<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/e5afe89c-75f5-4b23-a224-472dbee1047c" />

- En continuar endavant, haurem d'elegir la quantitat de recursos que li volem donar al maquinari. Com he dit, necessitarem saber els requisits mínims del SO. En el meu cas, per a fer un ús còmode i fluid, faré servir 4GB de RAM i 4 vCPUs. 

<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/634c88da-c290-4092-9d06-7d90d3bdf5ad" />

- Continuant amb la configuració, en el meu cas faré servir 80 GB de almacenament virtual, ja que així podré dedicar-li 40 GB a Ubuntu i 40 GB lliures.

- Per a poder fer servir la Xarxa NAT, hem de crear una xarxa primer, i això es fa anant al VirtualBox, on diu "Eines -> NAT Networks" i després li donem a "Crea".

<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/eb228744-f52a-453c-b5fc-3028b22a0739" />

- Ara, haurem d'entrar a les opcions de la nostra màquina virtual, "Paràmetres -> Xarxa", i canviarem el que ja surt per defecte, "NAT" a "Xarxa NAT", ja que la Xarxa NAT és una xarxa virtual compartida que permet que diverses màquines virtuals es vegin entre elles i tinguin accés a internet i l'opció "Nom" ficarem la que hem creat.

<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/3e74a99a-d48f-4192-965f-682086ad6821" />

- Una vegada ja tenim tota la preparació prèvia, iniciem la màquina virtual. Quan ja hagi carregat, ens sortirà la següent pantalla i li premem ENTER a la primera opció "Try or Install Ubuntu".

<img width="1920" height="1080" alt="6" src="https://github.com/user-attachments/assets/12a734cc-06fb-4f9c-a688-e0a6ca8f5c4b" />

- Seguidament ens sortirà per a provar o instal·lar. Elegim instal·lar.

<img width="1920" height="1080" alt="7" src="https://github.com/user-attachments/assets/78c48989-7f3c-4024-bc79-fac443556b59" />

- Elegirem les opcions que volem.

<img width="1920" height="1080" alt="8" src="https://github.com/user-attachments/assets/2ebfae9d-4174-4b03-a41d-3bf99c0e69d2" />

- En aquest cas, com volem crear nosaltres les particions, elegim la segona opció.

<img width="1920" height="1080" alt="9" src="https://github.com/user-attachments/assets/780cd432-38d6-45aa-a698-23775564e665" />

- Per a crear les noves particions, li donem a "New partition table", escollirem "free space" i després li donem al signe "+" per a crear-la.

<img width="1920" height="1080" alt="10" src="https://github.com/user-attachments/assets/0f61629a-c903-42d2-a486-637d0663a6c0" />

- Les particions que faré servir seran les següents:
  - EXT4 / -> 20GB
  - EXT4 /home -> 15GB
  - EXT4 /boot -> 1GB
  - Swap -> 4GB (Si no tens la partició swap i el sistema es queda sense memòria RAM, podria començar a tenir    problemes, com ara lentitud o inclús falles. Però, si tens 8 GB de RAM o més, pots prescindir d'aquesta partició, especialment si no tens pensat dur a terme tasques molt exigents respecte a memòria).

  Especialment en aquest cas, per a poder fer la instal·lació m'està demanant que agregui una partició per a la EFI i una altra per al boot (biosgrub).
  - EFI -> 400MB
  - Biosgrub -> 1MB

<img width="1920" height="1080" alt="19" src="https://github.com/user-attachments/assets/6bfd4138-ee17-46c8-a66f-a2d206c25bc9" />

- Elegim la nostra regió.

<img width="1920" height="1080" alt="20" src="https://github.com/user-attachments/assets/3ec831a2-585f-4f28-9a55-358023220d85" />

- Configurem el nom d'usuari i contrasenya, si escau.

<img width="1920" height="1080" alt="21" src="https://github.com/user-attachments/assets/79ef6304-13b0-4eaf-8902-571e3b3b243d" />

- Finalment, li donarem a continuar i esperarem que s'instal·li. Una vegada instal·lat, ens demanarà reiniciar i ja tindrem la màquina funcionant.

## Llicenciament

Llicència de Ubuntu
### 1. Ubuntu és programari lliure i de codi obert

- Ubuntu està basat en Linux i altres programaris que són lliures i de codi obert.
- Això vol dir que pots usar-lo, copiar-lo, modificar-lo i distribuir-lo lliurement.

### 2. Quina llicència fa servir?

Ubuntu es distribueix sota la llicència **GPL** i inclou un amplíssim catàleg de programes de programari lliure que permeten realitzar qualsevol tipus de tasca.

La **GPL** és una llicència de programari lliure creada per la **Free Software Foundation**.

La versió més habitual per Ubuntu és la **GPLv3** (versió 3), encara que molts projectes també usen la versió 2 o versions posteriors.

Principis bàsics de la **GPL (GNU General Public License)**:
1. Llibertat per usar el programari
   - Pots fer servir el programari per a qualsevol propòsit, personal o comercial.

3. Llibertat per estudiar i modificar el codi font
   - Tens accés al codi font complet, i pots modificar-lo per adaptar-lo a les teves necessitats.

4. Llibertat per distribuir còpies
   - Pots distribuir còpies del programa, tant la versió original com la modificada.

5. Distribució amb la mateixa llicència (copyleft)
   - Si distribueixes una versió modificada, has de fer-ho sota la mateixa llicència GPL.
   - Això assegura que totes les versions futures continuïn sent programari lliure.
   - Aquesta és la part “forta” de la GPL, coneguda com a copyleft.

### 3. Què significa per a l’usuari?

- Pots descarregar Ubuntu i utilitzar-lo sense cap cost.
- Pots instal·lar-lo en tants equips com vulguis.
- Pots modificar-lo o adaptar-lo per a les teves necessitats, sempre respectant les condicions de les llicències dels components.
- Pots compartir còpies amb altres persones.

### 4. La marca i el nom “Ubuntu”

Tot i que el programari és lliure, la marca “Ubuntu” i el logotip estan protegits per Canonical (l’empresa que publica Ubuntu). Això significa que si fas una distribució modificada i la vols anomenar “Ubuntu”, has de complir amb les normes de Canonical, però si només uses o distribueixes el sistema tal qual, no tens problema.

## Gestors d'arrencada per a instal·lacions DUALS
## Punts de restauració
## Configuració de la xarxa
## Comandes generals i instal·lacions
