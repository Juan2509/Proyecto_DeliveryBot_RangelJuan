# Proyecto DeliveryBot Juan Rangel

Antes de empezar quiero aclarar que algunas capturas son para probar el desarrollo de esta actividad, pero no son para usarse de guía.



## Planteamiento del problema:

La gestión de pedidos de cafetería suele ser ineficiente, provocando errores y filas largas debido a la falta de un sistema digital que permita al personal de cocina organizar sus tareas.



## Objetivo:

Implementar un sistema de pedidos digital mediante una interfaz conversacional intuitiva a través de Telegram que sea capaz de automatizar cálculo de  totales, gestionar el ciclo de vida de pedidos, optimizar la comunicación, generar reportes diarios de ventas y centralizar el inventario y menú.



## Proceso llevado a cabo:

Para iniciar es necesario crear una cuenta en la página oficial de n8n y acceder a una prueba gratuita de 14 días para poder trabajar en la nube.

### Telegram:

Para crear un asistente conversacional en Telegram se debió de buscar el bot llamado "BotFather" ya que este es quien maneja todos los bots de telegram.



<a href="https://ibb.co/zWgptHdT"><img src="https://i.ibb.co/nqy9K8pN/image.png" alt="image" border="0"></a>

Estando ya con BotFather se ejecutó el comando que permite crear un nuevo bot, posteriormente preguntó que nombre iba a tener para luego finalizar agregándole un identificador de usuario.

Así se ve el bot creado a pesar de su nombre y foto, se busca que funcione igualmente con lo descrito en el planteamiento del problema.

<a href="https://imgbb.com/"><img src="https://i.ibb.co/PGFxDLPd/image.png" alt="image" border="0"></a>



## Google Sheets

Para integrar el funcionamiento de los pedidos hay que ingresar a Google Sheets y crear un nuevo archivo que contenga toda la información requerida como el menú, pedidos o usuarios.

<a href="https://ibb.co/RTx5QsKS"><img src="https://i.ibb.co/1GDSRPNn/image.png" alt="image" border="0"></a>

Posteriormente debemos de vincular Google Sheets en el apartado de credenciales de n8n para que el sistema tenga acceso a la información como menús, pedidos o demás información que necesite.

Para que el sistema sea automatizado e intuitivo para el usuario será necesario usar nodos de agentes de IA, esto ya que al otorgar un buen prompt y acceso a los datos de Google Sheets, la IA tendrá un mejor dominio de lo que debe hacer. Para asignar estsos detalles a la IA es importante usar lo apartados de Chat Model, Memory y Tool.
El flujo se puede ver de esta manera:

<a href="https://ibb.co/fWkfpLL"><img src="https://i.ibb.co/Cybg7DD/image.png" alt="image" border="0"></a>

Podemos ver que se  inicia con un telegram trigger el cuál lleva el token del bot y además un Trigger On que captura el mensaje enviado, luego tenemos el nodo edit fields 1 que contiene el session_id y el chatInput.
<a href="https://ibb.co/xKwGqhwY"><img src="https://i.ibb.co/hxrHFsrc/image.png" alt="image" border="0"></a>

El siguiente nodo es el que permite darle vida al bot, es el agente de IA y como se mecionó anteriormente se conforma de un chat model, memory y tool.

<a href="https://ibb.co/Vfq8MLR"><img src="https://i.ibb.co/SF3pV6b/image.png" alt="image" border="0"></a>

La imagen anterior corresponde al interior del nodo, principalmente contiene el prompt principal que le indica a la IA que debe hacer y como se relacionan las herramientas que tiene a su disposición con la labor que tiene a cargo.

En el apartado Tool se pudo presenciar bastantes nodos que le permiten a la IA dirigirse a las diferentes hojas y así mismo poder interpretar los datos, aunque para ello hay que ajustarse manual o que la IA lo interprete dependiendo el caso.

Un ejemplo de como suelen lucir:
<a href="https://ibb.co/F4DwPRHt"><img src="https://i.ibb.co/whzWv5c1/image.png" alt="image" border="0"></a>

<a href="https://ibb.co/5W11xYLB"><img src="https://i.ibb.co/mCRRFv46/image.png" alt="image" border="0"></a>

Continuando, es importante recalcar que para un buen funcionamiento del sistema se debió crear un bot adicional de telegram el cuál se encargue de ser un asistente de la IA principal que ayude a gestionar los pedidos.

<a href="https://ibb.co/ymb2H3K8"><img src="https://i.ibb.co/CpTLRr4n/image.png" alt="image" border="0"></a>

Aquí podemos ver la descripción del bot asistente junto al prompt que le ayudará a identificar que hacer, además es importante verificar las credenciales para que no se confundan con las del bot principal, ya una vez con eso hecho, le damos también acceso a Google Sheets. Este bot tiene un node telegram el cuál le permite enviar mensajes como indicaciones.
Ya finalmente, al lado derecho de el nodo principal del agente de IA, se encuentra un nodo send a text message que es el que permitirá conversar.

Usamos el botón execute workflow y al hablar con el bot este nos deberá de contestar.



### Capturas de funcionamiento:

<a href="https://ibb.co/qYzKBJGj"><img src="https://i.ibb.co/GQXy7Hwp/image.png" alt="image" border="0"></a>

<a href="https://ibb.co/HDS30wsG"><img src="https://i.ibb.co/6RML29xD/image.png" alt="image" border="0"></a>

<a href="https://ibb.co/yngvh8Ts"><img src="https://i.ibb.co/Fb8ZsBv7/image.png" alt="image" border="0"></a>

### Enlace Google Sheets

https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing

### JSON Workflow

https://drive.google.com/file/d/1Ev00jChuhY03bR2eBPeX2HnuDMzXCFmu/view?usp=drive_link

### Agradecimientos:

Este proyecto no habría sido posible sin la ayuda de Santiago Villamizar, Paula Navarro y Juan Conde, sin la orientación, explicación y motivación de ellos este proyecto no podría haberse completado.

