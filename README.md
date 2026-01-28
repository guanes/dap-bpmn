# DAP-BPMN Viewer

Un visualizador web para diagramas de Business Process Model and Notation (BPMN), diseñado para el proyecto DAP. Esta aplicación permite a los usuarios navegar, visualizar y analizar diagramas de procesos de negocio directamente en su navegador.

## Características

*   Visualización interactiva de diagramas BPMN.
*   Navegación entre diferentes diagramas de proceso.
*   Funcionalidad de búsqueda de elementos específicos del proceso.
*   Aplicación 100% cliente, no requiere un backend para la visualización.

## Stack Tecnológico

*   **HTML5**
*   **CSS3:** Utiliza el framework Foundation para la base de estilos.
*   **JavaScript:**
    *   jQuery
    *   Backbone.js
    *   KineticJS

## Cómo Empezar

Esta es una aplicación web estática y no requiere ningún proceso de compilación.

1.  **Clona el repositorio:**
    ```bash
    git clone https://github.com/guanes/DAP-BPMN.git
    cd DAP-BPMN
    ```

2.  **Inicia un servidor web local:**
    Para ver la aplicación, necesitas servir los archivos a través de un servidor HTTP local. Una forma sencilla de hacerlo es con el servidor incorporado de Python.

    ```bash
    # Para Python 3
    python3 -m http.server
    ```

3.  **Abre la aplicación en tu navegador:**
    Abre tu navegador y navega a `http://localhost:8000`.

## Despliegue

Esta aplicación está desplegada en: `https://guanes.github.io/dap-bpmn/`
