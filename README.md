# RENFE OPE 2026 · Entrenador adaptativo

Aplicación web estática para estudiar el temario de la convocatoria **POE26-09/3395 — Operador/a de Entrada de Centros de Gestión de Incidencias y Operaciones**.

## Versión 2.0

Esta versión sustituye el antiguo banco basado en frases para completar por un banco orientado a comprobar comprensión real.

El banco contiene **1.169 preguntas** y no incluye preguntas del tipo «Completa correctamente...».

Las preguntas se organizan en tres tipos:

- **Aplicación**: casos, situaciones y consecuencias de una regla o concepto.
- **Comprensión**: discriminación entre conceptos, definiciones o interpretaciones próximas.
- **Memoria exacta**: cifras, fechas, límites y otros datos que sí conviene memorizar literalmente.

Cuando se estudia **Todo el temario**, la aplicación alterna sesiones con estas composiciones:

- 6 Aplicación + 3 Comprensión + 1 Memoria exacta.
- 6 Aplicación + 2 Comprensión + 2 Memoria exacta.

A largo plazo equivale aproximadamente a **60 % aplicación, 25 % comprensión y 15 % memoria exacta**.

Además, se han incorporado preguntas especialmente diseñadas para conceptos importantes: tiempos de conducción, responsabilidades en seguridad, discriminación indirecta, corresponsabilidad, transversalidad, mejora continua, Cultura de Seguridad, Atención al Cliente frente a Experiencia de Cliente, NPS, material remolcado, frenado, detección de trenes y señalización, entre otros.

## Tests de 10 preguntas y tiempo máximo

Cada test contiene exactamente **10 preguntas** y dispone de un máximo global de:

**5 minutos y 30 segundos**

El cronómetro:

- comienza al iniciar el test;
- no se pausa al cambiar de pregunta;
- sigue computando aunque la pestaña quede temporalmente en segundo plano;
- finaliza automáticamente el test al llegar a 00:00.

Para que el tiempo tenga sentido como simulación de examen, la aplicación **no revela si una respuesta es correcta o incorrecta durante el test**. La corrección, la respuesta correcta y la explicación se muestran al finalizar.

Si se agota el tiempo, las preguntas no respondidas cuentan como no acertadas en la puntuación de la sesión, pero no se registran como un fallo de aprendizaje del concepto.

## Usuarios

Hay exactamente dos perfiles locales e independientes:

- **Pin**
- **Pon**

Cada usuario mantiene por separado:

- preguntas respondidas;
- aciertos y fallos;
- nivel de dominio por concepto;
- repetición espaciada;
- historial reciente;
- sesiones cronometradas;
- progreso;
- estimación de probabilidad de aprobar;
- configuración y copias exportadas.

## Progreso y probabilidad de aprobar

La pantalla muestra:

- **Cobertura**: conceptos trabajados al menos una vez.
- **Precisión**: porcentaje histórico de respuestas correctas.
- **Progreso global**: combinación de cobertura y dominio acumulado.
- **Revisiones pendientes**.
- **Prob. aprobar**: estimación orientativa.

La estimación comienza después de 30 respuestas. Cuando ya existen varias sesiones cronometradas, también incorpora el rendimiento obtenido en tests de 10 preguntas bajo el límite de 5:30.

No es una predicción oficial del resultado del examen.

## Persistencia

La aplicación utiliza únicamente `localStorage`.

Claves principales de esta versión:

- `renfe_tests_poe26_09_3395_progress_v2_pin`
- `renfe_tests_poe26_09_3395_progress_v2_pon`

El banco de preguntas forma parte de `index.html` y no se guarda en `localStorage`.

### Importante: progreso de la versión anterior

La versión 2 cambia de forma sustancial lo que mide cada pregunta. Por esa razón, **el progreso de la versión 1 no se migra automáticamente**.

Esto evita que los aciertos obtenidos con preguntas de completar inflen artificialmente el dominio y la probabilidad de aprobar del nuevo banco.

Los datos antiguos no se borran: simplemente se utilizan claves nuevas para esta versión.

## Exportar e importar

**Exportar progreso** descarga un JSON independiente para Pin o Pon.

**Importar progreso** valida:

- JSON válido;
- `schemaVersion`;
- `temarioId`;
- estructura mínima.

Un fichero incompatible no sustituye el progreso actual.

## Publicación en GitHub Pages

El paquete no necesita backend, npm, build ni librerías externas.

Sube a la raíz del repositorio:

- `index.html`
- `README.md`
- `CNAME`

Después:

1. GitHub → **Settings → Pages**.
2. Source: **Deploy from a branch**.
3. Branch: **main**.
4. Folder: **/(root)**.
5. En **Custom domain**, escribe `renfe-test.ramiro-rego.com`.
6. Cuando GitHub termine la comprobación DNS y genere el certificado, activa **Enforce HTTPS**.

## DNS en GoDaddy

El registro para el subdominio debe ser:

| Tipo | Nombre | Datos |
|---|---|---|
| CNAME | `renfe-test` | `foreswearer.github.io.` |

No añadas `/renfe-test` al destino del CNAME.

El archivo `CNAME` incluido en este paquete contiene exactamente:

`renfe-test.ramiro-rego.com`

## Privacidad

No hay backend, telemetría, Google Analytics, cookies publicitarias ni peticiones a servicios externos.

El progreso permanece en el navegador del usuario.

## Versiones

- `APP_VERSION = 2.0.0`
- `BANK_VERSION = 2026.09.22-comprension`
- `SCHEMA_VERSION = 2`
- `TEMARIO_ID = renfe-poe26-09-3395`
