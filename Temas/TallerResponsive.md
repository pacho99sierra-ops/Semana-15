# Práctica guiada: adapta una página con CSS responsive

En esta práctica vas a tomar la página HTML que ya existe y a modificar su hoja `estilos-base.css`. La página incluye una tabla de cursos para que practiques cómo hacer que su contenido se pueda consultar en pantallas estrechas. En cada paso copiarás un bloque de CSS, reemplazarás el bloque del mismo selector o lo agregarás al final, y observarás cómo cambia la página en el navegador.

El HTML ya contiene todo el contenido de la página, incluida la tabla. No necesitas modificarlo durante la práctica: aprenderás cómo CSS modifica la presentación de sus etiquetas y elementos.

## Cómo empezar

1. Abre `index.html` en el navegador. De forma predeterminada, usa `estilos-docente.css` para mostrar la versión responsive terminada.
2. Para realizar la práctica, cambia en `index.html` el enlace de `estilos-docente.css` a `estilos-base.css` y guarda el archivo.
3. Abre `estilos-base.css` en tu editor.
4. Reduce el ancho de la ventana y observa qué partes se salen o quedan apretadas.
5. En cada paso, busca el selector indicado y reemplaza su bloque completo con el código nuevo. Si el paso dice “agrega”, pega el bloque al final de `estilos-base.css`.
6. Guarda los archivos y recarga la página después de cada cambio. Cuando termines, puedes volver a enlazar `estilos-docente.css` para comparar el resultado final.

## Paso 1: adapta el ancho de toda la página

En el HTML, `body` contiene toda la página: encabezado, contenido y pie. En el CSS busca el selector `body`:

```css
body {
  width: 1200px;
  margin: 0 auto;
  background: #f7f8fc;
}
```

Reemplázalo por:

```css
body {
  width: 100%;
  max-width: 1440px;
  min-width: 320px;
  margin: 0 auto;
  background: #f7f8fc;
}
```

**Qué cambiaste:** quitaste el ancho fijo de `1200px`. `width: 100%` permite que el cuerpo use el ancho disponible; `max-width` limita su crecimiento en pantallas grandes; `min-width` establece un ancho mínimo.

**Qué debes observar en el HTML:** todo el contenido que está dentro de `<body>` ahora cabe en una ventana estrecha. Antes, el cuerpo seguía midiendo 1200 píxeles y la página se desbordaba horizontalmente.

## Paso 2: permite que el encabezado use el espacio necesario

En el HTML, el selector `.encabezado` corresponde a `<header class="encabezado">`, que contiene la marca y los enlaces de navegación. Reemplaza el bloque `.encabezado` en el CSS por:

```css
.encabezado {
  min-height: 5.5rem;
  padding: 1rem clamp(1.25rem, 5vw, 4.5rem);
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1.5rem;
}
```

**Qué cambiaste:** en vez de fijar una altura exacta y 72 píxeles de espacio a cada lado, usas una altura mínima y un espacio lateral que puede variar.

**Qué debes observar:** el encabezado ya no conserva una altura rígida y sus elementos tienen una separación definida. En una pantalla muy estrecha la marca y los enlaces aún pueden quedar apretados; en un paso posterior los pondrás en filas separadas.

## Paso 3: ajusta las columnas de la portada

En el HTML, `.portada` corresponde a la primera sección principal (`<section class="portada">`). Reemplaza su bloque CSS completo por:

```css
.portada {
  min-height: 26rem;
  margin: 0 clamp(1.25rem, 5vw, 4.5rem);
  padding: clamp(2rem, 5vw, 4rem);
  display: grid;
  grid-template-columns: minmax(0, 1.1fr) minmax(16rem, 0.9fr);
  align-items: center;
  gap: clamp(2rem, 5vw, 4.5rem);
  border-radius: 1.5rem;
  background: #eeecff;
}
```

**Qué cambiaste:** las columnas ahora comparten el espacio disponible y el margen, el relleno y la separación pueden variar. `minmax()` define límites para el tamaño de las columnas y `clamp()` mantiene los espacios entre un mínimo y un máximo.

**Qué debes observar en el HTML:** el texto y la ilustración siguen uno al lado del otro cuando hay espacio, pero la sección deja de tener márgenes rígidos. Todavía cambiarán a una sola columna más adelante.

## Paso 4: haz que el título cambie de tamaño gradualmente

En el HTML, el selector `h1` corresponde al título principal de la portada. Busca el bloque `h1` y reemplázalo por:

```css
h1 {
  max-width: 490px;
  margin-bottom: 1rem;
  font-size: clamp(2.5rem, 5vw, 4.5rem);
  line-height: 1.05;
}
```

**Qué cambiaste:** el tamaño del título ya no es siempre `3.5rem`. `clamp()` lo ajusta al ancho de la ventana, pero evita que quede demasiado pequeño o demasiado grande.

**Qué debes observar:** al cambiar el ancho del navegador, el título se adapta gradualmente. Su contenido sigue siendo el mismo; solo cambia su presentación.

## Paso 5: adapta la ilustración de la portada

En el HTML, `.ilustracion` corresponde al `<div class="ilustracion">` que contiene el planeta y las estrellas. Reemplaza el bloque `.ilustracion` completo por:

```css
.ilustracion {
  position: relative;
  width: 100%;
  min-height: 17.5rem;
  aspect-ratio: 1.4;
  display: grid;
  place-items: center;
  overflow: hidden;
  border-radius: 1.25rem;
  background: #28224f;
  color: white;
}
```

**Qué cambiaste:** quitaste el ancho y la altura fijos de 390 y 280 píxeles. Ahora la ilustración ocupa el ancho de su columna y conserva una proporción con `aspect-ratio`.

**Qué debes observar:** la ilustración se hace más ancha o más estrecha junto con su columna, sin mantener un ancho fijo que pueda salirse de la portada.

## Paso 6: deja que el espacio entre secciones se adapte

En el HTML, `.seccion` corresponde a las secciones de cursos y proyecto. Reemplaza el bloque `.seccion` por:

```css
.seccion {
  margin: clamp(4rem, 8vw, 6rem) clamp(1.25rem, 5vw, 4.5rem);
}
```

**Qué cambiaste:** reemplazaste los márgenes fijos de 88 y 72 píxeles por espacios que pueden variar según el ancho disponible.

**Qué debes observar:** las secciones de cursos y proyecto conservan un margen cómodo en ventanas estrechas y no se separan excesivamente en pantallas grandes.

## Paso 7: permite que las tarjetas se encojan

En el HTML, `.rejilla-cursos` corresponde al `<div>` que contiene las tres tarjetas. Reemplaza su bloque CSS por:

```css
.rejilla-cursos {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1.5rem;
}
```

Después busca `.tarjeta`, que corresponde a cada `<article class="tarjeta">`, y reemplaza ese bloque por:

```css
.tarjeta {
  min-width: 0;
  padding: clamp(1.25rem, 2.5vw, 1.75rem);
  border: 1px solid #e7e8ef;
  border-radius: 1rem;
  background: white;
}
```

**Qué cambiaste:** `minmax(0, 1fr)` permite que cada columna de la cuadrícula se reduzca; `min-width: 0` permite que la tarjeta se encoja con su columna; `clamp()` ajusta el relleno de la tarjeta.

**Qué debes observar:** en una pantalla grande siguen apareciendo tres tarjetas en una fila. En una pantalla estrecha las columnas todavía pueden quedar demasiado angostas; en los siguientes pasos cambiarás su cantidad.

## Paso 8: permite que las descripciones ocupen el alto que necesitan

En el HTML, cada descripción de tarjeta es un párrafo dentro de `.tarjeta`. Busca el selector `.tarjeta > p:not(.categoria)` y reemplaza su bloque por:

```css
.tarjeta > p:not(.categoria) {
  color: #687186;
  line-height: 1.5;
}
```

**Qué cambiaste:** quitaste la altura mínima fija de 48 píxeles de las descripciones. El párrafo puede ocupar las líneas que necesite.

**Qué debes observar:** si una descripción ocupa más líneas, la tarjeta puede crecer para mostrar todo el texto en vez de intentar mantener el mismo alto para ese párrafo.

## Paso 9: organiza el bloque del proyecto

En el HTML, `.proyecto` corresponde a la sección oscura que aparece debajo de las tarjetas. Reemplaza su bloque CSS completo por:

```css
.proyecto {
  margin: 0 clamp(1.25rem, 5vw, 4.5rem) clamp(4rem, 7vw, 5rem);
  padding: clamp(1.75rem, 4vw, 3rem);
  display: grid;
  grid-template-columns: 4.75rem minmax(0, 1fr) auto;
  align-items: center;
  gap: clamp(1rem, 3vw, 2rem);
  border-radius: 1.25rem;
  background: #242044;
  color: white;
}
```

**Qué cambiaste:** el espacio exterior e interior ahora puede ajustarse. Las tres columnas tienen límites más flexibles: sello, texto y botón.

**Qué debes observar:** en una pantalla amplia, el sello, el texto y el botón quedan alineados en una fila. Si la ventana se estrecha, estos elementos necesitan reorganizarse; lo harás con consultas de medios.

## Paso 10: permite que el pie use más de una línea

En el HTML, `.pie` corresponde al `<footer id="contacto" class="pie">`. Reemplaza su bloque CSS por:

```css
.pie {
  min-height: 6.25rem;
  padding: 1.5rem clamp(1.25rem, 5vw, 4.5rem);
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  gap: 1rem 2rem;
  border-top: 1px solid #e7e8ef;
  color: #687186;
}
```

**Qué cambiaste:** `flex-wrap: wrap` permite que el contenido pase a otra línea cuando no cabe. El relleno también se ajusta al ancho.

**Qué debes observar:** en una ventana ancha, la marca, el texto y el correo aparecen en una fila; al reducirla, pueden distribuirse en varias líneas en lugar de salirse.

## Paso 11: permite consultar la tabla en una pantalla estrecha

En el HTML, `.tabla-contenedor` envuelve la tabla de cursos y `.tabla-cursos` identifica la tabla. Las columnas tienen un ancho mínimo para conservar la legibilidad. Agrega estas reglas al final de `estilos-base.css`:

```css
.tabla-contenedor {
  overflow-x: auto;
}

.tabla-contenedor:focus-visible {
  outline: 3px solid #5143c7;
  outline-offset: 3px;
}
```

**Qué cambiaste:** `overflow-x: auto` permite desplazarse horizontalmente dentro del contenedor cuando la tabla no cabe. El contorno muestra el área desplazable cuando se navega con el teclado.

**Qué debes observar:** en una pantalla angosta, las columnas conservan espacio para leer el contenido. Puedes desplazarte horizontalmente dentro de la tabla para consultar todos sus datos.

## Paso 12: apila la portada y reduce las columnas en una pantalla mediana

Hasta ahora cambiaste tamaños y espacios, pero en algunas ventanas la portada y las tarjetas aún quedan apretadas. Al final de `estilos-base.css`, agrega:

```css
@media (max-width: 760px) {
  .portada {
    grid-template-columns: minmax(0, 1fr);
    gap: 2rem;
  }

  .ilustracion {
    min-height: 15rem;
    aspect-ratio: 1.8;
  }

  .encabezado-seccion {
    align-items: start;
    flex-direction: column;
  }

  .rejilla-cursos {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .proyecto {
    grid-template-columns: 4.5rem minmax(0, 1fr);
  }

  .proyecto .boton {
    grid-column: 2;
    justify-self: start;
  }
}
```

**Qué cambiaste:** `@media (max-width: 760px)` aplica estas reglas cuando la ventana mide 760 píxeles o menos. La portada pasa de dos columnas a una; las tarjetas pasan de tres columnas a dos; el bloque del proyecto pasa a dos columnas y el botón ocupa otra fila.

**Qué debes observar en el HTML:** texto e ilustración de la portada quedan uno debajo del otro; las tarjetas se muestran en dos columnas; el botón del proyecto pasa debajo del texto.

## Paso 13: reorganiza los elementos para un teléfono

Debajo de la consulta anterior, agrega otra consulta:

```css
@media (max-width: 520px) {
  .encabezado {
    align-items: start;
    flex-direction: column;
    gap: 1rem;
  }

  .navegacion {
    width: 100%;
    justify-content: start;
    gap: 0.5rem 1.25rem;
    font-size: 0.95rem;
  }

  .portada {
    padding: 1.5rem;
    border-radius: 1rem;
  }

  h1 {
    font-size: clamp(2.5rem, 12vw, 3.5rem);
  }

  .ilustracion {
    min-height: 13rem;
  }

  .seccion {
    margin-top: 3.5rem;
    margin-bottom: 3.5rem;
  }

  .rejilla-cursos {
    grid-template-columns: minmax(0, 1fr);
  }

  .proyecto {
    grid-template-columns: minmax(0, 1fr);
    gap: 1.25rem;
  }

  .proyecto .boton {
    grid-column: 1;
  }

  .pie {
    align-items: start;
    flex-direction: column;
  }
}
```

**Qué cambiaste:** esta consulta se aplica cuando la ventana mide 520 píxeles o menos. La marca y la navegación se apilan, las tarjetas pasan a una columna y los elementos del proyecto y del pie también se organizan verticalmente.

**Qué debes observar en el HTML:** en un teléfono, puedes recorrer la marca y sus enlaces, leer cada tarjeta completa y encontrar el botón del proyecto sin que los elementos queden apretados lado a lado.

## Mira el resultado

Guarda el CSS, recarga `index.html` y cambia el ancho de la ventana. Comprueba estos valores:

- **360 px:** navegación, portada y tarjetas caben en una columna cuando corresponde.
- **430 px:** textos, enlaces y botones siguen siendo legibles.
- **768 px:** la portada está apilada y las tarjetas tienen espacio para dos columnas.
- **1280 px:** la portada y las tarjetas usan varias columnas sin ocupar un ancho excesivo.

Prueba también anchos cercanos a 520 y 760 píxeles. Verás cómo las reglas dentro de cada `@media` cambian la distribución de los elementos HTML al cumplirse esa condición.
