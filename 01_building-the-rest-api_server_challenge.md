## Setup

Dependencies

Como clonamos el repositorio, obtuvimos todas las dependencias necesarias en nuestro boilerplate por ahora. No olvide que, después de la clonación, debe ejecutar npm install o npm i dentro de la carpeta. Para iniciar la aplicación, use el comando npm run dev desde la carpeta del proyecto.

¡No instale paquetes npm relacionados con authentication (user) todavía! La paciencia es una virtud.

### Definamos el modelo

Nuestra aplicación ya está conectada a la base de datos y podemos ver cuando ejecutamos la aplicación en nuestra terminal:
Connected to Mongo! Database name: "project-management-server".

¡Así que comencemos! Estamos creando una aplicación de gestión de proyectos, así que definamos un esquema para nuestros proyectos.

Agreguemos un archivo project.js en la carpeta models:

```js
// ---> models/project.js <----

// requerimos el paquete mongoose y a su 'Schema'

// declaramos un nuevo Schema en la constante 'projectSchema'

const projectSchema = new Schema({
  // ...
});

// utilizamos 'mongoose.model' para crear el modelo con el schema que acabamos de definir...

// ... y exportamos dicho modelo
```

Nuestros proyectos tendrán un título y una descripción tanto de tipo String como tasks de tipo ObjectId que hagan referencia al Task model. Más adelante agregaremos la owner property. Mongo agregará automáticamente un campo de id único generado automáticamente, por lo que no necesitamos especificarlo.

Nuestros proyectos tendrán algunas tareas, así que veamos qué podemos hacer con su estructura de datos. Dentro de la carpeta de models, cree un nuevo archivo task.js. Las tareas tendrán las siguientes propiedades:

```js
// ---> models/task.js <---

// requerimos el paquete mongoose y a su 'Schema'

// declaramos un nuevo Schema en la constante 'taskSchema'

const taskSchema = new Schema({
  // ...
});

// utilizamos 'mongoose.model' para crear el modelo con el schema que acabamos de definir...

// ... y exportamos dicho modelo
```

¡Excelente! Las estructuras de datos están definidas, así que procedamos a definir las rutas.

### Define the routes

Adoptando la arquitectura REST, proporcionaremos las siguientes rutas en nuestra API:

Project routes

<table>
<thead>
<tr>
<th>URL</th>
<th>HTTP verb</th>
<th>Request body</th>
<th>Action</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>/api/projects</code></td>
<td>GET</td>
<td>(empty)</td>
<td>Returns all the projects</td>
</tr>
<tr>
<td><code>/api/projects</code></td>
<td>POST</td>
<td>JSON</td>
<td>Adds a new project</td>
</tr>
<tr>
<td><code>/api/projects/:id</code></td>
<td>GET</td>
<td>(empty)</td>
<td>Returns the specified project</td>
</tr>
<tr>
<td><code>/api/projects/:id</code></td>
<td>PUT</td>
<td>JSON</td>
<td>Edits the specified project</td>
</tr>
<tr>
<td><code>/api/projects/:id</code></td>
<td>DELETE</td>
<td>(empty)</td>
<td>Deletes the specified project</td>
</tr>
</tbody>
</table>

Task routes

<table>
<thead>
<tr>
<th>URL</th>
<th>HTTP verb</th>
<th>Request body</th>
<th>Action</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>/api/tasks</code></td>
<td>POST</td>
<td>JSON</td>
<td>Adds a new task</td>
</tr>
<tr>
<td><code>/api/tasks/:id</code></td>
<td>GET</td>
<td>(empty)</td>
<td>Returns the specified task</td>
</tr>
<tr>
<td><code>/api/tasks/:id</code></td>
<td>PUT</td>
<td>JSON</td>
<td>Edits the specified task</td>
</tr>
<tr>
<td><code>/api/tasks/:id</code></td>
<td>DELETE</td>
<td>(empty)</td>
<td>Deletes the specified task</td>
</tr>
</tbody>
</table>

Avancemos y creemos dos archivos dentro de la carpeta de rutas: project-routes.js y task-routes.js.

Ya le mostramos todas las rutas para projects y tasks, así que comencemos a ingresarlas en estos archivos.

GET and POST

Lo primero que haremos es permitir que nuestros usuarios agreguen algunos proyectos en la base de datos, entonces comenzamos con la ruta POST para crear un proyecto. En el archivo project-routes.js, deberemos escribir la mencionada ruta:

```js
// ---> routes/project-routes.js <---

// requerimos 'express', 'mongoose' y asignamos el resultado de ejecutar 'express.Router' en una variable

// a su vez, requerimos nuestros modelos antes creados, asignándolos a constantes

// POST route => para crear un nuevo project
router.post("/projects", (req, res, next) => {
  // utilizamos el método create de mongoose y pasamos los parámetros del body para crear nuestro nuevo 'project' y guardarlo en BDD.

  Project.create({
    // datos provenientes del body
  })
    .then
    // devolvemos la respuesta en forma de json
    ()
    .catch
    // atrapamos el error y lo devolvemos en forma de json
    ();
});

module.exports = router;
```

Aquí utilizamos el método create() y pasamos los parámetros del body en la solicitud para crear un nuevo proyecto y guardarlo en la base de datos.
Ahora, vamos a app.js y, hacia el final del archivo, solicitemos el archivo project-routes.js recién creado. No olvides agregarles el prefijo api (este paso no es obligatorio pero nos ayudará a largo plazo).

```js
// ---> app.js <---



...

var projectsRouter = require("./routes/project-routes"); // <<< chequear que esta línea está declarada

...

// var app = express();

// Middleware Setup

...

app.use("/api", projectsRouter); // <<< verificar que esta línea esté declarada

// app.use('/api', require('./routes/task-routes'));
```

Puedes seguir adelante y usar Postman para probar esta ruta.

Ahora pasaremos a definir la primera ruta GET para la collection de proyectos:

```js
// ---> routes/project-routes.js <---


...

// GET route => para obtener todos los projects
router.get('/projects', (req, res, next) => {

//  1ro - utilizamos la referencia a nuestro modelo de projects;


//  2do - usamos el método find() para recuperar TODOS los projects y populamos el campo 'tasks'


//  3ro - utilizamos una promesa de Javascript (o async/await) para obtener una respuesta de nuestra BDD y la recuperamos como un objecto json.


//  4to - atrapamos, si los hay, los errores con un catch en forma de json.

});
```

Para estar seguros de que todo funciona, podemos/debemos probar la funcionalidad de esta ruta utilizando Postman.

### Complete the API

Ahora que validamos nuestras dos primeras rutas, completemos la API REST:

```js
// ---> routes/project-routes.js <---



...

// GET route => para traer un project específico
router.get('/projects/:id', (req, res, next)=>{

  // Validamos si el valor que recibimos a través de 'req.params.id' es un ObjectId válido (pista: método 'isValid' de MongoDB, retorna un booleano)

  // en caso que no, devolvemos una respuesta con un status 400, en forma de json con un mensaje de 'Specified id is not valid', y salimos de la función.

  //  utilizamos el método 'findById' y el id que validamos antes para encontrar nuestro projecto. Debemos usar promises o async/await.

  // nuestros projects tienen un array de tasks, por lo que podemos utilizar el método populate para obtener todos esos objectos (una vez hecha la búsqueda)

  // de encontrar el project buscado, devolvemos la respuesta en formato json, con un status nº 200.

  // en caso de error, atrapamos el mismo con un catch y lo devolvemos como json.

})

// --------------------------------------------------

// PUT route => para hacer un update de un project específico
router.put('/projects/:id', (req, res, next)=>{

  // Validamos si el valor que recibimos a través de 'req.params.id' es un ObjectId válido (pista: método 'isValid' de MongoDB, retorna un booleano)

  // en caso que no, devolvemos una respuesta con un status 400, en forma de json con un mensaje de 'Specified id is not valid', y salimos de la función.

  // utilizamos el método 'findByIdAndUpdate' y el id que validamos antes para encontrar y actualizar nuestro project (con el contenido de nuestro req.body)

  // ...then (o async/await), devolvemos la respuesta con un message, en json, de `Project with ${req.params.id} is updated successfully.`

  // en caso contrario, de haber un error, lo capturamos y devolvemos el mismo en forma de json

})

// --------------------------------------------------

// DELETE route => para eliminar un project específico
router.delete('/projects/:id', (req, res, next)=>{

  // Validamos si el valor que recibimos a través de 'req.params.id' es un ObjectId válido (pista: método 'isValid' de MongoDB, retorna un booleano)

  // en caso que no, devolvemos una respuesta con un status 400, en forma de json con un mensaje de 'Specified id is not valid', y salimos de la función.

  // utilizamos el método 'findByIdAndRemove' y el id que validamos antes para encontrar y borrar nuestro project de nuestra BDD.

  // ...then (o async/await), devolvemos la respuesta con un message, en json, de `Project with ${req.params.id} is removed successfully.`

  // en caso contrario, de haber un error, lo capturamos y devolvemos el mismo en forma de json

})

// module.exports = router;
```

Acabamos de utilizar 3 métodos Mongoose integrados para lograr lo que necesitábamos:

     - Un método para obtener el project especificado;
     - otro para actualizar el project especificado y;
     - un último método para eliminar el project especificado.

Probemos project-routes en Postman.

Al principio, creamos dos archivos dentro de la carpeta de rutas. Uno fue project-routes.js, que llenamos con rutas, y ahora haremos lo mismo para task-routes.js:

```js
// ---> routes/task-routes.js <---

const express = require("express");
const mongoose = require("mongoose");
const Task = require("../models/task");
const Project = require("../models/project");

const router = express.Router();

// GET route => para obtener un task específico
router.get("/tasks/:taskId", (req, res, next) => {
  //  buscamos 'task' por id, y devolvemos respuesta o error en formato json
});

// --------------------------------------------------

// POST route => para crear un nuevo task
router.post("/tasks", (req, res, next) => {
  // 1ro - utilizamos método create para generar una nueva 'task' con la información proveniente del cuerpo del request.
  // 2do - con la respuesta, haremos una búsqueda del project al cual pertenece el id que está dentro del task, y 'empujaremos' dicha respuesta dentro de nuestro projecto
  // 3ro - devolveremos la respuesta en formato json
  // 4tp - en caso de existir un error, lo atraparemos y devolveremos el mismo en forma de json.
});

// --------------------------------------------------

// PUT route => para hacer update a un task específico
router.put("/tasks/:id", (req, res, next) => {
  // Validamos si el valor que recibimos a través de 'req.params.id' es un ObjectId válido (pista: método 'isValid' de MongoDB, retorna un booleano)
  // en caso que no, devolvemos una respuesta con un status 400, en forma de json con un mensaje de 'Specified id is not valid', y salimos de la función.
  // utilizamos el método 'findByIdAndUpdate' y el id que validamos antes para encontrar y actualizar nuestro task (con el contenido de nuestro req.body)
  // ...then (o async/await), devolvemos la respuesta con un message, en json, de `Task with ${req.params.id} is updated successfully.`
  // en caso contrario, de haber un error, lo capturamos y devolvemos el mismo en forma de json
});

// --------------------------------------------------

// DELETE route => para eliminar un task específico
router.delete("/tasks/:id", (req, res, next) => {
  // Validamos si el valor que recibimos a través de 'req.params.id' es un ObjectId válido (pista: método 'isValid' de MongoDB, retorna un booleano)
  // en caso que no, devolvemos una respuesta con un status 400, en forma de json con un mensaje de 'Specified id is not valid', y salimos de la función.
  // utilizamos el método 'findByIdAndRemove' y el id que validamos antes para encontrar y borrar nuestra task de nuestra BDD.
  // ...then, devolvemos la respuesta con un message, en json, de `Task with ${req.params.id} is removed successfully.`
  // en caso contrario, de haber un error, lo capturamos y devolvemos el mismo en forma de json
});

module.exports = router;
```

También asegúrese de requerir estas rutas en app.js:

```js
// ---> app.js <---


...

// Here are routes:
...
app.use('/api', require('./routes/task-routes'));
```

Prueba todas las rutas a través de Postman. Cuando se asegure de que todo funciona correctamente, continúe con el siguiente paso para que podamos finalizar nuestro backend (al menos por ahora).

## Enable CORS requests

Sabemos que usaremos esta API para solicitudes provenientes de una aplicación diferente. Para el desarrollo, utilizaremos el servidor de react, que se ejecuta en el mismo puerto que nuestra API, y eso es 3000, así que ahora cambiemos el puerto de nuestra API a 4000 en lugar de 3000. Vaya al archivo .env y cambie el puerto a 4000.

Por defecto, los navegadores bloquearán la comunicación entre las aplicaciones por razones de seguridad, por lo que debemos configurar nuestro servidor para permitirlas.

Afortunadamente, hay un node module que puede ayudarnos: CORS

**Verificar que ya puede estar instalado si usas el boilerplate**

Instalarlo con:

```
$ npm install cors
```

Y en app.js, impórtelo al principio con todas los demás imports y úselo hacia el final del archivo, justo antes de las rutas, donde verá: // ADD CORS SETTINGS HERE TO ALLOW CROSS-ORIGIN INTERACTION:

```js
// ---> app.js <---



...

const cors = require('cors');

...

app.use(cors({
  credentials: true,
  origin: ['http://localhost:3000'] // <== esta será la url de nuestra React app (se ejecutará en el puerto 3000)
}));

// ROUTES MIDDLEWARE STARTS HERE:
...
```

credentials entrará en juego cuando presentemos a los users y origin points a las direcciones URL permitidas, en nuestro caso, es el servidor de React que se ejecuta en el puerto 3000. Como puede ver, origin es el array, de modo que nos da la oportunidad de agregar tantas URL como las necesitemos.
