# Plataforma CAI · Centro de Aplicación de Idiomas — UVP

Plataforma de registro y control de asistencia para las actividades del CAI
(talleres, clubes de conversación, asesorías y sesiones de práctica de
certificación), inspirada en el formato de
https://zujaely.github.io/tedtalk-rubricas/ y adaptada a las necesidades
del CAI, con la identidad visual de la UVP.

## ⚠️ Sobre el problema de "no le aparece a otros lo programado"

Hasta ahora la sincronización con Firebase se guardaba **solo en tu
navegador** (a través de la pestaña Configuración, que además está oculta
para estudiantes). Por eso, aunque tú vieras todo sincronizado, los
estudiantes —que nunca pasan por esa pantalla— jamás se conectaban a
Firebase: sus dispositivos seguían trabajando en modo local, sin ver nada
de lo que tú programabas.

**La solución real**: las claves de una app web de Firebase no son
secretas (la seguridad vive en las "Reglas" de Firestore que ya
configuraste), así que ahora la configuración se incrusta directamente en
`data.js`, en la constante `FIREBASE_CONFIG`. Así, **todo el que abra el
link —estudiantes incluidos— se conecta automáticamente**, sin que nadie
tenga que entrar a ninguna pantalla de configuración.

**Qué tienes que hacer tú:**
1. Entra a tu plataforma → Acceso staff → Configuración → Sincronización
   en tiempo real, y copia los 6 valores que ya tenías guardados ahí
   (API Key, Auth Domain, Project ID, Storage Bucket, Messaging Sender ID,
   App ID). También los encuentras en la consola de Firebase, en
   **Configuración del proyecto → Tus apps**.
2. Pásamelos (puedes pegarlos tal cual o mandarme una captura) y yo los
   incrusto en `data.js` por ti.
3. Subes el `data.js` actualizado a GitHub reemplazando el actual.

A partir de ahí, cualquier estudiante que entre al link —sin hacer nada
especial— verá en tiempo real las actividades, sesiones y cupos que tú
programes, y el indicador de arriba a la derecha les aparecerá como
"● Sincronizado" a todos, no solo a ti.

## Programar un taller que se repite varias veces por semana

Ya no tienes que agregar una fecha a la vez. En cualquier tarjeta de
actividad, al dar clic en **"+ Agregar otra fecha"**, marca la casilla
**"Esta actividad se repite"** y aparecerán dos cosas:

- Los días de la semana en que se imparte (puedes marcar más de uno, por
  ejemplo Lunes y Miércoles si es dos veces por semana).
- Hasta qué fecha quieres programarla (por defecto te propone 60 días
  adelante, pero puedes cambiarlo).

Al guardar, la plataforma crea automáticamente **todas** las sesiones
correspondientes a esos días entre la fecha de inicio y la fecha final —
una por una, con la misma hora y cupo — sin que tengas que repetir el
proceso cada semana. Si necesitas ajustar o cancelar alguna fecha puntual
más adelante, puedes eliminarla individualmente desde la tabla de fechas
programadas de esa tarjeta, sin afectar a las demás.

## Compartir asistencia con los docentes de inglés (sin hacerlos staff)

Agregué un tercer nivel de acceso, **"Reporte docentes"**, que aparece
como pestaña visible para cualquiera (junto a "Registro"), pero pide su
propia contraseña — distinta a la tuya de staff — y es **de solo
lectura**: los docentes ven registros y asistencia de sus grupos, pero no
pueden editar actividades, tomar asistencia, ver sanciones ni entrar a
Configuración.

**Cómo funciona:**
1. La contraseña inicial para docentes es **`CAIdocentes`**. Cámbiala
   desde **Acceso staff → Configuración → Contraseña para docentes** (es
   una tarjeta separada de tu contraseña de staff).
2. Comparte con los docentes el mismo link de la plataforma + esa
   contraseña. Ellos entran, dan clic en "Reporte docentes", la escriben,
   y ven una tabla filtrable por su nombre (tal como lo escribieron en
   "Docente de inglés" al momento del registro de cada estudiante) con:
   actividad, fecha, estudiante, matrícula, carrera, grupo y si asistió
   o no, más un resumen de totales y % de asistencia.
3. Tú (staff) puedes entrar a esa misma pestaña sin necesidad de la
   contraseña de docentes, ya que tu sesión de staff ya te da acceso.

Como es de solo lectura y con su propia contraseña, los docentes nunca
tienen acceso a programar actividades, tomar asistencia ni cambiar nada —
esas herramientas siguen siendo exclusivas de tu acceso de staff.

## Qué cambió en esta versión

- **Colores institucionales** ahora en morado `#4E0D8C` (color exacto
  que nos diste).
- **Logo del CAI retirado** del encabezado (no se distinguía bien a
  tamaño pequeño); volví a un distintivo de texto "CAI" simple.
- **"+ Nueva actividad" ahora incluye fecha, hora y cupo** en el mismo
  formulario — ya no hace falta un paso aparte para programar la primera
  sesión. Quité los campos "Dirigido a" y "Objetivo" del formulario para
  simplificarlo.
- **Sincronización de Firebase incrustada en el código** (ver sección de
  arriba) para que funcione automáticamente para todos, no solo para
  quien configuró Firebase en su propio navegador.

## Qué cambió antes de esta versión

- **Pestañas de staff completamente ocultas** para cualquier persona que
  no haya iniciado sesión — un estudiante que entra al link solo ve la
  pestaña "Registro". El botón "Acceso staff" (arriba a la derecha) pide
  la contraseña y, solo entonces, aparecen el resto de las pestañas.
  Ten en cuenta que esto sigue siendo una protección básica pensada para
  que nadie entre "por curiosidad", no un sistema de autenticación de
  nivel empresarial.
- **"Actividades" y "Sesiones" ahora son una sola pestaña.** Cada tarjeta
  de actividad incluye, al final, sus fechas programadas (fecha, hora,
  cupo, registrados, asistieron, estado) con un botón "+ Agregar otra
  fecha" propio.
- **Nuevos tipos de actividad**: Taller, Club de conversación, Asesoría
  general, Sesión de práctica APTIS y Sesión de práctica TOEFL — ya
  precargué "Asesoría general de inglés", "Práctica de certificación
  APTIS" y "Práctica de certificación TOEFL" como actividades reales del
  catálogo (antes eran solo un aviso de texto); ahora también se
  programan por sesión y llevan asistencia igual que los talleres.

## ¿Qué hace la plataforma?

- **Registro de estudiantes**: matrícula, nombre, docente de inglés, grupo
  y **carrera detectada automáticamente** a partir de las 2 primeras letras
  de la matrícula (editable si no se detecta o si cambia la nomenclatura).
- **Catálogo de actividades editable**: título, tipo, a quién va dirigido,
  nivel, duración, ubicación y objetivo, con sus sesiones programadas —
  todo editable desde la plataforma, sin tocar código.
- **Asistencia por escaneo de QR**: en la pestaña "Asistencia" seleccionas
  la sesión del día y dejas el cursor en el campo de escaneo — tu lector
  de códigos funciona como teclado, así que al escanear la matrícula del
  estudiante se marca su asistencia automáticamente, sin comparar bases
  manualmente.
- **Sanción automática**: al cerrar una sesión, todo el que se registró
  pero no fue escaneado queda como "No asistió" y no puede volver a
  registrarse a esa misma actividad durante 7 días (configurable en
  `data.js`, constante `SANCTION_DAYS`).
- **Estadísticas**: totales generales, asistencia por actividad, asistencias
  por carrera, matriz carrera × actividad y sanciones activas.
- **Exportación**: Excel (varias hojas), CSV y respaldo/restauración en JSON.
- **Modo staff**: la pestaña "Registro" es la única visible públicamente;
  el resto solo aparece tras introducir la contraseña de staff.

## Cómo publicarla (GitHub Pages, gratis)

1. Crea un repositorio nuevo en GitHub (puede ser público o privado).
2. Sube estos archivos a la raíz: `index.html`, `styles.css`, `app.js`,
   `data.js` y la carpeta `assets/` completa (contiene el logo).
3. Ve a **Settings → Pages**, selecciona la rama `main` y carpeta `/root`.
4. En un par de minutos tu plataforma estará en
   `https://tuusuario.github.io/tu-repositorio/` — ese es el link que
   compartes con tus estudiantes para el registro.

También puedes subir los archivos a cualquier otro hosting estático
(Netlify, Vercel, un servidor de la universidad, etc.).

## Sincronizar varios dispositivos (necesario para uso real)

1. Ve a https://console.firebase.google.com/ y crea un proyecto nuevo.
2. Dentro del proyecto, activa **Firestore Database** (modo producción).
3. En **Configuración del proyecto → Tus apps**, crea una app web y copia
   los valores (`apiKey`, `authDomain`, `projectId`, etc.).
4. En reglas de Firestore, durante pruebas puedes usar temporalmente:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /cai/{doc} {
         allow read, write: if true;
       }
     }
   }
   ```
   (Para producción, restringe la escritura con autenticación si lo
   necesitas — pregúntame si quieres que lo configuremos así.)
5. Copia esos 6 valores y pásamelos (aquí en el chat) para que los incruste
   directamente en `data.js`, en la constante `FIREBASE_CONFIG`. Esa es la
   diferencia clave: así se conecta **cualquiera que abra el link**, sin
   que cada dispositivo tenga que configurarlo por su cuenta.
6. Sube el `data.js` actualizado a tu repositorio de GitHub.

A partir de ahí, cualquier registro o asistencia escaneada en un
dispositivo aparece al instante en los demás — para ti, para otro staff y
para cualquier estudiante — y el indicador de la esquina superior derecha
mostrará "● Sincronizado" en todos los dispositivos, no solo en el tuyo.

Nota: la pantalla de Configuración → Sincronización sigue estando
disponible por si en algún momento quieres cambiar de proyecto de Firebase
sin pedirme que edite el código — solo recuerda que lo que pongas ahí
únicamente aplica a tu propio navegador, no a los de tus estudiantes.

## Contraseña de staff

La contraseña inicial es **`CAI2026`**. Cámbiala en cuanto publiques la
plataforma desde **Acceso staff → Configuración → Contraseña de staff**.

## Editar la nomenclatura de carreras

Si la universidad actualiza los prefijos de matrícula por ciclo, ve a
**Configuración → Nomenclatura de matrícula → carrera** y agrega, edita o
elimina prefijos ahí mismo, sin tocar código.

## Estructura de archivos

```
index.html   → estructura de la página y las pestañas
styles.css   → todo el diseño (colores institucionales UVP, tipografía, layout)
app.js       → toda la lógica: registro, escaneo, sanciones, estadísticas,
               exportación, Firebase, control de acceso de staff
data.js      → datos iniciales: nomenclatura de carreras, tipos de
               actividad y catálogo de actividades tomado de la propuesta
               A26-E27
assets/      → logo del CAI
```

## Notas

- El lector de códigos de barras/QR que usan en el CAI funciona como
  teclado (escribe el texto y presiona Enter automáticamente), así que
  no requiere ninguna integración especial: solo mantén el cursor en el
  campo "Escanear matrícula" de la pestaña Asistencia.
- Si algún día quieres usar la cámara del celular como lector (en vez del
  lector físico), es una mejora que se puede agregar después.

