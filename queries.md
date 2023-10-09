# QUERY

le query sono contenute nelle liste, sotto le consegne
## 06/10/2023

 
 
- **Selezionare tutti gli studenti nati nel 1990 (160)**

    - SELECT * FROM `students` WHERE YEAR(`date_of_birth`) = 1990;
    
- **Selezionare tutti i corsi che valgono più di 10 crediti (479)**
    
    - SELECT * FROM `courses` WHERE(`cfu`) > 10;
    
- **Selezionare tutti gli studenti che hanno più di 30 anni (3462 ?)**
    
    - SELECT * FROM students WHERE TIMESTAMPDIFF(YEAR, date_of_birth, CURRENT_DATE()) > 30;
    
- **Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)**

    - SELECT * FROM `courses` WHERE (`period`) = 'I semestre' AND (`year`) = 1;
    
- **Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)**

   - SELECT * FROM `exams` WHERE `date` = '2020-06-20' AND HOUR (`hour`) >= 14;
    
- **Selezionare tutti i corsi di laurea magistrale (38)**
    
   - SELECT * FROM `degrees` WHERE `level` = 'magistrale';
    
- **Da quanti dipartimenti è composta l'università? (12)**
    
   - SELECT COUNT(*) FROM `departments`
    
- **Quanti sono gli insegnanti che non hanno un numero di telefono? (50)**
    
   - SELECT COUNT(*) FROM `teachers` WHERE `phone` IS NULL;

    

## 09/10/2023

### Group by


- Contare quanti iscritti ci sono stati ogni anno

    - SELECT YEAR(`enrolment_date`) AS anno, COUNT(*) AS totale_iscritti FROM `students` GROUP BY YEAR(`enrolment_date`)

- Contare gli insegnanti che hanno l'ufficio nello stesso edificio

    - SELECT `office_address`, COUNT(*) AS `numero_insegnanti` FROM `teachers` GROUP BY `office_address`;

- Calcolare la media dei voti di ogni appello d'esame

    - SELECT `exam_id`, AVG(vote) AS `media` FROM `exam_student` GROUP BY `exam_id`;

- Contare quanti corsi di laurea ci sono per ogni dipartimento

    - SELECT (`department_id`) AS dipartimento, COUNT(id) AS numero_di_corsi FROM `degrees` GROUP BY `department_id`;

### Join


- Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

    - SELECT * FROM `students` INNER JOIN `degrees` ON degrees.name = 'Corso di Laurea in Economia';


- Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

    - SELECT * FROM `degrees` INNER JOIN `departments` ON departments.name = 'Dipartimento di Neuroscienze' WHERE `level`= 'magistrale';


- Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

    - SELECT * FROM `courses` INNER JOIN `course_teacher` ON courses.id = course_teacher.course_id WHERE course_teacher.teacher_id = 44;


- Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
    
    - SELECT `students`.`name`,`students`.`surname`,`degrees`.`name`,`degrees`.`level`,`degrees`.`address`,`departments`.`name` FROM `students` INNER JOIN `degrees` ON `students`.`degree_id` = `degrees`.id INNER JOIN `departments` on `degrees`.`department_id` = `departments`.`id` ORDER BY `students`.`name`,`students`.`surname`;


- Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

    - 


- Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

    - SELECT `teachers`.`name`,`teachers`.`surname`,`departments`.`name` FROM `teachers` INNER JOIN `course_teacher`ON `teachers`.`id` = `course_teacher`.`teacher_id` INNER JOIN `courses` ON `courses`.`id` = `course_teacher`.`course_id` INNER JOIN `degrees` ON `degrees`.id = `courses`.`degree_id` INNER JOIN `departments` ON `departments`.`id` = `degrees`.`department_id` WHERE `departments`.`name` = 'Dipartimento di Matematica';


### BONUS

Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18.