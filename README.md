# Pruebas de Consumo de Web API en Diferentes Plataformas (.NET)
##  Resumen de Pruebas

| Prueba | Plataforma | Tecnología | Descripción |
|--------|------------|------------|-------------|
| Nº1 | Web API | ASP.NET Core - Swagger | Endpoint que saluda por nombre |
| Nº2 | Escritorio | Windows Forms | App que pide nombre y consume API |
| Nº3 | Web App | ASP.NET MVC | App web que llama al API por POST/GET |
| Nº4 | Worker Service | .NET Core + MailKit | Servicio que envía saludos por correo cada minuto |
| Nº5 | Móvil | .NET MAUI | App Android que muestra el saludo |

### IDE: Visual Studio 2022 
### TECNOLOGIAS:
- **ASP.NET Core Web API (.NET 8,9)**
- **MAUI (.NET 8, emulador Android)**
- **Windows Forms (.NET 8)**
- **ASP.NET Core MVC (para la app web)**
- **Worker Service (Ejeución de tareas)**
- **MailKit (envío de correos)**
- **Swagger (documentación y pruebas de API)**


# PRUENA Nº1 - Web API : recibe el nombre y le devuelve un saludo con ese nombre.
Creamos un proyecto utilizando: `ASP.NET Core Web Api`, para implementar un controller de nombre `SaludoController.cs` que contenga el siguiente código:

- **Método GET:** Porque solo queremos recuperar la informacion que reciba, solo de lectura sin ninguna modificación
![controller_api_saludo](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/Controller_ApiSaludo.png)

Ejecutamos con Protocolo `HTTP` para no tener problemas con el certificado mas adelante y podemos ver el `EndPoint` de nuestra API, con una facil documentacion de `Swagger` que nos facilita las pruebas con APIs
![ejecucion_pruebas_api](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/swagger_api.png)

>![+] URL
>
>http://localhost:5054/saludo/{nombre} - Esta URL usaremos para seguir consumiendo nuestra API en las demas *Pruebas de Concepto*

Si en la URL, colocamos lo que nos indica el `endpoint`, devuelve el saludo:
![saludo_endpoint](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/saludo_endpoint.png)

## CONSIDERACIONES PARA LAS SIGUIENTES PoC:
- **La API tiene que estar en ejecución para que podamos consumirla**

# PRUEBA Nº2 - Aplicacion de escritorio: Pide su nombre y lo saluda con su nombre. Consumiendo el API
Para esta siguiente prueba crearemos un proyecto utilizando `Aplicación de Windows Forms (.NET)`, y construiremos un formulario con el `cuadro de herramientas` que nos proporciona el IDE, agregamos:

- **1 Label:** Texto: Ingresa tu nombre:
- **1 TextBox:** Nombre: txtNombre
- **1 Button:** Texto: Saludar | Nombre: btnSaludar
- **1 Label: Nombre:** lblResultado (para mostrar el saludo), texto en Blanco

![forms_api](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/forms_api.png)

Entonces para que pueda funcionar el que actua es el boton `SALUDAR`, por lo que le agregaremos código para que resuelva:
Podemos observar que utiliza declaraciones como:

- **HttpClient:** Enviar peticiones GET hacia el API
- **Validacion de Ingreso Nombre**
- **Llamada a la API**: Construye la URL con el nombre ingresado configurado en el Forms

![code_button](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/Code_button.png)

Ejecutamos e Ingresamos nuestro nombre y podemos ver que consume la API de manera correcta:
![ejecucion_forms](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/ejecucion_forms.png)


# PRUEBA Nº3 - Web App: la misma funcionalidad, consumiendo el API

Para esta `PoC`, crearemos un proyecto utilizando `ASP.NET Core MVC Web App`, para editar el codigo de `HomeController.cs` el cual contiene la lógica para poder consumir nuestra API
USAMOS `POST` para guardar el dato en controlador y el controlador sigue el flujo haciendo un GET hacia la API, recibe el mensaje y lo muestra
![home_controller_code](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/home_controller_cs.png)

Sigue la misma lógica que los PoC anteriores, porque se trata de consumir una misma API con el mismo EndPoint

Ahora Vamos a editar el `index.cshtml`, para poder personalizar nuestra interfaz con un ingreso de nombre y que se Aproveche del `endpoint` y pueda consumir la API gracias al `homeController.cs`
Utilizamos `Bootstrap` para crear un diseño responsive si es necesario
Uso de `ViewBag` para pasar datos del `Controller` hacia las `Views`
![index_cshtml](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/index_cshtml.png)

Compilamos
![compila_api](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/compila_api.png)
Podemos ver que si realiza en consumo de la API correctamente gracias a una Linea en agregada en el controlador para ver si hace la depuracion correcta.


# PRUEBA Nº4 - Background service tambien llamados worker services o windows services: envia un correo saludando (consumiendo el API) cada 1 minuto. 
Para esta `PoC` crearemos un proyecto utilizando `Worker Service (.NET Core)` eh instalaremos un paquete llamado `MailKit` para trabajar con protocolos de `Correo Electrónico(SMTP,POP3,IMAP)`
```ruby
dotnet add package MailKit
```
Con ese comando instalaremos MailKit 

Ahora editamos código en `worker.cs`
![worker_cs](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/worker_cs.png)

Para generar una contraseña de `autenticacion de aplicaciones` se ingresa a la siguiente url `https://myaccount.google.com/apppasswords`
![contraseña_aplicaciones](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/contrase%C3%B1as_aplicaciones.png)

Ingresan el nombre, puede ser cualquiera y les generará una contraseña y esa copian en el código

Ejecutamos:
![saludo_enviado](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/saludo_enviado.png)
![verificacion_saludo](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/verificacion_saludo.png)

Podemos ver que si se esta ejecutando de manera correcta y enviando los saludos automaticos Cada minuto

![cada_min](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/cada_min.png)

# PRUEBA Nº5 - Aplicación movil: igual, pide su nombre y le saluda con su nombre (consumiendo el API)

Para esta `PoC` crearemos un proyecto utilizando `.NET MAUI App (Multi-platform App UI)` y editaremos algunos archivos para permitir trajar correctamente:
`MainPage.xaml.cs`
![MainPage.xaml.cs](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/MainPage_xaml_cs.png)

y `MainPage.xaml`
![mainpage_xaml](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/mainpage_xaml.png)

Son códigos basados en la misma logica que los anteriores, un archivo importante para no tener errores es `Android_Manifest.xml`

![android_manifest_xml](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/Android_Manifest.png)

Una vez configurado todo `Ejecutamos`

Primero probamos en la web para ver si se esta comunicando de manera correcta con la API

![ejecucion_web](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/Ejecucion_Web.png)

Como no tenemos problemas, se esta aplicando el consumo correcto en `web`
Ahora ejecutamos en La Aplicación de XAML DE MAUI

![ejecucion_app](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/ejecucion_android.png)

Podemos ver que si esta consumiento de manera correcta en la aplicacion de MAUI, se estan tramitando las peticiones HTTP correctamente hacia la API

![prueba2_app](https://github.com/Harley25-sys/PoC-APIS/blob/main/images/ejecucion_android2.png)

# GRACIAS




