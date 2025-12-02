## Esquema de la base de datos: 
┌──────────────┐          ┌────────────────┐          ┌─────────────┐
│    USERS     │          │   USER_COURSES │          │   COURSES   │
└──────────────┘          └────────────────┘          └─────────────┘
│ userId (PK)  │     ┌──▶ │ joinId (PK)    │ ◀──┐     │ courseId(PK)│
│ username     │     │    │ userId (FK)    │    │     │ courseName  │
│ password     │     │    │ courseId (FK)  │    │     │ description │
│ email        │     │    └────────────────┘    │     │ courseType  │
│ budget       │     │                          │     │ etsCredits  │
│ createdAt    │     │                          │     │ semester    │
│ updatedAt    │     └──────────────────────────┘     │ vacancies   │
└──────────────┘                                      └─────────────┘

## API endpoints 

### GET
`users` router.get('/all', userController.getAllUser) <br/>
Devuelve todos los usuarios <br/>
`users` router.get('/get/:id', userController.getUserById) <br/>
Devuelve un objeto de tipo UserDto <br/>
`users` router.get('/get/email/:email', userController.getUserByEmail) <br/>
Devuelve un objeto de tipo UserDto <br/>
-------------------------------------------------------------------------------
`courses` router.get('/all', courseController.getAllCourses) <br/>
Devuelve un array de objetos de tipo CourseDto <br/>
`courses` router.get('/get/:id', courseController.getCourseById) <br/>
Devuelve un objeto de tipo CourseDto <br/>
`courses` router.get('/get/join/:id', courseController.getJoinById) <br/>
Devuelve un objeto de tipo JoinDto <br/>
`courses` router.get('/get/join/all', courseController.getAllJoins) <br/>
Devuelve un array de objetos de tipo JoinDto <br/>
`courses` router.get('/get/join/user/:id', courseController.getUserJoins) <br/>
Devuelve un array de objetos de tipo JoinDto <br/>
`courses` router.get('/get/courses/user/:id', courseController.getUserCourses) <br/>
Devuelve un array de objetos de tipo CourseDto <br/>
`courses` router.get('/getVacancies/:id', courseController.checkVacancies) <br/>
Devuelve el número de vacantes disponibles <br/>

### POST
`users` router.post('/add', userController.addUser) <br/>
Devuelve el usuario creado o un código de error <br/>
`courses` router.post('/add', courseController.addCourse) <br/>
Devuelve el curso creado <br/>
`courses` router.post('/withdraw', courseController.withdrawCourse) <br/>
Devuelve un booleano o un mensaje dependiendo del resultado <br/>
`courses` router.post('/updateVacancies', courseController.updateVacancies) <br/>
Devuelve un booleano <br/>
`courses` router.post('/enrole', courseController.enroleCourse) <br/>
Devuelve un string que dependiendo del resultado de la operación puede tener varios valores: <br/>
1.'created' → Primera inscripción, se crea nueva fila JoinDto <br/>
2.'updated' → El usuario ya estaba inscrito, se actualiza la inscripción <br/>
3.'full' → No hay vacantes disponibles <br/>
4.'undefined' → Error desconocido <br/>

## Inicialización del proyecto

npm install typescript -D <br/>  
npm install --save express <br/>  
npm install --save @types/express -D <br/>  
npm install --save ts-node-dev -D <br/>  
npm run tsc -- --init <br/>  

### Ejecutar el servidor

npx ts-node-dev src/index.ts <br/>  
*(Asegúrate de reemplazar `src/index.ts` con la ruta de tu archivo principal de Express si es distinta)*

## Ejemplos de Request / Response

### ➕ Crear usuario  
POST /users/add <br/>  

Body <br/>

{
  "userId": "u001",
  "username": "alice",
  "password": "1234",
  "email": "alice@mail.com",
  "budget": 120
}

Response <br/>

{
  "userId": "u001",
  "username": "alice",
  "email": "alice@mail.com",
  "budget": 120
}

###  Crear curso  
POST /courses/add <br/>
Body <br/>

{
  "courseId": "c101",
  "courseName": "Quantum Computing",
  "courseDescription": "Intro to QIT",
  "courseType": "Major",
  "etsCredits": 7,
  "semester": 1,
  "vacancies": 25
}

Response <br/>

{
  "courseId": "c101",
  "courseName": "Quantum Computing",
  "vacancies": 25
}

### Inscribir usuario
POST /courses/enrole <br/>
Body <br/>
{
  "userId": "u001",
  "courseId": "c101"
}

Response <br/>
1. Curso tiene plazas : "created" <br/>
2. Curso lleno: "full" <br/>