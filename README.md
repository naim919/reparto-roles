# Sala de juegos

Tripulantes y El impostor para jugar pasando un móvil. Todo ocurre en
navegador: no hay servidor, no hay cuentas, no se envía nada a ninguna parte.

## Archivos

| Archivo | Para qué sirve |
|---|---|
| `index.html` | El juego entero: pantallas, lógica y estilos en un solo archivo |
| `manifest.webmanifest` | Nombre, colores e iconos con los que se instala en el móvil |
| `sw.js` | Service worker: guarda una copia local para jugar sin cobertura |
| `icon-192.png` / `icon-512.png` | Icono de la app |

## Publicarlo

Súbelos todos juntos, en la raíz del proyecto (no dentro de una carpeta).
Cualquier alojamiento estático vale: Vercel, Netlify, Cloudflare Pages o
GitHub Pages. No hace falta configurar nada: no hay build, ni framework,
ni variables de entorno.

## El historial

Se guarda con `localStorage`, en el propio móvil. Sobrevive a cerrar la app,
pero es de ese navegador: no se sincroniza entre dispositivos y desaparece si
borras los datos de navegación. El botón de descargar `.txt` sigue ahí como
copia de seguridad.

Para un historial compartido entre móviles harían falta una base de datos y
un servidor. Ver más abajo.

## Si algún día quieres salas online

El reparto de roles tiene que hacerse en el servidor. Si se sortea en el
navegador, cualquiera puede abrir las herramientas de desarrollador y ver
quién es el impostor. Con Supabase serían tres piezas:

- tabla `partidas` (código de sala, juego, estado, palabra secreta)
- tabla `jugadores` (nombre, sala, rol asignado)
- una función de Postgres que hace el sorteo, más políticas RLS que impidan
  a cada jugador leer las filas de los demás

## Actualizar la app

Cuando cambies `index.html`, sube el número de versión en `sw.js`
(`const CACHE = 'sala-v1'` → `'sala-v2'`). Si no, los móviles que ya la
tengan instalada seguirán viendo la versión antigua guardada en caché.
