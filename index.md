# db-university

## EX - Query con SELECT

1. Selezionare tutti gli studenti nati nel 1990 (160)

```sql
SELECT *
FROM `students`
WHERE YEAR(date_of_birth) = 1990;
```

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

```sql
SELECT *
FROM `courses`
WHERE cfu > 10;
```

3. Selezionare tutti gli studenti che hanno più di 30 anni

```sql
SELECT *
FROM `students`
WHERE YEAR(date_of_birth) < 1994;
```

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)

```sql
SELECT *
FROM `courses`
WHERE `period` = 'I semestre' AND `year` = 1;
```

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)

```sql
SELECT *
FROM `exams`
WHERE `date` = '2020-06-20' AND HOUR(hour) >= 14;
```

6. Selezionare tutti i corsi di laurea magistrale (38)

```sql
SELECT *
FROM `degrees`
WHERE `level` = 'magistrale';
```

7. Da quanti dipartimenti è composta l'università? (12)

```sql
SELECT COUNT(`id`) AS `numero_dipartimenti`
FROM `departments`;
```

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

```sql
SELECT COUNT(`id`) AS `numero_insegnanti_senza_telefono`
FROM `teachers`
WHERE `phone` IS NULL;
```

## EX - Query con GROUP BY

1. Contare quanti iscritti ci sono stati ogni anno

```sql
SELECT COUNT(`id`) AS `numero_di_iscritti`, YEAR(enrolment_date) AS `anno`
FROM `students`
GROUP BY YEAR(enrolment_date);
```

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

```sql
SELECT COUNT(`id`) AS `numero_di_insegnanti`, `office_address` AS `ufficio`
FROM `teachers`
GROUP BY `office_address`;
```

3. Calcolare la media dei voti di ogni appello d'esame

```sql
SELECT AVG(vote) AS `media_voti`, `exam_id`
FROM `exam_student`
GROUP BY `exam_id`;
```

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

```sql
SELECT COUNT(`id`) AS `numero_di_corsi`, `department_id`
FROM `degrees`
GROUP BY `department_id`;
```

## EX - Query con JOIN

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```sql
SELECT `students`.*, `degrees`.`name` 
FROM `students`
JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';
```

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

```sql
SELECT `degrees`.*, `departments`.`name` 
FROM `degrees`
JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze';
```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```sql
SELECT `courses`.*, `teachers`.`name`, `teachers`.`surname`
FROM `courses`
JOIN `course_teacher`
ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
WHERE `teachers`.`name` = 'Fulvio'
AND `teachers`.`surname` = 'Amato'
AND `teachers`.`id` = 44;
```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

```sql
SELECT `students`.`surname`, `students`.`name`, `degrees`.`name`, `degrees`.`level`, `departments`.`name`
FROM `students`
JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
ORDER BY `students`.`surname` ASC, `students`.`name` ASC;
```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```sql
SELECT `degrees`.`name`, `courses`.*, `teachers`.`name`, `teachers`.`surname`
FROM `teachers`
JOIN `course_teacher`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
JOIN `courses`
ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `degrees`
ON `courses`.`degree_id` = `degrees`.`id`;
```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

```sql
SELECT DISTINCT `teachers`.`name`, `teachers`.`surname`, `departments`.`name`
FROM `teachers`
JOIN `course_teacher`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
JOIN `courses`
ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `degrees`
ON `courses`.`degree_id` = `degrees`.`id`
JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Matematica';
```

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18

```sql
SELECT `students`.`name`, `students`.`surname`, COUNT(*) AS `numero esami sostenuto`, MAX(`exam_student`.`vote`) AS `voto_massimo`, `courses`.`name`
FROM `exams`
JOIN `courses`
ON `exams`.`course_id` = `courses`.`id`
JOIN `exam_student`
ON `exam_student`.`exam_id` = `exams`.`id`
JOIN `students`
ON `exam_student`.`student_id` = `students`.`id`
WHERE `exam_student`.`vote` >= 18
GROUP BY `students`.`id`, `courses`.`id`;
```