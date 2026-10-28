# Stratum Sovereign Architecture Specification
> **Documento de Especificación Técnica Oficial**  
> *Versión 2.0.0 — Isora Cloud Group*

---

## 1. Topología de Red y Enrutamiento Split-Horizon

Stratum implementa un modelo de aislamiento donde ningún servicio de aplicación conoce la dirección de red directa de los motores de base de datos centrales.

### Flujo de Paquetes en Persistencia
1. Un microservicio en `stratum-python` o `stratum-spring` intenta conectarse a `postgres` usando la URL `postgresql://user:pass@stratum-database-nginx:5432/db_tenant`.
2. El contenedor busca el nombre `stratum-database-nginx` en el DNS interno de Docker.
3. Debido a la configuración de red de la organización (`stratum-org`), el alias `stratum-database-nginx` resuelve a la IP interna del router frontera de la organización (`stratum-org-<org>-nginx`).
4. El Nginx de la organización intercepta la conexión TCP en el puerto `5432` mediante un bloque `stream` y la reenvía a través de la red perimetral `stratum_dmz` hacia el proxy central de persistencia `stratum-database-core`.
5. El proxy central valida el tráfico y lo entrega a la instancia PostgreSQL ubicada en la red ciega `stratum_database_internal`.

```
[Workload Tier 3] 
       │
       │ (Conecta a: stratum-database-nginx:5432)
       ▼
[Nginx Org Router] (Dual-Homed: red interna <-> stratum_dmz)
       │
       │ (Proxy TCP Stream vía stratum_dmz)
       ▼
[Nginx Central DB Proxy] (Dual-Homed: stratum_dmz <-> stratum_database_internal)
       │
       │ (Entrega TCP local en red ciega internal: true)
       ▼
[PostgreSQL 16 / MongoDB 7]
```

---

## 2. Definición de Redes y Aislamiento

| Nombre de Red | Ámbito | Driver | Bandera `internal` | Conectividad Externa |
| :--- | :--- | :--- | :--- | :--- |
| `stratum_dmz` | Plataforma Central | `bridge` | `false` | Solo tráfico saliente hacia túneles Ingress autorizados. |
| `stratum_database_internal` | Plataforma Central | `bridge` | **`true`** | **Cero salida.** Sin puerta de enlace a internet. |
| `stratum_<org>_internal` | Por Organización | `bridge` | **`true`** | **Cero salida.** Confinada al perímetro del cliente. |

---

## 3. Contratos de Contenedores y Estándar de Ejecución

Cada contenedor de la arquitectura debe respetar:
1. **PID 1 Supervisado:** Uso obligatorio de `tini` o `init: true` en Docker Compose para garantizar recolección de procesos huérfanos.
2. **Capacidades Mínimas:** `cap_drop: [ALL]`. Si se requiere una capacidad específica (ej. `NET_BIND_SERVICE` para escuchar en puertos <1024), debe añadirse explícitamente en `cap_add`.
3. **No-New-Privileges:** `security_opt: ["no-new-privileges:true"]`.
4. **Sistemas de Archivos Inmutables:** Los directorios de escritura deben ser volúmenes nombrados dedicados o montajes en memoria RAM (`tmpfs`).
