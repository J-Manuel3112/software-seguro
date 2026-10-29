# Objetivo 4: Primer Reporte Técnico Oficial

**Cliente:** `Software Seguro`  
**Aplicación / Entorno:** `Turnero`  
**Ventana de prueba:** `Octubre 2026` a `Octubre 2026`  
**Fecha:** `[27/10/2026]` | **Clasificación:** `[Confidencial]`  
**Elaborado por:** `Juan Arones` 

---

## 1. Resumen Ejecutivo (para directivos)

### 1.1 Objetivo del Análisis
El propósito de este análisis fue evaluar la seguridad de la API del sistema de gestión de turnos médicos "Turnero". EL objetivo era revisar si un usuario cualquiera podría acceder o eliminar recursos pertenecientes a otros usuarios, comprometiendo la integridad de la base de datos de turnos.

### 1.2 Nivel de Riesgo
    Crítico

### 1.3 Resultados Clave e Impacto al Negocio
* **Total de hallazgos:** `1` hallazgo (`1` Críticos, `0` Altos, `0` Medios, `0` Bajos).
* **Riesgo más importante:** Permite que cualquier usuario que ingrese puede modificar turnos pertenecientes a otros usuarios(IDOR). Esto representa un grave problema de privacidad, posible denegación de servicio (al borrar turnos) y una violación directa de las normativas de protección de datos de salud.
* **Fortalezas observadas:** El sistema requiere autenticación previa para acceder a la API, y utiliza identificadores únicos (IDs) para los usuarios y sus respectivos turnos.

### 1.4 Prioridades de Remediación
1. Implementar validación de autorización a nivel de objeto.
2. Auditar los logs de acceso para determinar si hubo explotación previa.
3. Migrar el uso de IDs secuenciales a UUIDs para mitigar ataques de enumeración. 

---

## 2. Detalles Técnicos

|     ID    | Título del Hallazgo| Gravedad  | CVSS Base |      CWE    |           Activo Afectado          | Estado  |
| **PT-01** |  IDOR en turnero   | `[Alto]`  |  `[8.1]`  | `[CWE-639]` | `/api/{user_id}/appointments/{id}` | Abierto |

---

## 3. Métricas CVSS

### Puntuación de Severidad del Hallazgo [PT-01]
* **Versión CVSS:** `3.1`
* **Puntuación Base:** `8.1`
* **Severidad:** `Alto`
* **Vector CVSS String:** `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:L/I:H/A:N`

---

## 4. Prueba de Concepto (PoC) detallada paso a paso

### Hallazgo [PT-01]: IDOR en API de Turnos

#### Descripción de la Vulnerabilidad:
El backend del sistema verifica que el usuario tenga una sesión activa, pero omite validar la autorización a nivel de objeto. Al recibir una petición HTTP `DELETE`, el servidor confía ciegamente en el ID de usuario y el ID de turno enviados en la URL, permitiendo que un atacante elimine turnos que no le pertenecen simplemente cambiando dichos números.

#### Pasos de Reproducción
1. **Prerrequisitos:** Iniciar sesión en la aplicación con una cuenta estándar (en este caso, el usuario "Luis").
2. **Reconocimiento:** Utilizando las Herramientas de Desarrollador, se identificó que al cancelar un turno propio, se enviaba la petición `DELETE /api/1/appointments/1` (donde `1` era el ID de Luis y `1` el ID del turno).
3. **Enumeración:** Se manipuló la URL cambiando el ID de usuario de forma secuencial. Se descubrió que el usuario "xdalvik" (el objetivo) poseía el ID `101`. Al realizar un `GET /api/101/appointments`, el servidor devolvió la lista completa de turnos de xdalvik (IDs `10`, `12` y `13`).
4. **Explotación:** Se enviaron tres peticiones `DELETE` manipuladas: 
   - `DELETE /api/101/appointments/10`
   - `DELETE /api/101/appointments/12`
   - `DELETE /api/101/appointments/13`
5. **Resultado:** El servidor respondió con un `200 OK` y el mensaje "turno eliminado correctamente" en cada caso, borrando los turnos del usuario víctima sin afectar a otros pacientes.

---

## 5. Recomendaciones de Remediación
- **Acción Correctiva:** Implementar controles de acceso a nivel de objeto. Antes de procesar una acción (GET, DELETE, PUT), el backend debe verificar que el `user_id` que viaja en la URL coincida con el ID del usuario autenticado en la sesión actual (Token o Cookie), a menos que el usuario tenga un rol de administrador explícito.
- **Ejemplo de Código Seguro:**

```javascript
// Ejemplo de validación correcta en el Backend antes de borrar
app.delete('/api/:user_id/appointments/:turno_id', (req, res) => {
    // 1. Obtener el ID del usuario logueado desde la sesión/token
    const usuarioLogueadoId = req.session.userId; 
    const usuarioUrlId = req.params.user_id;

    // 2. Verificar si el que hace la petición es el dueño del recurso
    if (usuarioLogueadoId !== usuarioUrlId) {
        return res.status(403).json({ error: "No tienes permiso para borrar turnos de otro usuario" });
    }

    // 3. Si es el dueño, proceder a borrar el turno
    borrarTurnoEnBaseDeDatos(req.params.turno_id)
        .then(() => res.status(200).json({ message: "Turno eliminado" }))
        .catch(err => res.status(500).json({ error: "Error interno" }));
});