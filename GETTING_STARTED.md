# Guía de Inicio Rápido: Despliegue de Stratum
> **Paso a Paso para Operadores y Administradores de Sistemas**

Esta guía te llevará desde un servidor limpio con Docker instalado hasta el despliegue de una organización multi-inquilino con una carga de trabajo en ejecución.

---

## Prerrequisitos
Asegúrate de contar con:
1. Servidor Linux con Docker Engine 24+ y Docker Compose v2.
2. Python 3.10+ instalado en el host (para el CLI de administración).
3. Acceso a una cuenta de Cloudflare Zero Trust (o túnel de borde equivalente).

---

## Paso 1: Clonar y Desplegar `stratum-core`

El núcleo de la plataforma debe levantarse antes que cualquier organización.

```bash
git clone https://github.com/sandra-puerto/stratum-core.git
cd stratum-core

# 1.1 Iniciar la red DMZ y el contenedor ancla
cd dmz
docker compose up -d

# 1.2 Iniciar el clúster central de base de datos y proxy L4
cd ../database
cp .env.example .env
# Configura las contraseñas maestras de Postgres y MongoDB en .env
docker compose up -d

# 1.3 Iniciar el gateway de entrada
cd ../gateway
cp .env.example .env
# Configura tu token de Cloudflare Tunnel
docker compose up -d
```

---

## Paso 2: Instalar y Configurar `stratum-cli`

`stratum-cli` no requiere dependencias externas; funciona directamente con la biblioteca estándar de Python:

```bash
git clone https://github.com/sandra-puerto/stratum-cli.git
cd stratum-cli

# Instalar enlace simbólico global
./install.sh

# Verificar salud general de la plataforma
stratum doctor
```

---

## Paso 3: Aprovisionar una Nueva Organización (Tenant)

Crea la frontera aislada para un nuevo cliente (por ejemplo, `acme-corp`):

```bash
# Crear la organización
stratum org create acme-corp

# Aprovisionar su base de datos PostgreSQL aislada
stratum db create acme-corp --type postgres
```

Esto generará la configuración de `stratum-org` con:
* Red privada `stratum_acme-corp_internal`.
* Enrutador frontal Nginx con proxy split-horizon.
* Contenedor Redis 7 soberano local.

---

## Paso 4: Desplegar una Carga de Trabajo (Workload)

Selecciona la plantilla requerida (ej. `stratum-python` o `stratum-laravel`):

```bash
git clone https://github.com/sandra-puerto/stratum-python.git
cd stratum-python

# Copiar variables de entorno configurando TENANT_ORG=acme-corp
cp .env.example .env
nano .env

# Construir y levantar
docker compose up -d
```

¡Listo! La aplicación estará corriendo dentro de la red interna de `acme-corp`, comunicándose de forma transparente con la base de datos central a través de `stratum-database-nginx` y sin exponer un solo puerto al host.
