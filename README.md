# Noticing Triádico Reflexivo — PUCV

Aplicativo web para el registro y análisis colaborativo del noticing reflexivo en tríadas formativas durante la práctica docente final en pedagogías en ciencias naturales.

Desarrollado en el marco del proyecto **"Fortalecimiento de las triadas formativas en ciencias naturales: análisis del Noticing reflexivo en práctica final"** (Proyectos en Docencia Universitaria 2026, PUCV), dirigido por la profesora Roxana Jara Campos, Instituto de Química, Pontificia Universidad Católica de Valparaíso.

---

## ¿Qué hace este aplicativo?

Permite que los tres actores de una tríada formativa —estudiante en formación, tutor universitario y profesor mentor— respondan simultáneamente un cuestionario de noticing reflexivo basado en la bitácora del estudiante, vean las respuestas de todos en tiempo real, y reciban un mapa de trayectoria reflexiva generado por IA que sirve como objeto de reflexión colaborativa en el encuentro triádico.

---

## Base conceptual

El instrumento articula dos marcos teóricos:

**Noticing docente** (Jacobs, Lamb & Philipp, 2010): conjunto de habilidades interrelacionadas que incluye (a) atender a estrategias pedagógicamente relevantes, (b) interpretar su significado desde el conocimiento disciplinar y pedagógico, y (c) decidir cómo responder en base a lo comprendido.

**Modelo ALACT** (Korthagen, 2001): ciclo de reflexión en cinco fases — Acción, Looking Back, Awareness, Alternatives, Trial — que describe la trayectoria reflexiva del docente en formación desde la descripción de lo ocurrido hasta la proyección hacia la acción futura.

El cruce de ambos marcos permite mapear no solo *qué* notaron los actores, sino *desde qué nivel de profundidad reflexiva* lo hicieron, y cómo esa profundidad varía según el tipo de pregunta y el rol en la tríada.

---

## Estructura del cuestionario

Siete preguntas organizadas en tres dimensiones de noticing × tres tipos de pregunta:

| Tipo | Código | Función | Fase ALACT esperada |
|------|--------|---------|---------------------|
| Lectura de bitácora | L | Ancla en la evidencia textual del episodio | Acción / Looking Back |
| Reflexiva | R | Activa el "noticing del noticing" desde cada rol | Awareness / Alternatives |
| Dialógica | D | Formula una pregunta hacia otro actor de la tríada | Alternatives / Trial |

Las preguntas L son idénticas para los tres actores. Las preguntas R y D también son iguales en formulación, pero cada actor las responde desde su posición diferencial en la tríada — esa divergencia es el dato analítico central del estudio.

Las preguntas D no se responden dentro de la app: quedan registradas como agenda de la discusión triádica cuando se proyecta el mapa generado por IA.

---

## Análisis con IA

La IA se llama a través de un Cloudflare Worker (`cloudflare_worker.js`) que oculta la API key y usa OpenRouter (modelo por defecto: `poolside/laguna-s-2.1:free`). La pestaña **Mapa IA** genera, en dos etapas:

1. **Análisis de noticing**: cada respuesta se ubica en ALACT × Noticing × práctica científica (indagación, modelización, argumentación, NdC), con profundidad reflexiva, movimiento entre dimensiones, convergencias, divergencias, nudos reflexivos, patrón ALACT, resumen del encuentro y perfil por rol.
2. **Borrador del informe** en el formato institucional "Reunión de Triada Formativa".

Visualizaciones: mapa de trayectorias por rol (rol × Atender/Interpretar/Decidir en columnas; fase ALACT × práctica en filas; flechas con el orden de respuesta), trayectoria ALACT a lo largo de las 7 preguntas y ciclo de noticing con el nivel ALACT medio por rol.

Una barra de estado sincronizada muestra en todos los dispositivos el modelo empleado, la etapa en curso, el tiempo transcurrido y los tokens usados.

El informe es editable (las ediciones se sincronizan), se exporta a Word (.docx, con el mapa como figura y las respuestas como anexo) o se imprime a PDF.

---

## Tecnología

- HTML / CSS / JavaScript — archivo único sin build
- Firebase Realtime Database — sincronización en tiempo real
- Cloudflare Worker + OpenRouter — análisis con IA
- docx.js (cargado solo al exportar) — informe en Word
- Desplegable en GitHub Pages

---

## Configuración

1. Clonar o descargar este repositorio
2. En `index.html`, localizar el bloque `firebaseConfig` y reemplazar los valores con los de tu proyecto Firebase:

```javascript
const firebaseConfig = {
  apiKey: "TU_API_KEY",
  authDomain: "TU_PROYECTO.firebaseapp.com",
  databaseURL: "https://TU_PROYECTO-default-rtdb.firebaseio.com",
  projectId: "TU_PROYECTO",
  storageBucket: "TU_PROYECTO.appspot.com",
  messagingSenderId: "TU_SENDER_ID",
  appId: "TU_APP_ID"
};
```

3. Crear una base de datos Realtime en Firebase Console en modo de prueba
4. Subir `index.html` al repositorio y activar GitHub Pages desde Settings → Pages

---

## Uso en sesión triádica

1. El facilitador genera un código de sesión y lo comparte con los tres actores
2. Cada actor abre la URL, selecciona su rol e ingresa el código
3. El estudiante relata verbalmente el episodio pedagógico de su bitácora
4. Los tres actores responden las 7 preguntas de forma simultánea e independiente
5. Al terminar, el facilitador abre la pestaña **Mapa IA** y proyecta el resultado
6. La tríada discute el mapa: convergencias, divergencias y nudos reflexivos

---

## Estructura de datos en Firebase

La raíz depende del período académico que el tutor elige al crear la sesión: 2° semestre 2026 → `sessions_v2`; otros períodos → `sessions_{año}_s{semestre}` (p. ej. `sessions_2027_s1`). Un índice permite que los integrantes entren solo con el código.

```
session_index/
  └── {CODIGO}: { root, anio, semestre, createdAt }

sessions_v2/
  └── {CODIGO}/
        ├── meta/              ← datos de la reunión, período, estado (abierta/cerrada), bitácora tipo
        ├── presence/          ← roles conectados
        ├── answers/{rol}/{pregunta}
        ├── ai_status/         ← estado de la IA en tiempo real
        ├── map/               ← análisis IA (+ modelo, tokens, tiempo)
        ├── report/            ← borrador IA del informe
        └── report_edits/      ← ediciones del tutor
```

Si las reglas de Firebase no están en modo de prueba, deben permitir lectura y escritura en `session_index` y en `sessions_v2`.

---

## Referencias

- Jacobs, V. R., Lamb, L. L. C., & Philipp, R. A. (2010). Professional noticing of children's mathematical thinking. *Journal for Research in Mathematics Education, 41*(2), 169–202.
- Korthagen, F. A. J. (2001). *Linking practice and theory: The pedagogy of realistic teacher education*. Lawrence Erlbaum.
- Van Es, E. A., & Sherin, M. G. (2021). Expanding on prior conceptualizations of teacher noticing. *ZDM: Mathematics Education, 53*(1), 17–27.
- Männikkö, I., & Husu, J. (2019). Examining teachers' adaptive expertise through personal practical theories. *Teaching and Teacher Education, 77*, 126–137.

---

## Equipo

Proyecto en Docencia Universitaria 2026 — Pontificia Universidad Católica de Valparaíso  
Instituto de Química · Instituto de Biología  
Investigadora Responsable: Roxana Jara Campos

---

## Licencia

Uso académico y formativo. Para adaptaciones o reutilización en otros contextos de formación docente, contactar a roxana.jara@pucv.cl
