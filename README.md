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
        `students`
    WHERE
        `date_of_birth` BETWEEN '1990-01-01' AND '1990-12-31'

oppure

    SELECT 
        COUNT(*) AS `1990_nati`
    FROM
        `students`
    WHERE
        `date_of_birth` BETWEEN '1990-01-01' AND '1990-12-31'

### 2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

    SELECT 
        `name`,`cfu`
    FROM
        `courses`
    WHERE
        `cfu` > 10

oppure

    SELECT 
        COUNT(*) AS `cfu_dieci_oltre`
    FROM
        `courses`
    WHERE
        `cfu` > 10

### 3. Selezionare tutti gli studenti che hanno più di 30 anni

    SELECT 
        *
    FROM
        `students`
    WHERE
        `date_of_birth` < '1995-06-05'
    ORDER BY `date_of_birth`

oppure

    SELECT 
        COUNT(*) AS `trentenni`
    FROM
        `students`
    WHERE
        `date_of_birth` < '1995-06-05'
    ORDER BY `date_of_birth`

### 4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)

    SELECT 
        `name`,`period`,`year`
    FROM
        `courses`
    WHERE
        `period` = 'I semestre' AND `year` = 1
    
oppure

    SELECT 
        COUNT(*) AS `anno_e_semestre_uno`
    FROM
        `courses`
    WHERE
        `period` = 'I semestre' AND `year` = 1

### 5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)

    SELECT 
        *
    FROM
        `exams`
    WHERE
        `hour` > '14:00:00' AND `date` = "2020-06-20"

oppure

    SELECT 
        COUNT(*) AS `esami_post_quattordici`
    FROM
        `exams`
    WHERE
        `hour` > '14:00:00'
            AND `date` = '2020-06-20'

### 6. Selezionare tutti i corsi di laurea magistrale (38)

    SELECT 
        *
    FROM
        `degrees`
    WHERE
        `level` = 'magistrale'

oppure

    SELECT 
        COUNT(*) AS `magistrali`
    FROM
        `degrees`
    WHERE
        `level` = 'lauree_magistrali_numero'

### 7. Da quanti dipartimenti è composta l'università? (12)

    SELECT 
        COUNT(*) AS `dipartimenti_numero_totale`
    FROM
        `departments`

### 8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

    SELECT 
        COUNT(*) AS `numero_prof_senza_telefono`
    FROM
        `teachers`
    WHERE
        `phone` IS NULL

---
## Consegna day-3:

## 3.1 - GROUP BY

### 1. Contare quanti iscritti ci sono stati ogni anno

    SELECT 
        COUNT(*), YEAR(`enrolment_date`) AS `enroled_per_year`
    FROM
        `students`
    GROUP BY `enroled_per_year`
    ORDER BY `enroled_per_year`

### 2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

    SELECT 
        COUNT(*), `office_address`
    FROM
        `teachers`
    GROUP BY `office_address`


### 3. Calcolare la media dei voti di ogni appello d'esame

    SELECT 
        AVG(`vote`) AS `average_vote`
    FROM
        `exam_student`
    GROUP BY `vote` 

### 4. Contare quanti corsi di laurea ci sono per ogni dipartimento

    SELECT 
        COUNT(`name`)
    FROM
        `degrees`
    GROUP BY `department_id`

## 3.2 - JOIN

### 1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

    SELECT 
        `students`.*,  `degrees`.`name`
    FROM
        `students`
            INNER JOIN
        `degrees` ON `degrees`.`id` = `students`.`degree_id`
    WHERE
        `degrees`.`name` LIKE '%economia%'

### 2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

    SELECT 
        `degrees`.name, `departments`.`name`, `degrees`.`level`
    FROM
        `degrees`
            INNER JOIN
        `departments` ON `departments`.`id` = `degrees`.`department_id`
    WHERE
        `departments`.`name` = 'Dipartimento di Neuroscienze'
            AND `degrees`.`level` = 'Magistrale'

### 3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

    SELECT 
        `teachers`.`surname`, `course_teacher`.`course_id`
    FROM
        `teachers`
            INNER JOIN
        `course_teacher` ON `course_teacher`.`teacher_id` = `teachers`.`id`
            INNER JOIN
        `courses` ON `course_teacher`.`course_id` = `courses`.`id`
    WHERE
        `teachers`.`id` = 44

### 4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

    SELECT 
        `students`.surname,
        `students`.name,
        `degrees`.`name` AS `course_name`,
        `departments`.`name` AS `department_name`
    FROM
        `students`
            INNER JOIN
        `degrees` ON `degrees`.`id` = `students`.`degree_id`
            INNER JOIN
        `departments` ON `departments`.`id` = `degrees`.`department_id`
    ORDER BY `surname`

### 5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

    SELECT 
        `degrees`.`name` AS `degree_name`,
        `courses`.`name` AS `course_name`,
        `teachers`.surname AS `teacher_surname`,
        `teachers`.`name` AS `teacher_name`
    FROM
        `teachers`
            INNER JOIN
        `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
            INNER JOIN
        `courses` ON `course_teacher`.`course_id` = `courses`.`id`
            INNER JOIN
        `degrees` ON `courses`.`id` = `degrees`.`id`

### 6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

        SELECT DISTINCT
        (`teachers`.`id`),
        `teachers`.`surname` AS `teacher_surname`,
        `teachers`.`name` AS `teacher_name`,
        `departments`.`name` AS `department_name`
    FROM
        `teachers`
            INNER JOIN
        `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
            INNER JOIN
        `courses` ON `course_teacher`.`course_id` = `courses`.`id`
            INNER JOIN
        `degrees` ON `courses`.`degree_id` = `degrees`.`id`
            INNER JOIN
        `departments` ON `degrees`.`department_id` = `departments`.`id`
    WHERE
        `departments`.`name` = 'Dipartimento di Matematica'
    ORDER BY `teachers`.`surname`


### 7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.