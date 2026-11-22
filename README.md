# Álbum de cromos con realidad aumentada

## Estructura de carpetas (en tu repositorio de GitHub)

```
index.html                  ← la página (ya hecha)
assets/
  targets.mind              ← lo generas tú con el compilador de MindAR
  videos/
    cromo-01.mp4
    cromo-02.mp4
    cromo-03.mp4
  img/                      ← (futuro) imágenes con transparencia
  models/                   ← (futuro) modelos 3D .glb
libs/                       ← (recomendado) copias locales de las 3 bibliotecas
  aframe.min.js
  mindar-image.prod.js
  mindar-image-aframe.prod.js
```

## Pasos para ponerlo en marcha

1. **Compila las fotos.** Abre el [compilador de MindAR](https://hiukim.github.io/mind-ar-js-doc/tools/compile), sube TODAS las fotos de los cromos a la vez, pulsa *Start* y descarga `targets.mind`. El orden en que subes las fotos es el orden de los cromos (0, 1, 2…).
2. **Prepara los vídeos.** MP4 con H.264 y audio AAC, 720p, 10-20 s. Cada archivo debe pesar menos de 25 MB para poder subirlo desde el navegador.
3. **Sube todo a GitHub** (Add file → Upload files) respetando las carpetas de arriba y activa GitHub Pages.
4. **Edita la lista `cards`** al principio de `index.html` (hay un bloque enmarcado con instrucciones). Un cromo por foto, en el mismo orden que en el compilador.
5. **Prueba** abriendo la dirección de GitHub Pages en el móvil. Genera el QR con cualquier generador gratuito.

## Para que dure años (muy recomendable)

La página usa tres bibliotecas. Si existe la carpeta `libs/`, usa esas copias; si no, las descarga de internet. Para no depender de ningún servicio externo, abre cada enlace en el navegador del ordenador, guarda el archivo con el nombre indicado (clic derecho → Guardar como, o Ctrl+S) y súbelo a `libs/`:

| Enlace | Guardar como |
|---|---|
| https://aframe.io/releases/1.4.2/aframe.min.js | `aframe.min.js` |
| https://cdn.jsdelivr.net/npm/mind-ar@1.2.2/dist/mindar-image.prod.js | `mindar-image.prod.js` |
| https://cdn.jsdelivr.net/npm/mind-ar@1.2.2/dist/mindar-image-aframe.prod.js | `mindar-image-aframe.prod.js` |

Copia y pega los enlaces enteros en lugar de escribirlos. Las tres versiones están elegidas porque se usan juntas en ejemplos públicos que funcionan; no las mezcles con otras.

## Añadir un cromo nuevo

El `id` es solo una etiqueta para ti: ponle cualquier nombre sin repetir. Lo que enlaza cada cromo con su foto es el **número de orden en el que subiste la foto al compilador** (empieza en 0). Por defecto es la posición del cromo en la lista `cards`.

1. Vuelve a compilar con **todas** las fotos (las antiguas en el mismo orden y la nueva al final) y sustituye `targets.mind`.
2. Sube el archivo nuevo (vídeo, imagen o modelo).
3. Añade un bloque **al final** de `cards`. Si lo colocas al final, no hace falta indicar el número.
4. Sube `appVersion` de `"1"` a `"2"` para que los móviles no usen archivos antiguos.

Ejemplo: tienes 3 cromos (números 0, 1 y 2) y añades un cuarto con una imagen:

```js
{
  id: "cromo-04",
  type: "image",
  name: "Cromo 4",
  src: "assets/img/cromo-04.png",
  scale: 1
}
```

Si lo pones en otro sitio de la lista, indica el número a mano: `targetIndex: 3`.

## Qué se puede poner sobre un cromo

| Tipo | Para qué | Notas |
|---|---|---|
| `video` | Vídeo sobre la foto | El más probado. Un solo reproductor compartido, así que aguanta muchos cromos. |
| `image` | Sello, marco, texto en PNG | Usa PNG o WebP con transparencia. |
| `model` | Modelo 3D `.glb` | Reproduce sus animaciones si las tiene. |

Para inventar un tipo nuevo, añade una entrada en `RENDERERS` (dentro del script de lógica) con las funciones `build`, `found` y `lost`.

## Si algo falla

- **Pantalla “Falta el archivo de reconocimiento”**: `assets/targets.mind` no está subido o la ruta no coincide.
- **No abre la cámara**: abre el enlace en Safari o Chrome (no dentro de WhatsApp, Instagram, etc.) y acepta el permiso.
- **El vídeo se ve sin sonido**: el navegador lo ha bloqueado; aparece un botón “Activar sonido”.
- **No reconoce la foto**: usa fotos con buen contraste y detalle, sin brillos, e imprime con buena calidad.
- **Mensajes técnicos**: añade `?debug` al final de la dirección y mira la consola del navegador.
- Si algún día `targets.mind` deja de funcionar tras cambiar de versión de MindAR, vuelve a compilarlo con el compilador actual.
