# Test_FluidosG2
Universidad de América · Facultad de Ingeniería · Mecánica de Fluidos
Profesor: Ph.D. Juan Sandoval Herrera

Cuestionario de conceptos en un único archivo HTML (sin dependencias, sin servidor, sin base de datos) que cubre tres bloques del curso: bombas hidráulicas, sistemas de tuberías en serie y selección y operación de bombas centrífugas.

Archivo principal: 
cuestionario_Fluidos_2.html
Banco de preguntas: 50 preguntas de opción múltiple, nivel intermedio
Versión anterior: 
cuestionario_Fluidos_1.html
 (20 preguntas)
1. Temario evaluado
#	Tema (topic)	Preguntas	Fuente
1	Tuberías en serie	11 (id 1–11)	Sesión web "Sistemas de tuberías en serie" (problemas clase I, II y III)
2	Bombas hidráulicas	9 (id 12–20)	Presentación teórica de selección de bombas
3	Curvas del fabricante	5 (id 21–25)	Selección y operación de bombas centrífugas (págs. 2–6)
4	Curvas del sistema	5 (id 26–30)	Ídem (págs. 7–10)
5	Punto de operación	5 (id 31–35)	Ídem (págs. 11–13, 24–25)
6	Control de capacidad	5 (id 36–40)	Ídem (págs. 14–16, 23)
7	Efectos del fluido	5 (id 41–45)	Ídem (págs. 17–18, 23, 26)
8	Arreglos múltiples	5 (id 46–50)	Ídem (págs. 19–21)
Contenido conceptual cubierto: curva característica H–Q (parabólica descendente), familias de curvas y recorte de impulsor, curva de resistencia del sistema SRC (carga estática + carga dinámica ∝ Q²), ecuación de carga total, intersección oferta–demanda (punto de operación real), BEP y selección del motor, control por válvula vs. variador de frecuencia (leyes de afinidad, P ∝ N³), corrección por viscosidad y gravedad específica, y arreglos en paralelo (suma de caudales) y en serie (suma de alturas).

Solo dos preguntas requieren cálculo aritmético y ambas muestran el procedimiento en la explicación: la intersección Q = 10 / H = 65 m y el efecto de la G.E. 1,0 → 1,2 sobre la BHP.

2. Funcionalidad
Función	Descripción
Pistas	Botón "💡 Mostrar pista" antes de responder, con una ayuda conceptual por pregunta.
Retroalimentación inmediata	Al elegir una opción se marca la correcta y la elegida, con explicación del concepto.
Cronómetro	Contabiliza el tiempo del intento y lo reporta en el acta.
Aleatorización	Cada intento reordena preguntas y opciones (shuffle tipo Fisher–Yates).
Acta de resultados	Puntaje, porcentaje, calificación A–F, tiempo, fecha y repaso pregunta por pregunta.
Descarga .txt	Acta en texto plano, con símbolos matemáticos normalizados para lectura en consola.
Impresión	Hoja de estilos @media print que imprime solo el acta (sin botones ni navegación).
Responsive	Adaptación a móvil (< 600 px): una columna, botones a ancho completo.
Escala de calificación: A ≥ 90 % · B ≥ 80 % · C ≥ 70 % (aprobado) · D ≥ 60 % · F < 60 %.

3. Uso
Abra 
cuestionario_Fluidos_2.html
 con doble clic en cualquier navegador moderno (Chrome, Edge, Firefox, Safari). No requiere internet ni instalación.
(Opcional) Escriba el nombre del estudiante.
Pulse ▶ Comenzar intento y responda las 50 preguntas.
Al finalizar, descargue el acta (.txt) o imprímala en PDF.
🔄 Nuevo intento (aleatorio) genera un examen con otro orden de preguntas y opciones.
Nota: el estado no se guarda entre sesiones; si recarga la página, el intento se reinicia.

4. Publicación en GitHub Pages
Bash

git init
git add cuestionario_Fluidos_2.html README.md
git commit -m "Cuestionario interactivo Mecánica de Fluidos 2026 (50 preguntas)"
git branch -M main
git remote add origin https://github.com/<usuario>/<repositorio>.git
git push -u origin main
Luego, en el repositorio: Settings → Pages → Source: Deploy from a branch → Branch: main / (root) → Save.

La página queda disponible en https://<usuario>.github.io/<repositorio>/cuestionario_Fluidos_2.html.

Sugerencia: renómbrelo a index.html si quiere que abra directamente en la raíz del sitio.

5. Estructura del archivo
text

cuestionario_Fluidos_2.html
├── <head>
│   ├── <style>  ......... hoja de estilos completa (variables CSS, tarjetas, opciones,
│   │                      feedback, acta, @media print, @media max-width 600px)
│   └── <link>   ......... fuente Inter desde Google Fonts (degradación elegante a
│                          system-ui si no hay red)
├── <body>
│   ├── <header>  ........ cabecera institucional (navy + franja dorada)
│   ├── #start-screen .... portada, nombre del estudiante, instrucciones
│   ├── #quiz-screen ..... progreso, badge de tema, cronómetro, pregunta, pistas,
│   │                      opciones, retroalimentación, botón siguiente
│   ├── #results-screen .. puntaje, calificación, métricas, repaso, acciones
│   └── <footer> ......... créditos y fuentes
└── <script> (IIFE)
    ├── const BANK[50] ... banco de preguntas
    ├── const state ...... estado del intento (nombre, orden, respuestas, tiempo)
    ├── renderQuestion() . pinta la pregunta y sus opciones aleatorizadas
    ├── selectAnswer() ... evalúa, marca clases .correct/.wrong y muestra feedback
    ├── showResults() .... calcula puntaje, calificación y construye el repaso
    ├── downloadActa() ... genera y descarga el acta en .txt
    └── startQuiz() / newAttempt()
Esquema de una pregunta
JavaScript

{
  id: 34,                              // consecutivo único
  topic: "Punto de operación",         // aparece en el badge y en el repaso
  q: "Operar la bomba lejos del BEP...",// enunciado
  options: [                           // exactamente 4 opciones
    "Reduce la vida útil...",
    "Es indiferente...",
    "Aumenta la eficiencia...",
    "Elimina por completo..."
  ],
  correct: 0,                          // índice 0–3 de la respuesta correcta
  hint: "Fuera del rango preferido...",// pista opcional
  explanation: "El punto de operación..." // retroalimentación y repaso
}
Al iniciar cada intento, el script convierte options + correct en objetos {text, correct} y los baraja, de modo que el índice correcto cambia de posición en cada intento; por eso todas las preguntas del banco se escriben con correct: 0 y no hay que preocuparse por el balance de posiciones.

6. Cómo ampliar el cuestionario
Abra el archivo en un editor y ubique const BANK = [ (cerca de la línea 319).
Agregue un objeto nuevo al final de la lista, antes de ];, con id único y consecutivo.
Mantenga exactamente 4 opciones y los cuatro campos de texto (q, hint, explanation, topic).
Si desea un tema nuevo, use un topic distinto: el badge y el repaso lo toman automáticamente.
Nada más que cambiar: el contador "Pregunta X de N", la barra de progreso, el puntaje y el acta se recalculan solos.
Verificación rápida del banco (Node.js):

Bash

node -e "
const fs=require('fs');
const s=fs.readFileSync('cuestionario_Fluidos_2.html','utf8');
const bank=s.slice(s.indexOf('const BANK = ['), s.indexOf('const state = {')).replace('const BANK = ','module.exports = ');
fs.writeFileSync('/tmp/bank.js', bank);
const B=require('/tmp/bank.js');
const ids=new Set(B.map(q=>q.id));
console.log('preguntas:', B.length, '| ids únicos:', ids.size===B.length);
console.log('mal formadas:', B.filter(q=>q.options.length!==4||new Set(q.options).size!==4||!q.q||!q.hint||!q.explanation||q.correct<0||q.correct>3).map(q=>q.id));
"
7. Personalización visual
Los colores y la tipografía están centralizados en variables CSS dentro de :root:

CSS

:root{
  --navy:#0e2a3f;        /* color institucional principal */
  --navy-dark:#091d2b;   /* cabecera y pie */
  --gold:#c9a227;        /* franja dorada y acentos */
  --bg:#f2f6fa;          /* fondo */
  --ok:#1e7a46;          /* respuesta correcta */
  --bad:#b3261e;         /* respuesta incorrecta */
  --radius:14px;
  --font:'Inter',system-ui,...;
}
Cambiar --navy y --gold re-ambienta todo el cuestionario a otra facultad o curso sin tocar el resto de la hoja de estilos. Los cortes de calificación están en showResults() (línea ~529) y en downloadActa() (línea ~621).

8. Referencias del curso
Mott, R. (2006). Mecánica de Fluidos (cap. 11 y 13). Pearson Educación.
Goulds Water Technology. Introducción a las curvas de bombas.
Crane Co. TP-410: Flujo de fluidos en válvulas, accesorios y tuberías (pérdidas locales, Le/D).
Hydraulic Institute. ANSI/HI 9.6.7 — Effects of Liquid Viscosity on Rotodynamic Pump Performance (corrección por viscosidad).
Material de clase: Selección y operación de bombas centrífugas (Prof. J. Sandoval Herrera, 2026).
9. Estado del proyecto
Elemento	Estado
Banco de 50 preguntas (8 temas)	✅ Validado (4 opciones, 1 correcta, sin duplicados)
Flujo completo de 50 preguntas en navegador	✅ Probado de extremo a extremo
Retroalimentación, pistas y cronómetro	✅ Funcionando
Acta .txt e impresión	✅ Funcionando (50 líneas de detalle)
Errores de JavaScript en consola	✅ Ninguno
Corrección aplicada v2	El puntaje final ya no duplica el signo % (mostraba "20%%")
Última actualización: 5 de noviembre de 2026.
