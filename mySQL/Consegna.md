# CONSEGNA


1. Selezionare tutti gli studenti nati nel 1990 (160)
SELECT `id`, `name`, `surname`, `date_of_birth`
FROM `university`.`students`
WHERE YEAR(`date_of_birth`) = 1990;

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
SELECT `id`,`name`
FROM `university`.`courses`
WHERE `cfu` > 10;

3. Selezionare tutti gli studenti che hanno più di 30 anni
SELECT `id`, `name`, `surname`, `date_of_birth`
FROM `university`.`students`
WHERE TIMESTAMPDIFF(YEAR, `date_of_birth`, CURDATE()) > 30;

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
laurea (286)
SELECT `id`, `year`, `period`, `name`
FROM `university`.`courses`
WHERE	`year` = 1 AND
		`period` = "I semestre";

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
20/06/2020 (21)
SELECT `id`, `date`, `hour`
FROM `university`.`exams`
WHERE	DATE(`date`) = "2020-06-20" AND
		TIME(`hour`) > "14:00:00";

6. Selezionare tutti i corsi di laurea magistrale (38)
SELECT `id`, `name`, `level`
FROM `university`.`degrees`
WHERE `level` = "magistrale";

7. Da quanti dipartimenti è composta l'università? (12)
SELECT COUNT(*)
FROM `university`.`departments`;

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
SELECT `id`, `name`, `surname`, `phone`
FROM `university`.`teachers`
WHERE `phone` IS null;

9. Inserire nella tabella degli studenti un nuovo record con i propri dati (per il campo
degree_id, inserire un valore casuale)
INSERT INTO `university`.`students` (`degree_id`, `name`, `surname`, `date_of_birth`, `fiscal_code`, `enrolment_date`, `registration_number`, `email`)
VALUES ("75", "Camillo", "Rosanio", "1992-12-20", "ABCDEF92T10C123C", "2012-01-01", "12345", "pincopallo@iloveboolean.it");

10. Cambiare il numero dell’ufficio del professor Pietro Rizzo in 126
NOTA: necessario disabilitare la Safe Mode in Prefereces.
UPDATE `university`.`teachers`
SET `office_number` = "126"
WHERE `name` = "Pietro";

11. Eliminare dalla tabella studenti il record creato precedentemente al punto 9
NOTA: a scopo didattico ho aggiunto 2 records per cui ne elimino 2 tramite "IN".
DELETE FROM `university`.`students`
WHERE `id` IN (5002, 5007);


# GROUP BY

1. Contare quanti iscritti ci sono stati ogni anno
SELECT YEAR(`enrolment_date`) AS `year`, COUNT(`id`) AS `enrolled_students`
FROM `university`.`students`
GROUP BY YEAR(`enrolment_date`)
ORDER BY YEAR(`enrolment_date`);

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT `office_address` AS `building`, COUNT(*) AS `total_teachers`
FROM `university`.`teachers`
GROUP BY `office_address`
ORDER BY `total_teachers` DESC;

3. Calcolare la media dei voti di ogni appello d'esame
SELECT `exam_id`, AVG(`vote`)
FROM `university`.`exam_student`
GROUP BY `exam_id`;

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT `department_id`,COUNT(*) AS `available_degrees`
FROM `university`.`degrees`
GROUP BY `department_id`
ORDER BY `available_degrees` DESC;

