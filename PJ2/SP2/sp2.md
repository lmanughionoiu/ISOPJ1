---
layout: default
title: "Sprint 2: Instal·lació, configuració de programari de base i gestió de fitxers"
---

# Fase 1 – Preparació del sistema

## Pas 1. Afegir un nou disc virtual a la màquina virtual

![alt text](image.png)

## Pas 2. Iniciar Windows i obrir Gestió de discs

![alt text](image-1.png)

## Pas 3. Inicialitzar el disc, crear dues particions: una anomenada Dades i una en FAT32 anomenada Portable

![alt text](image-2.png)

![alt text](image-3.png)

![alt text](image-4.png)

![alt text](image-5.png)

![alt text](image-6.png)

![alt text](image-7.png)

![alt text](image-8.png)

![alt text](image-9.png)

## Pas 4. Assignar lletres i comprovar amb diskpart la configuració

![alt text](image-10.png)

# Fase 2 – Quotes i usuaris

## Pas 5. Activar quotes de disc a la partició Dades (NTFS)

![alt text](image-11.png)

![alt text](image-12.png)

![alt text](image-13.png)

## Pas 6. Establir límit de 300 MB per usuari, amb notificació d’advertència

![alt text](image-14.png)

## Pas 7. Crear dos usuaris locals: alumne1 i alumne2

![alt text](image-15.png)

![alt text](image-16.png)

![alt text](image-17.png)

![alt text](image-18.png)

![alt text](image-19.png)

![alt text](image-20.png)

## Pas 8. Afegir-los a un grup nou anomenat Limitats

![alt text](image-21.png)

## Pas 9. Provar la còpia de fitxers dins Dades per veure com actuen les quotes (superar límit)

![alt text](image-22.png)



# Fase 3 – Script de còpia i automatització

## Pas 10. Afegir tercer disc virtual, formatar-lo en NTFS com a Backups

## Pas 11. Crear carpeta CòpiesUsuaris dins Backups

## Pas 12. Crear un script .bat que copiï C:\Users\%USERNAME% a E:\CòpiesUsuaris\%USERNAME%

## Pas 13. Obre gpedit.msc → Configuració d’usuari → Scripts → Inici de sessió

## Pas 14. Assigna l’script perquè s’executi automàticament quan alumne1 o alumne2 inicien sessió

# Fase 4 – Verificació i documentació

## Pas 15. Inicia sessió amb alumne1, comprova que l’script fa la còpia a Backups i que la quota de Dades bloqueja si supera el límit, així com comprovacions que tot el que has configurat funciona correctament

# Fase 5 – Gestió de processos i serveis

## Pas 19. Llistar processos actius

- Inicia sessió com alumne1
- Obre la consola (cmd) com a usuari
- Executa: tasklist
- Copia el resultat a un fitxer: tasklist > C:\Users\%USERNAME%\processos_inici.txt
- Observa processos típics: explorer.exe, SearchIndexer.exe, OneDrive.exe, etc.

## Pas 20. Identificar processos prescindibles

- Busca processos no essencials per a l’usuari, com: OneDrive.exe, Teams.exe, SkypeApp.exe
- Fes una taula amb:
    - Nom del procés
    - Memòria usada
    - Justificació per eliminar-lo

## Pas 21. Eliminar processos manualment

- Encara dins de la consola, executa: taskkill /IM OneDrive.exe /F
- Comprova amb tasklist si ha desaparegut - Fes una captura abans i després

## Pas 22. Automatitzar-ho a l’inici de sessió

- Modifica l’script d’inici de sessió afegint: taskkill /IM OneDrive.exe /F taskkill /IM Teams.exe /F
- Reengega sessió com alumne2 i comprova que aquests processos no es llencen o es tanquen

## Pas 23. Documentació

- Afegeix fitxer de tasklist i taula justificativa a la doc amb MkDocs
- Explica què passa si mates un procés crític com explorer.exe (prova controlada)
- Comenta com aquesta gestió pot millorar el rendiment de màquines virtuals o amb pocs recursos

# Fase 6 – Gestió de permisos (ACLs)

## Què són les ACLs i com funcionen a Windows

A Windows, cada fitxer i carpeta té una llista de control d'accés (ACL, Access Control List).

Aquesta llista defineix qui pot fer què amb aquell recurs.

Cada entrada d'una ACL es diu ACE (Access Control Entry) i indica:

- Quina identitat (usuari o grup) està afectada
- Quins permisos té (lectura, escriptura, execució, control total, etc.)

Els permisos ACL són molt més detallats que els permisos "normals" de compartició en
xarxa, perquè:
- Permeten configurar permisos per fitxer o carpeta específica
- S'apliquen tant a usuaris com a grups
- Permeten combinacions com "només lectura", "només esborrar", "control total excepte canviar permisos", etc.
- Poden ser heretats d’una carpeta superior o assignats manualment
- Exemple típic: una carpeta pot tenir permisos diferents per a alumne1 i alumne2, tot i que estiguin al mateix grup.

Volem controlar qui pot accedir i modificar la carpeta Projectes, creada dins la partició Dades. El grup Limitats tindrà accés total, però alumne2 tindrà només lectura, tot i formar part del grup.

## Pas 24. Crear la carpeta Inicia sessió com a administrador i crea la carpeta

## Pas 25. Assignar permisos normals al grup
1. A les propietats de la carpeta D:\Projectes, ves a la pestanya Seguretat
2. Fes clic a Avançat → Desactiva la herència i conserva els permisos existents
3. Elimina Users o Everyone si hi apareixen
4. Afegeix el grup Limitats i dona-li Control total
5. Aplica els canvis

Ara qualsevol usuari del grup Limitats té accés complet

## Pas 26. Comprovar accés amb alumne1
1. Inicia sessió com alumne1
2. Crea un fitxer dins D:\Projectes, modifica’l i elimina’l
3. Tot hauria de funcionar (perquè té permisos heretats del grup Limitats)

## Pas 27. Aplicar excepció per alumne2
Torna a iniciar sessió com administrador i executa:icacls "D:\Projectes" /grant:r
alumne2:(R)
Això substitueix qualsevol permís anterior d’alumne2 i li dona només lectura

## Pas 28. Comprovar l'excepció amb alumne2
1. Inicia sessió com alumne2
2. Intenta obrir un fitxer dins D:\Projectes → ha de poder llegir-lo
3. Intenta editar-lo o crear-ne un de nou → ha de rebre un missatge de denegació

## Pas 29. Consultar els permisos aplicats Torna a la consola com a admin i escriu: icacls
"D:\Projectes" i veuràs alguna cosa com: D:\Projectes
Limitats:(OI)(CI)(F) alumne2:(R)
Això confirma que el grup té control total i alumne2 té només lectura