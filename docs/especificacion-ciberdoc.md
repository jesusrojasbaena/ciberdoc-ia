# Especificación · Plataforma web CiberDoc IA

**Versión:** 1.0 · 7 de julio de 2026
**Autor:** Jesús Rojas Baena (con planificación asistida en Claude)
**Destino:** documento de referencia para ejecutar en Claude Code (Plan mode). Guardar como `docs/especificacion-ciberdoc.md` en el repositorio.

---

## 1. Qué es este proyecto

Web pública de la marca **CiberDoc IA** con dos funciones integradas:

1. **Landing comercial** que presenta el producto y capta contactos B2B (academias, centros de formación, docentes, pymes).
2. **Plataforma de curso** que muestra el catálogo y una demo navegable del curso real, generada automáticamente a partir de los documentos Word originales mediante un pipeline de conversión.

No es un LMS completo en su Fase 1: no hay registro de alumnos, ni pagos, ni backend. Es una web estática desplegada en GitHub Pages, diseñada para poder crecer hacia esas funciones sin rehacer nada (ver §10, Fases).

## 2. Posicionamiento (fuente de verdad: LEEME_CIBERDOC_IA.docx)

**CiberDoc IA es una consultoría que crea cursos de ciberseguridad informática desde cero, usando inteligencia artificial como apoyo educativo.**

- Producto insignia y demo comercial: curso **"Ciberseguridad básica con IA para usuarios y pequeñas empresas"** — 30 horas, nivel iniciación, 8 módulos, con fichas, actividades, tests, solucionarios, resúmenes, glosarios, proyecto final, guía docente y prompts IA por módulo.
- Cliente objetivo: academias de informática, centros de formación, docentes y pymes que quieren impartir formación en ciberseguridad sin crear el material ellos mismos.
- Enfoque declarado del contenido: práctico, progresivo, ético, legal y defensivo.
- Eslogan (reutilizado de los textos de marca): **"Aprende IA. Aplícala de verdad."** puede adaptarse; propuesta alineada con el posicionamiento real: **"Cursos de ciberseguridad listos para impartir."**

**Decisión cerrada:** los cinco cursos de IA de los documentos de landing (IA desde Cero, Prompt Engineering, etc.) **quedan fuera de la web** hasta que existan como producto. No se anuncian ni como "próximamente". La web solo vende lo que existe.

**Material aprovechable de los documentos previos** (`Prompt Maestro Oficial`, `Textos Web`): paleta de colores, estructura visual de la landing, tono de los textos ("sin humo tecnológico"), método en 5 pasos, campos del formulario. Los textos de secciones se reescribirán adaptados al posicionamiento de consultoría de ciberseguridad.

## 3. Arquitectura del sitio

Web estática multipágina (HTML5 + CSS3 + JS vanilla, sin frameworks). Estructura de páginas:

```
/                          → Landing comercial (index.html)
/cursos/                   → Catálogo (de momento, 1 curso)
/cursos/ciberseguridad-basica/          → Página del curso (ficha comercial + índice de módulos)
/cursos/ciberseguridad-basica/modulo-1/ → Demo pública: lecciones del módulo 1
    lección de contenido · ficha del alumno · actividad · resumen · glosario · test interactivo
/contacto/                 → Formulario (también embebido en la landing)
/aviso-legal/ /privacidad/ → Páginas legales (obligatorias con formulario, RGPD)
```

### 3.1 Landing (index.html)

Secciones, adaptando la estructura de los documentos previos al posicionamiento real:

1. **Header** sticky: logo, menú (Inicio · El curso · Demo · Método · Contacto), CTA "Solicitar información".
2. **Hero:** titular sobre el producto real. Propuesta: "Cursos de ciberseguridad listos para impartir, creados con apoyo de IA". Subtítulo con el cliente objetivo. CTA primario "Ver la demo del curso", secundario "Solicitar información". El hero debe mostrar algo real, no una ilustración abstracta: una vista previa del propio material del curso (tarjeta de módulo, fragmento de test) como elemento visual.
3. **Qué incluye el producto:** 8 módulos, y por cada módulo: contenido, ficha del alumno, actividad con solucionario, test con solucionario, resumen, glosario; más proyecto final con plantilla y rúbrica, guía docente y prompts IA. Estos números son el argumento de venta: mostrarlos como datos concretos, no como adjetivos.
4. **Para quién:** academias, centros de formación, docentes, pymes. Qué problema les resuelve (material listo, actualizable, con apoyo IA).
5. **Demo navegable:** bloque destacado que enlaza al módulo 1 completo online. Este es el diferenciador de la web: la academia puede *ver* el producto antes de contactar.
6. **Método CiberDoc IA** (5 pasos, del material existente): Entender · Aprender · Practicar · Crear · Aplicar.
7. **Sobre CiberDoc IA:** presentación breve de Jesús y la marca. Fondo real (electrónica, comunicación, formación, desarrollo web con IA) — genera más confianza B2B que un texto corporativo anónimo.
8. **CTA final + formulario de contacto** (§7).
9. **Footer:** logo, eslogan, menú breve, enlaces legales, año generado con JS.

### 3.2 Página del curso

- Ficha comercial: título, duración (30 h), nivel, público objetivo, índice completo de los 8 módulos con sus títulos reales.
- Módulo 1 enlazado como demo navegable. Módulos 2–8 listados con título y descripción pero **sin contenido público** — marcados como "incluido en el curso completo", con CTA a contacto.
- Bloque "qué recibe la academia": desglose del material entregable (incluida la guía docente y los solucionarios, que se mencionan pero nunca se publican).

### 3.3 Visor de lección (demo módulo 1)

- Layout de dos columnas en escritorio: navegación lateral del módulo (lecciones y materiales) + contenido. En móvil, navegación colapsable.
- Navegación anterior/siguiente al pie de cada lección.
- Tipos de página dentro del módulo: contenido, ficha del alumno, actividad, resumen, glosario y test interactivo.
- **No se publican:** solucionario de actividad, solucionario de test, guía docente, prompts IA. El test interactivo incorpora la corrección en JS (§5.3) sin exponer el documento de solucionario.

## 4. Modelo de datos

Todo el contenido del curso vive como datos generados, no como HTML escrito a mano. Un único fichero `data/curso-ciberseguridad.json` generado por el pipeline (§5), con esta forma:

```json
{
  "slug": "ciberseguridad-basica",
  "titulo": "Ciberseguridad básica con IA para usuarios y pequeñas empresas",
  "duracion_horas": 30,
  "nivel": "Iniciación",
  "modulos": [
    {
      "numero": 1,
      "slug": "introduccion-ciberseguridad",
      "titulo": "Introducción a la ciberseguridad",
      "duracion_horas": 3,
      "publico": true,
      "lecciones": [
        { "tipo": "contenido",  "titulo": "...", "html": "..." },
        { "tipo": "ficha",      "titulo": "...", "html": "..." },
        { "tipo": "actividad",  "titulo": "...", "html": "..." },
        { "tipo": "resumen",    "titulo": "...", "html": "..." },
        { "tipo": "glosario",   "titulo": "...", "html": "..." }
      ],
      "test": {
        "titulo": "Test · Módulo 1",
        "preguntas": [
          {
            "enunciado": "...",
            "opciones": ["a) ...", "b) ...", "c) ..."],
            "correcta": 1
          }
        ]
      }
    }
  ]
}
```

Reglas:

- Solo los módulos con `"publico": true` generan páginas. En Fase 1, únicamente el módulo 1.
- Las respuestas correctas del test del módulo 1 sí van en el JSON (necesarias para la autocorrección). **Los tests de los módulos 2–8 no se incluyen en el JSON publicado** — el pipeline los excluye por completo, no basta con ocultarlos en la interfaz, porque el JSON es legible por cualquiera.
- Los documentos privados (solucionarios, guía docente, prompts) **nunca entran en el repositorio público**. GitHub Pages sirve todo lo que hay en el repo; la privacidad se garantiza en origen: la carpeta fuente del curso vive fuera del repo y el pipeline solo copia lo publicable.

## 5. Pipeline de conversión (Word → web)

Script Node.js (`scripts/convertir-curso.mjs`) ejecutado localmente. No forma parte de la web desplegada; es la herramienta de mantenimiento. Reutilizable para futuros cursos.

### 5.1 Entrada

La carpeta original del curso (`07_Entrega_Final/03_Material_Didactico/...`), fuera del repositorio. La ruta se pasa por parámetro.

### 5.2 Restricción conocida y estrategia

**Los .docx no usan estilos de Word** (auditado: todos los párrafos en estilo "Normal", sin encabezados, sin tablas, sin imágenes). Una conversión genérica no produce jerarquía. La estrategia es un **parser por reglas** apoyado en dos regularidades verificadas del corpus:

1. **La nomenclatura de archivos es sistemática** y determina el tipo de documento: `Modulo_N_*.docx` (contenido), `Ficha_Alumno_*`, `Actividad_*`, `Solucionario_Actividad_*`, `Test_*`, `Solucionario_Test_*`, `Resumen_*`, `Glosario_*`. El nombre de la carpeta `Modulo_N_<Tema>` da número y tema.
2. **Los patrones internos son constantes:** líneas-etiqueta terminadas en dos puntos ("Objetivo del módulo:", "Contenidos:", "Explicación sencilla:", "Actividad práctica:", "Instrucciones:"), títulos en mayúsculas ("MÓDULO 1: ..."), y en los tests el patrón pregunta → opciones `a)/b)/c)` → línea "Respuesta correcta: x".

Reglas de transformación mínimas:

- Primera línea en mayúsculas → `<h1>` de la lección.
- Línea corta terminada en `:` → `<h2>` de sección.
- Listas con guiones → `<ul>`.
- Resto → `<p>`.
- En los tests: parsear cada bloque pregunta/opciones/respuesta a la estructura JSON de §4; la línea "Respuesta correcta" se elimina del contenido visible.
- Extracción de texto desde .docx con la librería `mammoth` (convierte a HTML preservando negritas y listas); el parser de reglas trabaja sobre esa salida.

### 5.3 Salida

- `data/curso-ciberseguridad.json` (solo contenido publicable, según §4).
- Páginas HTML estáticas generadas por plantilla (el generador escribe los HTML finales; no se renderiza JSON en cliente, para que el sitio funcione sin JS y sea indexable). El test interactivo sí es un componente JS que lee las preguntas del JSON embebido en su página.
- El generador debe ser **idempotente**: reejecutarlo regenera todas las páginas del curso sin tocar la landing ni las páginas manuales.

### 5.4 Test interactivo (módulo 1)

- Preguntas con opciones tipo radio, botón "Corregir test".
- Al corregir: marca aciertos/errores, muestra puntuación (X de N) y permite reintentar.
- JS vanilla, sin dependencias. Accesible por teclado, con estados visibles de foco.
- Este componente es argumento de venta: demuestra a la academia el valor añadido de la versión web frente al Word.

## 6. Identidad visual

Paleta (de los documentos de marca, verificada con buen contraste):

| Rol | Color | Hex |
|---|---|---|
| Fondo oscuro / secciones destacadas | Azul noche | `#0B1F3A` |
| Acción / enlaces / CTA | Azul eléctrico | `#2563EB` |
| Acento tecnológico (con moderación) | Cian luminoso | `#22D3EE` |
| Fondo claro base | Blanco limpio | `#F8FAFC` |
| Texto secundario | Gris pizarra | `#334155` |
| Estados de éxito (tests) | Verde | `#10B981` |

Tipografías: **Poppins** para títulos, **Inter** para texto, **JetBrains Mono** para detalles técnicos (etiquetas de módulo, datos como "30 h · 8 módulos"). Cargadas desde Google Fonts con `font-display: swap`.

Directrices:

- Estilo profesional, limpio, educativo. Evitar explícitamente (según los propios documentos de marca): estética hacker, robots, neones excesivos, aspecto infantil, plantillas genéricas.
- Elemento firma propuesto: el motivo "documento inteligente" — tarjetas con esquina doblada tipo documento y una línea de nodos sutil, coherente con el nombre CiberDoc. Usarlo en el hero, en las tarjetas de módulo y en el logo textual. Un solo elemento memorable; el resto, sobrio.
- Logo en Fase 1: logotipo textual "CiberDoc IA" + icono SVG de documento con nodos, hecho a mano en SVG inline. Sin generadores de imágenes.
- Accesibilidad: contraste AA mínimo, foco visible, HTML semántico, `prefers-reduced-motion` respetado. Animaciones discretas (reveal al hacer scroll como máximo).
- Responsive: móvil primero; el visor de lección debe leerse cómodamente en un móvil de 380 px.

Nota: la identidad "Estratos" del portfolio personal (grafito + verde fluorescente) **no se reutiliza**. CiberDoc IA es una marca comercial independiente con su propia paleta; mezclarlas diluiría ambas.

## 7. Formulario de contacto

- **Formspree** (mismo proveedor ya validado en el portfolio; crear un endpoint nuevo específico para CiberDoc IA — no reutilizar el del portfolio, para separar los leads).
- Campos: nombre, email, teléfono (opcional), tipo de consulta (select: Curso para academia/centro · Formación para empresa · Docente · Otra consulta), mensaje.
- Validación HTML5 + JS ligera. Mensaje de éxito/error real según respuesta de Formspree. **Prohibido** el patrón "formulario sin servidor que solo muestra confirmación": todo envío debe llegar a un buzón real.
- Checkbox de consentimiento RGPD enlazando a la política de privacidad. Las páginas legales (aviso legal, privacidad) son obligatorias al captar datos personales de contacto en España.

## 8. Requisitos técnicos

- HTML5, CSS3 (un solo fichero `css/estilos.css` con custom properties para la paleta), JS vanilla en módulos ES.
- Sin frameworks ni build tools en el sitio publicado. El único tooling es el script de conversión (Node.js + mammoth) que corre en local.
- Repositorio nuevo en la cuenta `jesusrojasbaena` (propuesta: `ciberdoc-ia`), desplegado en GitHub Pages. Dominio propio (p. ej. `ciberdocia.es`) como decisión posterior — la estructura de URLs relativa debe soportarlo sin cambios.
- SEO básico: `title` y `meta description` únicos por página, Open Graph, `sitemap.xml`, `robots.txt`, HTML semántico. Palabra clave eje: "curso de ciberseguridad para academias".
- Rendimiento: sin librerías externas salvo fuentes; imágenes (si las hubiera) en WebP; objetivo Lighthouse ≥ 90 en las cuatro métricas.

## 9. Estructura del repositorio

```
ciberdoc-ia/
├── index.html
├── contacto/index.html
├── aviso-legal/index.html
├── privacidad/index.html
├── cursos/
│   ├── index.html
│   └── ciberseguridad-basica/
│       ├── index.html
│       └── modulo-1/
│           ├── contenido.html · ficha.html · actividad.html
│           ├── resumen.html · glosario.html · test.html
├── css/estilos.css
├── js/ (navegación, test interactivo, formulario)
├── data/curso-ciberseguridad.json
├── scripts/convertir-curso.mjs      ← pipeline Word→web
├── plantillas/ (plantillas HTML usadas por el generador)
└── docs/especificacion-ciberdoc.md  ← este documento
```

La carpeta fuente del curso (`07_Entrega_Final/`) **permanece fuera del repositorio** (§4).

## 10. Fases

**Fase 1 (este proyecto):** todo lo especificado arriba. Resultado: web comercial completa con demo navegable del módulo 1 y pipeline reutilizable. Coste de infraestructura: 0 €.

**Fase 2 (solo si hay tracción comercial):** área de cliente. Registro/login y acceso de las academias compradoras al curso completo online, con Supabase (auth + Postgres, tier gratuito) o entrega alternativa como sitio privado por cliente. Requiere decidir modelo de entrega (¿acceso web o paquete descargable?) según lo que pidan los primeros clientes reales.

**Fase 3 (futuro, sin diseñar aún):** más cursos en catálogo (el pipeline ya lo soporta), seguimiento de progreso de alumnos, y —solo si aparece demanda B2C— pagos. La venta B2B a academias se gestiona por contacto directo y factura; no se integra pasarela de pago en Fases 1–2.

## 11. Plan de trabajo en Claude Code (Plan mode)

Orden recomendado de tareas, cada una como sesión/commit independiente:

1. **Inicializar repo** con estructura de §9, CSS base con la paleta como custom properties, y este documento en `docs/`.
2. **Pipeline de conversión:** script + plantillas. Probarlo contra la carpeta real del curso y revisar manualmente el HTML del módulo 1 completo antes de seguir. *Criterio de aceptación: las 5 lecciones y el test del módulo 1 se generan correctos y legibles sin retoques manuales.*
3. **Visor de lección + test interactivo** sobre el contenido generado.
4. **Página del curso y catálogo.**
5. **Landing** (la última: así el hero puede enseñar material real ya generado).
6. **Formulario + páginas legales.**
7. **SEO, accesibilidad, Lighthouse, despliegue** en GitHub Pages.

Advertencias para la ejecución:

- No dejar que Claude Code "mejore" la estructura del JSON o de las carpetas sobre la marcha: este documento es la referencia; los cambios se deciden primero aquí (en chat de planificación) y se actualiza la especificación.
- Verificar tras cada generación que ningún solucionario ni documento privado haya acabado en el repo (`git status` antes de cada commit; añadir la ruta del curso fuente a `.gitignore` global por si acaso).
- El texto comercial de la landing se redacta en la sesión de planificación (chat), no se improvisa en Code.

## 12. Criterios de aceptación de la Fase 1

1. La web se despliega en GitHub Pages y todas las páginas funcionan sin errores de consola.
2. El módulo 1 completo es navegable públicamente; los módulos 2–8 aparecen listados pero sin contenido accesible, y su material no existe en el repositorio ni en el JSON.
3. El test del módulo 1 se autocorrige y muestra puntuación.
4. Un envío real del formulario llega al buzón de Formspree.
5. Reejecutar el pipeline regenera el módulo 1 idéntico (idempotencia).
6. Lighthouse ≥ 90 en rendimiento, accesibilidad, buenas prácticas y SEO en la landing y en una lección.
7. La web se lee y navega correctamente en un móvil de 380 px de ancho.
