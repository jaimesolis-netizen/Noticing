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

Nueve preguntas organizadas en tres dimensiones de noticing × tres tipos de pregunta:

| Tipo | Código | Función | Fase ALACT esperada |
|------|--------|---------|---------------------|
| Lectura de bitácora | L | Ancla en la evidencia textual del episodio | Acción / Looking Back |
| Reflexiva | R | Activa el "noticing del noticing" desde cada rol | Awareness / Alternatives |
| Dialógica | D | Formula una pregunta hacia otro actor de la tríada | Alternatives / Trial |

Las preguntas L son idénticas para los tres actores. Las preguntas R y D también son iguales en formulación, pero cada actor las responde desde su posición diferencial en la tríada — esa divergencia es el dato analítico central del estudio.

Las preguntas D no se responden dentro de la app: quedan registradas como agenda de la discusión triádica cuando se proyecta el mapa generado por IA.

---

## Análisis con IA

Al completar las respuestas, la pestaña **Mapa IA** llama a Claude Sonnet (Anthropic) y genera automáticamente:

- **Mapa de trayectoria triádica**: cada respuesta ubicada en el cruce ALACT × Noticing, etiquetada por rol y tipo de pregunta
- **Práctica científica inferida**: la IA clasifica el episodio como indagación, modelización, argumentación o naturaleza de la ciencia (NoS) desde el contenido de las respuestas
- **Profundidad reflexiva**: cada respuesta clasificada como descriptiva, analítica o crítica, agrupada por tipo de pregunta (L / R / D)
- **Patrón de trayectoria**: descripción del movimiento reflexivo de la tríada en conjunto
- **Convergencias y divergencias** entre los tres actores
- **Nudos reflexivos**: tensiones identificadas por la IA que estructuran la agenda de la discusión triádica en vivo

El mapa generado se guarda en Firebase y puede exportarse como JSON para análisis posterior en ATLAS.ti.

---

## Tecnología

- HTML / CSS / JavaScript — archivo único sin dependencias de build
- [Firebase Realtime Database](https://firebase.google.com/) — sincronización en tiempo real entre dispositivos
- [Anthropic API](https://www.anthropic.com/) — análisis y mapeo con Claude Sonnet
- Desplegable en GitHub Pages sin servidor

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
4. Los tres actores responden las 9 preguntas de forma simultánea e independiente
5. Al terminar, el facilitador abre la pestaña **Mapa IA** y proyecta el resultado
6. La tríada discute el mapa: convergencias, divergencias y nudos reflexivos

---

## Estructura de datos en Firebase

```
sessions/
  └── {CODIGO-SESION}/
        ├── presence/          ← roles conectados
        ├── answers/
        │     ├── student/     ← respuestas por pregunta (att-L, att-R, ... dec-D)
        │     ├── tutor/
        │     └── mentor/
        └── map/               ← resultado del análisis IA
```

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
