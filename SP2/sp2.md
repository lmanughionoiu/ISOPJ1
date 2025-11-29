---
layout: default
title: "Sprint 2: Instal·lació, configuració de programari de base i gestió de fitxers"
---

# Sistemes de fitxers i particions

## Mides

### Mida sector

És la unitat mínima d'emmagatzematge físic al disc dur. Ve determinada pel fabricant del hardware i no la pots canviar fàcilment.

- Com funciona: El disc està dividit físicament en petites caselles anomenades sectors.
- Mida estàndard: Tradicionalment eren 512 bytes, però els discos moderns utilitzen el "Format Avançat" de 4096 bytes (4KB) per ser més eficients.

### Mida block

És la unitat mínima d'assignació que utilitza el sistema de fitxers (NTFS, EXT4, FAT32) per guardar dades.

- Com funciona: El sistema operatiu agrupa diversos sectors físics per crear un "Cluster" (en Windows) o "Bloc" (en Linux). Quan guardes un fitxer, el sistema li assigna un o més clusters sencers. Un fitxer mai pot compartir un cluster amb un altre fitxer.
- Mida habitual: Normalment 4KB (4096 bytes), però pot variar (512B, 64KB, etc.) segons com formatis el disc.

### Comprobacions

Si fem **fdisk -l** podem veure la mida del sector a la nostra partició.

![Imatge 19](images/19.png)

Per mirar la mida del bloc de la nostra partició utilitzem **tune2fs** i el filtrem per Block

![Imatge 20](images/20.png)

## Fragmentacions

### Fragmentació interna

Ocorre quan un fitxer és més petit que la mida del cluster, o quan l'últim tros d'un fitxer no omple del tot el seu últim cluster. Com que un cluster no es pot compartir, l'espai sobrant es perd.

Per exemple:
- Tenim un cluster de 4 KB i guardem un fitxer de text petit de només 1 KB. El fitxer ocupa tot el cluster de 4 KB.
- Resultat: Hem malgastat 3 KB d'espai. Això és fragmentació interna.

Això ho podem comprobar fent un **echo "Bon dia" > Hola**, que és crear un arxiu de text amb una línea de text que diu Bon dia.

Podem veure a la captura que la mida del fitxer es de 4 KB, que és la mida de un bloc.

![Imatge 21](images/21.png) 

### Fragmentació externa

Ocorre quan l'espai lliure al disc no és contigu (està separat en trossets petits) o quan un fitxer gran es guarda en trossos separats físicament pel disc perquè no hi havia un forat prou gran per posar-lo tot junt.

Per exemple:
- Volem guardar un vídeo gran, el sistema operatiu busca espai, però només troba forats petits entre altres fitxers. Per això, el sistema parteix el vídeo en 5 trossos i els posa en diferents llocs del disc.

Això ocorre especialment als discs mecànics HDD, ja que ha de moure el capçal lector d'un lloc a l'altre per llegir el fitxer sencer. Això alenteix molt el sistema.

Per altra banda, en els discos SSD, la fragmentació externa no afecta tant el rendiment perquè no tenen parts mòbils.

Per a veure si una partició necessita fragmentació externa utilitzem la comanda **e4defrag** i ens ho dirà.

![Imatge 22](images/22.png)

## Tipus de formateig

- **Baix nivell:** Borra sistema de fitxers, borra formateig, etc. És a dir, que borra totes les dades i el deixa com a nou.
Des del sistema operatiu no es pot formatar, es necessiten programes adients.

- **Mig nivell:** Només borra sistema de fitxers pero si hi han sectors defectuosos els marca pero no els arregla.

- **Alt nivell:** El format d'alt nivell només borra el sistema de fitxers.

## Gestió de particions

La gestió de particions és l'acció de modificar l'estructura lògica d'un disc dur o SSD un cop ja està en funcionament o quan el preparem per primera vegada.¡

### GPARTED

GParted (GNOME Partition Editor) és una eina per gestionar particions.

#### Característiques Clau:

**Visual i intuïtiu:** Mostra el disc com una barra horitzontal de colors. Cada color representa una partició diferent, i l'espai gris és l'espai buit. Això fa molt fàcil entendre com està organitzat el disc.

**Universal:** Entén gairebé tots els sistemes de fitxers existents:
- Windows: NTFS, FAT32.
- Linux: EXT4, Btrfs, XFS.
- Apple: HFS+ (i parcialment APFS).

***Fes sempre una còpia de seguretat (Backup) abans d'obrir GParted.***

![Imatge 23](images/23.png)

### Comandes

Amb la comanda **fdisk** podem crear particions i modificarles a través de linea de comandes a la nostra terminal.

![Imatge 25](images/25.png)

Ara premem la tecla "n" i després premem enter (per a deixar-ho en default) i després fiquem la mitat de la mida del disc per a fer la partició, en aquest cas, 26000000.

![Imatge 26](images/26.png)

Acte seguit, ficarem "n" i li premem enter a totes les opcions. Finalment escrivim "w" per a començar a escriure la partició.

![Imatge 27](images/27.png)

![Imatge 28](images/28.png)

Ara hem de formatar amb una mida de block diferent la primera partició amb **mkfs.ext4 -b 2048 /dev/sdb1**.

![Imatge 29](images/29.png)

Ho comprobem fent **tune2fs -l /dev/sdb1 | grep Block**.

![Imatge 30](images/30.png)

La segona partició la fem en format NTFS amb la comanda **mkfs.ntfs /dev/sdb2**

![Imatge 31](images/31.png)

Comprobem tots aquestos passos al GPARTED.

![Imatge 32](images/32.png)

### Muntatge

Per a poder fer servir aquestes particions que hem creat, les haurem de montar. Si ara creem una carpeta i creem un arxiu dins i montem la partició, veurem que el arxiu creat ja no es trobarà.

![Imatge 33](images/33.png)

![Imatge 34](images/34.png)

Per a fer que estos cambis que fem siguin permanents, hem d'entrar al fitxer ***/etc/fstab*** i editarlo amb la següent linea.

![Imatge 35](images/35.png)

Si ara reiniciem i mirem la carpeta, els arxius segueixen allí.

![Imatge 36](images/36.png)

# Gestió de procesos



# Gestió d'usuaris i grups i permisos

### Usuari

Un ***usuari*** és qualsevol entitat que pot executar processos (programes) i ser propietària de fitxers.

El sistema operatiu no t'identifica pel teu nom (ex: "Joan"), sinó per un número únic anomenat UID.

Podem classificar-los en tres nivells segons el seu poder i el seu UID:

- **Root (Superusuari)**
    - **UID:** 0
    - **Descripció:** Té control total, pot llegir/esborrar qualsevol fitxer i aturar qualsevol procés.
- **Usuaris del sistema**
    - **UID:** 1 - 999
    - **Descripció:** Són usuaris creats perquè els serveis funcionin. Exemple: l'usuari "Jesus". No poden fer "login" (no tenen pantalla).
- **Usuaris Normals**
    - **UID:** 1000+
    - **Descripció:** Són usuaris creats per a les persones. Tenen una carpeta personal (/home/joan) i no poden modificar fitxers del sistema sense permís.

Tots aquests usuaris es llisten al fitxer de text ***/etc/passwd***.

### Grup

Un ***grup*** és una col·lecció lògica d'usuaris que comparteixen certs permisos. Els grups s'identifiquen internament amb un GID (Group ID). La funció dels grups és facilitar l'administració.

Tipus de grups per a un usuari:

- Quan crees un usuari, aquest pot pertànyer a diversos grups alhora:
    - **Grup principal:**
        - És obligatori.
        - Normalment té el mateix nom que l'usuari. Si crees l'usuari Cire, es crea automàticament el grup Cire.
        - Quan Cire crea un fitxer nou, aquest fitxer pertanyerà automàticament al grup Cire.
    - **Grups secundaris:**
        - Són opcionals.
        - Serveixen per donar privilegis extra.
        - ***Exemple:*** Si vols que Cire pugui usar l'ordre sudo, l'afegeixes al grup sudo. Si vols que pugui usar Docker, l'afegeixes al grup docker.

La definició de grups està al fitxer ***/etc/group***.


## Directoris i fitxers importants

### Directoris

#### /etc/skel

El nom ve de "Skeleton" (Esquelet). És la plantilla base per als nous usuaris.

- **Com funciona:** Quan crees un usuari nou (amb l'ordre useradd -m usuari), el sistema no crea la seva carpeta /home/usuari buida. El que fa és copiar automàticament tot el contingut de /etc/skel dins de la nova carpeta de l'usuari.
    - Si mirem a dins, normalment veurem fitxers ocults de configuració:
        - ***.bashrc:*** Configuració de la terminal (colors, àlies).
        - ***.profile:*** Variables d'entorn.
        - ***.bash_logout:*** Què passa quan tanques la sessió.

Modificar /etc/skel només afecta els usuaris futurs. Els usuaris que ja existeixen no rebran els canvis.

![Imatge 51](images/51.png)

Quan fem un adduser agafa aquests directoris.

![Imatge 52](images/52.png)

Si creem una carpeta compartida dins de /etc/skel, podem veure que als nous usuaris que creem, la carpeta també es crea.

![Imatge 53](images/53.png)

### Fitxers importants

#### /etc/passwd

És un fitxer de text pla que conté la llista de tots els comptes creats al sistema.

![Imatge 1](images/1.png)

![Imatge 2](images/2.png)

Cada línia del fitxer correspon a un usuari. Els camps estan separats per dos punts ( : ).

***manu(nomusuari):x(contra):1000:1000(identificador de grup principal):manu:/home/manu:/bin/bash***

Per a explicar més detalladament cada punt, tenim que:

- **Nom d'usuari (manu):** És el nom que fem servir per fer login.
- **Contrasenya (x):** Indica que la contrasenya està encriptada i guardada a /etc/shadow. Si aquí no hi hagués una x, l'usuari no tindria contrasenya.
- **UID (1000):** El "User ID". El DNI numèric de l'usuari, explicat abans.
- **GID (1000):** El "Group ID". Identifica el grup principal d'aquest usuari.
- **GECOS / Comentari (Manu):** És un camp de text lliure. Normalment s'hi posa el nom complet de la persona, però pot contenir el número de telèfon o despatx.
- **Directori Personal (/home/manu):** És la carpeta de l'usuari quan entra al sistema. Si canviem això, l'usuari anirà a parar a un altre lloc en fer login.
- **Shell (/bin/bash):** És el programa que s'executa quan l'usuari entra.
    - Per a usuaris normals sol ser /bin/bash o /bin/zsh.
    - Per a usuaris de sistema sol ser /usr/sbin/nologin o /bin/false.


#### /etc/shadow

Aquest fitxer conté la informació de les contrasenyes i la seva caducitat. La diferència clau amb l'anterior és que només l'usuari root el pot llegir.

![Imatge 3](images/3.png)

Per a explicar més detalladament cada punt, tenim que:

- **Nom d'usuari (manu):** La clau per enllaçar amb /etc/passwd.
- **El hash de la contrasenya:** Aquest és el camp llarg i estrany que comença per $. Això **NO** és la contrasenya, és el resultat d'una operació matemàtica sobre ella.
- **Camps de caducitat (La resta de números):**
    - Quan es va canviar la contrasenya per últim cop (des de 1970).
    - Dies mínims abans de poder canviar-la de nou.
    - Dies màxims abans que caduqui (obligar a canviar-la).
    - Dies d'avís abans que caduqui.

#### /etc/group

Aquí estan tots los grups del sistema i a quin grup el usuari está.

![Imatge 4](images/4.png)

Per a explicar més detalladament cada punt, tenim que:

- **Nom del grup:** Tal i com diu, el nom identificatiu del grup.
- **Contrasenya de grup (x):**
    - La x indica que, si n'hi hagués, estaria guardada de forma segura a /etc/gshadow.
    - Serveix perquè un usuari que no és del grup pugui entrar-hi temporalment amb la comanda newgrp si sap la clau.
- **GID:** L'identificador numèric únic. És el que fa servir el nucli (Kernel) de Linux internament.
- **Llista de membres:**
    - És una llista d'usuaris separats per comes (sense espais).

#### /etc/gshadow

Veem qui forma part del grup i qui es l'administrador. Igual que /etc/shadow, aquest fitxer només el pot llegir l'usuari root.

![Imatge 5](images/5.png)

Per a explicar més detalladament cada punt, tenim que:

- **Nom del grup:** L'enllaç amb /etc/group.
- **Contrasenya encriptada:**
    - Si aquí hi ha un hash, el grup té contrasenya.
    - Si hi ha un ! o un *, el grup no té contrasenya (está bloquejat), que és el més habitual.
- **Administradors del grup:**
    - Aquesta llista d'usuaris té permís per afegir o treure gent d'aquest grup concret utilitzant la comanda gpasswd.
- **Membres del grup:**
    - Llista d'usuaris que pertanyen al grup (igual que a /etc/group).

## Comandes bàsiques

En Linux, podem gestionar els usuaris amb interfície gràfica o comandes.

### Interfície gràfica

Des de configuració, podem entrar a creació d'usuaris i afegir un usuari nou.

![Imatge 37](images/37.png)

A la següent pantalla podem fer ja el nou usuari, afegint **nom, nom d'usuari, contraseña** i si volem que sigui **administrador**.

![Imatge 38](images/38.png)

Una altra opció per a crear usuaris i que també podem gestionar grups, amb interfície gràfica, és amb el ***gnome-system-tools***.

*Hem d'instal·lar l'aplicació amb: **sudo apt install gnome-system-tools***

Una vegada instalada, la tindrém al nostre tauler d'aplicacions. L'iniciem.

![Imatge 39](images/39.png)

Com es pot veure en totes les opcions que apareixen, podem fer tot el que necessitem amb interfície gràfica.

![Imatge 40](images/40.png)

### Comandes Usuaris

#### adduser

La forma més comú de crear un usuari amb comandes, es fa amb la comanda ***adduuser***. Aquesta comanda ens demanarà la *contrasenya, nom complet, num de habitació, etc*.

![Imatge 6](images/6.png)

Podem fer la comprobació de l'usuari creat, mirem tots els arxius que hem mencionat amb anterioritat per a veure tots els seus valors, a part de la creació de la carpeta home de l'usuari que hem creat.

![Imatge 7](images/7.png)

A part, una vegada creat l'usuari, podem entrar per interfície gràfica a l'usuari.

![Imatge 41](images/41.png)

#### useradd

A banda de la comanda anterior, aquesta comanda ens deixa agregar l'usuari sense preguntar cap paràmetre, ja que els haurem d'anar afegint. S'ha de crear la carpeta home, cambiar de permisos, afegir contrasenya i cambiar d'interpret, ja que l'interpret que li fica de base és l'antic *SH*.

![Imatge 42](images/42.png)

Per a cambiar l'interpret, de *SH* a *BASH*, hem d'utilitzar la comanda ***usermod*** amb el paràmetre ***-s***.

![Imatge 11](images/11.png)

Per a poder iniciar sessió gràficament, haurém d'afegir la carpeta ***/home*** i cambiar els permisos i després establir una contrasenya.

Per a crear la carpeta home hem de fer les següents dos comandes:

![Imatge 43](images/43.png)

![Imatge 44](images/44.png)

Ara creem la contrasenya:

![Imatge 12](images/12.png)

Comprobem que a ***/etc/shadow*** apareix que ja té contrasenya.

![Imatge 45](images/45.png)

Una vegada fet això, comprobem que l'usuari apareix.

![Imatge 13](images/13.png)

#### deluser

Si volem eliminar un usuari que hem creat amb anterioritat, podem fer servir aquesta comanda, la part negativa és que la carpeta /home de l'usuari quedarà intacta. Pot fer-se servir si no volem eliminar els seus arxius per si creem l'usuari de nou en un futur.

![Imatge 8](images/8.png)

#### userdel

A diferència de l'anterior, per a poder eliminar també la carpeta /home d'aquell usuari, fem servir la comanda ***userdel -r***, és important afegir el **-r**, ja que, si el deixem igual, no eliminaria tampoc la carpeta de l'usuari.

![Imatge 9](images/9.png)


#### Bloqueijar i desbloqueijar un usuari

Una cosa que podem fer per a poder gestionar un usuari, és poder bloqueijar un usuari per a que no pugui iniciar sessió.

Amb la comanda **usermod -L "usuari"** podem bloqueijar l'usuari. Si ens fixem, a la contrasenya nova hi ha un **!** davant de la contrasenya, mostrant que l'usuari no podrà entrar al seu usuari. 

![Imatge 14](images/14.png)

Amb la comanda **usermod -U "usuari"** podem desbloqueijar l'usuari.

![Imatge 15](images/15.png)

### Comandes Grups

#### addgroup

Aquesta és la forma més fàcil de crear un grup, fent un ***addgroup nomgrup***.

![Imatge 16](images/16.png)

A part, podem cambiar el nom del grup, tal i com es veu, amb la comanda ***groupmod -n nomgrup nomgrupnou***.

#### delgroup

Per eliminar un grup, és tan simple com fer un ***delgroup nomgrup***.

![Imatge 17](images/17.png)

#### groupadd

Aquesta comanda és igual que el *addgroup*, pero aquí podem especificar més coses, com ara, si fiquem un *-g*, podrem especificar un GID específic per al grup (>1000).

![Imatge 46](images/46.png)

Si fiquem un *-r*, direm que aquell grup serà de sistema. GID <1000.

![Imatge 47](images/47.png)

#### adduser

Per a poder afegir un usuari a un grup que tenim creat, tal i com hem vist a la creació d'usuaris, aquesta comanda té la mateixa sintaxi, però, després del nom de l'usuari (que ja estigui creat), fiquem el nom del grup (ja creat amb anterioritat).

Aquestes són 3 formes d'afegir un usuari a un grup.

![Imatge 18](images/18.png)

Com podem veure, l'usuari Cire, ja està al grup que l'hem afegit.

![Imatge 19](images/54.png)

### Chage

Aquesta comanda ens permet visualitzar i modificar la caducitat i expiració d'un usuari.

![Imatge 48](images/48.png)

Com veem, la cuenta no caduca mai, podem ficar un ***-E 0*** per afegir que la cuenta hagi caducat, ja que ficarà la data més anterior.

![Imatge 49](images/49.png)

Si intentem entrar a l'usuari, no ens deixarà.

![Imatge 50](images/50.png)

Per a traure la caducitat, fem un ***-E -l***.

## Permissos

### UGO

A Linux, tot fitxer i directori té 3 nivells d'accés definits per a 3 tipus de persones.

- Quan mirem els permisos, sempre s'apliquen en aquest ordre estricte:
    - **U - User (Propietari):** L'amo del fitxer. Té els permisos més alts.
    - **G - Group (Grup):** El grup assignat al fitxer. Tots els usuaris que formin part d'aquest grup compartiran aquests permisos.
    - **O - Others (Altres):** Tota la resta. Qualsevol usuari del sistema que no sigui el propietari ni pertanyi al grup.

Per a poder veure quins permisos tenen els arxius, fem un ***ls -l***.

![Imatge 55](images/55.png)

Tal i com està separat, tenim que:

- **1r caràcter:** El tipus d'aquell arxiu.
    - **d =** Directori
    - **- =** Fitxer
- **2n, 3r i 4t caràcter:** Usuari
    - El que pot fer el propietari.
- **5è, 6è i 7è caràcter:** Grup
    - El que pot fer el grup.
- **8è, 9è i 10è caràcter:** Altres
    - El que pot fer la resta.

Ara bé, que vol dir cada lletra? Tenim les següents:

**Lletra_____Valor_____Significat en FITXER_______________Significat en CARPETA**

**R (Read)**_____4_______Pots obrir i llegir el contingut._______Pots llistar els fitxers de dins (ls).

**W (Write)**____2_______Pots modificar i desar el fitxer._______Pots crear i esborrar fitxers a dins.

**X (Execute)**___1_______Pots executar-lo com a programa.____Pots entrar a la carpeta (cd).

**-**____________0_______Cap permís._______________________Cap permís.

Per a canviar els permisos, podem aplicar la matemàtica, ja que serà mes fàcil una vegada féssem ús de la comanda *chmod* per a canviar els permisos.

Tenim que:
- **R =** 4
- **W =** 2
- **X =** 1

Exemple:
- **7 (4+2+1) =** rwx (Tot permès: Llegir, escriure i executar)
- **6 (4+2) =** rw- (Permès llegir i escriure, però no executar)
- **5 (4+1) =** r-x (Permès llegir i executar, però no escriure)
- **4 (4) =** r-- (Només llegir)
- **0 =** --- (Accés denegat al arxiu)

#### Canviar permisos

##### CHMOD

Per a canviar permisos en chmod, tenim dues formes:

- **Mode numèric**
    - Aquest és el més rapid, ja que podrem canviar tots els permisos (Usuari, grup i altres) en una mateixa comanda.
        - Exemple: ***chmod 750 fitxer***

            Tenim aquest fitxer, amb els permisos base que es creen amb la màscara d'usuari (ho veurém mes endavant: *umask*).
            
            ![Imatge 56](images/56.png)

            Una vegada apliquem la comanda abans dita tenim:

            ![Imatge 57](images/57.png)

            Com podem veure, hem aplicat que per als primers 3 dígits (7 - Tots els permisos al administrador), següents 3 (5 - Llegir i executar per al grup) i els últims 3 (0 - Cap permís per a altres).

- **Mode simbòlic**
    - Aquest pot ser un poc més fàcil de recordar, ja que podem afegir permisos i treure a uns en concret amb les lletres respectives.
        - Exemple: ***chmod u+r fitxer***

            **u =** user |
            **g =** group |
            **o =** others |
            **a =** all

            En aquest exemple afegim al usuaris el permís de llegir.

            ![Imatge 58](images/58.png)

##### CHOWN

Aquesta comanda el que fa és canviar el propietari i el grup del fitxer.

La comanda és: ***chown usuari:grup fitxer***

Si, a part fiquem abans de "usuari:grup" un *-R*, farem que aquest canvi sigui recursiu, és a dir, que tots els arxius de dins de la carpeta també s'apliqui així.

![Imatge 59](images/59.png)

#### Exemple d'ús

Primerament, creem la carpeta palomes i el fitxer prova, tenim els següents permisos:

![Imatge 71](images/71.png)

Si fem a un dels nostres usuaris, la comanda ***chown -R nick prova***, ficara l'usuari que hem ficat (nick) com a propietari, pero el grup queda igual.

![Imatge 72](images/72.png)

Ara, si fem ***chown -R nick:nick prova*** canviarem l'usuari i el grup a nick.

![Imatge 73](images/73.png)

Fem el mateix per a la carpeta palomes.

![Imatge 74](images/74.png)

Ara, el que fem es un ***chgrp -R paloma palomes***, per a canviar el grup de la carpeta *palomes* a *paloma*.

![Imatge 75](images/75.png)

Afegim l'usuari *cire* al grup paloma i fiquem els permisos 750 a la carpeta *palomes*.

![Imatge 76](images/76.png)

Traem de la carpeta *palomes*, del *other* el permís *read*.

![Imatge 77](images/77.png)

Ens connectem amb l'usuari *nick* i fem les comprobacions adients, veem que podem entrar a la carpeta, fer un *ls* i crear un fitxer.

![Imatge 78](images/78.png)

Però, si ara entrem amb l'usuari *cire*, veem que el que no podem fer es ni crear un arxiu ni borrar, és a dir, modificar.

![Imatge 79](images/79.png)

Ara, si intentem entrar a la carpeta palomes amb un usuari que no està al grup paloma, no podrem veure res d'aquesta carpeta.

![Imatge 80](images/80.png)

### Especials

#### SUID

Permet a usuaris normals fer tasques d'administrador puntualment.

S'aplica amb ***chmod 4777 fitxer***, ja que el SUID té com a bit per devant un 4.

El fitxer acabaría amb aquests permisos: rw**s**rwxrwx

#### SGID

Aquest permís és diferent per a directoris i fitxers. S'aplica amb ***chmod 2777 arxiu***, ja que el bit és 2.

- **Directoris:**
    - Si activem aquest permís, qualsevol fitxer nou creat dins d'aquest directori, heretarà el grup de la carpeta pare, no de l'usuari que ha creat el fitxer.

- **Fitxers:**
    - És paregut a la SUID, però en lloc d'executar-se com el propietari, s'executa amb els permisos del grup. 

El fitxer o directori acabaría amb aquests permisos: rwxrw**s**rwx

#### Sticky bit

L'sticky bit és necessari per evitar que els usuaris esborrin fitxers que no són seus en carpetes compartides, per això, s'afegeix aquest permís especial.

Encara que en la carpeta compartida tinguis permís 777, si té el sticky bit, només podrás esborrar o reanomenar els fitxers que siguin teus.

Per afegir aquest permís fem ***chmod +t carpeta***

I per a treure'l ***chmod -t carpeta***

Però, també podem afegir-lo de forma numérica, ja que el valor del Sticky Bit és 1: ***chmod 1777 carpeta***

##### Exemple d'ús

Hem afegit al exemple que hem fet amb anterioritat l'usuari *deivy* i l'usuari *ferran* a la carpeta *paloma*.

Primerament, comprobem que, si creem un fitxer amb un usuari (deivy) i ens connectem amb un altre usuari (ferran) i intentem eliminar-lo, funciona perfectament i l'elimina.

![Imatge 81](images/81.png)

Ara, si afegim a la carpeta l'sticky bit, amb la comanda vista anteriorment, ens quedarà el directori tal que així:

![Imatge 85](images/85.png)

![Imatge 82](images/82.png)

Una vegada afegit el sticky bit, entrem amb l'usuari deivy i creem de nou el fitxer pt1varas, com abans. Si tornem a entrar amb l'usuari ferran i intentem eliminar-lo ens dirà el següent:

![Imatge 83](images/83.png)

Però, si creem el fitxer en el mateix usuari i intentem eliminar aquell mateix fitxer, el podrem eliminar, obviament, ja que és del mateix usuari.

![Imatge 84](images/84.png)

### ACLs

L'ACL és una llista detallada per a veure d'un fitxer o directori per superar les limitacions dels permisos básics de Linux.

#### Comandes

- Tenim dues comandes:
    - **setfacl:** Posar/modificar ACLs.
    - **getfacl:** Veure els ACLs.

##### Getfacl

![Imatge 66](images/66.png)

##### Setfacl

Per a modificar un fitxer, la comanda és: ***setfacl -m u/g/o:usuari:permisos fitxer***

Un exemple de modificació, fem que l'usuari *segon* no tingui cap permís:

![Imatge 67](images/67.png)

Si entrem amb l'usuari *primer*, veem que si que ens deixa entrar a la carpeta i crear un fitxer dins, però amb l'usuari *segon* no.

![Imatge 68](images/68.png)

![Imatge 69](images/69.png)

Si fem un ***setfacl -b fitxer***, deixem el fitxer com estava de base, reseteijant els permissos.

![Imatge 70](images/70.png)

### Màscara

L'umask (User Mask) serveix per definir els permisos inicials per defecte.

Linux aplica per als directoris i per als fitxers els permisos màxims sempre, però es l'umask el que fa que canvii aquests permisos al crear-los.

- **Directoris:** 777 (rwxrwxrwx)
- **Fitxers:** 666 (rw-rw-rw)

Usualment, la màscara d'un usuari normal és 002 i la del root 022, ho podem mirar d'aquesta forma:

![Imatge 60](images/60.png)

Una vegada creem els directoris o fitxers, el nostre sistems fa una operació amb l'umask per a sapiguer quins permisos finals tindrá.

L'operació és: **Base - Umask = Resultat Final** (per exemple; 777 - 022 = 755)

També podem fer l'operació en binari, com per exemple:

Permís base = 777 = 111 111 111

Hem de fer el NOT del permís Umask, és a dir, si l'umask és = 022 = 000 010 010, NOT Umask sería = 111 101 101

Calculem amb un AND del permís base i el NOT Umask.

- **PB =** 111 111 111
- **NU =** 111 101 101
- **AND =** 111 101 101

El **Resultat final** será = 111 101 101 = 755

En el següent exemple creem un directori i un fitxer amb la màscara d'usuari (002).

![Imatge 61](images/61.png)

Com podem veure, el directori té els permisos ***rwxrwxr-x***, ja que ha aplicat: 777 - 002 = 775 = rwx(7)rwx(7)r-x(5)

I el fitxer té els permisos ***rw-rw-r--***, ja que ha aplicat: 666 - 002 = 664 = rw-(6)rw-(6)r--(4)

En el cas del *root*, faría els següents càlculs:
- **Directori:** 777 - 022 = 755 = rwx(7)r-x(5)r-x(5)
- **Fitxer:** 666 - 022 = 644 = rw-(6)r--(4)r--(4)

A part, podem canviar l'umask temporalment, fent un ***umask x*** (x = número de màscara desitjada).

![Imatge 62](images/62.png)

Si creem un directori i un fitxer de nou, com abans, ens quedarà amb els següents permisos:

![Imatge 63](images/63.png)

**Directori:** 777 - 004 = 773 = rwx(7)rwx(7)-wx(3)

**Fitxer:** 666 - 004 = 662 = rw-(6)rw-(6)-w-(2)

Altres formes de canviar la màscara, però de forma permanent, és:

**login.defs**

En aquest fitxer, si ho modifiquem, el que farà será canviar la màscara a tots els usuaris que es creein des d'aquell moment.

![Imatge 64](images/64.png)

**.profile**

Si entrem a aquest fitxer i editem l'umask aquí, el que farà és canviar la màscara permanentment per aquell usuari.

![Imatge 65](images/65.png)

## Gestió avançada



## PAM



## EXERCICIS EXTRA

### USERADD - ASIGNAR HOME PASSWORD ETC EN NOMÉS UNA COMANDA

Podem afegir un usuari amb useradd, amb tot el que necessitem des de una mateixa comanda, que és la següent:

***sudo useradd -m -d /home/max_personal -s /bin/bash -c "Max Power, Departament IT, 666-555-444" -u 2050 -g users -G sudo,docker,adm -e 2025-12-31 -k /etc/skel_especial -p $(openssl passwd -6 contrasenya) max***

- **-m:** Crea el directori Home.
- **-d /home/max_personal (Directory):** Opcional. Per defecte la carpeta home seria /home/max, pero amb això podem modificar-la.
- **-s /bin/bash:** Definim que faigui servir l'interpret Bash.
- **-c "..." (Comment/GECOS):** Omple el camp 5 de /etc/passwd. Podem posar el que vulguem.
- **-u 2050 (UID):** Per defecte, Linux assigna el següent número lliure (ex: 1001, 1002...). Amb aquesta opció forcem un número concret.
- **-g users (Grup principal):** Defineix el grup primari (GID).
    - Si no ho poses, es crea un grup nou anomenat max.
    - Si poses -g users, l'usuari pertanyerà al grup genèric "users" i no tindrà grup propi.
- **-G sudo,docker,adm (Grups secundaris):** 
    - sudo: Per ser administrador.
    - docker: Per usar contenidors.
- **-e 2025-12-31 (Expire Date):** Seguretat. El compte es bloquejarà automàticament en aquesta data (format AAAA-MM-DD).
- **-k /etc/skel_especial (Skeleton):** En lloc de copiar els fitxers base de /etc/skel, copiarà els fitxers d'una altra carpeta que haguem creat amb anterioritat.
- **-p $(openssl passwd -6 la_meva_contrasenya):** Definim la contrasenya. El que passa aqui, és que per a poder afegir la contrasenya, ha de ser encriptada, en *hash*. Requereix tenir el Openssl instal·lat per generar el hash al moment.


### QUINA COMANDA O QUINES COMANDES HE D'UTILITZAR QUAN VULL CAMBIAR UN NOM D'USUARI "CORRECTAMENT" (tot lo relacionat)

- Per cambiar el nom d'usuari: ***sudo usermod -l usuari usuariNou***
- Per cambiar el nom del grup: ***sudo groupmod -n grup grupNou***
- Per cambiar la carpeta personal i moure el contingut a la vegada: ***sudo usermod -d /home/usuari -m usuariNou***

# Còpies de seguretat i automatització de tasques
# Quotes d'usuari

