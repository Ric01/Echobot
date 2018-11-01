# Echobot :speech_balloon:
Tutorial para principiantes con un Bot desarrollado en **Azure Bot Framework** que repite lo que recibe (tutorial básico) utilizando el SDKv3.


## Instalación y primeros pasos ## 

Antes de poder chatear con tu bot debes completar algunos pasos para preparar tu ambiente de desarrollo:

### Instalación de NodeJs ###
Si bien el Framework o marco de trabajo de Azure soporta C# como lenguaje de programación, nosotros nos enfocaremos en **NodeJs** que es una arquitectura modular basada en lenguaje javascript ejecutada del lado del servidor.

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

Como el nombre lo indica, el bot framework emulator permite realizar pruebas y debugging de modo local de los chatbots que desarrolles.

![Bot Framework Emulator](BotFrameworkEmulator.png?raw=true "Emulador de conversaciones Bot Framework")


## Manos al código ##


### El módulo restify ###

## El Azure bot Framework ##

### Chat Connector ###

### Universal Bot de Azure ###

## Test y debug con el Azure Bot Emulator ##

