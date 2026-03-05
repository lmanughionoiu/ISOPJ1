---
layout: default
title: "Sprint 5: Monitoratge, Auditories i Programari Client/Servidor"
---

# Directoris importants

Fer teoria rendiment Monitor del sistema fer captures i explicar.



# Logs

![Imatge 0](image.png)

Per a veure els logs fem un cat del arxiu `syslogs` i podem veure tots els registres del sistema.

![Imatge 1](image-1.png)

En aquest directori podem presonalitzar la rotació dels logs.

![Imatge 2](image-2.png)

Aquest fitxer el podem modificar per personalitzar la rotació dels logs.

![Imatge 3](image-3.png)

En aquest fitxer ens diu on hem de anar per modificar el fitxer default dels logs, anirem alli ara.

![Imatge 4](image-4.png)

Primerament, fem una prova per veure com afecta una notificació `mail` i en quins arxius apareix.

Fem una simulació per a veure si apareix el log al moment d'enviar una notificació de mail i si després apareix al arxiu de mail.log.

En una terminal enviem la notificació amb el missatge i a l'altra obrim el syslog en directe, per a veure els canvis al moment.

![Imatge 6](image-6.png)

Si mirem l'arxiu `mail.log`, s'ha guardat aquesta notificació al arxiu.

![imatge 8](image-8.png)

Farem una altra prova modificant el `mail`. Volem que ens guardi al `mail.log` els logs de error només, ni nivells inferiors ni superiors a aquest. Si fiquem un `*`, apareixerien tots i es guardarà com abans.

![Imatge 5](image-5.png)

Quan el modifiquem, hem de fer un restart al servei.

![Imatge 7](image-7.png)

Ara tornem a fer la comanda anterior i veem que a l'arxiu `mail.log` no apareix, ja que el que enviem és un `mail.notice` i aquest no és tipus `err`.

![Imatge 9](image-9.png)

Finalment, si en comptes de un `mail.notice` fiquem `mail.err` si que ens ho guardarà al arxiu dels logs.

![Imatge 10](image-10.png)

Ara modifiquem una altra vegada el tipus de logs que volem guardar de tipus `mail`. En aquest cas traem el =. Això el que farà serà guardar tots els logs de tipus err i superiors.

![Imatge 11](image-11.png)

Reiniciem el servei i seguim.

Per fer una prova d'aquest funcionament, cambiem el tipus d'alerta a `crit` que és un tipus superior i veem que el guarda.

![Imatge 12](image-12.png)

Podem crear una ruta personalitzada per guardar els logs que més ens interessa, en aquest cas, el que fem és que volem guardar tot tipus de logs que siguin `crit` i li afegim el directori on volem que es guardi i reiniciem el servei.

![Imatge 13](image-13.png)

Ara enviem la notificació, en aquest cas de `cron.crit` i veem que s'ha creat un nou arxiu anomenat mireia.log, que ho hem especificat al arxiu anterior.

![Imatge 14](image-14.png)

Amb aquesta comanda podem veure tots els logs de tipus `crit`.

![Imatge 15](image-15.png)

Podem trobar entre elles els que hem fet abans.

![Imatge 16](image-16.png)

Depenent del paràmetre que li afegim, podem filtrar les búsquedes. En aquest cas, volem mirar els que hem enviat abans de tipus mail.

![Imatge 17](image-17.png)

