# DAP-BPMN Viewer

DAP-BPMN Viewer es un visualizador web para diagramas de Business Process Model and Notation (BPMN), diseñado para el proyecto DAP. Permite navegar, visualizar y analizar diagramas de procesos de negocio directamente en el navegador.

La aplicación funciona completamente en el cliente, por lo que no requiere un backend para visualizar los diagramas. Puede ejecutarse como un sitio web estático de forma independiente o integrarse en otra aplicación web mediante un `<iframe>`.

## Características

* Visualización interactiva de diagramas BPMN.
* Navegación entre diferentes diagramas de proceso.
* Búsqueda de elementos específicos del proceso.
* Ejecución 100% en el cliente, sin backend para la visualización.

## Stack Tecnológico

* **HTML5**
* **CSS3**, con Foundation como base de estilos.
* **JavaScript**, con jQuery, Backbone.js y KineticJS.

## Cómo Empezar

DAP-BPMN Viewer es una aplicación web estática y no requiere un proceso de compilación. Para ejecutarla localmente, clona el repositorio y sirve sus archivos mediante un servidor HTTP:

1. **Clona el repositorio:**

    ```bash
    git clone https://github.com/guanes/dap-bpmn.git
    cd dap-bpmn
    ```

2. **Inicia un servidor web local.** Por ejemplo, usando el servidor incorporado de Python 3:

    ```bash
    python3 -m http.server
    ```

3. **Abre la aplicación** en tu navegador visitando [http://localhost:8000](http://localhost:8000).

## Despliegue

El visor está desplegado en [https://guanes.github.io/dap-bpmn/](https://guanes.github.io/dap-bpmn/).

Además de utilizarse de forma independiente, el visor puede integrarse en cualquier aplicación web. La siguiente sección describe ese escenario.

## Integración en Otra Aplicación

El visor de procesos es una aplicación web autocontenida. La forma más sencilla de integrarlo en otro desarrollo es mediante el uso de un `<iframe>`.

Este enfoque aísla completamente el visor, evitando cualquier conflicto de estilos (CSS) o de librerías (JavaScript) con la aplicación principal.

## Pasos de Integración

Siga estos pasos para embeber el visor en su aplicación.

### 1. Desplegar los archivos del visor

Copie la totalidad de este proyecto (las carpetas `files`, `libs` y los archivos `index.html`, `key.json.js`, etc.) en un directorio accesible a través de un servidor web.

**Ejemplo de estructura de archivos en el servidor:**

```
/su-servidor-web/
└── /visor-procesos/
    ├── index.html
    ├── key.json.js
    ├── README.md
    ├── files/
    └── libs/
```

En este ejemplo, el visor será accesible en la URL: `http://<su-dominio>/visor-procesos/`

### 2. Identificar el ID del Diagrama

Para poder mostrar un diagrama específico, necesita su identificador único. Este ID se encuentra en el archivo `libs/js/json/configuration.json.js`.

Abra el archivo y busque en el objeto `Bizagi.AppModel`. La propiedad `pages` contiene un arreglo de todos los diagramas disponibles.

**Ejemplo del contenido de `configuration.json.js`:**

```javascript
Bizagi.AppModel = {
    // ... otras propiedades
    "pages": [
        {
            "id": "689b6522-cb38-4252-b9f4-51eed4c36741",
            "name": "PROCESOS FLUJO DE INFORMACIÓN",
            // ...
        },
        {
            "id": "f2c1b7a2-1234-5678-9abc-deff2d3f4b5c",
            "name": "Otro Proceso",
            // ...
        }
    ]
};
```

Copie el valor de la propiedad `id` del diagrama que desea mostrar.

### 3. Construir la URL y Embeber en un `<iframe>`

La URL para mostrar un diagrama específico sigue este formato:

`[URL_BASE_VISOR]#diagram/[ID_DEL_DIAGRAMA]`

Usando el ejemplo anterior, para mostrar el diagrama "PROCESOS FLUJO DE INFORMACIÓN", la URL sería:

`http://<su-dominio>/visor-procesos/index.html#diagram/689b6522-cb38-4252-b9f4-51eed4c36741`

Ahora, agregue un `<iframe>` en el código HTML de su aplicación apuntando a esa URL.

## Ejemplo de Código Completo

Aquí tiene un ejemplo de una página HTML que embebe el visor de procesos:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplicación con Visor de Procesos Integrado</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            overflow: hidden; /* Para evitar barras de scroll duplicadas */
        }
        .visor-container {
            width: 100%;
            height: 100vh; /* Ocupa toda la altura de la pantalla */
            border: none;
        }
    </style>
</head>
<body>

    <h1>Mi Aplicación Corporativa</h1>
    <p>A continuación se muestra el diagrama de flujo del proceso:</p>

    <iframe 
        id="visor-bpmn"
        class="visor-container"
        src="http://localhost/visor-procesos/index.html#diagram/689b6522-cb38-4252-b9f4-51eed4c36741"
        title="Visor de Procesos BPMN">
    </iframe>

    <script>
        // Opcional: puede cambiar dinámicamente el diagrama mostrado
        // cambiando el atributo 'src' del iframe.
        
        function cargarDiagrama(diagramId) {
            const iframe = document.getElementById('visor-bpmn');
            const urlBase = 'http://localhost/visor-procesos/index.html#diagram/';
            if (iframe) {
                iframe.src = urlBase + diagramId;
            }
        }

        // Ejemplo de uso:
        // Cargar otro diagrama después de 5 segundos
        /*
        setTimeout(() => {
            console.log("Cambiando al diagrama 'Otro Proceso'...");
            cargarDiagrama('f2c1b7a2-1234-5678-9abc-deff2d3f4b5c');
        }, 5000);
        */
    </script>

</body>
</html>
```

**Nota:** Reemplace `http://localhost/visor-procesos/` con la URL donde haya desplegado los archivos del visor.

## Consideraciones Adicionales

*   **Paginación:** Si su aplicación tiene una lista paginada de diagramas, puede usar JavaScript para cambiar dinámicamente el atributo `src` del `<iframe>` cuando el usuario seleccione un diagrama diferente. El ejemplo de código incluye una función `cargarDiagrama` para este propósito.
*   **Rendimiento:** El visor cargará todos sus recursos dentro del `<iframe>`. El impacto en el rendimiento de la página principal debería ser mínimo.
*   **Sin modificaciones:** No es necesario modificar el código fuente del visor para que esta integración funcione.
