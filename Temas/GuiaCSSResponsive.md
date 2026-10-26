# Guía de consulta: CSS responsive por función

Esta guía reúne propiedades y técnicas para crear páginas que se adapten a diferentes espacios de visualización. Está organizada por lo que necesitas resolver: preparar el viewport, definir medidas, distribuir elementos, cambiar el diseño en puntos concretos y evitar problemas con el contenido. Los ejemplos son genéricos y pueden adaptarse a cualquier página.

## 1. Preparar la página para el viewport

El **viewport** es el área visible de una página dentro del navegador. En HTML se suele incluir esta etiqueta en el `<head>`:

```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

- `width=device-width` pide al navegador que use el ancho real del dispositivo como ancho de la página.
- `initial-scale=1` establece la escala inicial.

Sin esta etiqueta, algunos navegadores móviles pueden mostrar la página como si estuviera dentro de una ventana más ancha y reducirla para que quepa. La etiqueta no hace responsive el CSS por sí sola: permite que las reglas CSS respondan al ancho esperado.

## 2. Elegir unidades para tamaños y espacios

Las unidades indican cómo se calcula una medida. Algunas son fijas y otras dependen del texto, del contenedor o del viewport.

| Unidad | Depende de | Usos habituales |
| --- | --- | --- |
| `px` | Un píxel CSS | Bordes finos o detalles que deben tener un grosor controlado. |
| `rem` | El tamaño de fuente raíz del documento | Tipografía, rellenos, márgenes y separaciones escalables. |
| `em` | El tamaño de fuente del elemento (o del elemento padre para algunas propiedades) | Espacios relacionados con la tipografía local. |
| `%` | El tamaño del bloque contenedor o, según la propiedad, de otro elemento de referencia | Anchos flexibles y posiciones relativas. |
| `vw` | El ancho del viewport | Valores que cambian con el ancho de la ventana. |
| `vh` | El alto del viewport | Secciones relacionadas con la altura visible; úsala con cuidado en móviles. |
| `fr` | El espacio disponible dentro de una cuadrícula CSS Grid | Repartir espacio entre columnas o filas. |
| `ch` | El ancho aproximado del carácter “0” en la fuente actual | Limitar el ancho de una línea de texto, por ejemplo `max-width: 65ch`. |

### ¿Cuándo conviene cada unidad?

- Usa `rem` para tamaños y espacios que deberían conservar una relación con el texto y las preferencias de tamaño del navegador.
- Usa `%` cuando un elemento deba ocupar una parte del espacio disponible.
- Usa `fr` dentro de Grid cuando varias columnas deban compartir el espacio restante.
- Usa `vw` con límites si una medida debe cambiar con la ventana. Un título con `font-size: 5vw` podría volverse demasiado pequeño o grande; por eso suele combinarse con `clamp()`.
- Conserva `px` para detalles que necesitan un valor preciso, como un borde de `1px`. No es necesario eliminar todos los píxeles para hacer una página responsive.

Un ejemplo de ancho de texto expresado en caracteres:

```css
.introduccion {
  max-width: 65ch;
}
```

Esto limita el largo de las líneas y ayuda a evitar párrafos demasiado anchos en pantallas grandes.

## 3. Definir anchos, altos y límites

### `width` y `height`

`width` establece un ancho y `height` una altura. Si se asigna un valor fijo, el elemento intenta conservarlo aunque el espacio disponible o el contenido cambien. Por ejemplo, `width: 900px` puede ser más ancho que una pantalla de teléfono.

Para un reproductor de audio que debe usar el ancho de su columna:

```css
.reproductor {
  width: 100%;
}
```

`100%` significa que el elemento usa el ancho del bloque que lo contiene; no significa necesariamente que ocupe el ancho de toda la ventana.

### `min-width`, `max-width`, `min-height` y `max-height`

Estas propiedades establecen límites:

- `min-width`: el elemento no se hará más estrecho que el valor definido.
- `max-width`: el elemento no crecerá más allá del valor definido.
- `min-height`: el elemento tendrá por lo menos esa altura, pero puede crecer si el contenido lo necesita.
- `max-height`: el elemento no crecerá más allá de esa altura.

Una biografía puede ocupar el ancho disponible, pero tener un límite para conservar líneas legibles:

```css
.biografia {
  width: 100%;
  max-width: 42rem;
}
```

`min-width` puede ser útil para evitar columnas demasiado angostas, pero un valor mínimo grande también puede causar desbordamiento. Las alturas fijas, por su parte, pueden cortar contenido cuando el texto ocupa más líneas de las previstas. Para componentes con texto variable, considera `min-height` en lugar de `height`.

### Centrar un bloque con márgenes automáticos

Si un bloque tiene un ancho máximo y es más estrecho que su contenedor, los márgenes automáticos pueden centrarlo:

```css
.contenido-principal {
  width: min(100% - 2rem, 70rem);
  margin-inline: auto;
}
```

`margin-inline` aplica márgenes al inicio y al final de la línea, que normalmente son los lados izquierdo y derecho en una página en español. `auto` reparte el espacio libre entre ambos lados.

## 4. Hacer que medidas y espacios cambien gradualmente

### `clamp()`

`clamp(mínimo, preferido, máximo)` permite que una medida cambie, pero nunca sea menor o mayor que los límites indicados:

```css
.titulo-noticia {
  font-size: clamp(2rem, 5vw, 4rem);
}
```

El navegador calcula el tamaño preferido a partir del viewport (`5vw`), pero lo mantiene como mínimo en `2rem` y como máximo en `4rem`. También sirve para espacios:

```css
.panel {
  padding: clamp(1rem, 4vw, 3rem);
}
```

De esta forma el relleno de un panel puede aumentar gradualmente y dejar de crecer al llegar al máximo.

### `min()`, `max()` y `calc()`

- `min(a, b)` devuelve el menor valor.
- `max(a, b)` devuelve el mayor valor.
- `calc()` permite combinar valores y unidades con operaciones matemáticas.

```css
.hoja {
  width: min(68rem, calc(100% - 2rem));
  margin-inline: auto;
}
```

La hoja mide como máximo `68rem`; si el contenedor es estrecho, ocupa el ancho disponible dejando un margen de `1rem` a cada lado.

### `minmax()`

`minmax(mínimo, máximo)` establece un intervalo para una pista (columna o fila) de CSS Grid. Se usa dentro de `grid-template-columns` o `grid-template-rows`:

```css
.galeria {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
}
```

Cada una de las tres columnas puede encogerse hasta `0` y compartir el espacio restante. El mínimo `0` es útil cuando el contenido largo no debe obligar a la columna a ensanchar la cuadrícula.

## 5. Distribuir elementos con Flexbox

Flexbox distribuye elementos en una dirección principal: una fila o una columna. Es útil para barras de navegación, grupos de botones, controles y otras filas de contenido.

```css
.barra-herramientas {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
}
```

- `display: flex` activa Flexbox en el contenedor.
- `justify-content` distribuye los elementos en el eje principal.
- `align-items` alinea los elementos en el eje transversal.
- `gap` establece espacio entre los elementos sin tener que asignar márgenes a cada uno.

### `flex-wrap` y `flex-direction`

Por defecto, los elementos flexibles intentan mantenerse en una sola fila. `flex-wrap: wrap` permite que continúen en otra línea cuando no caben:

```css
.etiquetas {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
```

`flex-direction` cambia la dirección:

```css
.perfil {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
```

El valor predeterminado es `row` (fila); `column` coloca los elementos uno debajo de otro. Esta propiedad suele cambiarse dentro de una consulta de medios cuando una fila deja de caber.

### La propiedad abreviada `flex`

`flex` controla cuánto puede crecer o encogerse un elemento y cuál es su tamaño inicial:

```css
.busqueda input {
  flex: 1 1 14rem;
}
```

Los tres valores representan, en orden: factor de crecimiento, factor de reducción y tamaño base. En este caso, el campo puede crecer, encogerse y parte de una base de `14rem`.

## 6. Distribuir elementos con CSS Grid

Grid organiza filas y columnas. Es útil para galerías, catálogos, paneles y diseños con varias áreas.

```css
.catalogo {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1.25rem;
}
```

- `display: grid` activa Grid en el contenedor.
- `grid-template-columns` define columnas.
- `repeat(3, ...)` repite la definición tres veces.
- `1fr` reparte una fracción igual del espacio disponible.
- `gap` define la separación entre filas y columnas.

### `auto-fit` y `auto-fill`

Cuando se quiere que Grid decida cuántas columnas caben según un ancho mínimo:

```css
.portadas {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(13rem, 1fr));
  gap: 1rem;
}
```

Cada columna intenta medir al menos `13rem`. Se agregan tantas columnas como quepan; el espacio restante se reparte entre ellas. `auto-fit` colapsa las pistas vacías para que las columnas ocupen el espacio disponible. `auto-fill` conserva las pistas vacías, lo cual puede dejar espacio reservado cuando hay pocos elementos.

### `grid-column` y `justify-self`

Dentro de una cuadrícula, `grid-column` indica qué columna o columnas ocupa un elemento. `justify-self` alinea ese elemento en el eje horizontal de su propia celda:

```css
.accion-principal {
  grid-column: 2;
  justify-self: start;
}
```

Estas propiedades son útiles al cambiar la cuadrícula con una consulta de medios y colocar, por ejemplo, un botón debajo del texto en vez de dejarlo en una columna lateral.

## 7. Cambiar el diseño con consultas de medios

Una consulta de medios aplica reglas CSS solamente cuando se cumple una condición. Para cambiar el diseño de una agenda cuando el viewport es estrecho:

```css
.agenda {
  display: grid;
  grid-template-columns: 10rem 1fr;
  gap: 1rem;
}

@media (max-width: 44rem) {
  .agenda {
    grid-template-columns: 1fr;
  }
}
```

### Anchos máximos y mínimos

- `@media (max-width: 44rem)` aplica las reglas cuando el viewport tiene ese ancho o menos.
- `@media (min-width: 60rem)` aplica las reglas desde ese ancho en adelante.

Se pueden combinar condiciones, pero es preferible comenzar con reglas claras y fáciles de probar.

### Elegir un punto de cambio

El número de la consulta, por ejemplo `44rem`, es un **breakpoint** o punto de cambio. No es un tamaño universal para todos los teléfonos o tabletas. Se elige cuando la distribución actual deja de caber o se vuelve incómoda.

Para encontrarlo:

1. Cambia gradualmente el ancho de la ventana.
2. Observa cuándo los textos, botones o columnas empiezan a apretarse.
3. Agrega el punto de cambio cerca de ese ancho.
4. Prueba tanto antes como después del punto.

### Orden de las reglas

Las reglas dentro de una consulta de medios se combinan con las reglas generales. Si ambas tienen la misma especificidad, una regla posterior suele prevalecer sobre la anterior. Por eso es común colocar las consultas después de los estilos generales del componente y mantener el orden consistente.

## 8. Evitar desbordamientos y controlar el contenido

### `box-sizing`

Por defecto, `width` puede no incluir el relleno (`padding`) y el borde. Con `box-sizing: border-box`, el ancho declarado sí los incluye:

```css
* {
  box-sizing: border-box;
}
```

Esto facilita calcular el tamaño final de columnas, tarjetas y campos. No adapta una página por sí solo, pero hace que las dimensiones sean más previsibles.

### `min-width: 0`

Los elementos dentro de Grid y Flexbox pueden conservar un ancho mínimo basado en su contenido. `min-width: 0` permite que un elemento se encoja por debajo de ese tamaño mínimo:

```css
.columna {
  min-width: 0;
}
```

Es especialmente útil si una palabra larga, una URL o una imagen empuja una columna más allá de su espacio.

### `overflow-wrap`

Permite que una palabra larga se divida si de otra forma se saldría del contenedor:

```css
.comentario {
  overflow-wrap: anywhere;
}
```

Úsalo en texto que puede contener palabras, códigos o direcciones muy largos. No reemplaza una distribución de columnas correcta.

### `overflow`

`overflow: hidden` recorta lo que sobresale de los límites de un elemento. Puede ser apropiado para una imagen dentro de un marco, pero no debe usarse para esconder contenido que se desborda por un error de diseño. Primero identifica por qué el contenido no cabe.

## 9. Mantener imágenes y formas proporcionales

Para que una imagen no sea más ancha que su contenedor y conserve sus proporciones:

```css
.fotografia {
  display: block;
  max-width: 100%;
  height: auto;
}
```

`aspect-ratio` establece una relación entre ancho y alto. Por ejemplo, `aspect-ratio: 4 / 3` mantiene una forma apaisada:

```css
.marco {
  width: 100%;
  aspect-ratio: 4 / 3;
}
```

Para una imagen que debe llenar completamente ese marco:

```css
.marco img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}
```

- `object-fit: cover` llena el marco recortando lo que no quepa, sin deformar la imagen.
- `object-position` cambia qué parte de la imagen queda visible.
- `aspect-ratio` establece la forma del marco, mientras que `object-fit` controla cómo se adapta la imagen a ese marco.

## 10. Consultas de contenedor

Una consulta de medios responde al ancho de la ventana. Una **container query** puede responder al ancho de un componente contenedor, aunque este esté dentro de una columna estrecha:

```css
.columna-perfil {
  container-type: inline-size;
}

@container (max-width: 28rem) {
  .tarjeta-perfil {
    grid-template-columns: 1fr;
  }
}
```

`container-type: inline-size` permite consultar el tamaño del contenedor en el eje horizontal. `@container` aplica las reglas cuando ese contenedor cumple la condición. Puede ser útil para componentes reutilizables; comprueba que los navegadores que necesitas soporten esta característica.

## 11. Adaptar tipografía, movimiento y preferencias

### Tipografía adaptable

`font-size` define el tamaño de letra y `line-height` la altura de línea. Un tamaño flexible puede usar `clamp()` para evitar extremos:

```css
.titulo {
  font-size: clamp(1.8rem, 4vw, 3.5rem);
  line-height: 1.1;
}
```

No reduzcas el texto solamente para hacer que quepa. También se puede reorganizar el diseño, permitir que el texto envuelva o aumentar el espacio disponible.

### Preferencia de movimiento reducido

Algunas personas prefieren reducir animaciones y desplazamientos. `prefers-reduced-motion` detecta esa preferencia del sistema:

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

Esta consulta no responde al ancho de pantalla, pero es otra forma de adaptar la experiencia a una preferencia del usuario.

## 12. Resumen de propiedades y cuándo usarlas

| Necesidad | Propiedades o técnica |
| --- | --- |
| Permitir que un elemento ocupe el espacio disponible | `width: 100%` |
| Limitar el crecimiento o encogimiento | `max-width`, `min-width`, `min-height` |
| Ajustar tamaño o espacio entre límites | `clamp()`, `min()`, `max()`, `calc()` |
| Repartir espacio en columnas | Grid, `fr`, `repeat()`, `minmax()` |
| Organizar una fila o columna | Flexbox, `flex-direction`, `flex-wrap` |
| Separar elementos en Grid o Flexbox | `gap` |
| Cambiar distribución según la ventana | `@media`, `max-width`, `min-width` |
| Cambiar distribución según el contenedor | `container-type`, `@container` |
| Evitar que columnas excedan su espacio | `min-width: 0`, `minmax(0, 1fr)` |
| Mantener o llenar la forma de una imagen | `aspect-ratio`, `object-fit`, `object-position` |
| Adaptar tipografía al espacio | `font-size` con `clamp()` y `rem` |
| Respetar preferencias de movimiento | `prefers-reduced-motion` |

Una página responsive no consiste en usar una propiedad especial, sino en combinar medidas flexibles, distribuciones adecuadas y cambios de diseño cuando el contenido lo necesita. Prueba diferentes anchos, revisa que no haya desplazamiento horizontal y comprueba que los textos sigan siendo legibles.
