# Data Normalization and Entity-Relationship Diagramming

## Introduction

This report shows the normalization process applied to a dataset representing students' grades in courses at a university. The original dataset's structure and dependencies have been analyzed and converted to the fourth normal form (4NF), considering the reality of how courses work in most universities.

## Original Dataset

The table containing the original data set is shown as follow:

| assignment_id | student_id | due_date  | professor | assignment_topic            | classroom | grade | relevant_reading     | professor_email     |
|---------------|------------|-----------|-----------|-----------------------------|-----------|-------|----------------------|---------------------|
| 1             | 1          | 23.02.21  | Melvin    | Data normalization          | WWH 101   | 80    | Deumlich Chapter 3   | l.melvin@foo.edu    |
| 2             | 7          | 18.11.21  | Logston   | Single table queries        | 60FA 314  | 25    | Dümmlers Chapter 11  | e.logston@foo.edu   |
| 1             | 4          | 23.02.21  | Melvin    | Data normalization          | WWH 101   | 75    | Deumlich Chapter 3   | l.melvin@foo.edu    |
| 5             | 2          | 05.05.21  | Logston   | Python and pandas           | 60FA 314  | 92    | Dümmlers Chapter 14  | e.logston@foo.edu   |
| 4             | 2          | 04.07.21  | Nevarez   | Spreadsheet aggregate functions | WWH 201 | 65    | Zehnder Page 87      | i.nevarez@foo.edu   |

## Non-Compliance with 4NF

The initial dataset displayed several non-compliance issues with 4NF, including:

1. **Data Redundancy**: In the original data set, we observe data redundancy with professor names, emails, classroom locations, and other attributes being repeated in multiple records for different assignments and sections. This not only unnecessarily increases the database size but also creates more workloads during data modifications.

2. **Multi-valued Dependencies**: The original data set violates 4NF due to the existence of multi-valued dependencies. Since we've made assumptions that multiple professors could teach different sections of the same course, the original data set fails to separate these multi-valued facts into distinct entities. This can lead to ambiguous relationships and potential data anomalies.

3. **Other Dependency Issues**: In the original data set, attributes such as professor, assignment_topic, classroom, due_date, relevant_reading, and professor_email are likely only dependent on assignment_id and not on student_id. This shows partial dependency and violates 2NF. Also, there are instances like the 'professor_email' is not directly dependent on the primary key (assignment_id and student_id) but rather on another non-key attribute 'professor'. This violates 3NF.

4. **Assignment and Section Confusion**: This does not violates 4NF directly but may create confusions, so it will still be a problem to solve. In the orginal data set, assignments were linked directly to courses without considering that they are actually specific to sections. It doesn't consider the reality that the same course can have multiple sections taught by different professors with different assignments, due dates, and relevant readings.

## 4NF-Compliant Version of the Data Set

To comply with 4NF, the dataset was divided into the following tables:

### Students Table

| student_id | student_name | student_email |
|------------|--------------|---------------|
| 1          | Keven Smith  | skeven@bar.edu|
| 2          | Adam John    | jadam@bar.edu |
| 2          | Denis Ben    | bdenis@bar.edu|
| ...        | ...          | ...           |

### Professors Table

| professor_id | professor_name | professor_email   |
|--------------|----------------|-------------------|
| 1            | Melvin         | l.melvin@foo.edu  |
| 2            | Logston        | e.logston@foo.edu |
| 3            | Nevarez        | i.nevarez@foo.edu |
| ...          | ...            | ...               |

### Courses Table

| course_id | course_name           |
|-----------|-----------------------|
| 1         | Database design       |
| 2         | Introduction to data science|
| 3         | Introduction to programming|
| ...       | ...                   |

### Classrooms Table

| classroom_id | location  |
|--------------|-----------|
| 1            | WWH 101   |
| 2            | 60FA 314  |
| 3            | WWH 201   |
| ...          | ...       |

### Sections Table

| section_id | course_id | professor_id | classroom_id |
|------------|-----------|--------------|--------------|
| 1          | 1         | 1            | 1            |
| 2          | 2         | 2            | 2            |
| 3          | 1         | 3            | 3            |
| 4          | 1         | 3            | 4            |
| ...        | ...       | ...          | ...          |

### Assignments Table

| assignment_id | section_id | due_date  | assignment_topic       | relevant_reading  |
|---------------|------------|-----------|------------------------|-------------------|
| 1             | 1          | 23.02.21  | Data normalization     | Deumlich Chapter 3|
| 2             | 2          | 18.11.21  | Single table queries   | Dümmlers Chapter 11|
| 3             | 3          | 04.07.21  | Spreadsheet aggregate functions| Zehnder Page 87|
| ...           | ...        | ...       | ...                    | ...               |

### Grades Table

| student_id | assignment_id | grade |
|------------|---------------|-------|
| 1          | 1             | 80    |
| 2          | 2             | 92    |
| 2          | 3             | 65    |
| 4          | 1             | 75    |
| 7          | 4             | 25    |
| ...        | ...           | ...   |

## Changes Made for 4NF Compliance

The normalization process involved:

1. **Separating the Entities**: By dividing the original dataset into separate tables based on distinct entities: Students, Professors, Courses, Classrooms, Sections, Assignments, and Grades, we eliminates data redundancy and clarify the relationships between different types of data.

2. **Solving Multi-valued Dependencies**: By creating a Sections table, the multi-valued dependencies between courses and professors is solved. Each section is now a unique combination of course, professor, and classroom, which reflects the reality of how courses work in most universities. This provides a more flexible and accurate representation of the assignments.

3. **Creating Assignments Table**: Assignments table is created to ensure that they are directly linked to sections, not just courses. This will clearly inform others that assignments are specific to particular sections and may vary by professor, classroom, and time slot.

4. **Normalization of Attributes**: Each table now contains only attributes directly related to the primary key so that all data dependencies are functional and fully normalized. This eliminates transitive and partial dependencies.

## Entity-Relationship Diagram

The ER diagram showing the 4NF-compliant version of the data set is shown below:

[ER Diagram](images/ER_Diagram.drawio.svg)

## Conclusion

The normalization process has significantly improved the structure of the dataset, making it compliant with 4NF. This restructuring reduces redundancy, eliminates multi-valued dependencies, and clarifies the relationships between different entities within the university's course grading system.
