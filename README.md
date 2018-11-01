# Echobot :speech_balloon:
Construye un chatbot completamente funcional desde cero con este tutorial que está enfocado para principiantes, con un Bot desarrollado en **Azure Bot Framework** que repite lo que recibe (tutorial básico) utilizando el SDKv3.

![Echobot en Funcionamiento!](http://g.recordit.co/u404D68ybO.gif?raw=true){height="50%" width="50%"}


## Instalación y primeros pasos ## 

Antes de poder chatear con tu bot debes completar algunos pasos para preparar tu ambiente de desarrollo:

### Instalación de NodeJs ###
Si bien el Framework o marco de trabajo de Azure soporta C# como lenguaje de programación, nosotros nos enfocaremos en **NodeJs** que es una arquitectura modular basada en lenguaje **javascript** ejecutada del lado del servidor.

Dependiendo de tu sistema operativo puedes descargar el archivo correspondiente para instalar NodeJs en tu computador. En este caso yo estoy utilizando un Macbook con OSX EL CAPITAN (v10.11.6) para el desarrollo del Chatbot.
Puedes descargar NodeJS [aquí!](https://nodejs.org/es/)

Para este tutorial estoy utilizando la versión 8.9.4 de NodeJs. Si quieres revisar cuál versión tienes instalada puedes ejecutar el comando 

`node --version`

### El REPL de NodeJs ###

Node viene instalado con una implementación de REPL para que puedas ir aprendiendo y conociendo más de este programa.
Las siglas REPL significan **Read-Evaluate-Print-Loop**, algo así como Leer-Evaluar-Imprimir-Volver al inicio. Para utilizar el REPL de Node basta con ejecutar el comando `node` desde el terminal e ingresarás al loop de node del cual puedes salir escribiendo `.exit` en el mismo terminal.


### NPM / El manejador de paquetes de NodeJs ###
NodeJs viene con un manejador de paquetes de instalación llamado Node Package Manager (npm) y es el programa que permite llevar la instalación/configuración/desinstalación de los paquetes/módulos de node y sus respectivas dependencias.
Para utilizarlo basta con escribir en un terminal `npm --help` para tener información de los comandos disponibles.


### Preparando nuestro archivo de dependencias ###

Para preparar nuestro ambiente de desarrollo en Node es importante que iniciemos la construcción de los módulos sobre los cuales se ejecutará nuestro Bot.
La manera en la que NodeJs lleva control de los paquetes requeridos e instalados para una aplicación es a través de un archivo denominado package.json. 

#### El archivo package.json ####
Este archivo está construido en formato JSON (Javascript Object Notation) y contiene información general sobre la aplicación desarrollada así como también de los módulos requeridos y sus dependencias para nuestra aplicación de NodeJs específica.
Para crear el archivo Json de dependencias de nuestro bot debemos ejecutar el comando

`npm init`

y una vez finalizado obtendremos un archivo package.json en la raíz de nuestro directorio que contiene las prinicipales informaciones sobre nuestra aplicación.

![Lleando básico package.json](imagen_package_basico.png?raw=true "Archivo package.json")

### Instalando el SDK3 Del Azure Bot Framework ###

Microsoft ya tiene disponible en Beta el SDK4 del Bot Framework que tiene muchas novedades para los desarrolladores, sin embargo, todavía no recomiendan el desarrollo en producción de bots con el SDK4. En este tutorial nos enfocaremos en el SDK3 (última versión estable). Para instalar la versión del SDK3 debes utilizar el manejador de paquetes con el comando en la terminal:

`npm install botbuilder@3.15.0`

Este comando instalará las librerias en version 3.15.0 del SDKv3 de Azure Bot Framework. Si no incluyes el comando @3.15.0 se instalará la versión más reciente del bot framework (puede ser la sdkv4) y no funcionará este tutorial correctamente.
Si instalaste por error la versión 4 hacia arriba puedes desinstalar fácilmente el módulo ejecutando en el terminal: 

`npm uninstall botbuilder`

### Listado de dependencias en package-lock.json ###

Si lograste instalar correctamente la libreria botbuilder verás que se modificó el archivo package.json incluyendo automáticamente en las dependencias el módulo recién instalado.
Adicional a eso se creó un archivo package-lock.json que incluye todas las dependencias y sus versiones respectivas que necesita el módulo botbuilder para su correcto funcionamiento.
Mira un vistazo del archivo aquí.

Package.json incluyendo la dependencia del botbuilder



Ejemplo del package-lock.json




### Instalación del bot framework emulator ###

Como el nombre lo indica, el bot framework emulator permite realizar pruebas y debugging de modo local de los chatbots que desarrolles. Sólo debes elegir la conexión local (localhost) al puerto 3978. 
Incluso puedes realizar debugging de conexiones remotas utilizando ngrok, pero eso escapa del objetivo de nuestro tutorial. Si quieres hacer tunneling para tus conexiones puedes ver la información de ngrok [aquí!](https://ngrok.com/)

![Bot Framework Emulator](BotFrameworkEmulator.png?raw=true "Emulador de conversaciones Bot Framework")


## Manos al código ##
El código de este primer bot es muy sencillo, sin embargo engloba las librerías más importantes para trabajar en cualquier bot. Ahora voy a darte el detalle de cada módulo/librería utilizada para trabajar sobre nuestro primer Chatbot.

### El módulo restify ###

Para poder construir nuestras conexiones con el bot de azure necesitamos un middleware que nos permita realizar solicitudes REST. Para eso utilizamos [Restify!](http://restify.com/) que es un marco de trabajo que nos facilita las solicitudes y respuestas a APIS Rest.
Para instalar restify en tu directorio de trabajo sólo debes ejecutar el comando npm i, es igual a npm install, 

```javascript
npm i restify
```

Automáticamente el manejador de paquetes npm incluirá la dependencia en el archivo package.json.

Para incluir restify en tu archivo de trabajo app.js debes incluir la siguiente línea de código:

```javascript
var restify = require('restify');
```

Utilizaremos el módulo restify para enviar mensajes usando POST al ChatConnector de Azure

```javascript
// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});
```

La función (restify.createServer(); crea una instancia para enviar y recibir solicitudes REST. Luego la línea server.listen(...) abre una conexión al puerto 3978 de nuestro computador para escuchar solicitudes a ese puerto específico.

Las variables de entorno process.env.port y process.env.PORT son utilizadas cuando haces deploy sobre servidores remotos en los que no necesariamente esté disponible el puerto 3978 o en donde otras configuraciones hagan que se utilicen puertos distintos. Para este tutorial utilizaremos el puerto 3978.



## El Azure bot Framework ##

El Marco de Trabajo (Framework) de Azure utiliza dos componentes principales para funcionar

### Chat Connector ###

Está encargado del **enrutamiento de solicitudes de distintos canales** de los usuarios (SMS, email, Slack, Web) hacia la lógica de tu chatbot. Para lograr crear un conector se necesita tener una clave de Aplicación (AppId) y una contraseña de Aplicación(AppPassword) para autenticación. En el caso de uso local no necesitas tener clave. Cuando quieras colocar en producción tu chatbot vas a necesitar generar estas claves.
```javascript
var inMemoryStorage = new builder.MemoryBotStorage();
// Create chat connector for communicating with the Bot Framework Service
var connector = new builder.ChatConnector({
    appId: process.env.MicrosoftAppId,
    appPassword: process.env.MicrosoftAppPassword
});
```
Para escuchar los mensajes enviados necesitamos conectar nuestro servicio restify con el ChatConnector de Azure. 
Eso lo realizamos con la siguiente línea de código.
```javascript
// Enviamos mensajes al Endpoint /api/messages del ChatConnector 
server.post('/api/messages', connector.listen());
```
### Universal Bot de Azure ###

Por último necesitamos incluir la lógica de negocios de nuestro Chatbot en la que se procesarán los mensajes enviados por el usuario y responderemos según corresponda. En este primer tutorial sólo repetiremos el mensaje recibido por el usuario.
```javascript
// Recibir los mensajes del ChatConnector y agregar lógicas de respuesta
var bot = new builder.UniversalBot(connector, function (session) {
    session.send("Tú dijiste: %s", session.message.text);
});
```
En este código instanciamos una variable bot del objeto UniversalBot que toma una conexión del ChatConnector (connector) y ejecuta una función al recibir mensajes a través de ese Conector.
La variable session.message.text contiene el texto enviado por los usuarios.
El método session.send(...) envía mensajes de vuelta al usuario a través del ChatConnector con el texto "Tu dijiste: mensaje" donde mensaje es el texto enviado por el usuario.



## Test y debug con el Azure Bot Emulator ##

**FELICITACIONES**

Si has llegado hasta aquí ya tienes un chatbot funcionando!!!
Para probarlo sólo debes ejecutar en un terminal:

```javascript
node app.js

```
Y abrir el botframework emulator para seleccionar la conexión con localhost al puerto 3978 y tendrás un chatbot que repite lo que le envías! No es muy emocionante pero ya has revisado el contenido básico de desarrollo de Chatbots mucho más complejos!

## Deja una estrella :star: :star2: ##

Encontraste útil este tutorial??. **Deja una estrella en el repositorio como muestra de agradecimiento!**

