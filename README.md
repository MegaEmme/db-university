# **Esercizio: db-university**

## Consegna day-1:
Modellizzare la struttura di un database per memorizzare tutti i dati riguardanti una università:
- sono presenti diversi Dipartimenti (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
- ogni Dipartimento offre più Corsi di Laurea (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
- ogni Corso di Laurea prevede diversi Corsi (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
- ogni Corso può essere tenuto da diversi Insegnanti;
- ogni Corso prevede più appelli d'Esame;
- ogni Studente è iscritto ad un solo Corso di Laurea;
- ogni Studente può iscriversi a più appelli di Esame;
- per ogni appello d'Esame a cui lo Studente ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente.

---
Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella.

Utilizzare `https://www.drawio.com/` per la creazione dello schema.

Esportare quindi il diagramma in jpg e caricarlo nella repo.

---

## Consegna day-2:

Dopo aver creato un nuovo database in MySQL Workbench e aver importato lo schema allegato, eseguire le seguenti query e riportarle in forma testuale in questo stesso file.

### 1. Selezionare tutti gli studenti nati nel 1990 (160)

SELECT 
    `name`, `surname`
FROM
    db_university.students
WHERE
    `date_of_birth` BETWEEN '1990-01-01' AND '1990-12-31'

    oppure

SELECT 
    COUNT(*) AS nati_1990
FROM
    db_university.students
WHERE
    `date_of_birth` BETWEEN '1990-01-01' AND '1990-12-31'

### 2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

SELECT 
    `name`,`cfu`
FROM
    db_university.courses
WHERE
    `cfu` > 10

    oppure

SELECT 
    COUNT(*) AS cfu_oltre_dieci
FROM
    db_university.courses
WHERE
    `cfu` > 10

### 3. Selezionare tutti gli studenti che hanno più di 30 anni

SELECT 
    *
FROM
    db_university.students
WHERE
    `date_of_birth` < '1995-06-05'
ORDER BY `date_of_birth`

    oppure

SELECT 
    COUNT(*) AS trentenni
FROM
    db_university.students
WHERE
    `date_of_birth` < '1995-06-05'
ORDER BY `date_of_birth`

### 4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)

SELECT 
    `name`,`period`,`year`
FROM
    db_university.courses
WHERE
    `period` = 'I semestre' AND `year` = 1
    
    oppure

SELECT 
    COUNT(*) AS anno_e_semestre_uno
FROM
    db_university.courses
WHERE
    `period` = 'I semestre' AND `year` = 1

### 5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)

SELECT 
    *
FROM
    db_university.exams
WHERE
    `hour` > '14:00:00' and `date` = "2020-06-20"

    oppure

SELECT 
    COUNT(*) AS esami_post_quattordici
FROM
    db_university.exams
WHERE
    `hour` > '14:00:00'
        AND `date` = '2020-06-20'

### 6. Selezionare tutti i corsi di laurea magistrale (38)

SELECT 
    *
FROM
    db_university.degrees
WHERE
    `level` = 'magistrale'

    oppure

SELECT 
    COUNT(*) AS magistrali
FROM
    db_university.degrees
WHERE
    `level` = 'magistrale'

### 7. Da quanti dipartimenti è composta l'università? (12)



### 8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)