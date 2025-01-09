# CONSEGNA - JOIN


1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
SELECT
	`students`.`id` AS `student_id`,
	`students`.`name` AS `student_name`,
    `students`.`surname` AS `student_surname`,
    `degrees`.`id` AS `degree_id`,
    `degrees`.`name` AS `degree_name`
FROM `university`.`students`
INNER JOIN `university`.`degrees`
ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = "Corso di Laurea in Economia";

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
SELECT
	`degrees`.`id` AS `degree_id`,
    `degrees`.`name` AS `degree_name`, 
    `departments`.`name` AS `department_name`
FROM `university`.`degrees`
INNER JOIN `university`.`departments`
ON `degrees`.`department_id` = `departments`.`id`
WHERE
	`university`.`degrees`.`level` = "Magistrale" AND
    `departments`.`name` = "Dipartimento di Neuroscienze";

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
SELECT
	`courses`.`id` AS `course_id`,
    `courses`.`name` AS `course_name`,
    `teachers`.`id` AS `teacher_id`,
    `teachers`.`name` AS `teacher_name`,
    `teachers`.`surname` AS `teacher_surname`
FROM `university`.`courses`
INNER JOIN `university`.`course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`
INNER JOIN `university`.`teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
WHERE `teachers`.`id` = 44;

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome
SELECT
	`students`.`id` AS `student_id`,
    `students`.`surname` AS `student_surname`,
    `students`.`name` AS `student_name`,
	`degrees`.*,
    `departments`.`name` AS `department_name`
FROM `university`.`students`
INNER JOIN `university`.`degrees`
ON `students`.`degree_id` = `degrees`.`id`
INNER JOIN `university`.`departments`
ON `degrees`.`department_id` = `departments`.`id`
ORDER BY
	`students`.`surname`,
    `students`.`name`;

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
SELECT
	`degrees`.`id` AS `degree_id`,
    `degrees`.`name` AS `degree_name`,
    `courses`.`id` AS `course_id`,
    `courses`.`name` AS `course_name`,
    `teachers`.`id` AS `teacher_id`,
    `teachers`.`name` AS `teacher_name`,
    `teachers`.`surname` AS `teacher_surname`
FROM `university`.`degrees`
INNER JOIN `university`.`courses`
ON `degrees`.`id` = `courses`.`degree_id`
INNER JOIN `university`.`course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`
INNER JOIN `university`.`teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
ORDER BY `degrees`.`name`;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)
SELECT
	`teachers`.`id` AS `teacher_id`,
    `teachers`.`name` AS `teacher_name`,
    `teachers`.`surname` AS `teacher_surname`,
    `departments`.`name` AS `department_name`
FROM `university`.`teachers`
JOIN `university`.`course_teacher`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `university`.`courses`
ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `university`.`degrees`
ON `degrees`.`id` = `courses`.`degree_id`
JOIN `university`.`departments`
ON `departments`.`id` = `degrees`.`department_id`
WHERE `departments`.`name` = "Dipartimento di Matematica"
GROUP BY `teachers`.`id`;

Correzione: soluzione con SELECT DISTINCT, operazione molto più leggera.
Si poteva usare una SELECT DISTINCT che elimina i doppioni, quindi il risultato (i questo caso) è uguale al GROUP BY.

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18.
SELECT 
	`exam_student`.`student_id`,
    `students`.`name` AS `student_name`,
    `students`.`surname` AS `student_surname`,
    COUNT(`exam_student`.`vote`) AS `attempts`,
    MAX(`exam_student`.`vote`) AS `vote_max`
FROM `university`.`exam_student`
JOIN `university`.`students`
ON `students`.`id` = `exam_student`.`student_id`
WHERE `exam_student`.`vote` >= 18
GROUP BY `exam_student`.`student_id`;

Correzione: soluzione corretta con utilizzo di un doppio GROUP BY + HAVING.
Uso HAVING perchè devo scremare i dati solo DOPO aver eseguito l'aggregazione, altrimenti lavorerei su una base dati non completa.
In altri termini, è HAVING alla fine che screma tramite il voto MAX, quindi escludo tutti gli studenti che NON hanno passato l'esame.

SELECT
    `exam_student`.`student_id`,
    COUNT(`exam_student`.`student_id`) AS `numero_tentativi`,
    MAX(`exam_student`.`vote`) AS `voto_assimo`
FROM `exam_student`
INNER JOIN `students`
ON `students`.`id` = `exam_student`.`student_id`
GROUP BY
    `exam_student`.`student_id`,
    # qui devo filtrare per ID del Corso (non c'è scritto il JOIN della tabella ma dovrebbe esserci)
HAVING MAX(`exam_student`.`vote`) >= 18;