**1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia**

SELECT `students`.`id`,
`degrees`.`id` `degree_id`,
`degrees`.`name`,
`students`.`name`,
`students`.`surname`

FROM `university`.`students`

JOIN `university`.`degrees`
ON `degrees`.`id` = `students`.`degree_id`

WHERE `degree_id` = 53;

**2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze**

SELECT `degrees`.`id` `degree_id`,
`degrees`.`name` `degree_name`,
`departments`.`name` `department_name`

FROM `university`.`degrees`

JOIN `university`.`departments`
ON `departments`.`id` = `degrees`.`department_id`

WHERE `departments`.`id` = 7 AND `degrees`.`level` = 'magistrale';

**3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)**

SELECT `course_teacher`.\*,
`courses`.`degree_id`,
`courses`.`name` `course_name`,
`teachers`.`name` `teacher_name`,
`teachers`.`surname` `teacher_surname`

FROM `university`.`courses`

INNER JOIN `university`.`course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`

JOIN `university`.`teachers`
ON `teachers`.`id` = `course_teacher`.`teacher_id`

WHERE `teachers`.`id` = 44;

**4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome**

SELECT `students`.`id` `student_id`,
`students`.`name` `student_name`,
`students`.`surname` `student_surname`,
`degrees`.\*,
`departments`.`name` `department_name`

FROM `university`.`students`

INNER JOIN `university`.`degrees`
ON `students`.`degree_id` = `degrees`.`id`

INNER JOIN `university`.`departments`
ON `degrees`.`department_id` = `departments`.`id`

ORDER BY `students`.`surname`, `students`.`name`;

**5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti**

SELECT `degrees`.`id` `degree_id`,
`degrees`.`name` `degree_name`,
`courses`.`id` `course_id`,
`courses`.`name` `course_name`,
`teachers`.`id` `teacher_id`,
`teachers`.`name` `teacher_name`,
`teachers`.`surname` `teacher_surname`

FROM `university`.`degrees`

INNER JOIN `university`.`courses`
ON `degrees`.`id` = `courses`.`degree_id`

INNER JOIN `university`.`course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`

INNER JOIN `university`.`teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`

ORDER BY `degrees`.`name`;

**6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)**

SELECT `teachers`.`id` `teacher_id`,
`teachers`.`name` `teacher_name`,
`teachers`.`surname` `teacher_surname`,
`departments`.`name` `department_name`

FROM `university`.`teachers`

JOIN `university`.`course_teacher`
ON `teachers`.`id` = `course_teacher`.`teacher_id`

JOIN `university`.`courses`
ON `course_teacher`.`course_id` = `courses.id`

JOIN `university`.`degrees`
ON `degrees`.id = `courses`.`degree_id`

JOIN `university`.`departments`
ON `departments`.`id` = `degrees`.`department_id`

WHERE `departments`.`name` = "Dipartimento di Matematica"

GROUP BY `teachers`.`id`;

**7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.**

SELECT `exam_student`.`student_id`,
`students`.`name` `student_name`,
`students`.`surname` `student_surname`,

COUNT(`exam_student`.`vote`) `attempts`,

MAX(`exam_student`.`vote`) `vote_max`

FROM `university`.`exam_student`

JOIN `university`.`students`
ON `students`.`id` = `exam_student`.`student_id`

WHERE `exam_student`.`vote` >= 18

GROUP BY `exam_student`.`student_id`;
