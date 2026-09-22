# Tests del temario

Aplicación web estática para estudiar el temario de la convocatoria **POE26-09/3395 - Operador/a de Entrada de Centros de Gestión de Incidencias y Operaciones**.

Incluye **1.178 preguntas** repartidas por los cinco bloques del temario, sesiones de 10 preguntas, selección adaptativa, repetición espaciada, seguimiento de errores y cobertura, y copia/restauración del progreso.

## Uso

No requiere instalación, backend ni proceso de compilación.

1. Descarga `index.html`.
2. Ábrelo con un navegador moderno (Chrome, Edge, Firefox o Safari).
3. Elige el modo y, si quieres, un bloque concreto.
4. Pulsa **Empezar test de 10**.

Toda la aplicación —HTML, CSS, JavaScript y banco de preguntas— está contenida en `index.html`.

## Publicación en GitHub Pages

1. Crear un repositorio en GitHub.
2. Subir `index.html` y `README.md`.
3. Ir a **Settings**.
4. Abrir **Pages**.
5. En **Build and deployment**, elegir **Deploy from a branch**.
6. Seleccionar la rama **main**.
7. Seleccionar **/(root)**.
8. Guardar.
9. Esperar a que GitHub publique el sitio.
10. Abrir la URL publicada.

No hay rutas absolutas ni dependencias externas, por lo que funciona tanto en una página de usuario como en una subruta de proyecto, por ejemplo `https://usuario.github.io/nombre-repositorio/`.

## Usuarios

La aplicación incluye exactamente dos perfiles locales independientes:

- **Pin**
- **Pon**

Cada perfil conserva por separado sus respuestas, estadísticas, repetición espaciada, configuración, progreso y copias exportadas. Cambiar de usuario no mezcla datos.

Si existe progreso de una versión anterior de la aplicación, se conserva y se migra automáticamente al perfil **Pin** la primera vez que se abre esta versión. El dato antiguo no se borra.

## Persistencia

El progreso se guarda automáticamente después de cada respuesta en `localStorage`, bajo una única clave versionada:

`renfe_tests_poe26_09_3395_progress`

El banco de preguntas **no** se almacena en `localStorage`; forma parte del código de la aplicación.

Ten en cuenta que `localStorage` pertenece al navegador, perfil y origen. Por tanto:

- Chrome y Edge no comparten automáticamente el progreso.
- Dos ordenadores no comparten automáticamente el progreso.
- La navegación privada puede eliminarlo.
- Borrar los datos del sitio puede eliminarlo.
- Cambiar de URL/origen puede crear un almacenamiento independiente.

### Exportar e importar

**Exportar progreso** descarga un JSON con estadísticas, conceptos vistos, preguntas, repetición espaciada, historial reciente y configuración.

**Importar progreso** valida antes de sustituir el estado actual:

- JSON válido.
- `schemaVersion`.
- `temarioId`.
- estructura mínima.

Un JSON inválido no modifica el progreso existente.

Este mecanismo permite estudiar en un ordenador, exportar, abrir la misma aplicación en otro y continuar allí.

## Progreso y probabilidad de aprobar

La pantalla muestra para cada usuario:

- **Cobertura**: porcentaje de conceptos del banco que se han trabajado al menos una vez.
- **Progreso global**: indicador que combina cobertura y dominio acumulado.
- **Precisión**: porcentaje histórico de respuestas correctas.
- **Prob. aprobar**: estimación heurística disponible a partir de 30 respuestas.

La probabilidad de aprobar **no es una predicción oficial del resultado del examen**. Se calcula internamente a partir de precisión ajustada, cobertura, dominio y volumen de respuestas, y sirve como indicador comparativo de preparación. No incorpora posibles reglas oficiales de penalización salvo que se programen expresamente en una futura versión.

La tabla **Pin y Pon** permite comparar ambos perfiles sin mezclar sus historiales.

## Sistema de aprendizaje

El modo **Adaptativo** prioriza:

1. conceptos todavía no vistos;
2. preguntas cuya revisión ya ha vencido;
3. conceptos con errores;
4. conceptos con menor nivel de dominio;
5. preguntas no usadas recientemente.

Tras un acierto aumenta el nivel de dominio y se amplía el intervalo de revisión. Tras un error baja el dominio y la pregunta/concepto vuelve a quedar disponible para repaso próximo.

Además existen:

- **Solo conceptos nuevos** (se detiene si ya no quedan suficientes conceptos nuevos)
- **Repaso de fallos y vencidas** (se limita a material pendiente de repaso)
- **Aleatorio**

## Actualizaciones

Constantes actuales:

- `APP_VERSION = "1.1.0"`
- `BANK_VERSION = "2026.09.22"`
- `SCHEMA_VERSION = 1`
- `TEMARIO_ID = "renfe-poe26-09-3395"`

Los `conceptId` y `questionId` son estables. Para una actualización del banco:

- conserva el ID de una pregunta corregida si sigue evaluando exactamente el mismo concepto;
- asigna IDs nuevos a preguntas o conceptos nuevos;
- no renumeres IDs existentes;
- no cambies `TEMARIO_ID` si sigue siendo el mismo temario;
- incrementa `BANK_VERSION` cuando cambie el banco;
- incrementa `APP_VERSION` cuando cambie la aplicación;
- cambia `SCHEMA_VERSION` solo si cambia la estructura del estado y se implementa una migración.

Al sustituir `index.html` en GitHub, el `localStorage` del mismo origen se conserva. Los IDs ya conocidos mantienen su progreso y los nuevos empiezan sin historial.

## Reinicio

**Reiniciar progreso** requiere dos confirmaciones, incluida la escritura explícita de `REINICIAR`. Solo borra el estado de aprendizaje; no modifica el banco de preguntas.

## Privacidad

La aplicación no incluye Google Analytics, trackers, cookies publicitarias, telemetría ni peticiones a servicios externos.

**Todo el progreso permanece exclusivamente en el navegador mediante `localStorage`; GitHub no recibe esos datos.**

## Funcionamiento offline

La copia descargada de `index.html` puede abrirse directamente mediante `file://`.

La versión de GitHub Pages necesita conexión para cargar inicialmente el archivo publicado. No se utiliza Service Worker ni PWA para evitar problemas de caché al actualizar el banco.
