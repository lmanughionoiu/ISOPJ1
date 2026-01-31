---
layout: default
title: "Sprint 3: IAdministració de Dominis i Seguretat"
---

# Configuració Servidor - Client

Primerament, per a poder fer aquesta configuració, necessitarem dos màquines ubuntu, una per a fer la configuració de servidor i una altra per a la configuració de client.

Després, necessitarem crear una nova xarxa Nat, per a tenir les màquines en la mateixa.

![Imatge 16](image-16.png)

![imatge 17](image-17.png)

![Imatge 18](image-18.png)

## Servidor

Una vegada iniciat, el que hem de fer és cambiar la IP a estàtica.

![Imatge 19](image-19.png)

Fem un ping per a veure si tenim connexió.

![Imatge 20](image-20.png)

Una vegada afegida la IP estàtica, canviarem el hostname.

![Imatge 21](image-21.png)

Hem de canviar també al arxiu hosts a la segona IP, el nom que hi havia abans, amb el nom que li hem ficat al hostname, i després afegir una nova linea amb la IP estàtica que li hem ficat i el host, és a dir -> `hostname.domini.cat(com) hostname`, com a la següent captura.

![Imatge 22](image-22.png)

### Instal·lació LDAP

Una vegada ja hem configurat aquests arxius, fem un `apt update` per a poder instal·lar el següent paquet.

![Imatge 23](image-23.png)

Quan el instal·lesem, li haurem de configurar una contrasenya.

Ara que ja el tenim instal·lat al nostre servidor, l'haurem de modificar, però abans fem un `slapcat` per veure la configuració inicial.

![Imatge 24](image-24.png)

#### Configuració LDAP

Si fem un `dpkg-reconfigure` podrem configurar el LDAP com vulguem nosaltres. Aquesta serà la nostra configuració:

![Imatge 25](image-25.png)

![Imatge 26](image-26.png)

![Imatge 27](image-27.png)

![Imatge 28](image-28.png)

![Imatge 29](image-29.png)

![Imatge 30](image-30.png)

Ara ja el tenim configurat com nosaltres volem, però afegirem a part una configuració de UOs, usuaris i grups, que tenim en l'arxiu `arxius.zip`.

Descomprimim el .zip.

![Imatge 31](image-31.png)

Modifiquem els arxius `uo.ldif`, `usu.ldif` i `grup.ldif` per a que concordin amb el nostre domini.

![Imatge 32](image-32.png)

![Imatge 33](image-33.png)

![Imatge 34](image-34.png)

Una vegada modificats, els afegim al ldap.

![Imatge 35](image-35.png)

![Imatge 36](image-36.png)

![Imatge 37](image-37.png)

Comprovem amb un `slapcat`.

![Imatge 38](image-38.png)

Ara ja tenim l'entorn servidor correctament modificat.


## Client

Haurem de fer el mateix que amb el servidor al principi, modificar la IP a estàtica i comprovar que podem fer-li ping al servidor.

![Imatge 39](image-39.png)

![Imatge 40](image-40.png)

### Instal·lació LDAP

Aquí també haurem de instal·lar ldap, pero no és el mateix que en el de servidor.

![Imatge 41](image-41.png)

### Configuració LDAP

Una vegada instal·lat, ens apareixerà directament per a configurar el LDAP, la meva configuració es la següent.

![Imatge 42](image-42.png)

![Imatge 43](image-43.png)

![Imatge 44](image-44.png)

![Imatge 45](image-45.png)

![Imatge 46](image-46.png)

![imatge 47](image-47.png)

![Imatge 48](image-48.png)

![Imatge 49](image-49.png)

![Imatge 50](image-50.png)

Igualment, si ens hem equivocat o trobem algún error, podem fer un `dpkg-reconfigure ldap-auth-config`.

![Imatge 51](image-51.png)

### Modificació fitxers

#### nsswitch

Hem d'afegir `ldap compat` al principi per a que primer comprovi al ldap per fer l’autenticació.

![Imatge 52](image-52.png)

#### common-session

Hem d'afegir l'última linea que apareix al fitxer.

![Imatge 53](image-53.png)

#### common-password

En aquest fitxer, on aparegui `use_authtok`, ho haurem d'eliminar.

![Imatge 54](image-54.png)

Ha de quedar així:

![Imatge 55](image-55.png)

#### 50-ubuntu.conf

Afegim el greeter.

![Imatge 56](image-56.png)

### Comprovació

Una vegada tenim tot això configurat, per a poder veure si ens hem pogut connectar al servidor, busquem un usuari que només està al servidor quan hem afegit els fitxers `.ldif`.

![Imatge 57](image-57.png)

Comprovem que podem entrar.

![Imatge 58](image-58.png)

Ara, volem comprovar que puguem entrar per interfície gràfica. 

Hem d'afegir-lo i ficar la contrasenya d'aquest usuari (en aquest cas alu1).

![Imatge 59](image-59.png)

![Imatge 60](image-60.png)

![Imatge 61](image-61.png)

Comprovació final:

![Imatge 62](image-62.png)


Ara ja tenim el client i el servidor configurat correctament.


# Servidor Samba

## Teoria

Un servidor Samba és per a compartir recursos, siguen fitxers, impresores, etc. Es pot compartir en clients Linux, Windows, i l'autenticació és a nivell d'usuari, que poden ser usuaris propis de Samba o usuaris de LDAP.

## Servidor

Instalem Samba

![Imatge 1](image.png)

Creem carpeta i modifiquem els permisos, el grup i usuari.

![Imatge 2](image-2.png)

![Imatge 3](image-1.png)

Creem 3 usuaris nous, 1 grup i afegim aquests usuaris al grup.

![Imatge 4](image-4.png)

Li fiquem contrasenyes per poder entrar al client.

![Imatge 11](image-10.png)

Hem d'afegir un recurs compartit nou al fitxer smb.conf.

![Imatge 5](image-5.png)

Ara hem de fer un restart al servei de samba.

![Imatge 6](image-3.png)

## Client

Hem de veure primerament si tenim connexió amb el servidor amb un ping.

![Imatge 7](image-6.png)

Instalem smbclient.

![Imatge 8](image-7.png)

Ara iniciem les proves.

Ens hem de connectar entrant a "Otras ubicaciones" i afegir "smb://ip_servidor/nom_recurscompartit/".

![Imatge 9](image-8.png)

Ens hem connectat com anònim i hem creat una carpeta i ha funcionat, això és gràcies als permisos.

![Imatge 10](image-9.png)

Ens connectem com naim i provem llegir i crear carpeta, tal i com hem ficat als permisos.

![Imatge 11](image-12.png)

![Imatge 12](image-13.png)

Ara ens connectem com eros, que pot llegir, pero no crear carpeta.

![Imatge 13](image-11.png)

![Imatge 14](image-14.png)

Finalment, ens connectem com edgar, que no pot accedir.

![Imatge 15](image-15.png)

Quan li donem a connectar, no surt cap error, si no que el que fa es tornar a demanar la autenticació.

## Extra



# Servidor NFS

A nivell servidor

# Exercici LDAP

Per a aquest exercici hem de descarregar un entorn gràfic per a poder administrar LDAP.

Hi han diferents opcions per a descarregar, però jo ho faré amb `Apache Directory Studio (ADS)`.

## Instal·lació ADS

Primerament necessitem instal·lar java.

![Imatge 63](image-63.png)

Ara podem descarregar-mos ADS de la seva pàgina web.

![Imatge 64](image-64.png)

El descomprimim.

![Imatge 65](image-65.png)

Movem a la carpeta `/opt/` la carpeta descomprimida.

![Imatge 66](image-66.png)

Donem permisos d'execució.

![Imatge 67](image-67.png)

Un pas previ que hem de fer abans d'executar, és modificar el fitxer .ini, ja que la versió de Java que utilitza és la 11 i nosaltres tenim la 17 (instal·lada previament).

![Imatge 68](image-68.png)

Executem l'aplicació.

![Imatge 69](image-69.png)

Ara que ja tenim l'aplicació instal·lada, hem de crear una connexió de LDAP.

![Imatge 70](image-70.png)

Fiquem el nom de connexió que volem i la IP del nostre server.

![Imatge 71](image-71.png)

Li donem a next i ara ens demanarà el cn i dc i la contrasenya, que ja la tenim configurada anteriorment.

![Imatge 72](image-72.png)

Si li donem a check authentication veurem si es correcte i li donem Finish.

![Imatge 73](image-73.png)

Si entrem al LDAP Browser (a la esquerra), podrem veure ja el nostre servidor LDAP configurat.

![Imatge 74](image-74.png)

### Proves ADS

Ara procedire a crear un usuari i iniciar sessió per a mostrar el funcionament.

#### Crear usuari

Crearem una nova entrada fent-li clic dret al nostre domini.

![Imatge 75](image-75.png)

El crearem de 0.

![Imatge 76](image-76.png)

Ara ja podrem crear qualsevol objecte, afegint de la esquerra el tipus de OC que vulguem, crearem la UO.

![Imatge 77](image-77.png)

Ara buscarem `ou` per a fer la organizationalUnit i li ficarem un nom.

![Imatge 78](image-78.png)

Ara que ja tenim la nova UO creada, creem un usuari.

![Imatge 79](image-79.png)

Busquem inetOrgPerson.

![Imatge 80](image-80.png)

Al RDN li ficarem `uid` i li afegim el nom que volem.

![Imatge 81](image-81.png)

Per a afegir-li una contrasenya hem de afegir un nou atribut, fent clic dret i nou atribut.

![Imatge 82](image-82.png)

Busquem `userPassword`.

![Imatge 83](image-83.png)

Ara afegim la contrasenya (el meu cas, usuari), a part, podem afegir un métode de encriptació a la contrasenya si volem.

![Imatge 84](image-84.png)

Haurém d'afegir també el cn i sn.

![Imatge 85](image-85.png)

Ara podem veure que ja tenim l'usuari creat.

![Imatge 86](image-86.png)

#### Inici sessió client

Iniciem el client i li donem a canviar d'usuari.

Ara li donem a que no està en la llista i afegim el nom d'usuari que li hem ficat al LDAP.

![Imatge 87](image-87.png)

![Imatge 88](image-88.png)

Afegim també la contrasenya.

