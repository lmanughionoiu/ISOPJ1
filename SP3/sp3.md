---
layout: default
title: "Sprint 3: IAdministració de Dominis i Seguretat"
---

# Configuració Servidor - Client

Primerament, per a poder fer aquesta configuració, necessitarem dos màquines ubuntu, una per a fer la configuració de servidor i una altra per a la configuració de client.

Després, necessitarem crear una nova xarxa Nat, per a tenir les màquines en la mateixa.

![Imatge 16](images/image-16.png)

![imatge 17](images/image-17.png)

![Imatge 18](images/image-18.png)

## Servidor

Una vegada iniciat, el que hem de fer és cambiar la IP a estàtica.

![Imatge 19](images/image-19.png)

Fem un ping per a veure si tenim connexió.

![Imatge 20](images/image-20.png)

Una vegada afegida la IP estàtica, canviarem el hostname.

![Imatge 21](images/image-21.png)

Hem de canviar també al arxiu hosts a la segona IP, el nom que hi havia abans, amb el nom que li hem ficat al hostname, i després afegir una nova linea amb la IP estàtica que li hem ficat i el host, és a dir -> `hostname.domini.cat(com) hostname`, com a la següent captura.

![Imatge 22](images/image-22.png)

### Instal·lació LDAP

Una vegada ja hem configurat aquests arxius, fem un `apt update` per a poder instal·lar el següent paquet.

![Imatge 23](images/image-23.png)

Quan el instal·lesem, li haurem de configurar una contrasenya.

Ara que ja el tenim instal·lat al nostre servidor, l'haurem de modificar, però abans fem un `slapcat` per veure la configuració inicial.

![Imatge 24](images/image-24.png)

#### Configuració LDAP

Si fem un `dpkg-reconfigure` podrem configurar el LDAP com vulguem nosaltres. Aquesta serà la nostra configuració:

![Imatge 25](images/image-25.png)

![Imatge 26](images/image-26.png)

![Imatge 27](images/image-27.png)

![Imatge 28](images/image-28.png)

![Imatge 29](images/image-29.png)

![Imatge 30](images/image-30.png)

Ara ja el tenim configurat com nosaltres volem, però afegirem a part una configuració de UOs, usuaris i grups, que tenim en l'arxiu `arxius.zip`.

Descomprimim el .zip.

![Imatge 31](images/image-31.png)

Modifiquem els arxius `uo.ldif`, `usu.ldif` i `grup.ldif` per a que concordin amb el nostre domini.

![Imatge 32](images/image-32.png)

![Imatge 33](images/image-33.png)

![Imatge 34](images/image-34.png)

Una vegada modificats, els afegim al ldap.

![Imatge 35](images/image-35.png)

![Imatge 36](images/image-36.png)

![Imatge 37](images/image-37.png)

Comprovem amb un `slapcat`.

![Imatge 38](images/image-38.png)

Ara ja tenim l'entorn servidor correctament modificat.


## Client

Haurem de fer el mateix que amb el servidor al principi, modificar la IP a estàtica i comprovar que podem fer-li ping al servidor.

![Imatge 39](images/image-39.png)

![Imatge 40](images/image-40.png)

### Instal·lació LDAP

Aquí també haurem de instal·lar ldap, pero no és el mateix que en el de servidor.

![Imatge 41](images/image-41.png)

### Configuració LDAP

Una vegada instal·lat, ens apareixerà directament per a configurar el LDAP, la meva configuració es la següent.

![Imatge 42](images/image-42.png)

![Imatge 43](images/image-43.png)

![Imatge 44](images/image-44.png)

![Imatge 45](images/image-45.png)

![Imatge 46](images/image-46.png)

![imatge 47](images/image-47.png)

![Imatge 48](images/image-48.png)

![Imatge 49](images/image-49.png)

![Imatge 50](images/image-50.png)

Igualment, si ens hem equivocat o trobem algún error, podem fer un `dpkg-reconfigure ldap-auth-config`.

![Imatge 51](images/image-51.png)

### Modificació fitxers

#### nsswitch

Hem d'afegir `ldap compat` al principi per a que primer comprovi al ldap per fer l’autenticació.

![Imatge 52](images/image-52.png)

#### common-session

Hem d'afegir l'última linea que apareix al fitxer.

![Imatge 53](images/image-53.png)

#### common-password

En aquest fitxer, on aparegui `use_authtok`, ho haurem d'eliminar.

![Imatge 54](images/image-54.png)

Ha de quedar així:

![Imatge 55](images/image-55.png)

#### 50-ubuntu.conf

Afegim el greeter.

![Imatge 56](images/image-56.png)

### Comprovació

Una vegada tenim tot això configurat, per a poder veure si ens hem pogut connectar al servidor, busquem un usuari que només està al servidor quan hem afegit els fitxers `.ldif`.

![Imatge 57](images/image-57.png)

Comprovem que podem entrar.

![Imatge 58](images/image-58.png)

Ara, volem comprovar que puguem entrar per interfície gràfica. 

Hem d'afegir-lo i ficar la contrasenya d'aquest usuari (en aquest cas alu1).

![Imatge 59](images/image-59.png)

![Imatge 60](images/image-60.png)

![Imatge 61](images/image-61.png)

Comprovació final:

![Imatge 62](images/image-62.png)


Ara ja tenim el client i el servidor configurat correctament.


# Servidor Samba

## Teoria

Un servidor Samba és per a compartir recursos, siguen fitxers, impresores, etc. Es pot compartir en clients Linux, Windows, i l'autenticació és a nivell d'usuari, que poden ser usuaris propis de Samba o usuaris de LDAP.

## Servidor

Instalem Samba

![Imatge 1](images/image.png)

Creem carpeta i modifiquem els permisos, el grup i usuari.

![Imatge 2](images/image-2.png)

![Imatge 3](images/image-1.png)

Creem 3 usuaris nous, 1 grup i afegim aquests usuaris al grup.

![Imatge 4](images/image-4.png)

Li fiquem contrasenyes per poder entrar al client.

![Imatge 11](images/image-10.png)

Hem d'afegir un recurs compartit nou al fitxer smb.conf.

![Imatge 5](images/image-5.png)

Ara hem de fer un restart al servei de samba.

![Imatge 6](images/image-3.png)

## Client

Hem de veure primerament si tenim connexió amb el servidor amb un ping.

![Imatge 7](images/image-6.png)

Instalem smbclient.

![Imatge 8](images/image-7.png)

Ara iniciem les proves.

Ens hem de connectar entrant a "Otras ubicaciones" i afegir "smb://ip_servidor/nom_recurscompartit/".

![Imatge 9](images/image-8.png)

Ens hem connectat com anònim i hem creat una carpeta i ha funcionat, això és gràcies als permisos.

![Imatge 10](images/image-9.png)

Ens connectem com naim i provem llegir i crear carpeta, tal i com hem ficat als permisos.

![Imatge 11](images/image-12.png)

![Imatge 12](images/image-13.png)

Ara ens connectem com eros, que pot llegir, pero no crear carpeta.

![Imatge 13](images/image-11.png)

![Imatge 14](images/image-14.png)

Finalment, ens connectem com edgar, que no pot accedir.

![Imatge 15](images/image-15.png)

Quan li donem a connectar, no surt cap error, si no que el que fa es tornar a demanar la autenticació.

## Extra



# Servidor NFS

A nivell servidor

# Exercici LDAP

Per a aquest exercici hem de descarregar un entorn gràfic per a poder administrar LDAP.

Hi han diferents opcions per a descarregar, però jo ho faré amb `Apache Directory Studio (ADS)`.

## Instal·lació ADS

Primerament necessitem instal·lar java.

![Imatge 63](images/image-63.png)

Ara podem descarregar-mos ADS de la seva pàgina web.

![Imatge 64](images/image-64.png)

El descomprimim.

![Imatge 65](images/image-65.png)

Movem a la carpeta `/opt/` la carpeta descomprimida.

![Imatge 66](images/image-66.png)

Donem permisos d'execució.

![Imatge 67](images/image-67.png)

Un pas previ que hem de fer abans d'executar, és modificar el fitxer .ini, ja que la versió de Java que utilitza és la 11 i nosaltres tenim la 17 (instal·lada previament).

![Imatge 68](images/image-68.png)

Executem l'aplicació.

![Imatge 69](images/image-69.png)

Ara que ja tenim l'aplicació instal·lada, hem de crear una connexió de LDAP.

![Imatge 70](images/image-70.png)

Fiquem el nom de connexió que volem i la IP del nostre server.

![Imatge 71](images/image-71.png)

Li donem a next i ara ens demanarà el cn i dc i la contrasenya, que ja la tenim configurada anteriorment.

![Imatge 72](images/image-72.png)

Si li donem a check authentication veurem si es correcte i li donem Finish.

![Imatge 73](images/image-73.png)

Si entrem al LDAP Browser (a la esquerra), podrem veure ja el nostre servidor LDAP configurat.

![Imatge 74](images/image-74.png)

### Proves ADS

Ara procedire a crear un usuari i iniciar sessió per a mostrar el funcionament.

#### Crear usuari

Crearem una nova entrada fent-li clic dret al nostre domini.

![Imatge 75](images/image-75.png)

El crearem de 0.

![Imatge 76](images/image-76.png)

Ara ja podrem crear qualsevol objecte, afegint de la esquerra el tipus de OC que vulguem, crearem la UO.

![Imatge 77](images/image-77.png)

Ara buscarem `ou` per a fer la organizationalUnit i li ficarem un nom.

![Imatge 78](images/image-78.png)

Ara que ja tenim la nova UO creada, creem un usuari.

![Imatge 79](images/image-79.png)

Busquem inetOrgPerson i posixAccount, que són necessaris per a que ens aparegui el usuari al client.

![Imatge 80](images/image-89.png)

Al RDN li ficarem `uid` i li afegim el nom que volem.

![Imatge 81](images/image-81.png)

Per a afegir-li una contrasenya hem de afegir un nou atribut, fent clic dret i nou atribut.

![Imatge 82](images/image-90.png)

Busquem `userPassword`.

![Imatge 83](images/image-83.png)

Ara afegim la contrasenya (el meu cas, usuari), a part, podem afegir un métode de encriptació a la contrasenya si volem.

![Imatge 84](images/image-84.png)

Haurém d'afegir també l'informació que ens sortia en roig.

![Imatge 85](images/image-91.png)

Ara podem veure que ja tenim l'usuari creat.

![Imatge 86](images/image-92.png)

#### Inici sessió client

Iniciem el client i li donem a canviar d'usuari.

Ara li donem a que no està en la llista i afegim el nom d'usuari que li hem ficat al LDAP.

![Imatge 87](images/image-87.png)

![Imatge 88](images/image-88.png)

Afegim també la contrasenya i ja ens crearà l'usuari.

Ara ja podem veure a la terminal si està tot correcte.

![Imatge 89](images/image-93.png)


## Fitxers ldif i comandes ldap (exercici moodle)

### Fes un dpkg-reconfigure slapd al servidor per tal de deixar la base de dades buida i només amb el domini i l’usuari admin creat. Comprova-ho amb un slapcat.

![Imatge 90](images/image-95.png)

### Descarrega l'arxiu dades_pt10.ldif del moodle i amb la comanda ldapadd carrega els usuaris, grups i uos (Compte que el domini és vesper.cat, hauràs de modificar-lo pel teu)

![Imatge 91](images/image-96.png)

### Fes un altre slapcat per tal de comprovar que les dades s'han carregat correctament

![Imatge 92](images/image-97.png)

### 1. Crea un nou usuari directament al domini

![Imatge 93](images/image-98.png)

L'afegim.

![Imatge 94](images/image-104.png)

Ara el podem buscar.

![Imatge 95](images/image-105.png)

### 2. Crea una nova uo anomenada nòmines

![Imatge 96](images/image-102.png)

L'afegim.

![Imatge 97](images/image-103.png)

### 3. Mou l’usuari que has creat dintre de la uo nòmines

![Imatge 98](images/image-108.png)

Afegim i confirmem.

![Imatge 99](images/image-109.png)

### 4. Quants grups hi ha al domini vesper.cat?

![Imatge 100](images/image-110.png)

Hi han 2 grups, informatica i administracio.

### 5. Afegeix l’usuari que has creat dintre d’un dels grups del domini

![Imatge 111](images/image-111.png)

![Imatge 112](images/image-112.png)

### 6. D’un sol cop: Afegeix un nou atribut opcional a l’usuari sergi, modifica el cognom de l’usuari sergi al valor Pallarés

![Imatge 114](image-114.png)

No fiquem accents ja que no apareixerà el valor correctament.

![Imatge 115](images/image-115.png)

### 7. Quants usuaris hi ha dintre de la uo rrhh? Quins són?

![Imatge 116](images/image-116.png)

Hi han 3 usuaris, xavier, enric i sergi.

### 8. Esborra el gidNumber del grup informàtica

![Imatge 117](images/image-118.png)

No el podem eliminar ja que possixGroup necessita obligatoriament aquest atribut.

![Imatge 118](image/image-119.png)

### 9. Quantes uos hi ha al domini vesper.cat?

![Imatge 119](image/image-120.png)

Hi han 3 UOs.

### 10. Modifica el cn de Xavier per Francesc Xavier

![Imatge 120](images/image-121.png)

![Imatge 121](images/image-122.png)

### 11. Esborra la uo nòmines

![Imatge 122](images/image-123.png)

Com volem eliminar aquesta uo, pero no es pot, tal i com es veu, a causa dels usuaris que hi han afegits, eliminem aquests usuaris i tornem a provar.

![Imatge 123](images/image-125.png)

Ara ja no ens apareix ni l'usuari ni el grup. 

### 12. Mostra els usuaris que tinguin com a grup principal el grup administració

Administració té la gid 1002.

![Imatge 124](images/image-126.png)

![Imatge 125](images/image-127.png)

Només està l'usuari Sergi.

### 13. Quin usuari té el uidNumber 1003?

![Imatge 126](images/image-128.png)

No hi ha cap en aquest uidNumber.

### 14. Mostra quins són els usuaris on el seu cognom comenci per R i el seu uidNumber sigui més gran que 1003

![Imatge 127](images/image-129.png)

En aquest cas no ens apareix res, ja que en l'apartat 6 hem modificat el sn de Reus a Pallares i els altres dos tenen un uidNumber menor de 1003.

### 15. Mostra quins usuaris formen part del grup informàtica o aquells usuaris que tinguis de cognom Pallarés

El gidNumber de informàtica és 1001.

![Imatge 128](images/image-130.png)

Ens apareix 3 en total, 2 del grup informàtica i 1 que el seu cognom és Pallares.