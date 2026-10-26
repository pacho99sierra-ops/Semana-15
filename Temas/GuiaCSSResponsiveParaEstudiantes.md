# Guía de CSS responsive para estudiantes

Esta guía explica recursos de CSS que ayudan a adaptar una página a distintos tamaños de pantalla y a las necesidades de las personas usuarias. Complementa la [Guía de consulta: CSS responsive por función](./GuiaCSSResponsive.md); no la reemplaza.

La idea principal es reconocer tres tipos de recursos:

| Tipo | Qué representa | Ejemplo |
| --- | --- | --- |
| **Unidad** | Cómo se calcula una medida | `2rem`, `80%`, `5vw` |
| **Propiedad** | Qué característica del elemento se configura | `width`, `gap`, `font-size` |
| **Función** | Cómo se calcula el valor de una propiedad | `clamp(1rem, 4vw, 2rem)` |

En `font-size: clamp(1rem, 4vw, 2rem)`, por ejemplo, `font-size` es la **propiedad**, `clamp()` es la **función** y `rem` y `vw` son **unidades**.

> Una unidad, propiedad o función no vuelve responsive una página por sí sola. El resultado depende de cómo se combina con el contenido, el diseño y las condiciones de pantalla.

## 1. Unidades para tamaños y espacios

Una unidad da significado a un número. Algunas se relacionan con el texto, otras con el contenedor, la cuadrícula o el viewport.

| Unidad | Para qué sirve | Qué hace | Cómo se usaría |
| --- | --- | --- | --- |
| `px` | Detalles que necesitan una medida precisa, como bordes finos. | Representa píxeles CSS; no depende del tamaño de la ventana. | `border: 1px solid #ccc;` |
| `rem` | Tipografía y espacios que deben crecer junto con el tamaño de texto raíz. | Se calcula a partir del tamaño de fuente del elemento raíz. | `padding: 1rem;` |
| `em` | Espacios relacionados con la tipografía local. | Se calcula según el tamaño de fuente del elemento o, en algunas propiedades, del padre. Los valores pueden acumularse en elementos anidados. | `margin-inline: 1em;` |
| `%` | Anchos y espacios relativos a otro elemento. | Se calcula respecto a una referencia que depende de la propiedad; para `width`, normalmente es el bloque contenedor. | `width: 100%;` |
| `vw` | Medidas que cambian con el ancho del viewport. | `1vw` equivale al 1 % del ancho de la ventana. | `font-size: clamp(1.5rem, 4vw, 3rem);` |
| `vh` | Medidas relacionadas con la altura visible. | `1vh` equivale al 1 % de la altura del viewport. En móviles la interfaz del navegador puede cambiar el espacio visible. | `min-height: 100vh;` |
| `dvh` | Alturas que siguen el viewport dinámico, especialmente en móviles. | Se ajusta cuando cambia la parte visible de la ventana del navegador. | `min-height: 100dvh;`; puede acompañarse de `100vh` como valor de respaldo. |
| `fr` | Repartir el espacio restante entre pistas de CSS Grid. | Representa una fracción del espacio disponible en la cuadrícula; no es una unidad general para cualquier propiedad. | `grid-template-columns: 1fr 2fr;` |
| `ch` | Limitar el largo de una línea de texto. | Se aproxima al ancho del carácter `0` de la fuente actual. | `max-width: 65ch;` |
| `ms` | Configurar duraciones muy breves, por ejemplo al respetar movimiento reducido. | Representa milisegundos. | `transition-duration: 0.01ms;` |

### Recomendaciones para elegir una unidad

- Prefiere `rem` para tipografía, rellenos y separaciones que deben escalar de forma consistente.
- Usa `%` cuando un elemento deba ocupar una parte del espacio de su contenedor.
- Usa `fr` para repartir espacio dentro de Grid.
- Usa `vw` con límites, por ejemplo mediante `clamp()`, para que un valor fluido no quede demasiado pequeño o grande.
- Usa `px` cuando el detalle necesite precisión; no hace falta eliminar todos los píxeles para tener un diseño responsive.
- Usa `vh` con cuidado para alturas móviles. Cuando el contenido deba ocupar la altura visible dinámica, considera `dvh`.
- Usa `ch` para evitar líneas de texto demasiado largas, no como sustituto de un ancho de página.

## 2. Propiedades CSS que ayudan a adaptar una página

Una propiedad es el nombre que aparece antes de los dos puntos en una declaración, como `width` en `width: 100%`. Las propiedades se agrupan aquí por el problema que ayudan a resolver.

### 2.1. Anchos, alturas y límites

#### `width`

- **Para qué sirve:** definir el ancho de un elemento.
- **Qué hace:** asigna un ancho fijo o calculado. `100%` usa el ancho de referencia de su contenedor, no necesariamente el de toda la ventana.
- **Cómo se usaría:** `width: 100%;` para un campo que debe ocupar su columna.

#### `height`

- **Para qué sirve:** definir una altura.
- **Qué hace:** fija la altura; si el contenido necesita más espacio, puede desbordarse.
- **Cómo se usaría:** reserva `height` para componentes con altura realmente controlada, como una barra de progreso; para bloques con texto variable, suele ser más seguro `min-height`.

#### `min-width`

- **Para qué sirve:** establecer el ancho mínimo que puede tener un elemento.
- **Qué hace:** impide que el elemento se encoja por debajo del límite. Un mínimo grande puede provocar desplazamiento horizontal.
- **Cómo se usaría:** `min-width: 0;` en una columna de Flexbox o Grid permite que se encoja según el espacio disponible.

#### `max-width`

- **Para qué sirve:** limitar cuánto puede crecer un elemento.
- **Qué hace:** establece un ancho máximo sin obligar a que el elemento siempre tenga ese ancho.
- **Cómo se usaría:** `width: 100%; max-width: 70rem;` para que una página se estreche en móviles y no se extienda demasiado en monitores grandes.

#### `min-height`

- **Para qué sirve:** asegurar una altura mínima.
- **Qué hace:** permite que el bloque crezca si el contenido lo necesita.
- **Cómo se usaría:** `min-height: 100vh;` para una página que como mínimo ocupe la altura del viewport.

#### `max-height`

- **Para qué sirve:** establecer un límite superior para la altura.
- **Qué hace:** impide que un elemento crezca por encima del valor indicado; el contenido puede necesitar desplazamiento o recorte si supera el límite.
- **Cómo se usaría:** aplícala solo cuando el diseño defina qué hacer con el contenido adicional, por ejemplo una región con `overflow: auto`.

#### `box-sizing`

- **Para qué sirve:** hacer más previsible el cálculo del tamaño final.
- **Qué hace:** con `border-box`, el ancho y alto declarados incluyen el relleno y el borde.
- **Cómo se usaría:** `* { box-sizing: border-box; }` como regla general de inicio.

### 2.2. Espacios y separación

#### `padding`

- **Para qué sirve:** dejar espacio entre el contenido de un elemento y su borde.
- **Qué hace:** aumenta el espacio interior. Con `box-sizing: border-box`, ese relleno queda incluido en el ancho declarado.
- **Cómo se usaría:** `padding: clamp(1rem, 4vw, 2rem);` para que una tarjeta tenga más espacio interior en pantallas grandes, sin exceder los límites.

#### `margin-inline`

- **Para qué sirve:** definir los márgenes al inicio y al final de una línea.
- **Qué hace:** adapta el margen a la dirección del texto; en una página en español normalmente corresponde a izquierda y derecha.
- **Cómo se usaría:** `width: min(100% - 2rem, 70rem); margin-inline: auto;` para centrar un bloque y dejar espacio en los bordes.

#### `gap`

- **Para qué sirve:** separar elementos dentro de Flexbox o Grid.
- **Qué hace:** crea espacio entre filas y columnas sin añadir márgenes a cada hijo.
- **Cómo se usaría:** `.tarjetas { display: grid; gap: 1rem; }`.

### 2.3. Flexbox y alineación

#### `display`

- **Para qué sirve:** elegir cómo se organiza el contenido de un elemento.
- **Qué hace:** activa modelos de distribución como Flexbox (`flex`) o Grid (`grid`).
- **Cómo se usaría:** `display: flex;` para una fila de controles y `display: grid;` para una cuadrícula de tarjetas.

#### `flex`

- **Para qué sirve:** controlar el crecimiento, la reducción y el tamaño inicial de un elemento flexible.
- **Qué hace:** combina esos tres valores en una declaración abreviada.
- **Cómo se usaría:** `flex: 1 1 14rem;` permite que un campo crezca, se encoja y parta de una base aproximada de `14rem`.

#### `flex-direction`

- **Para qué sirve:** escoger la dirección principal de un contenedor Flexbox.
- **Qué hace:** organiza los hijos en una fila (`row`) o una columna (`column`).
- **Cómo se usaría:** `flex-direction: column;` dentro de un breakpoint cuando una fila de filtros ya no cabe.

#### `flex-wrap`

- **Para qué sirve:** permitir que los elementos flexibles ocupen más de una línea.
- **Qué hace:** evita que una fila obligue a sus hijos a desbordarse cuando no caben.
- **Cómo se usaría:** `flex-wrap: wrap;` en una barra de acciones o grupo de controles.

#### `align-items`

- **Para qué sirve:** alinear los hijos en el eje transversal de Flexbox o Grid.
- **Qué hace:** en Flexbox alinea los hijos en el eje transversal; en Grid controla su alineación en el eje de bloque. En una página con escritura horizontal, suele ser el eje vertical.
- **Cómo se usaría:** `align-items: center;` para centrar controles en una barra.

#### `justify-content`

- **Para qué sirve:** distribuir los hijos a lo largo del eje principal de Flexbox.
- **Qué hace:** en Flexbox distribuye los elementos a lo largo del eje principal; también puede alinear pistas en Grid.
- **Cómo se usaría:** `justify-content: space-between;` para separar los extremos de un encabezado.

### 2.4. CSS Grid

#### `grid-template-columns`

- **Para qué sirve:** definir las columnas de una cuadrícula.
- **Qué hace:** fija sus tamaños o establece cómo se reparten.
- **Cómo se usaría:** `grid-template-columns: repeat(3, minmax(0, 1fr));` para tres columnas que comparten el espacio disponible y pueden encogerse.

#### `grid-template-rows`

- **Para qué sirve:** definir las filas de una cuadrícula.
- **Qué hace:** establece el tamaño de cada pista de fila.
- **Cómo se usaría:** en una cuadrícula con altura disponible, `grid-template-rows: auto 1fr auto;` puede reservar una fila flexible entre el encabezado y el pie.

#### `grid-column`

- **Para qué sirve:** indicar qué columnas ocupa un elemento de Grid.
- **Qué hace:** coloca un elemento en una o más pistas de columna.
- **Cómo se usaría:** `grid-column: 1 / -1;` para que un mensaje ocupe todas las columnas.

#### `justify-self`

- **Para qué sirve:** alinear un elemento dentro de su propia celda en el eje horizontal.
- **Qué hace:** cambia la alineación del elemento individual, sin alterar a los otros hijos.
- **Cómo se usaría:** `justify-self: start;` para alinear una acción al inicio de su celda.

### 2.5. Texto, desbordamiento e imágenes

#### `font-size`

- **Para qué sirve:** definir el tamaño de letra.
- **Qué hace:** determina el tamaño del texto y también influye en otras medidas relativas como `em`.
- **Cómo se usaría:** `font-size: clamp(1.5rem, 4vw, 3rem);` para un título que crece con límites.

#### `line-height`

- **Para qué sirve:** definir la altura de línea del texto.
- **Qué hace:** controla el espacio vertical entre líneas y puede mejorar la lectura en pantallas estrechas.
- **Cómo se usaría:** `line-height: 1.5;` para párrafos.

#### `overflow`

- **Para qué sirve:** definir qué hacer cuando el contenido supera los límites de su caja.
- **Qué hace:** permite mostrarlo, recortarlo o habilitar desplazamiento.
- **Cómo se usaría:** `overflow: auto;` en una región que deba desplazarse. No uses `hidden` para esconder un problema de diseño.

#### `overflow-wrap`

- **Para qué sirve:** impedir que palabras, códigos o direcciones largos salgan de su contenedor.
- **Qué hace:** permite dividir una cadena larga cuando no cabe en una línea.
- **Cómo se usaría:** `overflow-wrap: anywhere;` en campos de texto que pueden contener valores extensos.

#### `aspect-ratio`

- **Para qué sirve:** conservar una proporción entre el ancho y el alto de un elemento.
- **Qué hace:** calcula una de las dimensiones a partir de la otra.
- **Cómo se usaría:** `.marco { aspect-ratio: 4 / 3; }` para reservar una forma estable de imagen o video.

#### `object-fit`

- **Para qué sirve:** decidir cómo se adapta una imagen o video a una caja con dimensiones definidas.
- **Qué hace:** conserva o ajusta la proporción del contenido dentro de la caja; `cover` llena la caja y puede recortar.
- **Cómo se usaría:** `img { width: 100%; height: 100%; object-fit: cover; }`.

#### `object-position`

- **Para qué sirve:** elegir qué parte de una imagen queda visible cuando se ajusta dentro de una caja.
- **Qué hace:** mueve el punto de encuadre del contenido.
- **Cómo se usaría:** `object-position: center;` o `object-position: top;` junto con `object-fit`.

### 2.6. Consultas de contenedor y preferencias

#### `container-type`

- **Para qué sirve:** permitir que un componente responda al tamaño de su contenedor.
- **Qué hace:** establece un contexto que puede ser consultado con `@container`.
- **Cómo se usaría:** `.panel { container-type: inline-size; }` para consultar el ancho del panel, no el del viewport.

#### `scroll-behavior`

- **Para qué sirve:** definir cómo se desplaza la página al ir a un ancla.
- **Qué hace:** permite desplazamiento instantáneo (`auto`) o suave (`smooth`).
- **Cómo se usaría:** para respetar movimiento reducido, `@media (prefers-reduced-motion: reduce) { html { scroll-behavior: auto; } }`.

#### `animation-duration`

- **Para qué sirve:** definir cuánto dura una animación.
- **Qué hace:** establece el tiempo de ejecución de una animación.
- **Cómo se usaría:** reducir o eliminar la duración dentro de `@media (prefers-reduced-motion: reduce)` si la interfaz incluye animaciones.

#### `transition-duration`

- **Para qué sirve:** definir cuánto tarda una transición entre estados.
- **Qué hace:** establece la duración de cambios como color, tamaño o posición.
- **Cómo se usaría:** usar una duración breve en estados `:hover` y respetar la preferencia de movimiento reducido.

## 3. Funciones para calcular medidas

Una función aparece como parte del valor de una propiedad. Puede combinarse con unidades para crear medidas fluidas o limitar el crecimiento.

#### `clamp(mínimo, preferido, máximo)`

- **Para qué sirve:** crear tamaños que cambian con el viewport sin llegar a extremos.
- **Qué hace:** mantiene el valor calculado entre un límite mínimo y uno máximo.
- **Cómo se usaría:** `font-size: clamp(1.5rem, 4vw, 3rem);`.

#### `min(a, b)`

- **Para qué sirve:** elegir el menor de varios valores.
- **Qué hace:** devuelve el primer límite que resulte menor para el cálculo actual.
- **Cómo se usaría:** `width: min(100%, 70rem);` para ocupar el contenedor sin superar `70rem`.

#### `max(a, b)`

- **Para qué sirve:** asegurar un valor mínimo calculado.
- **Qué hace:** devuelve el mayor de los valores comparados.
- **Cómo se usaría:** `padding-bottom: max(5rem, 10vh);` para reservar un espacio que no sea menor de `5rem`.

#### `calc(...)`

- **Para qué sirve:** combinar medidas con operaciones matemáticas.
- **Qué hace:** calcula una expresión que puede incluir distintas unidades compatibles.
- **Cómo se usaría:** `width: calc(100% - 2rem);` para dejar espacio a ambos lados de un bloque.

#### `minmax(mínimo, máximo)`

- **Para qué sirve:** definir el intervalo de tamaño de una pista de Grid.
- **Qué hace:** permite que una fila o columna crezca y se encoja dentro de límites.
- **Cómo se usaría:** `grid-template-columns: repeat(3, minmax(0, 1fr));`.

#### `repeat(cantidad, definición)`

- **Para qué sirve:** repetir un patrón de filas o columnas de Grid.
- **Qué hace:** evita escribir varias veces la misma definición.
- **Cómo se usaría:** `grid-template-columns: repeat(3, 1fr);`.

## 4. Palabras clave que suelen acompañar a Grid responsive

`auto-fit` y `auto-fill` no son unidades ni funciones: son palabras clave que se usan dentro de `repeat()`.

- **`auto-fit`:** colapsa las pistas vacías para que las columnas ocupen el espacio disponible.
- **`auto-fill`:** conserva pistas vacías, incluso si hay menos elementos que columnas posibles.

Ejemplo de cuadrícula que crea tantas columnas como quepan:

```css
.tarjetas {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 15rem), 1fr));
  display: flex;
  gap: 1rem;
}
```

## 5. Consultas de medios y puntos de cambio

`@media` no es una propiedad ni una función: es una regla que aplica estilos cuando se cumple una condición, como un ancho máximo.

```css
.filtros {
  display: flex;
  gap: 1rem;
}

@media (max-width: 44rem) {
  .filtros {
    flex-direction: column;
  }
}
```

El breakpoint (`44rem` en el ejemplo) no es universal. Escógelo cuando el contenido deje de caber o se vuelva difícil de usar, y prueba antes y después del punto de cambio.

### Consultas de contenedor

Una consulta de contenedor responde al tamaño de un componente, aunque la ventana sea ancha:

```css
.panel-interior {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.panel {
  container-type: inline-size;
}

@container (max-width: 28rem) {
  .panel-interior {
    grid-template-columns: 1fr;
  }
}
```

Úsala cuando el mismo componente pueda aparecer en contenedores de distintos anchos. Para cambios generales de página, suele bastar con `@media`.

## 6. Ejemplo completo: diseño que se adapta

```css
* {
  box-sizing: border-box;
}

.pagina {
  width: min(100% - 2rem, 70rem);
  margin-inline: auto;
  padding: clamp(1rem, 4vw, 2.5rem);
}

.titulo {
  max-width: 65ch;
  font-size: clamp(1.75rem, 5vw, 3.5rem);
  line-height: 1.1;
}

.tarjetas {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 16rem), 1fr));
  gap: clamp(0.75rem, 2vw, 1.5rem);
}

.tarjeta {
  min-width: 0;
  padding: 1rem;
  overflow-wrap: anywhere;
}

@media (max-width: 40rem) {
  .pagina {
    width: 100%;
    padding: 1rem;
  }
}
```

En este ejemplo:

- `width`, `margin-inline`, `font-size`, `line-height`, `display`, `grid-template-columns`, `gap`, `min-width`, `padding` y `overflow-wrap` son **propiedades**.
- `%`, `rem`, `vw`, `ch` y `fr` son **unidades**.
- `min()`, `clamp()`, `repeat()` y `minmax()` son **funciones**.
- `@media` es una **regla condicional** y `auto-fit` una **palabra clave**.

## 7. Preferencias de movimiento

Además del tamaño de pantalla, se puede adaptar la interfaz a una preferencia del sistema:

```css
@media (prefers-reduced-motion: reduce) {
  html {
    scroll-behavior: auto;
  }

  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

Esta consulta no hace que el diseño cambie de columnas; adapta animaciones y desplazamientos para quienes prefieren menos movimiento.

## 8. Errores comunes que conviene evitar

- **Fijar todo en píxeles:** una medida fija puede no caber en una pantalla estrecha. Usa límites flexibles cuando el contenido lo permita.
- **Dar una altura fija a texto variable:** puede cortar contenido. Considera `min-height` o deja que el bloque crezca.
- **Usar `width: 100%` sin revisar el modelo de caja:** el relleno y el borde pueden aumentar el ancho si no se aplica `box-sizing: border-box`.
- **Ocultar desbordamientos con `overflow: hidden`:** primero identifica qué elemento es demasiado ancho.
- **Usar `vw` sin límites para texto:** puede producir letras demasiado pequeñas o grandes. Combínalo con `clamp()`.
- **Elegir breakpoints por modelos de dispositivos:** decide según el punto en que tu contenido deja de caber.
- **Forzar todas las propiedades en una sola página:** utiliza cada recurso solo si resuelve una necesidad real.

## 9. Lista para probar el resultado

1. Revisa la página en pantallas estrechas, medianas y anchas.
2. Cambia gradualmente el ancho para detectar dónde se aprieta el contenido.
3. Comprueba que no haya desplazamiento horizontal en toda la página. Si una tabla necesita más ancho, limita el desplazamiento a su contenedor.
4. Verifica que botones, campos y texto se puedan usar sin zoom ni recortes.
5. Aumenta el tamaño del texto del navegador y revisa que la página siga siendo utilizable.
6. Prueba la preferencia de movimiento reducido si hay animaciones o desplazamiento suave.

El objetivo no es usar la mayor cantidad posible de propiedades, unidades o funciones, sino escoger las adecuadas para que el contenido se adapte, siga legible y no se desborde.
