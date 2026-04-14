---
layout: default
title: "Sprint 1: Instal·lació, Configuració Inicial i Programari de Base"
---

# Fase 1 - Instal·lació del sistema operatiu

## Pas 1 - Crear màquina virtual amb VirtualBox

![alt text](image.png)

## Pas 2 - Assignar recursos (RAM mínima 4 GB, disc mínim 40 GB)

![alt text](image-1.png)

![alt text](image-2.png)

## Pas 3 - Carregar la ISO de Windows 11

![alt text](<image-2 copy.png>)

## Pas 4 - Instal·lar el sistema (idioma, usuari, contrasenya)

![alt text](image-4.png)

![alt text](image-8.png)

## Pas 5 - Comprovar que arrenca correctament

![alt text](image-3.png)

# Fase 2 - Punts de restauració

## Pas 6 – Cercar “Crear un punt de restauració”

![alt text](image-5.png)

## Pas 7 - Activar la protecció del sistema al disc C:

![alt text](image-6.png)

## Pas 8 - Crear un punt manual

![alt text](image-7.png)

![alt text](image-9.png)

![alt text](image-10.png)

![alt text](image-11.png)

## Pas 9 - Fer un canvi (instal·lar app o configuració)

![alt text](image-19.png)

![alt text](image-12.png)

## Pas 10 - Restaurar i comprovar

![alt text](image-13.png)

![alt text](image-14.png)

![alt text](image-15.png)

![alt text](image-16.png)

![alt text](image-17.png)

![alt text](image-18.png)

# Fase 3 - Llicències de Windows

## Pas 11 - Obrir Configuració → Sistema → Activació

![alt text](image-20.png)

## Pas 12 - Veure si Windows està activat

![alt text](image-21.png)

## Pas 13 - Executar al cmd: slmgr /xpr

![alt text](image-22.png)

## Pas 14 - Esbrinar llicenciament Windows i explicar breument

| Tipus de Llicència | Descripció |
| :--- | :--- |
| **Retail (FPP)** | Llicència comercial comprada per separat. Pertany a l'usuari i no a la màquina. |
| **OEM** | Preinstal·lada en equips nous. Queda vinculada permanentment a la placa base d'aquell PC. |
| **Volum** | Orientada a empreses i institucions. Permet activar múltiples ordinadors simultàniament. |
| **Digital** | Llicència vinculada al maquinari i al compte de Microsoft, substituint la clau tradicional. |
| **Subscripció** | Pagament mensual o anual per usuari (ex. *Windows 365*), utilitzada principalment al núvol. |

## Pas 15 - Concultar preu aproximat d'una llicència Windows (web oficial o botigues)

| Edició de Windows | Microsoft Store (Oficial / Retail) | Botigues de tercers (Claus OEM / Mercat gris)* |
| :--- | :--- | :--- |
| **Windows 11 Home** | ~ 145 € | 10 € - 20 € |
| **Windows 11 Pro** | ~ 259 € | 15 € - 30 € |

*> *Nota important:** Les claus de 10-30€ que es venen per internet solen ser llicències OEM (excedents d'empreses o equips antics). Són legals a la Unió Europea a causa de la normativa de revenda de programari, però quedaran vinculades a la teva placa base per sempre i no inclouen suport tècnic directe de Microsoft.

# Fase 4 - Gestor d'arrencada

## Pas 16 - Obrir Command Prompt com administrador

![alt text](image-23.png)

## Pas 17 - Executar bcdedit

![alt text](image-24.png)

## Pas 18 - Identificar els blocs

### Administrador de arranque de Windows (Boot Manager)

![alt text](image-25.png)

### Cargador de arranque de Windows (Boot Loader)

![alt text](image-26.png)

## Pas 19 - Interpretar dades concretes

### Boot Manager

* **`default`**: `{current}` -> Aquest és el sistema que arrenca per defecte.
* **`timeout`**: `30` -> Aquest és el temps d'espera (en segons) abans d'arrencar automàticament.

### Boot Loader

* **`device`**: `partition=C:` -> És la partició on està instal·lat Windows.
* **`path`**: `\WINDOWS\system32\winload.efi` -> És el fitxer que carrega el sistema.
* **`description`**: `Windows 11` -> És el nom del sistema operatiu.

## Pas 20 - Respostes a les preguntes curtes

* **Quin sistema s'està arrencant?**
  S'està arrencant **Windows 11** (es veu al paràmetre `description` del Cargador d'arrencada).

* **A quin disc o partició està instal·lat?**
  Està instal·lat a la **partició C:** (es veu al paràmetre `device` del Cargador d'arrencada).

* **Quant temps espera abans d'arrencar?**
  Espera **30 segons** (es veu al paràmetre `timeout` de l'Administrador d'arrencada).

* **Quin fitxer inicia Windows?**
  L'inicia el fitxer **`\WINDOWS\system32\winload.efi`** (es veu al paràmetre `path` del Cargador d'arrencada).

## Pas 21 - Interpretació final

* **Qui decideix l'arrencada (Boot Manager):** És el gestor principal que llegeix la configuració inicial i decideix quin sistema operatiu s'ha d'executar, basant-se en l'opció per defecte i el temps d'espera.

* **Qui carrega el sistema (Boot Loader):** És el programa específic (com `winload.efi`) que rep l'ordre del Boot Manager i s'encarrega de carregar físicament el nucli de Windows des del disc dur a la memòria RAM per iniciar-lo.

# Fase 5 - Xarxa bàsica

## Pas 22 - Obrir configuració de xarxa

![alt text](image-27.png)

## Pas 23 - Consultar IP amb: ipconfig

![alt text](image-28.png)

## Pas 24 - Configurar IP dinàmica (DHCP automàtic)

![alt text](image-30.png)

![alt text](image-29.png)

## Pas 25 - Configurar IP fixa (manual: IP, màscara, gateway, DNS)

![alt text](image-31.png)

![alt text](image-32.png)

![alt text](image-33.png)

## Pas 26 - Comprovar connexió amb: ping google.com

![alt text](image-34.png)

# Fase 6 - Comandes generals

## Pas 27 - Obrir PowerShell

![alt text](image-35.png)

## Pas 28 - Diferenciar cmd i PowerShell

### Detalls de l'arrencada
* **BCD (Boot Configuration Data):** És l'arxiu d'on el Boot Manager llegeix la configuració d'arrencada (el substitut modern de l'antic `boot.ini`).
* **Kernel:** El Boot Loader (`winload.efi`) és qui agafa el nucli de Windows (`ntoskrnl.exe`) i els controladors bàsics i els carrega a la memòria RAM.

### Diferències clau: cmd vs. PowerShell
| Característica | cmd (Símbol del sistema) | Windows PowerShell |
| :--- | :--- | :--- |
| **Dades** | Treballa amb **text pla**. | Treballa amb **objectes** (.NET). |
| **Comandes** | Bàsiques i heretades de DOS (`dir`, `ping`). | Utilitza **Cmdlets** amb format *Verb-Nom* (`Get-Process`). |
| **Ús principal**| Comprovacions ràpides de xarxa o sistema. | Automatització de tasques i scripts per a administradors. |

## Pas 29 - Comandes bàsiques (provar-les)

### dir → veure fitxers

![alt text](image-36.png)

### cd → moure's per carpetes

![alt text](image-38.png)

### mkdir prova → crear carpeta

![alt text](image-39.png)

### echo hola > fitxer.txt → crear fitxer

![alt text](image-40.png)

### del fitxer.txt → eliminar fitxer

![alt text](image-41.png)

## Pas 30 - Comandes útils del sistema

### tasklist → veure processos actius

![alt text](image-42.png)

### taskkill /IM notepad.exe /F → tancar un procés

![alt text](image-43.png)

![alt text](image-44.png)

### systeminfo → informació completa del sistema

![alt text](image-45.png)

### hostname → nom de l'equip

![alt text](image-46.png)

### whoami → usuari actual

![alt text](image-47.png)

## Pas 31 - Comandes de xarxa

### ipconfig → veure configuració IP

![alt text](image-48.png)

### ping google.com → comprovar connexió

![alt text](image-49.png)

### netstat -an → connexions obertes

![alt text](image-50.png)

## Pas 32 - Comandes interessants (una mica més avançades)

### tree → veure estructura de carpetes

![alt text](image-51.png)

### cls → netejar pantalla

![alt text](image-52.png)

![alt text](image-53.png)

### help → veure ajuda

![alt text](image-54.png)

### shutdown /s /t 0 → apagar l'equip

![alt text](image-55.png)

![alt text](image-56.png)

## Pas 33 - Mini interpretació

### tasklist

* **`tasklist`**: Mostra una llista de tots els processos i programes que s'estan executant actualment al sistema.

### ipconfig

* **`ipconfig`**: Mostra la configuració bàsica de xarxa de l'equip (adreça IP, màscara de subxarxa, porta d'enllaç, etc.).

### systeminfo

* **`systeminfo`**: Mostra informació detallada del maquinari i del sistema operatiu (versió de Windows, memòria RAM, processador, temps d'activitat, etc.).

# Fase 7 - Instal·lació d'aplicacions

## Pas 34 - Descarregar un programa des del navegador (ex: Chrome o VS Code)

![alt text](image-57.png)

## Pas 35 - Instal·lar-lo seguint l'assistent

![alt text](image-58.png)

![alt text](image-59.png)

## Pas 36 - Obrir-lo i comprovar que funciona

![alt text](image-60.png)

## Pas 37 - Instal·lar una aplicació des de Microsoft Store

![alt text](image-61.png)

![alt text](image-62.png)

## Pas 38 - Obrir-la i comprovar funcionament

![alt text](image-63.png)

## Pas 39 - Desinstal·lar una aplicació: Configuració → Aplicacions → Desinstal·lar

![alt text](image-64.png)

![alt text](image-65.png)

![alt text](image-66.png)

![alt text](image-67.png)

![alt text](image-68.png)

## Pas 40 - Verificació: Comprovar que el programa ja no apareix al sistema

![alt text](image-69.png)

![alt text](image-70.png)
