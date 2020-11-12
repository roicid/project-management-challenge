Comencemos creando la aplicación React usando el comando CLI:

```
$ npx create-react-app project-management-client
```

Ya hemos actualizado el puerto donde se está ejecutando nuestro servidor; Lo cambiamos de 3000 a 4000, por lo que no necesitamos realizar ningún cambio en nuestro cliente. Resumiendo:

    project-management-server runs on http://localhost:4000
    project-management-client runs on http://localhost:3000

Para ejecutar nuestro servidor usamos npm run dev y para iniciar nuestro cliente, usamos npm start.

### Basic Structure

Todavía no profundizaremos en las rutas, pero por ahora crearemos dos componentes, para crear un proyecto y mostrarlo, y vamos desde allí.

Utilizaremos axios para realizar la llamada a nuestro servidor y para ese propósito, necesitamos instalarlo (ejecute la siguiente línea en la terminal del cliente. Asegúrese de ESTAR SITUADO EN LA CARPETA DEL PROYECTO recién creado):

```
$ npm install axios
```

Recuerde importar axios en la parte superior de cada componente que se está comunicando con el backend.

```
import axios from 'axios';
```

Como sabemos que tendremos algunas rutas, instalemos react-router-dom aquí también:

```
$ npm install react-router-dom
```

Entonces, comencemos: dentro de la carpeta src crea la carpeta components y en ella crea la carpeta projects. Dentro de la carpeta projects, cree AddProject.js y ProjectList.js. Hasta ahora, tenemos esta estructura:

```
src
├── components
    └── projects
            └── AddProject.js
            └── ProjectList.js
```

Comenzaremos construyendo el componente `<AddProject />`. Este componente mostrará el formulario y se encargará de su envío. Al manejar el envío del formulario, nos referimos al uso de axios para llegar a una ruta back-end y entregar algunos datos enviados desde el frontend (o simplemente podemos decir que los envió el usuario después de completar el formulario y enviarlo). Así que hagamos esto:

```jsx
// ---> components/projects/AddProject.js <---

// importamos React, React.Component y axios

class AddProject extends Component {
  //definimos nuestro constructor con props (no es necesario), y definimos un state con dos keys (title y description) con valor de String vacío.

  // definimos la función 'handleFormSubmit' para manipular el submit del formulario:
  handleFormSubmit = (event) => {
    // 1ro - evitamos el comportamiento por default que genera el evento 'submit'

    // 2do - definimos las variables que necesitamos para obtener los valores del state (title y description)

    // 3ro - utilizamos axios, y su método "post" para conectar con nuestra ruta del backend que crea un nuevo project. Le pasamos un objeto con las dos variables antes creadas.
    axios
      .post(
        [
          /*-> ruta post del backend<-*/
        ],
        {
          /* variables necesarias para crear una nuevo project */
        }
      )
      .then(() => {
        // this.props.getData()  <--- dejamos esta linea comentada por ahora (tendrá sentido muy pronto ^^)
        // 4to - volvemos a setear nuestros valores en el state a valor inicial
      });
    // 5to - de haber errores, los atrapamos en un catch y mostramos el error en consola.
  };

  // definimos la función 'handleChange' para manipular los cambios en los inputs del formulario
  handleChange = (event) => {
    // 1ro - declaramos variables para los valores name y value del event.target
    // 2do - seteamos para con cada uno de ellos, su homónimo en el state.
  };

  render() {
    // 1ro- devolvemos un form, y utilizamos la función para manipular el submit del form (handleFormSubmit)

    // 2do - agregamos al form inputs que estén vinculados con su homónimo en el state (pista: componente controlado), su 'value' y utilizamos el método para manipular los cambios (handleChange)

    // 3ro - agregamos finalmente un input de tipo 'submit' para enviar la información.
    return <div>(...)</div>;
  }
}

export default AddProject;
```

::: Nota al margen: Deje this.props.getData() comentado por ahora, pronto lo necesitaremos.

La mayor parte del código parece muy familiar, ¿verdad? Porque ya cubrimos los conceptos básicos del uso de formularios en React.
Importemos el componente `<AddProject />` a App.js y lo renderizaremos allí por ahora.

```jsx
// ---> App.js <---



// importar React y React.Component
// importar nuestro fichero .css (si existiera)
// importamos el componente AddProject que acabamos de crear

class App extends Component {
  render() {
    return (
      <div className="App">
        --->  llamamos al componente  <---
      </div>
    );
  }
}

export default App;
```

### CHECKPOINT

Ahora puede ir a http://localhost:3000 y verá un formulario allí. Asegúrese de que su servidor se esté ejecutando e intente agregar un nuevo proyecto. Verifique su base de datos y debería ver un nuevo project en la projects collection.
Entonces, cuando tenemos al menos un project en nuestra base de datos, procedamos a crear el componente `<ProjectList />`. Dentro de este componente usaremos axios para obtener todos los projects del backend.

```jsx
// ---> components/projects/ProjectList.js <---



// importamos React, React.Component, axios y el objeto 'Link' de react-router-dom

// además, importamos a nuestro componente AddProject


// 1ro - definiremos nuestro componente como componente de clase. Definiremos constructor y super (opcional).
class ProjectList extends Component {
  constructor() {
    super();
    //2do - definiremos dentro del state una variable igual a un array vacío, que luego utilizaremos para nuestros projectos
    this.state = { /* ... */ };
  }


  //3ro - definiremos un método que traiga todos nuestros projects:

  getAllProjects = () => {

    // 3.1 - hacemos una llamada axios a la ruta que creamos específicamente para esa tarea, que 'traiga' nuestros projects

    // 3.2 - 'then' utilizaremos la respuesta de dicha llamada para poblar la key que creamos en nuestro state.

  };

  // 4to - utilizamos un lifecycle method para asegurarnos que el pedido de todos los projects se realice tan pronto como se monte el componente.

  render() {

  //5to - renderizamos un mapeo de nuestra lista de proyectos y, para cada uno de ellos, usamos 'Link' para dirigir al usuario a la página del detalle de cada project (utilizaremos aquí la ruta que creamos para dicho fin)

  //6to - Llamamos a nuestro componente 'AddProject' y le pasamos por props el método que definimos para traer todos nuestros projects.
    return (
      <div>
        <div>
          {[/* lista de projects*/].map((project) => {
            return (
              // usamos el '_id' de cada project como 'key'
              <div key={project._id}>
                <Link to={/* ruta del back*/}>
                  <h3>{project.title}</h3>
                </Link>
              </div>
            );
          })}
        </div>
        <div>
          --> componente AddProject <--
        </div>
      </div>
    );
  }
}

export default ProjectList;
```

Veamos qué es interesante aquí:

- utilizamos el método del ciclo de vida de componenteDidMount() para obtener los datos de la API (esta es una práctica muy estándar);
- usamos map() para enumerar los proyectos (no olvide dar a cada elemento el ID de la base de datos como clave con key = {project._id});
- usamos el componente `<Link />` de la biblioteca react-router-dom para poder cambiar dinámicamente la URL y, en nuestro caso, para ir a la página de detalles del proyecto (eso es lo siguiente en lo que debemos trabajar);
- vemos que el componente `<AddProject />` estará realmente anidado dentro de `<ProjectList />`, así que vamos y elimínelo de App.js y también dejemos de comentar this.props.getData() dentro de `<AddProject />`;
- comentamos la línea de project description ya que construiremos la página de detalles, la descripción será visible allí.

Antes de ir a http://localhost:3000 y ver la lista de proyectos, primero tenemos que configurar el Router. Si recuerdas, comenzamos el proceso configurando el núcleo de nuestra aplicación, index.js, donde representamos el componente madre `<App />`.

Después de importar BrowserRouter, lo usamos envolviendo la etiqueta `<App />`.

```jsx
// ---> index.js <---

import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import * as serviceWorker from "./serviceWorker";
import { BrowserRouter as Router } from "react-router-dom"; // <== !!!

ReactDOM.render(
  <Router>
    <App />
  </Router>,
  document.getElementById("root")
);
serviceWorker.unregister();
```

Ahora que 'ProjectList' tiene anidado a 'AddProject', podemos reemplazarlo en nuestro componente 'App':

```jsx
// ---> App.js <---

import React, { Component } from "react";
import "./App.css";
import ProjectList from "./components/projects/ProjectList";

class App extends Component {
  render() {
    return (
      <div className="App">
        <ProjectList />
      </div>
    );
  }
}

export default App;
```

Agreguemos otro archivo en la carpeta de components/projects, un ProjectDetails.js y agreguemos solo unas líneas de código por ahora:

```jsx
// ---> components/projects/ProjectDetails.js <---



// importamos React, React.Component, axios, y traemos de react-router-dom a 'Link'.


class ProjectDetails extends Component {

  // 1ro - definimos nuestra clase ProjectDetails

  // 2do - definimos su constructor con 'props' como argumento (ya que pasaremos los 'details' desde un componente padre). Esto es opcional.

    // 3ro - utilizamos el método 'super' para utilizar dichas 'props'

    // 4to - definimos el state en un ppio. como un objeto vacío.

  }

  render() {
    return <div>Welcome to project details page!</div>;
  }
}

export default ProjectDetails;
```

Avanzando, dentro de la carpeta components creemos otra carpeta navbar y dentro de esta carpeta creemos Navbar.js.
Hasta ahora, tenemos esta estructura:

```
src
├── components
    └── projects
    ├       └── AddProject.js
    ├       └── ProjectList.js
    ├       └── ProjectDetails.js
    ├
    └── navbar
          └── Navbar.js
```

Nuestro Navbar.js tendrá algunos enlaces en el futuro, pero en este momento solo uno en realidad:

```jsx
// ---> components/navbar/Navbar.js <---

// importamos React y 'Link' del paquete react-router-dom

// declaramos un componente funcional 'navbar' que devuelva, por el momento, una navbar con un único 'Link' dirigido a nuestra ruta '/projects'
const navbar = () => {
  return <nav className="nav-style">// ...</nav>;
};

export default navbar;
```

Ahora hemos configurado todo para poder ir a App.js para configurar algunas rutas allí:

```jsx
// ---> App.js <---



import React, { Component } from "react";
import "./App.css";
// importamos Switch y Route de 'react-router-dom
// importamos nuestros componentes ProjectList, Navbar y ProjectDetails

class App extends Component {
  render() {

    // renderizaremos nuestro componente 'Navbar'

    // utilizaremos al componente 'Switch' y dentro de él dos componentes 'Route' a los cuales les pasaremos paths específicos:

    // 1) uno irá a la ruta del back que muestra todos los projects (path), y utilizará al componente correspondiente a ello (component)

    // 2) el otro irá a la ruta que muestra el detalle de un project puntual (path), y utilizará el componente correspondiente a ello (component)


    return (
      <div className="App">
        <Navbar />
        <Switch>
          <Route exact path=/*ruta del back*/ component=/*componente correspondiente a la ruta*//>
          <Route exact path=/*ruta del back*/ component=/*componente correspondiente a la ruta*//>
        </Switch>
      </div>
    );
  }
}

export default App;
```

¡Ok genial! Además, no se preocupe de que nuestra / route todavía no se use, la guardaremos para login/signup, que seguirá en la próxima lección.

Por ahora podemos: crear un nuevo proyecto, mostrar todos los proyectos y al hacer click en el título del proyecto, se nos redirige a la página de detalles del project que por el momento no tiene mucho código. ¡Agreguemos un poco más!

```jsx
// ---> components/projects/ProjectDetails.js <---

//...

class ProjectDetails extends Component {
  //...

  // declaramos al método que nos garantice que nuestra función encargada de hacer la llamada a BDD (getSingleProject) se ejecute al montarse el componente

  // definimos el método que traiga el project individual buscado
  getSingleProject = () => {
    // 1ro - desestructuramos params de nuestro objeto this.props.match
    // 2do - hacemos una llamada axios a nuestra ruta del backend encargada de mostrar cada projecto individualmente
    // 3ro - 'then', con la respuesta, actualizamos nuestro state
    // 4to - atrapamos los errores, en caso de que hubiese, y los mostramos por consola.
  };

  render() {
    // retornamos en el render al title y description que tenemos en nuestro state (actualizado por nuestro método getSingleProject) y usamos al componente 'Link' para ir hacía nuestra ruta '/projects'
    return <div>// ...</div>;
  }
}

export default ProjectDetails;
```

### Checkpoint

Más o menos directo: componentDidMount() está ejecutando el método getSingleProject() que inicialmente se comunica con nuestra ruta de back-end a través de la llamada axios. Si todo tiene éxito, estamos actualizando el estado (usando nada más que setState ()) y equiparándolo al objeto del proyecto que obtuvimos de nuestra API. En la parte render(), accedemos a las propiedades del proyecto a través de this.state.title y this.state.description.

El próximo desafío es mostrar el formulario de edición y actualizar el proyecto. En primer lugar, crearemos un nuevo archivo dentro de la carpeta de components/projects: EditProject.js. Este componente tendrá el formulario que hará que onSubmit haga cambios en un project específico.

```jsx
// ---> components/projects/EditProject.js <---



// importamos React, React.Component y axios

// declaramos nuestra clase 'EditProject'
class EditProject extends Component {
  // definimos constructor y super para utilizar los props que recibirá este componente (opcional)
  constructor(props) {
    super(props);
    // definimos nuestro state con las keys del project que estaremos editando (title y description)
    this.state = {
      // ya que preveemos que recibiremos por props una variable con el project a editar, nos adelantaremos y lo llamaremos 'theProject'
      title: this.props.theProject.title,
      description: this.props.theProject.description,
    };
  }

  // definimos un método que se encargue del submit de nuestro form de edición
  handleFormSubmit = (event) => {
    // 1ro -  declaramos dos variables con los valores de nuestras keys del state (title y descripcion)

    // 2do - evitamos el comportamiento default al hacer el submit de un formulario.

    // 3ro - realizamos una llamada axios a nuestra ruta PUT del back encargada de actualizar nuestros projects, y le pasamos nuestras variables antes definidas para poder actualizar.

    .then(() => {
    // 4to - 'then', ejecutaremos el método 'getSingleProject' declarado en el componente padre de EditProject (es decir, ProjectDetails) que nos llega a través de props como 'getTheProject'...
        this.props.getTheProject();
    // ... y luego redirigimos a nuestra ruta '/projects'
        this.props.history.push("/projects");
      })
    // 5to - en caso de haber un error, lo atrapamos y mostramos en consola
  };



  // declaramos un método que se encargue de los cambios en el input de title y actualice su homónimo en el state
  handleChangeTitle = (event) => {

    //...

  };


  // declaramos un método que se encargue de los cambios en el input de description y actualice su homónimo en el state
  handleChangeDesc = (event) => {

    //...

  };



  render() {

    // retornamos en el render un form que ejecute, al hacer submit, la función que se encarga de ello y que, para cada input ejecute, ante algún cambio, las funciones antes declaradas que de ello se encargan (recordar que el componente debiera ser controlado, lo que hará que el value de cada input 'venga' del valor correspondiente del state).

    // por último, agregamos un input de tipo 'submit'

    return (
      <div>
        <hr />
        <h3>Edit form</h3>
        <form>

          // ...

        </form>
      </div>
    );
  }
}

export default EditProject;
```

En este punto, estamos muy familiarizados con los formularios: ya que construimos con éxito el componente `<AddProject />`. Como puede ver, este componente depende en gran medida de las props que se pasan a este componente y veremos justo ahora dónde y por qué.
Entonces, veamos ahora, dónde debemos renderizar este componente. Podemos hacer eso aquí dentro del componente `<ProjectDetails />`. Realmente es cuestión de su preferencia: si lo desea, también puede hacer que el formulario se muestre en una página separada. Procederemos a configurar la funcionalidad de edit/update dentro del details component.

```jsx
// ---> components/projects/ProjectDetails.js <---

//...

//importamos nuestro nuevo componente 'EditProject'

class ProjectDetails extends Component {

...

  // agregamos método para renderizar formulario de edit
    renderEditForm = () => {
      if(/* condición*/){
        // validamos que, si no existe key 'title' en nuestro state, deberiamos ejecutar el método que solicita los detalles del project individual

          } else {
            // retornamos el componente EditProject y le pasamos por props el project a editar (theProject), a getTheProject(ver componente EditProject) y al resto de props del componente ProjectDetails
            return <EditProject theProject={this.state} getTheProject={this.getSingleProject} {...this.props} />
          }
    }

   render(){


     return(
     // agregamos al return a nuestro 'renderEditForm' dentro de un <div>
      <div>
        <h1>{this.state.title}</h1>
        <p>{this.state.description}</p>
        <div>{this.renderEditForm()} </div> {/* <<<< this line */}
        <Link to={'/projects'}>Back to projects</Link>
      </div>
    )
    }
}
export default ProjectDetails;
```

Se llama al método renderEditForm() dentro del método render() y lo que hace es básicamente esto: comprueba si this.state tiene alguna propiedad (elegimos el título), y si eso no es cierto, invoca el método getSingleProject() que obtiene el objeto project de nuestra API y lo establece en el estado del componente. En la próxima ejecución de renderEditForm(), está renderizando el componente `<EditProject />` con props pasados ​​a sí mismo. Aquí podemos ver que lo que se está pasando es theProject, que es realmente EL project que estamos viendo en esta página de detalles y estamos pasando el método getSingleProject() como props getTheProject. Ahora, si miras hacia atrás a nuestro componente `<EditProject />`, todo tiene más sentido, ¿verdad?

Estamos casi al final de la primera parte de nuestra aplicación. Solo agreguemos la funcionalidad de eliminación para que podamos tener CRUD completo.

Dentro del componente `<ProjectDetails />`, agregaremos el método deleteProject() que hará llamadas axios al backend y eliminará el proyecto, y tendremos un botón que activará la ejecución de este método.
Nuestro componente `<ProjectDetails />` completo al final se ve así:

```jsx
// ---> components/projects/ProjectDetails.js <---


// ...

  // declaramos método de borrado de proyecto
  deleteProject = () => {
    // traemos valor de params del objeto que llega por props llamado 'match'

    // realizamos una llamada axios a la ruta de nuestro backend encargada de borrar un project puntual

    // 'then', redirigimos haciendo un push al objeto 'history' que llega por props con la ruta '/projects'

    // en caso de haber error, lo atrapamos y mostramos en consola
  };

  render() {
    return (
      <div>
        <h1>{this.state.title}</h1>
        <p>{this.state.description}</p>
        <div>{this.renderEditForm()} </div>
        ---> agregamos botón que, onClick, llame a la función que borra el project  <---
        <Link to={"/projects"}>Back to projects</Link>
      </div>
    );
  }
}

export default ProjectDetails;
```

Projects done!!! CRUD completo funciona, así que pasemos a la parte de tasks. En la mayoría de los casos, tendremos la situación de tener que anidar componentes uno dentro del otro. Así que hagámoslo: nuestras tasks pertenecen a projects, por lo que tendremos que anidarlas dentro del componente project.
Para comenzar a trabajar en esta parte de la aplicación, creemos primero la carpeta de components/tasks y nuestro primer componente relacionado con la tarea: AddTask.js y agreguemos el siguiente código:

```jsx
// ---> components/tasks/AddTask.js <---



// importamos React, React.Component y axios

// declaramos nuestro componente AddTask, con su constructor y super (opcional), que recibirán props del componente padre
class AddTask extends Component {
  constructor(props) {
    super(props);
    // definimos el state con el title y la description como strings vacíos, y un valor 'isShowing' que nos será util para mostrar/ocultar el form
  }

  // definimos el método que se encarga de gestionar lo que sucede ante el submit del form
  handleFormSubmit = (event) => {
    // prevenimos el comportamiento predefinido del evento submit

    // definimos tres variables: title y description a partir de la información que viene del state, y projectID como el id del project al cual perteneceran las task por crear, el cual llegará por props. (es importante saber que los nombres de las variables son importantes ya que deben coincidir con las definidas en la ruta del backend)

    // realizamos la llamada axios a la ruta POST del backend encargada de crear nuevas task, y pasamos las anteriores tres variables

    // 'then' llamamos al método 'getTheProject' que recibimos desde props a través de nuestro componente padre

    // reiniciamos los valores de state a strings vacíos para volver al comienzo

    // en caso de haber error, lo atrapamos y mostramos por consola.
  };


  //definimos un método que se encargue de controlar los cambios en los inputs y actualizar los valores en el state
  handleChange = (event) => {

    //...

};


  // creamos un método que se encargue de alternar el valor del booleano que definimos en nuestro state, el cual usaremos para mostrar/ocultar el form
  toggleForm = () => {

    //...

  };

  // definimos un método que controle lo que se mostrará cuando el valor this.state.isShowing sea verdadero (es decir, el formulario de Add Task)

  showAddTaskForm = () => {

    // en caso de que el valor de isShowing sea verdadero, mostramos el formulario, que tendra:
    // en cada input, un onChange que invoque a la función asignada a controlar el valor de dicho input y con él actualizar el valor del state; y un value, que se condiga con su homónimo del state.

    if (this.state.isShowing) {
      return (
        <div>
          <h3>Add Task</h3>
          <form onSubmit={-> método que controla submit <-}>

            //...

          </form>
        </div>
      );
    }
  };

  render() {
    // renderizamos un button que, onClick, oculta o muestra el form, y llamamos a nuestro método 'showAddTaskForm'
    return (
      <div>

      /...

      </div>
    );
  }
}

export default AddTask;
```

Ahora, cuando creamos el componente AddTask, tenemos que ponerlo en algún lugar de nuestra aplicación para que el usuario pueda verlo. Hagámoslo en la página de detalles del project.

```jsx
// ---> components/projects/ProjectDetails.js <---

...

// importamos el componente AddTask

class ProjectDetails extends Component {


  // ...

  // defino método para renderizar el formulario de addTask
  renderAddTaskForm = () => {
    if(!this.state.title){
      // si no existe this.state.title, llamo al método encargado de 'traer' el project individual
      } else {
        // pasamos el project y el método getSingleProject() como props al componente AddTask.
        return <AddTask theProject={this.state} getTheProject={this.getSingleProject} />
      }
  }


  render(){
    return(
      <div>
        <h1>{this.state.title}</h1>
        <p>{this.state.description}</p>
        {/* mostramos el heading de task sólo si hay tasks */}
        { this.state.tasks && this.state.tasks.length > 0 && <h3>Tasks</h3> }
        {/* mapeamos el array de tasks y...*/}
        { this.state.tasks && this.state.tasks.map((task, index) => {
            return(
                <div key={ index }>
                {/* ... hacemos que cada titulo sea un link a la página de detalle de dicha task */}
                    <Link to={`/tasks/${task._id}`}>
                        { task.title }
                    </Link>
                </div>
            )

        }) }
        <div>{this.renderEditForm()} </div>
        <button onClick={() => this.deleteProject()}>Delete project</button>
        <br/>
        <div>
            {/* llamamos al método renderAddTaskForm*/}
		    </div>

		<br/><br/><br/><br/><br/>

        <Link to={'/projects'}>Back to projects</Link>
      </div>
    )
  }
}

export default ProjectDetails;
```

Además, enumeremos las tasks debajo del título de cada project en la página principal /projects también. El usuario no puede hacer click en ellos, esa funcionalidad ya se encuentra en la página de detalles del proyecto.

```jsx
// ---> components/projects/ProjectList.js <---

// ...

class ProjectList extends Component {
  ...

   render(){
    return(
      <div>
        <div>
          { this.state.listOfProjects.map( project => {
            return (
              <div key={project._id}>
                <Link to={`/projects/${project._id}`}>
                  <h3>{project.title}</h3>
                </Link>
                {/*  agregamos una visualización de las tareas que existen a través de un mapeo de las tasks de cada project, y lo retornamos dentro de un <ul> (no olvidar la 'key')   */}
                <ul>
                  
                  //...

                </ul>
                {/* <p style={{maxWidth: '400px'}} >{project.description} </p> */}
              </div>
            )})
          }
        </div>
        <div>
            <AddProject getData={() => this.getAllProjects()}/> {/* <== !!! */}
        </div>
      </div>
    )
  }
}

export default ProjectList;
```

Terminaremos la lección mostrando la página de task details, así que creemos el archivo components/tasks/TaskDetails.js y agreguemos el siguiente fragmento de código:

```jsx
// ---> components/tasks/TaskDetails.js <---



// importamos React, React.Component y axios

// definimos nuestro componente TaskDetails, su constructor y super (opcional), los cuales recibiran 'props' desde su componente padre
class TaskDetails extends Component {
  constructor(props) {
    super(props);
    // definimos nuestro state por el momento como un objeto vacío.
    this.state = {};
  }

  // 1ro - nos aseguramos que se ejecute el método que obtiene la task individual tan pronto como se monte el componente


  // 2do - definimos el método que obtiene la task individual
  getTheTask = () => {
    // 2.1 - definimos 'params' a partir del objeto 'match' que tramos por props.
    
    // 2.2 - realizamos una llamada axios GET a nuestra ruta del backend creada para traer la información de una task individual
    
    // 2.3 - 'then', con la respuesta, seteamos el valor de nuestro state con el objeto que recibimos como respuesta.

    // 3ro - en caso de que hubiese un error, lo atrapamos y mostramos por consola.
  };

  render() {

    // por último, retornamos en el render el detalle del task, con tu title, description, y un button que nos permita regresar a la página anterior, utilizando el objeto 'history' que recibimos por props (pista, utilizar el 'onClick' para ejecutar el método 'goBack' de history.)
    return (
      <div>
        
        //...
        pro 
      </div>
    );
  }
}

export default TaskDetails;
```

Y ahora tenemos que "decirle" a nuestra app cuándo representar este componente, así que abramos App.js, importemos el componente TaskDetails y agreguemos la siguiente ruta entre la etiqueta `<Switch>` como la tercera ruta:

```jsx
// App.js

...

import TaskDetails from './components/tasks/TaskDetails'; // <== import the TaskDetails component


class App extends Component {
  render() {
    return (
      <div className="App">
       <Navbar />
        <Switch>
          <Route exact path="/projects" component={ProjectList}/>
          <Route exact path="/projects/:id" component={ProjectDetails} />
          {/* added to display task details page: */}
          <Route exact path="/tasks/:taskId" component={TaskDetails} /> {/* <== !!! */}
        </Switch>
      </div>
    );
  }
}

export default App;
```

Le dejamos ahora a usted agregar funciones de edición y eliminación de las tasks. Para los aventureros, intente agregar la propiedad “done” para las tareas completadas.
