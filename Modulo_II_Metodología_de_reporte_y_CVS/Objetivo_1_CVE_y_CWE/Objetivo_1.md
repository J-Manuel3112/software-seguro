# Objetivo 1: Estandarización de Vulnerabilidades (CVE y CWE)

En la Ciberseguridad y el desarrollo de software seguro, hay que saber comunicarnos para que lo que uno quiera transmitir sea entendible para todos. Para eso, existen estándares universales utilizados para facilitar esta interacción.

## CVE (Common Vulnerabilities and Exposures)
Es un diccionario de identificadores únicos de vulnerabilidades de seguridad generalmente públicas. Se encarga de darle un término común para referirse a algún fallo de un sistema. Se conforma por un número de identificación y una descripción a detalle del problema.

**Lugar de consulta:** Los identificadores y detalles técnicos de un CVE se pueden consultar públicamente en varios repositorios y bases de datos:
1. **Sitio Oficial de CVE (MITRE):** Repositorio central oficial, aquí se registra y publica en tiempo real.
2. **NVD(National Vulnerability Database):** Es la base de datos pública, también cuenta con puntuaciones para cada CVE con respecto a su gravedad y mapeos directos al CWE correspondiente.  
3. **Bases de Datos de proveedores:** Integran CVEs con el ecosistema de desarrollo de software.

## CWE (Common Weakness Enumeration)
Es un catálogo de clasificación de debilidades de seguridad. No referencia a un software comercial o específico, sino a un tipo de fallo, ejemplo CWE-89(SQL Injection).

## Diferencia entre CVE y CWE:
La diferencia es que, mientras el CVE le da un tipo de identificación al problema de un sistema en específico, el CWE se encarga de tener identificado los distintos tipos de problemas generales, para ayudar a prevenir, detectar y mitigar fallos.

## Autoridades de asignación de CVE (CNAs)
Los encargados de asignar los CVE son una red de organizaciones autorizadas conocidas como CNAs (CVE Numbering Authorities).
 * **Conformado por:** Organizaciones de tecnología, proveedores de software, empresas de ciberseguridad e instituciones de investigación autorizadas para asignar CVE a vulnerabilidades descubiertas en su sistema o dentro de su ámbito.
 * **Coordinación:** Está financiado por el Departamento de Seguridad Nacional de EE.UU y gestionado operativamente por MITRE Corporation, la cual actúa como la raíz del CNA.
 * **Ejemplo:** Google, Github, Cisco, etc.

## Herramientas de detección de CVEs
Son Herramientas automatizadas que escanean activos y dependencias para cruzarlos contra las bases de datos de CVEs:

- **Nessus o Qualys:** Escáneres de red y sistemas.
- **SonarQube:** Analizador sintáctico de código.
    