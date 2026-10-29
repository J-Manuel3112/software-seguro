# Objetivo 3: Armado de Plantilla Base de Reporte

**Cliente:** `Nombre de la empresa`  
**Aplicación / Entorno:** `Nombre de la app, versión, URL o repositorio`  
**Ventana de prueba:** `Fecha de inicio` a `Fecha de fin`  
**Fecha:** `[Fecha de emisión]` | **Clasificación:** `[Confidencial]`  
**Elaborado por:** `Mi Nombre` 

---

## 1. Resumen Ejecutivo (para directivos)

### 1.1 Objetivo del Análisis
`Explicación breve del propósito del análisis: auditoría periódica, evaluación previa a lanzamiento, cumplimiento normativo, etc.`

### 1.2 Nivel de Riesgo
`Critico / Alto / Medio / bajo`

### 1.3 Resultados Clave e Impacto al Negocio
* **Total de hallazgos:** `[N]` hallazgos (`[x]` Críticos, `[x]` Altos, `[x]` Medios, `[x]` Bajos).
* **Riesgo más importante:** `Descripción breve del riesgo principal expresada en términos de negocio o impacto operativo.`
* **Fortalezas observadas:** `Mención de controles o buenas prácticas de seguridad detectadas durante la evaluación.`

### 1.4 Prioridades de Remediación
1. `[Acción de alta prioridad, Responsable sugerido, Plazo estimado de resolución]`
2. `[Acción de prioridad media, Responsable sugerido, Plazo estimado de resolución]`
3. `[Acción de prioridad baja, Responsable sugerido, Plazo estimado de resolución]`

---

## 2. Detalles Técnicos

|     ID    |     Título del Hallazgo    | Gravedad | CVSS Base |    CWE    |     Activo Afectado    | Estado  |
| **PT-01** | `Título corto del fallo`   |   `Alto`   |  `x.x`  |   `CWE-x` |  `Endpoint / Archivo`  | Abierto |
| **PT-02** | `Título corto del fallo`   |   `Medio`  |  `x.x`  |   `CWE-x` |  `Endpoint / Archivo`  | Abierto |

---

## 3. Métricas CVSS

### Puntuación de Severidad del Hallazgo [PT-01]
* **Versión CVSS:** `3.1`
* **Puntuación Base:** `Score numérico, ej: 7.5`
* **Severidad:** `Critico / Alto / Medio / bajo`
* **Vector CVSS String:** `Vector de texto completo, ej: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N`

---

## 4. Prueba de Concepto (PoC) detallada paso a paso

### Hallazgo [PT-01]: `Título del Hallazgo`

#### Descripción de la Vulnerabilidad:
`Explicación técnica clara sobre la falla, origen del error en la lógica de control o código, y su comportamiento.`

#### Pasos de Reproducción
1. `Paso 1: Estado inicial o prerrequisitos.`
2. `Paso 2: Captura de petición o modificación de parámetros/cookies.`
3. `Paso 3: Comando, script o payload utilizado para la explotación.`

#### Evidencia Fotográfica / Registros
`Inclusión de capturas de pantalla, fragmentos de peticiones/respuestas HTTP, registros o salidas de terminal.`

---

## 5. Recomendaciones de Remediación
- **Acción Correctiva:** `[Solución técnica para PT-01].`
- **Ejemplo de Código Seguro:**...

---

# Explicación de la Estructura y Valor Generado por la Plantilla

## 1. Explicación de la Estructura
   1. **Resumen Ejecutivo:** Traduce el estado de seguridad y los riesgos a un lenguaje gerencial y de impacto al negocio.
   2. **Detalles Técnicos:** Lista las vulnerabilidades encontradas asociándolas con sus estándares correspondientes.
   3. **Métricas CVSS:**  Cuantifica objetivamente la gravedad mediante el cálculo estandarizado de la industria.
   4. **Prueba de Concepto (PoC):** Entrega la evidencia técnica reproducible de cómo se descubrió y explotó el fallo.
   5. **Recomendaciones de Remediación:** Brinda soluciones concretas y ejemplos de código seguro.

## 2. Valor que Genera al Usuario Final
* **Para Directivos / Gerencia:** 
   * Facilita la toma de decisiones rápida y la priorización de recursos orientada al impacto real en el negocio.
* **Para Desarrolladores / Equipo Técnico:**
   * Permite replicar y entender el fallo de inmediato gracias a las PoCs y métricas exactas.
   * Las recomendaciones concretas y ejemplos de código seguro reducen los tiempos de parcheo (MTTR) y evitan futuros errores.