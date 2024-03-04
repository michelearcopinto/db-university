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
