# Documentation Node


## Conexión y Consultas a la Base de Datos MySQL

- **Conexión a MySQL**: Para establecer una conexión con una base de datos MySQL, puedes utilizar el paquete `mysql2` en Node.js. Aquí tienes un ejemplo de cómo conectarte a una base de datos MySQL:

```javascript
const mysql = require('mysql2');

// Crear una conexión
const connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'contraseña',
  database: 'basedatos'
});

// Conectar a la base de datos
connection.connect((error) => {
  if (error) {
    console.error('Error al conectar a la base de datos:', error);
  } else {
    console.log('Conexión exitosa a la base de datos MySQL');
  }
});
```

- **Consultas**: Para ejecutar consultas SQL en una base de datos MySQL, puedes utilizar el método `query` del objeto de conexión. Tenemos un ejemplo de una consulta basica tipo get para obtener citas de forma ordenada.

```javascript
// Consulta get
storageCita.get('/', proxyCita, (req, res) => {
    const action = 'SELECT * FROM cita ORDER BY cit_fecha ASC';
    conx.query(
        action, (err, result)  => {
            if (err) {
                console.error('Error de conexion:', err.message);
                res.status(200);
            } else {
                res.send(JSON.stringify(result));
            }
        })
})
```


## Variables de Entorno (ENV)

- **Variables de Entorno**: Puedes utilizar variables de entorno para configurar valores específicos de tu aplicación, como las credenciales de conexión a la base de datos MySQL. Asegúrate de cargar estas variables desde un archivo `.env` utilizando el paquete `dotenv`. Aquí tienes un ejemplo:

1. Crea un archivo `.env` en la raíz de tu proyecto y agrega las variables de entorno:

```
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=contraseña
DB_DATABASE=basedatos
```

2. En tu archivo de configuración o archivo principal, utiliza `dotenv` para cargar las variables de entorno:

```javascript
require('dotenv').config();

const dbConfig = {
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_DATABASE
};

// Utiliza las variables de entorno en tu aplicación
console.log('Configuración de la base de datos:', dbConfig);
```

## Rutas (Routes)

- **Rutas**: Puedes definir rutas utilizando el enrutador (`Router`) de Express. Su principal funcion es modularizar y estructurar mejor las diferentes secciones que puede tener una aplicacion- Aquí tienes un ejemplo de cómo definir una ruta para manejar una solicitud GET a la ruta "/users":

```javascript
const express = require('express');
const app = express();
const userRouter = express.Router();

// Definición de la ruta
userRouter.get('/', (req, res) => {
  res.send('Obteniendo todos los usuarios');
});

// Agregar el enrutador a la aplicación
app.use('/users', userRouter);

// Iniciar el servidor
app.listen(3000, () => {
  console.log('Servidor iniciado en el puerto 3000');
});
```

## Middlewares

- **Middlewares**: Los middlewares son funciones que se ejecutan antes de que se maneje una solicitud o después de que se envía una respuesta. Puedes utilizar middlewares para realizar tareas como autenticación, manejo de errores o análisis de datos. Aquí tienes un ejemplo de un middleware de registro de solicitudes:

```javascript
function logRequest(req, res, next) {
  console.log(`Solicitud recibida: ${req.method} ${req.url}`);
  next();
}

app.use(logRequest);
```

## Operaciones de E/S no bloqueantes y Devoluciones de llamada (callbacks)

- **Operaciones de E/S no bloqueantes**: Utiliza métodos no bloqueantes y asíncronos para realizar operaciones de entrada/salida sin detener la ejecución principal de tu programa. Por ejemplo, en Node.js, puedes utilizar el módulo `fs` para realizar operaciones de archivo no bloqueantes. Aquí tienes un ejemplo de lectura de un archivo:

```javascript
const fs = require('fs');

fs.readFile('archivo.txt', 'utf8', (error, data) => {
  if (error) {
    console.error('Error al leer el archivo:', error);
  } else {
    console.log('Contenido del archivo:', data);
  }
});
```

- **Devoluciones de llamada (callbacks)**: En Node.js, los callbacks son funciones que se pasan como argumentos y se invocan después de que se complete una operación asincrónica. Aquí tienes un ejemplo de un callback para manejar el resultado de una consulta a la base de datos:

```javascript
function getUsers(callback) {
  connection.query('SELECT * FROM users', (error, results) => {
    if (error) {
      callback(error, null);
    } else {
      callback(null, results);
    }
  });
}

getUsers((error, users) => {
  if (error) {
    console.error('Error al obtener los usuarios:', error);
  } else {
    console.log('Usuarios obtenidos:', users);
  }
});
```

## Módulos y npm

- **Módulos**: Los módulos te permiten organizar tu código en fragmentos reutilizables que proporcionan funcionalidad específica. En Node.js, puedes exportar e importar módulos utilizando el sistema de módulos CommonJS. Aquí tienes un ejemplo de cómo exportar e importar un módulo en Node.js:

```javascript
// En el archivo "operaciones.js"
function sum(a, b) {
  return a + b;
}

module.exports = { sum };

// En otro archivo
const { sum } = require('./operaciones');

console.log(sum(2, 3)); // Resultado

: 5
```

- **npm**: npm es el sistema de gestión de paquetes para Node.js. Te permite instalar, administrar y compartir módulos y dependencias fácilmente. Utiliza el comando `npm install` para instalar paquetes desde el registro npm. Por ejemplo:

```bash
npm install express
```

## Process

El objeto global `Process` en Node.js proporciona información y control sobre el proceso actual en ejecución. Puedes acceder a las variables de entorno utilizando `process.env` y a los argumentos de línea de comandos utilizando `process.argv`.

### Ejemplo de uso de Process:

```javascript
// Acceder a una variable de entorno
const dbHost = process.env.DB_HOST;
console.log('DB_HOST:', dbHost);

// Leer los argumentos de línea de comandos
const args = process.argv.slice(2);
console.log('Argumentos:', args);
```

En este ejemplo, estamos accediendo a la variable de entorno `DB_HOST` y leyendo los argumentos de línea de comandos. Asegúrate de configurar las variables de entorno adecuadas y pasar los argumentos necesarios al ejecutar tu aplicación.

## Common JS

Common JS es una especificación utilizada por Node.js para definir el sistema de módulos y cómo se importan y exportan los módulos en una aplicación. Puedes exportar módulos utilizando `module.exports` y importar módulos utilizando `require`.

### Ejemplo de uso de Common JS:

En el archivo `operaciones.js`:

```javascript
// Exportar una función sum
exports.sum = (a, b) => a + b;
```

En otro archivo:

```javascript
// Importar el módulo de operaciones
const operaciones = require('./operaciones');

// Utilizar la función sum del módulo
console.log(operaciones.sum(2, 3)); // Resultado: 5
```

En este ejemplo, estamos exportando una función `sum` desde el módulo `operaciones.js` y luego importándola en otro archivo para utilizarla.

## Nodemon

Nodemon es una herramienta útil durante el desarrollo de aplicaciones Node.js. Monitorea cambios en los archivos y reinicia automáticamente tu aplicación cuando se detecta un cambio, lo que te permite ahorrar tiempo y esfuerzo al no tener que reiniciar manualmente después de cada modificación.

### Ejemplo de uso de Nodemon:

1. Instala Nodemon globalmente utilizando el siguiente comando:

```bash
npm install -g nodemon
```

2. Ejecuta tu aplicación con Nodemon:

```bash
nodemon app.js
```

Ahora, cada vez que realices cambios en tus archivos, Nodemon reiniciará automáticamente la aplicación, lo que facilita el proceso de desarrollo y prueba.

## DTO (Data Transfer Object)

Los objetos de transferencia de datos (DTO) son utilizados para transferir datos entre diferentes componentes de una aplicación. Los DTO generalmente contienen solo los campos necesarios para una operación específica y se utilizan para minimizar el tráfico de red y mejorar el rendimiento.

### Uso de DTO en TypeScript con Decoradores

En TypeScript, puedes utilizar decoradores para definir y validar tus DTO de manera más eficiente. Aquí tienes un ejemplo de cómo definir un DTO utilizando decoradores:

```typescript
import { IsString, IsEmail } from 'class-validator';

class CreateUserDto {
  @IsString()
  name: string;

  @IsEmail()
  email: string;

  @IsString()
  password: string;
}

const userDto = new CreateUserDto();
userDto.name = 'John Doe';
userDto.email = 'john@example.com';
userDto.password = 'password';

console.log('Usuario DTO:', userDto);
```

En este ejemplo, estamos utilizando el paquete `class-validator` para aplicar validaciones a los campos del DTO. El decorador `@IsString()` garantiza que el campo `name` y `password` sean cadenas de texto válidas, mientras que `@IsEmail()` garantiza que el campo `email` sea una dirección de correo electrónico válida.


## Licencia
Este proyecto está bajo la Licencia ISC.

## Contacto
Si tienes alguna pregunta o sugerencia, no dudes en ponerte en contacto conmigo a través de [email](angelgg2020@outlook.com) o [Linkeind](https://www.linkedin.com/in/angel-velascoo/).
