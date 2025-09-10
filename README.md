[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/D-71gcBD)
# Exam #2: "Class Assignments"

## React Client Application Routes

- Route `/`:                          Welcome page
- Route `/login`:                     user authentication
- Route `/teacher`:                   Homepage for authenticated teachers
- Route `/teacher/class`:             Main page dashboard for authenticated teachers
- Route `/student`:                   Homepage for authenticated students
- Route `/student/assignments`:      Main page dashboard for authenticated students

## API Server
## POST `/api/sessions` - Authentication
  - request parameters : none
  - request body content
      {
        "username" : "email",
        "password" : "password"
      }
  - response:  
    - `201 Created` (login success), 
    - `401 Unauthorized` (Incorrect credentials – wrong email or password)
    - `500 Internal Server Error` (internal error during login)
  - response body content
      {
          "id": 0,
          "username": "annalisa@student.it",
          "name": " Annalisa Porcelli",
          "role": "student"
      }

## GET `/api/sessions/current` - verification of current active session
  - request parameters : none
  - response: 
    - `200 OK` (active session found), 
    - `401 Unauthorized` (no active session found)
  - response body content:
      {
          "id": 21,
          "username": "fulviocorno@teacher.it",
          "name": " Fulvio Corno",
          "role": "teacher"
      }
## DELETE `/api/session/current` - Logs out the currently authenticated user by ending the session
  - request parameters : none
  - request body content: none
  - response:  
    - `200 OK` (Logout successful), 
    - `401 Unauthorized` (User not authenticated), 
    - `500 Internal Server Error` (internal error during logout)


## TEACHER
## GET `/api/teacher/assignments` - Get all assignments for a teacher
  - request parameters : none
  - response:  
    - `200 OK` (list of assingment created for logged-in teacher), 
    - `403 Forbidden` (User != teacher), 
    - `500 Internal Server Error` (DB error)
  - response body content: 
      [
    {
        "id": 37,
        "question": "What is the capital of Ireland? ",
        "isOpen": 0,
        "evaluation": 30,
        "teacher_id": 21,
        "answer": "Value of gravitational acceleration",
        "group": [
            {
                "id": 0,
                "name": " Annalisa Porcelli",
                "email": "annalisa@student.it",
                "role": "student"
            },
            {
                "id": 3,
                "name": " Silvia La Notte",
                "email": "silvia@student.it",
                "role": "student"
            },
            {
                "id": 4,
                "name": " Erica Porcelli",
                "email": "erica@student.it",
                "role": "student"
            },
            {
                "id": 5,
                "name": " Anna Valente",
                "email": "anna@student.it",
                "role": "student"
            }
        ]
    },
    {
        "id": 38,
        "question": "Value of gravitational acceleration",
        "isOpen": 1,
        "evaluation": null,
        "teacher_id": 21,
        "answer": "9.81",
        "group": [
            {
                "id": 0,
                "name": " Annalisa Porcelli",
                "email": "annalisa@student.it",
                "role": "student"
            },
            {
                "id": 1,
                "name": " Linda Raimondo",
                "email": "linda@student.it",
                "role": "student"
            }
        ]
    }
]

## POST `/api/teacher/assignments` - Create a new assignment 

  - request parameters : none
  - request body content:
        {
          "question": "What is the SI unit of mass?",
          "studentIds": [3, 4, 5]
        }
  - response:
    - `201 created` (Assignment created), 
    - `403 Unauthorized` (User != teacher), 
    - `409 Conflict`: (Group students already collaborated more than two times), 
    - `422 Unprocessable Entity`: (invalid data- question missing or empty or studentIds not valid array), 
    - `500 Internal Server Error`: errore DB

  - response body content:
        {
          "assignmentId": 39
        }

## GET `/api/teacher/class` - Get all classes for a teacher
  - request parameters : none
  - response:
    - `200 OK` (Details of students), 
    - `403 Forbidden` (User != teacher) , 
    - `500 Internal Server Error`(error DB)
  - response body content:
     [
    {
        "id": 0,
        "name": " Annalisa Porcelli",
        "openCount": 1,
        "closedCount": 1,
        "weightedAvg": 30
    },
    {
        "id": 1,
        "name": " Linda Raimondo",
        "openCount": 1,
        "closedCount": 0,
        "weightedAvg": null
    }
    ...]

## GET `/api/teacher/assignments/details` - Get all assignments for a teacher
  - request parameters : none
  - response:

      - `200 OK` – (detailed list of teacher’s assignments (group, answer, evaluation))
      - `403 Forbidden` – (user != teacher)
      - `500 Internal Server Error` – (database error)

  - response body content:
      [
    {
        "id": 37,
        "question": "What is the capital of Ireland? ",
        "isOpen": 0,
        "evaluation": 30,
        "teacher_id": 21,
        "answer": "Value of gravitational acceleration",
        "group": [
            {
                "id": 0,
                "name": " Annalisa Porcelli",
                "email": "annalisa@student.it",
                "role": "student"
            },
            {
                "id": 3,
                "name": " Silvia La Notte",
                "email": "silvia@student.it",
                "role": "student"
            },
            {
                "id": 4,
                "name": " Erica Porcelli",
                "email": "erica@student.it",
                "role": "student"
            },
            {
                "id": 5,
                "name": " Anna Valente",
                "email": "anna@student.it",
                "role": "student"
            }
        ],
        "students": [
            " Annalisa Porcelli",
            " Silvia La Notte",
            " Erica Porcelli",
            " Anna Valente"
        ]
    },
    {
        "id": 38,
        "question": "Value of gravitational acceleration",
        "isOpen": 1,
        "evaluation": null,
        "teacher_id": 21,
        "answer": "9.81",
        "group": [
            {
                "id": 0,
                "name": " Annalisa Porcelli",
                "email": "annalisa@student.it",
                "role": "student"
            },
            {
                "id": 1,
                "name": " Linda Raimondo",
                "email": "linda@student.it",
                "role": "student"
            }
        ],
        "students": [
            " Annalisa Porcelli",
            " Linda Raimondo"
        ]
    }
]

## POST `/api/assignments/:id/evaluate` - Evaluate an assignment
  - request parameters : `id` (ID assignment )
  - request body content:
        {
          "evaluation": 28
        }
  - response:  
    - `204 No Content`, (Score submitted)
    - `403 Forbidden` (User != teacher), 
    - `422 Unprocessable Entity`: (missing or non valid score), 
    - `500 Internal Server Error`: (errore DB)


## STUDENTS
## GET `/api/student/assignments` - Get all assignments for a student
  - request parameters : none
  - response:  
    - `200 OK` (list of assignments), 
    - `403 Forbidden`: (user != student) 
    - `500 Internal Server Error`: errore DB
  - response body content:
      [
    {
        "id": 37,
        "question": "What is the capital of Ireland? ",
        "isOpen": false,
        "evaluation": 30,
        "teacherId": 21,
        "createdAt": "2025-07-06T16:34:15.855Z"
    },
    {
        "id": 38,
        "question": "Value of gravitational acceleration",
        "isOpen": true,
        "evaluation": null,
        "teacherId": 21,
        "createdAt": "2025-07-06T16:34:15.855Z"
    }
]

## GET `/api/student/assignments/details` - Get all assignments details for a student with group, answer and score
  - request parameters : none
  - response:  
    - `200 OK` (details list of assignmets), 
    - `403 Forbidden` (User != student), 
    - `500 Internal Server Error`: errore DB

  - response body content:
[
    {
        "id": 37,
        "question": "What is the capital of Ireland? ",
        "isOpen": 0,
        "evaluation": 30,
        "teacher_id": 21,
        "answerText": "Dublin",
        "teacherName": " Fulvio Corno",
        "group": [
            {
                "id": 0,
                "name": " Annalisa Porcelli",
                "email": "annalisa@student.it",
                "role": "student"
            },
            {
                "id": 3,
                "name": " Silvia La Notte",
                "email": "silvia@student.it",
                "role": "student"
            },
            {
                "id": 4,
                "name": " Erica Porcelli",
                "email": "erica@student.it",
                "role": "student"
            },
            {
                "id": 5,
                "name": " Anna Valente",
                "email": "anna@student.it",
                "role": "student"
            }
        ]
    },
    {
        "id": 38,
        "question": "Value of gravitational acceleration",
        "isOpen": 1,
        "evaluation": null,
        "teacher_id": 21,
        "answerText": null,
        "teacherName": " Fulvio Corno",
        "group": [
            {
                "id": 0,
                "name": " Annalisa Porcelli",
                "email": "annalisa@student.it",
                "role": "student"
            },
            {
                "id": 1,
                "name": " Linda Raimondo",
                "email": "linda@student.it",
                "role": "student"
            }
        ]
    }
]


## POST `/api/assignments/:id/answer` - Answer an assignment
  - request parameters : `id` (ID assignment )
  - request body content: 
    {
      "text": "9.81"
    }

  - response:  
    - `204 No Content` (submitted answer), 
    - `403 Forbidden` (User != student),
    - `422 Unprocessable Entity`(missing text), 
    - `500 Internal Server Error`(error DB) 

## GET `/api/assignments/:id/answers` - Get all answers for an assignment
  - request parameters : `id` (ID assignment )

  - response:  
    - `200 OK` ( list of answers), 
    - `404 Not Found` (missing text), 
    - `500 Internal Server Error`: (error DB) 
  - response body content:
    [
        {
            "id": 1,
            "assignmentId": 37,
            "studentId": null,
            "text": "Dublin",
            "submissionDate": "2025-07-05T22:00:00.000Z"
        }
    ]

## GET `/api/students` - Get all students
  - request parameters : none
  - response:  
    - `200 OK` (list of students), 
    - `403 Forbidden` (User not logged-in), 
    - `500 Internal Server Error`: errore DB

  - response body content:
  
    [
      {"id": 0,"name": " Annalisa Porcelli"},
      {"id": 1,"name": " Linda Raimondo"},
      ...
    ]
 

## GET `/api/teachers` - Get all teachers
  - request parameters : none
  - response:  
    - `200 OK` (list of teachers), 
    - `403 Forbidden` (User not logged-in), 
    - `500 Internal Server Error`: errore DB

  - response body content:
  [
    {
        "id": 21,
        "name": " Fulvio Corno"
    },
    {
        "id": 22,
        "name": " Francesca Russo"
    }
    
  ]

## Database Tables
- Table `users` - contains the users registered on the platform:

  - "id"	INTEGER NOT NULL UNIQUE,
	- "name"	TEXT NOT NULL,
	- "email"	TEXT NOT NULL,
	- "password"	TEXT NOT NULL,
	- "salt"	TEXT NOT NULL,
	- "role"	TEXT NOT NULL CHECK("role" IN ('student', 'teacher')),
	- PRIMARY KEY("id" AUTOINCREMENT)

- Table `assignments` - contains the assignments created by teachers:
  - "id"	INTEGER NOT NULL,
	- "question"	TEXT NOT NULL,
	- "isOpen"	INTEGER NOT NULL DEFAULT 1 CHECK("isOpen" IN (0, 1)),
	- "evaluation"	INTEGER CHECK("evaluation" BETWEEN 0 AND 30),
	- "teacher_id"	INTEGER NOT NULL,
	- PRIMARY KEY("id" AUTOINCREMENT),
	- FOREIGN KEY("teacher_id") REFERENCES "user"("id")

- Table `answers` - contains the answers submitted by students:
  - id INTEGER PRIMARY KEY AUTOINCREMENT,
  - assignment_id INTEGER NOT NULL REFERENCES assignments(id),
  - student_id INTEGER, 
  - text TEXT NOT NULL,
  - submission_date TEXT NOT NULL

- Table `groups` - contains the groups created by teachers
  - assignment_id INTEGER NOT NULL,
  - student_id INTEGER NOT NULL,
  - PRIMARY KEY (assignment_id, student_id),
  - FOREIGN KEY (assignment_id) REFERENCES assignments(id),
  - FOREIGN KEY (student_id) REFERENCES "user"(id)

## Main React Components
- `AssignmentList` (in `AssignmentList.jsx`): Shows the list of teacher's assignments, allowing to view details, open answers, and evaluate students if the assignment is still open.

- `LoginForm` (in `AuthComponents.jsx`): Renders the login interface. Validates credentials and triggers the session creation via API.

- `ClassOverview` (in - `ClassOverview.jsx`): Displays a sortable summary of all students with counts of open/closed assignments and their weighted average score.

- `OpenAssignments` (in `OpenAssignments.jsx`): Displays all open assignments where the student can still modify their group answer.

- `ClosedAssignments` (in `ClosedAssignments.jsx`): Closed assignments with final evaluation scores, used in the student view.

- `CreateAssignment` (in `CreateAssignment.jsx`): Allows teachers to create new assignments by selecting a question and a group of students (2–6), avoiding duplicate groups more than twice.

- `DefaultLayout` (in `DefaultLayout.jsx`): Shared layout of the application taht includes the top navigation bar (with user info and logout), message alerts for errors and success, and renders the content of the current route

- `StudentAssignmentsPage` (in `StudentAssignmentsPage.jsx`): Displays all assignments the student is involved in.
Allows filtering by teacher/status, editing open assignments, and viewing scores and group details for closed ones.

- `StudentDashboard` (in `StudentDashboard.jsx`): Dashboard for students with submission percentage, average score progress bar, and pie chart of open vs closed assignments.

- `StudentSummary` (in `StudentSummary.jsx`): Card-like visual component used inside the teacher dashboard to show individual student statistics (used in `ClassOverview`).

- `TeacherDashboard` (in `TeacherDashboard.jsx`): Teacher's main interface, presenting navigation to assignment list, creation assignment, and class overview.

- `TeacherHome` (in `TeacherHome.jsx`): Teacher home showing the number of students and direct link to the class performance summary.


## Screenshot

[Screenshot1](https://github.com/polito-WA1-2025-exam/exam-2-annalisaporcelli/blob/main/screenshots/Screenshot%202025-07-06%20201355.png/)
[Screenshot2](https://github.com/polito-WA1-2025-exam/exam-2-annalisaporcelli/blob/main/screenshots/Screenshot%202025-07-06%20201430.png/)

## Users Credentials

- (email, password): fulviocorno@teacher.it , password 
- (email, password): francescarusso@teacher.it , password 
- (email, password): annalisa@student.it, password 
- (email, password): silvia@student.it, password 
