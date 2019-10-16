---


---

<h1 id="práctica-1">Práctica 1</h1>
<p>Creación de una API atraves de una VM</p>
<h2 id="paso-1">Paso 1</h2>
<p>Se creó una maquina virtual con la cuenta de estudiantes de la UNAM, se configuró los puertos  3000 para el localhost.</p>
<h2 id="paso-2">Paso 2</h2>
<p><em><strong>Instalando Node.js</strong></em><br>
Se instaló Node.js y se actualizaron los paquetes de este. “Node.js” es un entorno de <em>JavaScript</em> en tiempo de ejecución multiplataforma, de código abierto. Está diseñado para construir aplicaciones en redes escalables.<br>
<strong>sudo apt-get update<br>
sudo apt-get install nodejs</strong><br>
El comando “sudo” sirve para que con un usuario común y corriente(privilegios y permisos de usuario normal que no sea administrador), pueda ejecutar ciertos comandos restringidos a otros usuarios, por lo general restringidos al “superusuario” o administrador del sistema.</p>
<ul>
<li>Instalar el gestor de paquetes NPM de NodeJs<br>
<strong>sudo apt-get install npm</strong></li>
<li>Instalar el generador de aplicaciones<br>
<strong>sudo npm install  express-generator@4  -g</strong></li>
</ul>
<p>NPM (Node Package Manager) es un gestor de paquetes, podemos tener cualquier librería disponible con solo una línea de código, npm ayuda a administrar nuestros módulos, distribuir paquetes y agregar dependencias de una manera sencilla.</p>
<h2 id="paso-3">Paso 3</h2>
<p><em><strong>Instalación Postgresql</strong></em><br>
PostgreSQL es un gestor de bases de datos relacional y orientado a objetos y es multiplataforma.</p>
<ul>
<li>Instalar PostgreSQL : sudo apt install postgresql postgresql-contrib</li>
<li>Ingresar al súperusuario postgres: sudo -i -u postgres</li>
<li>Abrir PostgreSQL: $psql Salir: postgres=#\q</li>
<li>Regresar al usuario: su -usuario</li>
<li>Ingresar a PostgreSQL sin cambiar de usuario: sudo -u postgres psql</li>
<li>Contraseña del súperusuario postgres:<br>
sudo -u postgres psql<br>
\password postgres<br>
\q<br>
sudo service postgresql restart</li>
<li>Creamos un rol para PostgreSQL desde el súperusuario: sudo -u<br>
postgres createuser --interactive</li>
<li>Desde PostgreSQL: createuser --interactive</li>
<li>Creamos una b.d desde el<br>
súperusuario: sudo -u postgres createdb test</li>
<li>Desde PostgreSQL:    createdb</li>
</ul>
<h2 id="paso-4">Paso 4</h2>
<p><em><strong>Creando el proyecto</strong></em></p>
<ul>
<li>Nos situamos en la carpeta home</li>
<li>Creamos una carpeta para nuestro proyecto con los siguientes<br>
comandos:<br>
sudo mkdir practicas<br>
cd practicas<br>
Instalamos pg-promise:<br>
sudo npm install pg-promise@5 --save<br>
Bluebird:<br>
sudo npm install bluebird@3 --save<br>
En la raiz de nuestro proyecto creamos un archivo llamado <em>queries.js</em><br>
sudo nano queries.js</li>
</ul>
<p>var promise = require(‘bluebird’);</p>
<p>var options = {<br>
// Initialization Options<br>
promiseLib: promise<br>
};</p>
<p>var pgp = require(‘pg-promise’)(options); //Instancia de pg-promise<br>
var connectionString = ‘postgres://localhost:5432/puppies’;<br>
var db = pgp({<br>
host: ‘localhost’,<br>
port: 5432,<br>
database: ‘puppies’,<br>
user: ‘postgres’,<br>
password: ‘postgres’<br>
});</p>
<p>// add query functions</p>
<p>module.exports = {<br>
getAllPuppies: getAllPuppies,<br>
getSinglePuppy: getSinglePuppy,<br>
createPuppy: createPuppy,<br>
updatePuppy: updatePuppy,<br>
removePuppy: removePuppy<br>
};</p>
<h2 id="paso-5">Paso 5</h2>
<p><em><strong>Creando la B.D</strong></em><br>
Nos colocamos de nuevo en la raíz del proyecto y creamos un archivo llamado <em>puppies.sql</em><br>
sudo nano puppies.sql</p>
<p>DROP DATABASE IF EXISTS puppies;<br>
CREATE DATABASE puppies;</p>
<p>\c puppies;</p>
<p>CREATE TABLE pups (<br>
ID SERIAL PRIMARY KEY,<br>
name VARCHAR,<br>
breed VARCHAR,<br>
age INTEGER,<br>
sex VARCHAR<br>
);</p>
<p>INSERT INTO pups (name, breed, age, sex)<br>
VALUES (‘Tyler’, ‘Retrieved’, 3, ‘M’);<br>
Y checamos que nuestra base funcione<br>
sudo -u postgres psql<br>
\l<br>
\c puppies<br>
\dt<br>
select * from pups;<br>
Para correr el esquema<br>
sudo -u postgres psql -f puppies.sql</p>
<h2 id="paso-6">Paso 6</h2>
<p><em><strong>Configurando rutas</strong></em><br>
Nos ubicamos en cd/routes<br>
Editamos el archivo <em>index.js</em>  con sudo nano index.js<br>
Añadimos funciones queries al archivo <em>queries.js</em><br>
var express = require(‘express’);<br>
var router = express.Router();<br>
var db = require(’…/queries’);<br>
router.get(’/api/puppies’, db.getAllPuppies);<br>
En mi caso solo escogí All Puppies<br>
function getAllPuppies(req, res, next) {<br>
db.any(‘select * from pups’)<br>
.then(function (data) {<br>
res.status(200)<br>
.json({<br>
status: ‘success’,<br>
data: data,<br>
message: ‘Retrieved ALL puppies’<br>
});<br>
})<br>
.catch(function (err) {<br>
return next(err);<br>
});<br>
}</p>
<h2 id="paso-final">Paso Final</h2>
<p><em><strong>Instalando Forever</strong></em><br>
sudo npm install -g forever<br>
sudo forever list<br>
sudo forever stop PID<br>
sudo forever start bin/www<br>
Y con esto se puede checar nuestra API con la IP pública de nuestra maquina virtual<br>
<a href="https://drive.google.com/file/d/1p7yTqVnO9tjryZMoBap8rsgI-56x2ZXQ/view?usp=sharing">https://drive.google.com/file/d/1p7yTqVnO9tjryZMoBap8rsgI-56x2ZXQ/view?usp=sharing</a></p>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

