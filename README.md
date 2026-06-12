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



## Update Examen ( Examen # 1 )

Esta actualización consistía en que el bot ahora tuviese un horario de atención de Lunes a Viernes de 8am a 5pm el cuál si no se cumplía debía indicarse indicarse con un mensaje que la cafetería se encontraba cerrada y que por lo tanto no se podía confirmar el pedido pero que el cliente aún pueda consultar el menú.

Para esta actualización no se modifico la estructura del flujo, para incluir este aspecto solo fue necesario ingresar al nodo de AI Agent y entrar al prompt que se encuentra en System Message, una vez dentro del prompt solo fue necesario agregar una nueva sección que le indicase al modelo de IA una nueva regla a cumplir especificando lo que el cliente podía y no podía hacer fuera del horario de atención y además  incluyendo el mensaje a enviar cuando el cliente desee confirmar el pedido y el horario de atención no se esté cumpliendo.



### Capturas:

En esta primera captura se puede presenciar la estructura del flujo, para esta actualización la estructura no presentó cambios ya que para la nueva restricción solo fue necesario modificar el nodo principal de AI Agent.

<a href="https://imgbb.com/"><img src="https://i.ibb.co/8DCVDqwy/image.png" alt="image" border="0"></a>

En esta otra captura podemos ver el prompt en el nodo de AI Agent con los cambios realizados

<a href="https://imgbb.com/"><img src="https://i.ibb.co/SDcLPfjW/image.png" alt="image" border="0"></a>

Aquí podemos ver una captura en la cuál se estaba probando el funcionamiento del bot de telegram para revisar que si cumpliese con un horario de prueba establecido.
<a href="https://imgbb.com/"><img src="https://i.ibb.co/GQbCHFMP/image.png" alt="image" border="0"></a>

Por último aquí se puede presenciar ya la última prueba realizada al bot.
<a href="https://ibb.co/SXywg6DP"><img src="https://i.ibb.co/Z6XzrfRV/image.png" alt="image" border="0"></a>



### Flujo actualizado:

De momento este es el flujo final que integra el nuevo funcionamiento:

{
  "nodes": [
    {
      "parameters": {
        "updates": [
          "message"
        ],
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegramTrigger",
      "typeVersion": 1.3,
      "position": [
        1680,
        3312
      ],
      "id": "9c2d2e99-f81b-485a-9959-4460909766f0",
      "name": "Telegram Trigger",
      "webhookId": "c256e954-c496-47e5-b5d5-b611a8b22d33",
      "credentials": {
        "telegramApi": {
          "id": "zwEOGsQZSwhh2aCr",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "options": {
          "systemMessage": "Eres un asistente virtual inteligente encargado de gestionar la experiencia del usuario, controlar sesiones, registrar pedidos, calcular totales, administrar puntos de fidelización, coordinar notificaciones operativas y controlar el acceso a funciones administrativas basándote en un sistema de base de datos en Google Sheets.\n\nTu comportamiento debe ser determinista, consistente y orientado a procesos. Debes seguir estrictamente las reglas definidas a continuación.\n\n========================================\nOBJETIVOS PRINCIPALES\n\nRegistrar usuarios en la hoja USUARIOS.\n\nGestionar pedidos en la hoja PEDIDOS.\n\nMantener el estado de la sesión en la hoja SESSIONS.\n\nConsultar información de productos mediante la hoja MENU.\n\nCalcular subtotales y totales de compras.\n\nCoordinar el envío de pedidos a producción.\n\nAdministrar puntos de fidelización en la columna puntos_lealtad.\n\nControlar accesos administrativos mediante la hoja ADMIN.\n\nGenerar reportes comerciales basados en la hoja VENTAS.\n\n========================================\nREGLAS ABSOLUTAS\n\nResponde siempre con UN SOLO mensaje por turno.\n\nNunca envíes dos respuestas para una misma interacción.\n\nNunca repitas párrafos.\n\nNunca dupliques información.\n\nNunca inventes datos.\n\nNunca inventes productos.\n\nNunca inventes precios.\n\nNunca inventes pedidos.\n\nNunca inventes usuarios.\n\nNunca inventes estados.\n\nNunca inventes permisos administrativos.\n\nNunca inventes reportes.\n\nNunca asumas que una operación fue exitosa sin verificarla.\n\nSiempre consulta las hojas de cálculo correspondientes antes de responder.\n\nMantén respuestas claras, ordenadas y concisas.\n\nResponde siempre en español.\n\nPrioriza precisión antes que velocidad.\n\n========================================\nCONTEXTO DINÁMICO (HOJA SESSIONS)\n\nPantalla actual:\n{{ $json.body.pantalla_actual ?? \"Inicio\" }}\n\nID de usuario (Telegram ID):\n{{ $json.body.telegram_id ?? \"No iniciada\" }}\n\nDatos de carrito temporal:\n{{ JSON.stringify($json.body.carrito_temporal ?? {}) }}\n\nÚltimo mensaje:\n{{ $json.body.ultimo_cambio }}\n\n========================================\nESTADOS OFICIALES DE SESIÓN (pantalla_actual)\n\nLa conversación únicamente puede encontrarse en uno de estos estados dentro de la hoja SESSIONS:\n\nBienvenida\n\nRegistro\n\nMenú\n\nSelección\n\nCarrito\n\nPago\n\nCompletado\n\nAdmin\n\nToda transición de estado debe registrarse actualizando la hoja SESSIONS. Nunca omitas la actualización de la columna pantalla_actual.\n\nSi el estado es desconocido, inicializa automáticamente en: Bienvenida\n\n========================================\nESTADO BIENVENIDA\n\nCondiciones:\n\nSesión nueva.\n\nNo existen datos en la hoja USUARIOS para el telegram_id actual.\n\nNo existe pantalla_actual registrada.\n\nEl usuario inicia la conversación.\n\nAcciones obligatorias:\n\nActualizar pantalla_actual a \"Bienvenida\" en la hoja SESSIONS.\n\nSolicitar nombre_completo.\n\nSolicitar direccion.\n\nInformar la posibilidad de editar datos.\n\nMensaje obligatorio:\n\n¡Hola! Bienvenido.\n\nPara comenzar necesito:\n\n• Nombre completo\n• Dirección\n\nSi cometes algún error puedes corregirlo utilizando:\n/editar nombre\n/editar direccion\n\nNo solicites menú ni pedidos antes de obtener y registrar estos datos en la hoja USUARIOS.\n\n========================================\nCOMANDO /menu\n\nActivadores:\n\n/menu\n\nver menú\n\nmostrar menú\n\nver productos\n\nmostrar productos\n\nqué venden\n\nqué tienen disponible\n\nAcciones:\n\nConsultar la hoja MENU (columnas: nombre, descripcion, precio, categoria, stock).\n\nActualizar pantalla_actual a \"Menú\" en la hoja SESSIONS.\n\nMostrar los productos disponibles que cuenten con stock mayor a 0.\n\nFormato obligatorio:\n\nNuestro menú incluye:\n\n• [nombre]: $[precio] ([descripcion])\n• [nombre]: $[precio] ([descripcion])\n\n¿Qué deseas ordenar?\n\nRecuerda que puedes modificar tus datos usando:\n/editar nombre\n/editar direccion\n\nPROHIBIDO:\n\nMostrar la columna id_producto.\n\nMostrar códigos internos.\n\nMostrar columnas técnicas.\n\nMostrar JSON.\n\nMostrar tablas markdown.\n\nUtilizar |.\n\nUtilizar ---.\n\n========================================\nCOMANDO /editar\n\nFormatos válidos:\n\n/editar nombre\n\n/editar direccion\n\nAcciones:\n\nDetectar campo solicitado (nombre_completo o direccion).\n\nSolicitar nuevo valor.\n\nGuardar y actualizar la fila correspondiente al telegram_id en la hoja USUARIOS.\n\nConfirmar actualización.\n\nEjemplo:\nUsuario: \"/editar direccion\"\nRespuesta: Por favor envíame la nueva dirección que deseas registrar.\n\n========================================\nCONSTRUCCIÓN DEL PEDIDO\n\nCuando el usuario indique productos:\n\nConsultar la hoja MENU.\n\nIdentificar productos por su nombre.\n\nIdentificar cantidades solicitadas.\n\nValidar que los productos existan y tengan stock suficiente.\n\nObtener el precio real asignado.\n\nCalcular subtotales y total general.\n\nFórmula:\nSubtotal = Cantidad × precio\nTotal = Suma de subtotales\n\nSi algún producto no existe o no tiene stock:\nInformar el error de inmediato. Nunca inventar productos similares.\n\n========================================\nRESUMEN DEL PEDIDO\n\nFormato obligatorio:\n\nResumen del pedido:\n\n([cantidad] [nombre], [cantidad] [nombre])\n\nTotal: $[total_pago]\n\n¿Deseas confirmar el pedido?\n\nRecuerda que puedes corregir tus datos usando:\n/editar nombre\n/editar direccion\n\n========================================\nCONFIRMACIONES VÁLIDAS\n\nConsidera como afirmación:\n\nsi, sí, ok, okay, correcto, confirmar, confirmo, confirmar pedido, está bien, esta bien, perfecto, dale, de acuerdo.\n\nNo exijas coincidencia exacta.\n\n========================================\nESTADO PAGO\n\nUna vez confirmado el pedido por el usuario:\n\nActualizar pantalla_actual a \"Pago\" en la hoja SESSIONS.\n\nSolicitar método de pago para registrar en la columna estado o gestionar internamente.\n\nOpciones válidas:\n\nNequi\n\nTarjeta\n\nEfectivo\n\nSi el usuario indica otro método, responder:\nActualmente solo aceptamos:\n• Nequi\n• Tarjeta\n• Efectivo\n\nPor favor selecciona una de estas opciones. No continúes hasta obtener un método válido.\n\n========================================\nREGISTRO DEL PEDIDO (HOJA PEDIDOS)\n\nSolo ejecutar cuando el pedido esté confirmado y el método de pago sea válido.\n\nDestino: Hoja PEDIDOS\nColumnas a rellenar de forma consecutiva o mediante un nuevo id_pedido:\n\nid_pedido: [Autogenerar ID o correlativo numérico]\n\nid_usuario: [Insertar el telegram_id correspondiente]\n\ndetalles_pedido: [Insertar detalle formateado]\n\ntotal_pago: [Suma total calculada]\n\nestado: [Asignar estado inicial, ej: \"Pendiente\" o \"En preparación\"]\n\nfecha: [Fecha actual en formato DD/MM/AAAA]\n\nhora: [Hora actual en formato HH:MM:SS]\n\nFormato obligatorio estricto para la columna detalles_pedido:\n\nUn producto: (2 Cafe)\n\nVarios productos: (2 Cafe, 1 Empanada)\n\nMantener exactamente: Paréntesis (), comas de separación y la cantidad numérica antes del nombre del producto.\n\n========================================\nVERIFICACIÓN DEL PEDIDO\n\nDespués de realizar la inserción en la hoja PEDIDOS, realizar una lectura de control para verificar que el registro con el id_pedido o id_usuario quedó almacenado correctamente. Si no existe confirmación en la lectura, no continuar.\n\n========================================\nNOTIFICACIÓN A PRODUCCIÓN\n\nSolo ejecutar cuando el pedido esté confirmado, registrado en la hoja PEDIDOS, verificado y con un método de pago válido. Ejecutar la alerta interna pertinente hacia la cocina. Nunca ejecutar antes.\n\n========================================\nCAMBIO A COMPLETADO\n\nTras el registro, la verificación y la notificación exitosa, actualizar la columna pantalla_actual a \"Completado\" en la hoja SESSIONS.\n\n========================================\nMENSAJE FINAL AL CLIENTE\n\nFormato recomendado:\n\n✅ Pedido registrado correctamente.\n\nMétodo de pago: [Método seleccionado]\nNúmero de pedido: #[id_pedido]\n\nTu pedido ya fue enviado a preparación. ¡Gracias por tu compra!\n\n========================================\nCONSULTA DE ESTADO DE PEDIDOS\n\nActivadores:\n\nestado de mi pedido, dónde va mi pedido, consultar pedido, cómo va mi pedido.\n\nAcciones:\n\nBuscar en la hoja PEDIDOS utilizando el id_usuario (que mapea con el telegram_id del cliente).\n\nRecuperar los datos de las columnas id_pedido y estado.\n\nResponder únicamente con la información obtenida.\n\nEjemplo:\nEstado del pedido:\nPedido #[id_pedido]\nEstado: [estado]\n\nNunca inventes o supongas estados que no figuren en la celda correspondiente.\n\n========================================\nPROGRAMA DE FIDELIZACIÓN\n\nRegla: Si total_pago > 50000\nAcción: Sumar/Otorgar 10 puntos en la columna puntos_lealtad de la hoja USUARIOS para la fila correspondiente al usuario actual.\n\nSolo ejecutar cuando el estado pase a \"Completado\". Nunca antes.\n\n========================================\nMÓDULO ADMINISTRATIVO (HOJAS ADMIN Y VENTAS)\n\nEl acceso a este módulo es exclusivo para aquellos registros presentes en la hoja ADMIN que cuenten con las credenciales pertinentes. Los clientes normales nunca pueden acceder.\n\n========================================\nACTIVADORES DE ADMINISTRACIÓN\n\nadmin, administrador, panel admin, reporte ventas, ventas, dashboard, estadísticas, informe comercial.\n\n========================================\nAUTENTICACIÓN ADMINISTRATIVA\n\nSolicitar obligatoriamente los datos de acceso para contrastar con la hoja ADMIN:\n\ntelegram_id (o id_venta si se usa como identificador correlativo, validar correspondencia)\n\nid_admin\n\nclave\n\nNo mostrar pantallas ni reportes administrativos previos a la validación exitosa de la columna clave e id_admin.\n\n========================================\nACCESO DENEGADO\n\nSi los datos provistos no coinciden con ninguna fila de la hoja ADMIN, o las claves introducidas son erróneas:\n\nRespuesta:\nAcceso denegado.\nLos reportes administrativos están disponibles únicamente para administradores autorizados.\n\n========================================\nACCESO AUTORIZADO\n\nSi la comprobación con la hoja ADMIN es correcta:\n\nActualizar pantalla_actual a \"Admin\" en la hoja SESSIONS.\n\nLeer los datos históricos de la hoja VENTAS (columnas: id_venta, fecha_hora, producto, total, precio_unitario).\n\nResponder:\nHas ingresado al panel administrativo.\nPuedes salir usando: /exit\n\n========================================\nCOMANDO /exit\n\nSolo disponible cuando pantalla_actual = \"Admin\" en la hoja SESSIONS.\n\nAcciones:\n\nLimpiar o cerrar credenciales temporales de administración.\n\nActualizar la columna pantalla_actual a \"Bienvenida\" (o \"Inicio\") en la hoja SESSIONS.\n\nConfirmar salida.\n\nRespuesta:\nHas salido correctamente del modo administrador. Ahora puedes utilizar el sistema como cliente.\n\n========================================\nFORMATO DE REPORTES (PROVENIENTE DE LA HOJA VENTAS)\n\nCada registro extraído de la hoja VENTAS debe listarse con el siguiente formato:\n\n• ID venta: [id_venta]\n• Fecha y hora: [fecha_hora]\n• Producto: [producto]\n• Precio unitario: $[precio_unitario] COP\n• Total línea: $[total] COP\n\n========================================\nANÁLISIS COMERCIAL\n\nTras desplegar el listado de la hoja VENTAS, procesar matemáticamente las columnas fecha_hora y total para concluir:\n\nMes con mayores ventas.\n\nMes con menores ventas.\n\nDía de la semana con mayores ventas.\n\nDía de la semana con menores ventas.\n\nComportamiento o tendencia semanal.\n\nTotal acumulado (sumatoria completa de la columna total).\n\nFormato:\n\nResumen analítico:\n\n• Mes con más ventas: [Mes]\n• Mes con menos ventas: [Mes]\n• Día más fuerte: [Día]\n• Día más débil: [Día]\n• Tendencia semanal: [Análisis breve]\n\nTotal acumulado:\n$[Total Calculado] COP\n\n========================================\nREGLAS DE SEGURIDAD\n\nPrioridad máxima:\n\nSeguridad administrativa (Verificación estricta en hoja ADMIN).\n\nIntegridad de pedidos e inventarios (Validación contra hojas MENU y PEDIDOS).\n\nPersistencia de sesión (Actualizaciones en hoja SESSIONS).\n\nValidación de pago.\n\nRegistro de pedido.\n\nNotificación a cocina.\n\nFidelización (puntos_lealtad).\n\nExperiencia del usuario.\n\nSi dos reglas entran en conflicto, aplicar siempre la de mayor prioridad.\nSi una hoja de cálculo no responde o los datos están vacíos, no inventar resultados ni asumir éxitos; informar el problema de inmediato al usuario o solicitar los datos faltantes para reintentar la operación.\n\nIMPORTANTE\n\n* Es importante que te fijes en la fecha y hora que se encuentra el cliente ordenando, el horario de atención es de Lunes a Viernes de 8 am a 5 pm. Si el cliente desea ordenar fuera del horario de atención se le debe enviar un mensaje como \"🌙 Cafetería Cerrada. Nuestro horario es de Lunes a Viernes, 8am a 5pm. ¡Te esperamos pronto!\".\nSi la conversación está fuera del horario de atención, aún puede consultar el menú pero no puede confirmar el pedido de ninguna manera hasta que se cumpla con el horario de atención establecido."
        }
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3.1,
      "position": [
        2080,
        3312
      ],
      "id": "8e42e49b-0291-4714-bc8d-59b6b03eb642",
      "name": "AI Agent",
      "alwaysOutputData": false
    },
    {
      "parameters": {
        "model": "openai/gpt-oss-safeguard-20b",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGroq",
      "typeVersion": 1,
      "position": [
        1936,
        3520
      ],
      "id": "1957ee47-8529-47df-b0f1-9a9a1cfea253",
      "name": "Groq Chat Model",
      "credentials": {
        "groqApi": {
          "id": "0O1IvEqCPIke30Yb",
          "name": "Groq account"
        }
      }
    },
    {
      "parameters": {
        "sessionIdType": "customKey",
        "sessionKey": "={{ $json.sesion_id }}"
      },
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "typeVersion": 1.4,
      "position": [
        2112,
        3520
      ],
      "id": "0106d3ca-22e1-42c7-86c3-ca3819bd1db3",
      "name": "Simple Memory"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "cff44fb1-4304-4d2b-ab8b-0e57334cabed",
              "name": "sesion_id",
              "value": "={{ $json.message.chat.id }}",
              "type": "number"
            },
            {
              "id": "b2e8a3d9-4718-4735-afd5-bfbf8f5090cc",
              "name": "chatInput",
              "value": "={{ $json.message.text }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        1888,
        3312
      ],
      "id": "8fb357d0-b979-453b-b99a-c3c8b953b0ab",
      "name": "Edit Fields1",
      "alwaysOutputData": false
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "mode": "url",
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "__regex": "https:\\/\\/(?:drive|docs)\\.google\\.com(?:\\/.*|)\\/d\\/([0-9a-zA-Z\\-_]+)(?:\\/.*|)"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "MENU",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=0"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2400,
        3472
      ],
      "id": "f9521c4c-5c32-4791-aa7a-c870ca9cf01d",
      "name": "menu",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 584785584,
          "mode": "list",
          "cachedResultName": "PEDIDOS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=584785584"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2400,
        3632
      ],
      "id": "2926088e-368f-40bc-a1c5-95c85f64d649",
      "name": "pedidos",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 2048157396,
          "mode": "list",
          "cachedResultName": "USUARIOS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=2048157396"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2400,
        3792
      ],
      "id": "2d9e7f2b-c931-49d7-aafc-1592bd5048cf",
      "name": "usuarios",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1748126731,
          "mode": "list",
          "cachedResultName": "SESSIONS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=1748126731"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2400,
        3952
      ],
      "id": "408c2789-b438-4f06-b5ce-5dcb2f091559",
      "name": "sessions",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "toolDescription": "Agente inteligente encargado de gestionar pedidos del restaurante a través de Telegram. Consulta el menú disponible, interpreta solicitudes de los clientes, calcula cantidades y totales, solicita confirmación del pedido, registra la información en Google Sheets y genera respuestas organizadas con tiempo estimado de preparación y datos del cliente.",
        "text": "Eres Chef, el asistente virtual de un restaurante que atiende pedidos mediante Telegram.\n\nTu objetivo principal es ayudar al cliente a realizar pedidos, consultar el menú y registrar ventas de forma organizada.\n\nREGLAS OBLIGATORIAS:\n\n1. Siempre consulta la herramienta MENU para obtener los productos disponibles, precios y stock actual.\n\n2. Cuando un usuario solicite productos, identifica claramente:\n\n* Nombre del producto.\n* Cantidad solicitada.\n* Precio unitario.\n* Subtotal por producto.\n* Total estimado del pedido.\n\n3. Antes de registrar un pedido debes mostrar un resumen atractivo y ordenado del pedido al cliente utilizando texto corrido y emojis. No uses tablas ni líneas divisorias.\n\n4. El resumen debe incluir obligatoriamente:\n   🍽️ Resumen del pedido\n   🆔 ID del cliente\n   📦 Cantidad total de productos solicitados\n   🛒 Detalle de los productos y cantidades\n   💰 Total estimado a pagar\n   ⏱️ Tiempo estimado de preparación y entrega\n   ✅ Instrucción para confirmar el pedido\n\n5. El tiempo estimado debe calcularse de forma aproximada:\n\n* 1 a 2 productos: 15 a 20 minutos.\n* 3 a 5 productos: 20 a 30 minutos.\n* Más de 5 productos: 30 a 45 minutos.\n\n6. NO registres pedidos en la hoja PEDIDOS mientras el cliente no haya confirmado explícitamente.\n\n7. Considera como confirmación válida palabras o expresiones como:\n\n* confirmar\n* confirmar pedido\n* confirmado\n* sí\n* si\n* ok\n* okay\n* listo\n* está bien\n* esta bien\n* perfecto\n* correcto\n* de acuerdo\n* dale\n* hágale\n* hagale\n* acepto\n\n8. Cuando detectes una confirmación válida:\n\n* Guarda el pedido utilizando la herramienta Info_pedidos.\n* Registra el ID del cliente.\n* Guarda el detalle completo del pedido.\n* Guarda el total.\n* Guarda el estado como \"Confirmado\".\n* Genera un ID de pedido único.\n\n9. Después de registrar correctamente el pedido debes enviar exactamente un mensaje con este formato:\n\n🎉 ¡Pedido confirmado!\n\n🆔 Cliente: {id_cliente}\n\n📋 Pedido:\n{detalle_pedido}\n\n📦 Cantidad total de productos: {cantidad_total}\n\n💰 Total a pagar: ${total}\n\n⏱️ Tiempo estimado: {tiempo_estimado}\n\n🍳 Tu pedido ya fue enviado a cocina y comenzará a prepararse pronto.\n\n🙏 Gracias por comprar con Chef.\n\n10. Si el cliente modifica algún producto antes de confirmar, actualiza el resumen y vuelve a solicitar confirmación.\n\n11. Si falta información necesaria para completar el pedido, solicita únicamente los datos faltantes.\n\n12. Mantén respuestas amigables, breves y organizadas. Utiliza emojis para mejorar la presentación, pero siempre en formato de texto corrido. Nunca uses tablas, líneas separadoras ni bloques de texto complejos.\n\n13. El ID del cliente corresponde al telegram_id recibido en la conversación y debe aparecer tanto en el resumen previo como en la confirmación final.\n\n14. Nunca inventes productos, precios o stock. Siempre consulta la herramienta MENU antes de responder.\n",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.agentTool",
      "typeVersion": 3,
      "position": [
        2144,
        2896
      ],
      "id": "a001813d-2423-4051-a267-980c8ac87c95",
      "name": "AI Agent Tool"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 584785584,
          "mode": "list",
          "cachedResultName": "PEDIDOS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=584785584"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "id_pedido": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('id_pedido', ``, 'string') }}",
            "id_usuario": "={{ $json.sesion_id }}",
            "fecha": "={   \"currentDate\": \"{{$now.toFormat('yyyy-MM-dd')}}\" }",
            "hora": "={   \"currentTime\": \"{{$now.toFormat('HH:mm:ss')}}\" }",
            "detalles_pedido": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('detalles_pedido', ``, 'string') }}",
            "total_pago": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('total_pago', ``, 'string') }}",
            "estado": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('estado', ``, 'string') }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "id_pedido",
              "displayName": "id_pedido",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "id_usuario",
              "displayName": "id_usuario",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "detalles_pedido",
              "displayName": "detalles_pedido",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "total_pago",
              "displayName": "total_pago",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "estado",
              "displayName": "estado",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "fecha",
              "displayName": "fecha",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "hora",
              "displayName": "hora",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2528,
        3632
      ],
      "id": "943635fa-bf8a-4539-96dd-9f5000902c32",
      "name": "Info_pedidos",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 2048157396,
          "mode": "list",
          "cachedResultName": "USUARIOS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=2048157396"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "telegram_id": "={{ $json.sesion_id }}",
            "nombre_completo": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('nombre_completo', ``, 'string') }}",
            "direccion": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('direccion', ``, 'string') }}",
            "puntos_lealtad": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('puntos_lealtad', ``, 'string') }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "telegram_id",
              "displayName": "telegram_id",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "nombre_completo",
              "displayName": "nombre_completo",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "direccion",
              "displayName": "direccion",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "puntos_lealtad",
              "displayName": "puntos_lealtad",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2528,
        3792
      ],
      "id": "97dd48cc-49a4-4848-9b84-ee894664e8d8",
      "name": "info_usuarios",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1748126731,
          "mode": "list",
          "cachedResultName": "SESSIONS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=1748126731"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "telegram_id": "={{ $json.sesion_id }}",
            "ultimo_cambio": "={{ $now }}",
            "carrito_temporal": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('carrito_temporal', ``, 'string') }}",
            "pantalla_actual": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('pantalla_actual', ``, 'string') }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "telegram_id",
              "displayName": "telegram_id",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "pantalla_actual",
              "displayName": "pantalla_actual",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "carrito_temporal",
              "displayName": "carrito_temporal",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "ultimo_cambio",
              "displayName": "ultimo_cambio",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2528,
        3952
      ],
      "id": "2fa8549b-597e-4121-b948-b0e80c507cec",
      "name": "info_sessions",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1763416387,
          "mode": "list",
          "cachedResultName": "VENTAS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=1763416387"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2400,
        4112
      ],
      "id": "2685c783-0969-4529-bf23-6de2fcbf12d1",
      "name": "registro_ventas",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1763416387,
          "mode": "list",
          "cachedResultName": "VENTAS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=1763416387"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "id_venta": "={{ $('Telegram Trigger').item.json.message.from.id }}",
            "fecha_hora": "={   \"fecha_hora\": \"{{ $now }}\" }",
            "producto": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('producto', ``, 'string') }}",
            "total": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('total', ``, 'string') }}",
            "precio_unidad": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('precio_unidad', ``, 'string') }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "id_venta",
              "displayName": "id_venta",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "fecha_hora",
              "displayName": "fecha_hora",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "producto",
              "displayName": "producto",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "total",
              "displayName": "total",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "precio_unidad",
              "displayName": "precio_unidad",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2528,
        4112
      ],
      "id": "05cb1f82-bfd4-4495-b68f-d352a1a16932",
      "name": "info_ventas",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 250682117,
          "mode": "list",
          "cachedResultName": "ADMIN",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=250682117"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2400,
        4272
      ],
      "id": "2fd8637f-0519-4f74-a6b6-04b416c58990",
      "name": "admin",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 250682117,
          "mode": "list",
          "cachedResultName": "ADMIN",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=250682117"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "id_venta": "={{ $fromAI('id_venta', ``, 'string') }}",
            "telegram_id": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('telegram_id', ``, 'string') }}",
            "id_admin": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('id_admin', ``, 'string') }}",
            "clave": "={{ $fromAI('clave', ``, 'string') }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "id_venta",
              "displayName": "id_venta",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "telegram_id",
              "displayName": "telegram_id",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "id_admin",
              "displayName": "id_admin",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "clave",
              "displayName": "clave",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2528,
        4272
      ],
      "id": "0626092a-da6d-4a71-8dac-1566338b9526",
      "name": "info_admin",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit?usp=sharing",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1748126731,
          "mode": "list",
          "cachedResultName": "SESSIONS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1FL3YTuzirMgNoxGMbAk8No6jPpXQdcWiYU1W7B9I9uU/edit#gid=1748126731"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "telegram_id": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
            "pantalla_actual": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('pantalla_actual', ``, 'string') }}",
            "carrito_temporal": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('carrito_temporal', ``, 'string') }}",
            "ultimo_cambio": "={{ $now }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "telegram_id",
              "displayName": "telegram_id",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "pantalla_actual",
              "displayName": "pantalla_actual",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "carrito_temporal",
              "displayName": "carrito_temporal",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "ultimo_cambio",
              "displayName": "ultimo_cambio",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        2512,
        3152
      ],
      "id": "c8bc3dd9-f7e0-4d0f-a682-45792cc6e9eb",
      "name": "cocina",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "rU9VVjCePlVdAtnh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "model": "openai/gpt-oss-20b",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGroq",
      "typeVersion": 1,
      "position": [
        2144,
        3152
      ],
      "id": "14efd1d9-31f2-4897-b023-106c73f19d80",
      "name": "Groq Chat Model1",
      "credentials": {
        "groqApi": {
          "id": "jIBtZdI4wiUMCYOJ",
          "name": "chef"
        }
      }
    },
    {
      "parameters": {
        "sessionIdType": "customKey",
        "sessionKey": "={{ $json.sesion_id }}"
      },
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "typeVersion": 1.4,
      "position": [
        2384,
        3152
      ],
      "id": "56f62798-4819-43d1-b551-4e282f1f47e1",
      "name": "Simple Memory1"
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{  $json.output }}",
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        2608,
        3312
      ],
      "id": "5ebf829b-3231-4982-82a0-9d8d32fad6fd",
      "name": "Enviar mensaje bot principal",
      "webhookId": "1cd2fc64-5f26-4cb3-805f-722fb4a0abf6",
      "credentials": {
        "telegramApi": {
          "id": "zwEOGsQZSwhh2aCr",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "=8954234067",
        "text": "🚨 NUEVO PEDIDO PARA COCINA 🚨  👨‍🍳 Chef, se ha recibido una nueva orden: • Cliente ID: #{{ $json[\"Telegram_id\"] }} • Estado/Pantalla: {{ $json[\"Pantalla_actual\"] }}  📝 DETALLES DEL PEDIDO: {{ $json.detalles_pedido }}  ⏰ Hora de Registro: {{ $json[\"Ultimo_cambio\"] }} 👉 Por favor, confirmar inicio de preparación.",
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegramTool",
      "typeVersion": 1.2,
      "position": [
        2656,
        3152
      ],
      "id": "6dc9d0bd-09f2-4084-8efd-64e58d4d6dce",
      "name": "Enviar mensaje bot cocina",
      "webhookId": "d62a2e28-0aef-4bab-9a8c-03cf35d6171f",
      "credentials": {
        "telegramApi": {
          "id": "8oRD6dwvZ3hYSTZQ",
          "name": "Chef"
        }
      }
    }
  ],
  "connections": {
    "Telegram Trigger": {
      "main": [
        [
          {
            "node": "Edit Fields1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent": {
      "main": [
        [
          {
            "node": "Enviar mensaje bot principal",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Groq Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Simple Memory": {
      "ai_memory": [
        [
          {
            "node": "AI Agent",
            "type": "ai_memory",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields1": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "menu": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "pedidos": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "usuarios": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "sessions": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent Tool": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "Info_pedidos": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "info_usuarios": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "info_sessions": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "registro_ventas": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "info_ventas": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "admin": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "info_admin": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "cocina": {
      "ai_tool": [
        [
          {
            "node": "AI Agent Tool",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "Groq Chat Model1": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent Tool",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Simple Memory1": {
      "ai_memory": [
        [
          {
            "node": "AI Agent Tool",
            "type": "ai_memory",
            "index": 0
          }
        ]
      ]
    },
    "Enviar mensaje bot cocina": {
      "ai_tool": [
        [
          {
            "node": "AI Agent Tool",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    }
  },
  "pinData": {},
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "f6790a092357f74670966714b351d3bb92064ee20f5066d27d5833e27060523c"
  }
}
