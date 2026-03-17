# Sistema de Gestión de Inventario para Pequeños Negocios

## 1. Situación Problemática
**Enunciado:** En la actualidad, muchas pequeñas tiendas de abarrotes y emprendimientos locales llevan el control de sus productos y ventas de forma manual utilizando cuadernos o libretas. Esto genera problemas como pérdida de información, errores en los cálculos de inventario, y dificultad para conocer cuánta mercancía tienen realmente disponible. 

**Sector Enfocado:** Sector comercial minorista, como tiendas de abarrotes, pequeñas ferreterías, papelerías o negocios de comida rápida.

**Solución Propuesta:** Nuestro programa proporciona una interfaz web en donde el dueño del negocio puede registrar fácilmente la entrada de un nuevo producto, indicando su nombre, cantidad, categoría y precio. El sistema valida automáticamente que no se ingresen datos erróneos (como precios o cantidades negativas) y actualiza de inmediato una tabla visible con el inventario actual, resolviendo el problema del seguimiento manual y evitando pérdidas por falta de control.

## 2. Preguntas de Reflexión

### ¿Qué es Vue.js y cuál es su función dentro de la página web desarrollada?
Vue.js es un framework progresivo de JavaScript utilizado para construir interfaces de usuario. Su función principal en nuestra aplicación web es hacer que la interfaz sea dinámica e interactiva, sincronizando los datos (el inventario, lo que escribe el usuario, los errores de validación) con la vista (el HTML) de forma automática y reactiva, sin necesidad de recargar la página completa.

### Describa qué variables reactivas utilizó en su aplicación y cuál es la función de cada una
Se utilizaron 6 variables reactivas:
1. `name`: Almacena el nombre del producto que se está escribiendo en el formulario.
2. `quantity`: Guarda la cantidad de artículos del producto que se desea registrar.
3. `price`: Administra el precio del producto a ingresar.
4. `category`: Guarda la categoría seleccionada (Alimentos, Limpieza, etc.) del producto.
5. `products`: Un arreglo que almacena la lista completa de todos los productos que han sido añadidos exitosamente, el cual alimenta nuestro inventario virtual.
6. `errorMessage`: Almacena el mensaje temporal de error que se muestra cuando la validación falla (ej. si se deja un campo vacío o se pone una cantidad inválida).

### Explique la diferencia entre las directivas v-bind y v-model
- **v-bind**: Sirve para enlazar de forma reactiva un atributo HTML normal con una variable de JavaScript (unidireccional). En nuestra app lo usamos, por ejemplo, para cambiar dinámicamente una clase CSS basada en una condición (ej. `:class="{'btn-disabled': !name}"`).
- **v-model**: Crea un enlace bidireccional fluido entre la variable reativa de Vue y un elemento interactivo de entrada de formulario (`<input>`, `<select>`). Es decir, si el usuario escribe o interactúa con esta etiqueta en la página, la variable se actualiza automáticamente al valor de dicho campo en el código y viceversa.

### Mencione al menos un ejemplo de evento utilizado dentro de su aplicación
Se utilizó el evento **`@submit.prevent`** en la etiqueta `<form>` para detectar cuando el usuario realiza el intento mandar el formulario mediante el botón (agregar nuevo producto). El modificador `.prevent` es vital porque restringe la acción original del navegador y hace que la página no se recargue, ejecutando exclusivamente nuestra lógica reactiva (`addProduct`).

### Explique para qué utilizó la directiva v-for
La directiva **`v-for`** se utilizó para iterar sobre la lista de productos registrados u opciones (`products` y también en `categories`) y generar dinámicamente las filas o etiquetas repetitivas en el DOM como las de la tabla (`<tr>`) de nuestro inventario. De este modo, por cada producto guardado, Vue muestra automáticamente una nueva fila con sus campos correspondientes sin necesidad de repetir etiquetas a mano.

### Describa en qué situación utilizó v-if y qué problema resuelve
La directiva **`v-if`** se utilizó en repetidas ocasiones con la siguiente lógica:
1. Para mostrar u ocultar la cinta de alerta de error en la validación: `v-if="errorMessage"`. Esto resuelve el problema de tener cajas de texto prefabricadas en pantalla y las muestra sólo cuando hay fallas o errores, facilitando la comprensión del usuario final.
2. Para desplegar un texto informativo cuando el inventario está aún vacío: `v-if="products.length === 0"`, y su contraparte `v-else` que muestra netamente la lista completa de lo que la empresa vende. Esto prevé confusiones en interfaces visuales deshabitadas.

### Explique cómo se realiza la validación de datos en su aplicación y por qué es importante
La validación ocurre en el método `addProduct()` implementado en el núcleo de la aplicación al tratar de enviar el formato: antes de insertar un nuevo elemento, checamos gracias a un par de condicionales `if` que todas las variables correspondientes no presenten un rango falaz o en blanco. Además logramos inspeccionar numéricamente que el precio o cantidad pasen la prueba superior a 0 (`> 0`).
Es de vital importancia validarlo dado que, si insertamos datos corrompidos (como precios negativos en las mercancías de nuestra app o sin nombres explícitos), los componentes y tabulados arrastrarán una secuela de fallas y generarán discrepancias críticas que repercutirían en el reporte contable o el estado integral del sistema, arruinando nuestra credibilidad en el entorno.
