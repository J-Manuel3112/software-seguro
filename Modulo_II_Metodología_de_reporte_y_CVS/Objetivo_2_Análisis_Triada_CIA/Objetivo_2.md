# Objetivo 2: Análisis de la Triada CIA

La Triada CIA es el modelo fundamental sobre el que se construye la seguridad de la información.

## Los tres pilares:
1. **CONFIDENCIALIDAD:** Consiste en que la información sea accesible sólo para las personas autorizadas. En caso de que un externo sin permisos logre ver estos datos, se considera como rota la confidencialidad.

**Ejemplo:** IDOR, como cuando se modifica algún dato de la url y permite ver datos privados.

2. **INTEGRIDAD:** Consiste en que la información manejada sea confiable, que no haya sido alterada de mala forma, ya sea por accidente o por atacante. Si alguien logra modificar datos sin autorización, la integridad se rompió.

**Ejemplo:** Broken Access Control, cuando uno logra colarse a un sistema y modificar algún dato legítimo que tenía el sistema.

3. **DISPONIBILIDAD:** Consiste en garantizar que los sistemas estén disponibles para sus usuarios cuando quieran ingresar. Si una app se cae o se cuelga, la disponibilidad se rompió.

**Ejemplo:** DoS(Denegación de Servicios), Se satura una página con muchas peticiones falsas al mismo tiempo, hasta que el servidor colapsa y el sistema se cae.