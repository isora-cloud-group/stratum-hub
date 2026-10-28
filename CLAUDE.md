# CLAUDE.md — Directivas de Arquitectura y Lineamientos para Asistentes de IA

> **"Aislamiento por diseno, no por disciplina"** — *(“Isolation by design, not by discipline”)*

## 1. Vision General del Repositorio
`stratum-hub` es el repositorio central, catalogo navegable y punto de referencia arquitectonico para la suite **Stratum Sovereign Multi-Tenant Architecture**. Actua como el ancla de integracion para todos los repositorios satelite y cargas de trabajo operadas por **Isora Cloud Group (ICG)**.

---

## 2. Invariantes de Diseno Inquebrantables
Cualquier modificacion o sugerencia tecnica emitida para el ecosistema Stratum debe respetar de manera incondicional:

1. **Zero Host Ports:** No proponer jamas el mapeo directo de puertos a interfaces fisicas del host (`ports: - "8080:8080"` o similar). Todo el trafico de ingreso ingresa mediante tuneles perimetrales cifrados de borde hacia `stratum_dmz`.
2. **Blind Data Network:** La red `stratum_database_internal` debe mantener siempre `internal: true`. No posee pasarela de salida ni conectividad directa a internet.
3. **Mediacion L4 en Base de Datos:** Los motores de persistencia (PostgreSQL, MongoDB) nunca se enlazan directamente a `stratum_dmz`. Todo acceso desde las organizaciones debe ser mediado por el proxy TCP Stream `stratum-database-nginx`.
4. **Isla de Organizacion Ciega:** La subred privada de cada cliente (`stratum_<org>_internal`) es estrictamente ciega (`internal: true`). Las cargas de trabajo no tienen ruta hacia el DMZ ni hacia otros clientes.
5. **Redis Soberano Local Dedicado:** Cada inquilino opera su propio contenedor Redis 7 en su red interna. Queda terminantemente vetado el uso de instancias Redis multi-tenant compartidas.
6. **Cero Dependencias Externas en `stratum-cli`:** La herramienta CLI administrativa debe ejecutarse exclusivamente con la biblioteca estandar de Python 3.10+ (cero paquetes pip en runtime).
7. **Slugs Deterministas:** Todo identificador de organizacion o servicio debe verificarse contra:
   `^[a-z0-9]+(?:-[a-z0-9]+)*$`

---

## 3. Convencion de Enlaces Institucionales
Al referenciar repositorios del ecosistema Stratum en documentacion o respuestas, deben utilizarse siempre los enlaces canonicos de la organizacion:
* Hub Central: `https://github.com/isora-cloud-group/stratum-hub`
* Plano de Control: `https://github.com/isora-cloud-group/stratum-cli`
* Plataforma Base: `https://github.com/isora-cloud-group/stratum-core`
* Isla de Organizacion: `https://github.com/isora-cloud-group/stratum-org`
* Workload Python: `https://github.com/isora-cloud-group/stratum-python`
* Workload Node.js: `https://github.com/isora-cloud-group/stratum-node`
* Workload Java/Spring: `https://github.com/isora-cloud-group/stratum-spring`
* Workload PHP/Laravel: `https://github.com/isora-cloud-group/stratum-laravel`
* Workload Overleaf: `https://github.com/isora-cloud-group/overleaf-stratum`
* Workload Nextcloud: `https://github.com/isora-cloud-group/stratum-nextcloud`

---

## 4. Tono y Directivas de Estilo
* **Ingenieria Sobria y Profesional:** No utilizar emoticonos, iconos informales ni redaccion coloquial que transmita sensacion de software experimental o generado superficialmente. Mantener un estricto tono de ingenieria institucional de alto nivel.
* **Precisión Terminologica:** Utilizar terminos de ingenieria formales: *Aislamiento por diseno*, *Topologia Zero-Trust de 4 Capas*, *Mediacion L4 Stream*, *Hardening de Kernel*, *Contenedores Rootless*, *Gobernanza de Infraestructura*.

---

## 5. Atribucion Legal y Autoria
* **Titular de Derechos y Arquitecta:** Sandra Gabriela Puerto Torres (SGPT)
* **Organizacion & Distribuidor:** Isora Cloud Group (ICG) — `https://isora.com.co`
* **Licencia:** Apache License, Version 2.0 (Apache-2.0)

