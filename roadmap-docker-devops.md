# Roadmap Docker Intermedio-Alto — DevOps & Backend
## ¿Es necesario dominar todos los puntos de docker del roadmap para tener un nivel intermedio alto en un roll devops? ¿Cuales serian obligatorios?

No, no hace falta dominar todo. El roadmap está diseñado como mapa de terreno completo, no como lista de requisitos. Para un perfil DevOps intermedio-alto, la mayor parte del valor está concentrada en menos de la mitad de los contenidos.

**Obligatorio de verdad (el núcleo innegociable)**

- **Etapa 2 — Dockerfile y build.** Es lo que más se usa y lo que más se nota cuando falta. Multi-stage, orden de instrucciones para el cache, `.dockerignore`, `ARG` vs `ENV`, usuario no-root. Si solo pudieras dominar una etapa, sería esta.
- **Etapa 3 — Networking, a nivel funcional.** Redes de usuario vs bridge default, DNS interno, publicación de puertos, aislamiento por redes, y sobre todo el método de diagnóstico ante "A no se conecta con B". No necesitas iptables ni NAT a fondo.
- **Etapa 4 — Storage.** Volume vs bind mount vs tmpfs, persistencia, y el problema de permisos UID/GID. Este último aparece constantemente en la vida real.
- **Etapa 5 — Compose.** Es tu herramienta diaria de desarrollo y testing. Healthchecks con `depends_on: service_healthy`, variables de entorno, redes/volúmenes, separación dev/prod.
- **Etapa 6 — Imágenes y registries.** Capas, tags vs digests, push/pull, autenticación. Es la base del despliegue en cualquier entorno.
- **Etapa 9 — CI/CD.** Para un rol DevOps esto no es opcional: build, tag, push automatizado y estrategia de promoción sin rebuild.

**Importante pero con profundidad moderada**

- **Etapa 1 — Arquitectura interna.** Necesitas el modelo mental (CLI → daemon → containerd → runc, namespaces aíslan, cgroups limitan) porque es lo que te permite razonar ante fallos raros y lo que te preguntan en entrevistas. No necesitas explorar `/proc` ni syscalls salvo por curiosidad.
- **Etapa 7 — Seguridad.** Obligatorio: no-root, secretos fuera de la imagen, pinning, escaneo de CVEs. Opcional a nivel de detalle: seccomp, capabilities específicas, user namespace remapping (basta saber que existen).
- **Etapa 8 — Observabilidad.** Obligatorio: exit code 137/OOM, `docker stats`, `docker logs`, rotación de logs y límites de recursos. Con eso alcanza; no necesitas performance tuning profundo.

**Se puede diferir sin culpa**

- **Etapa 10** no es estudio, es una conversación de 30 minutos contigo mismo antes de empezar Kubernetes. No la "estudies" ahora, úsala como transición.
- Todo lo que marqué en "Qué NO necesitas aprender todavía": Swarm, overlay networks, perfiles seccomp propios, storage drivers alternativos.
- Multi-arquitectura con `buildx` y DinD avanzado: útil cuando te toque el caso concreto, no antes.

**El criterio que importa más que la lista**

Para un rol DevOps te van a evaluar menos por cuántos conceptos conoces y más por si puedes diagnosticar algo roto que nunca viste. Por eso los laboratorios de errores provocados (redes, permisos, OOM, cache) valen más que memorizar capabilities de Linux. Si tienes tiempo limitado, recorta profundidad teórica antes que práctica de troubleshooting.

Una ruta comprimida realista sería: Etapa 2 → 3 → 4 → 5 → 6 → 9, con lectura ligera de 1, 7 y 8 en el camino, y el proyecto final como cierre. Eso son unas 7-8 semanas en lugar de 12 y ya te sitúa en intermedio-alto defendible.

---

**Perfil de partida:** conoces imágenes, contenedores, Dockerfile, `build`/`run`, puertos, volúmenes y redes a nivel básico. Trabajas con Java/Spring Boot, Go, PostgreSQL, Linux/Ubuntu y Docker Desktop en WSL. Vas camino a Kubernetes, CI/CD (GitLab CI, GitHub Actions, ArgoCD).

**Filosofía de este roadmap:** cada etapa se estructura igual — objetivo, prerequisitos, conceptos (con el problema que resuelven y su funcionamiento interno), comandos (memorizar vs consultar), ejercicios, laboratorio, errores a provocar/solucionar, y criterio de dominio. El foco está en el *por qué*, no solo en el *cómo*.

---

## Índice

- [A. Roadmap visual por etapas](#a-roadmap-visual-por-etapas)
- [Niveles de conocimiento (clasificación)](#niveles-de-conocimiento-clasificación)
- [Etapa 0 — Repaso y nivelación](#etapa-0--repaso-y-nivelación)
- [Etapa 1 — Arquitectura interna de Docker](#etapa-1--arquitectura-interna-de-docker)
- [Etapa 2 — Dockerfile avanzado y build](#etapa-2--dockerfile-avanzado-y-build)
- [Etapa 3 — Networking profundo](#etapa-3--networking-profundo)
- [Etapa 4 — Storage y persistencia](#etapa-4--storage-y-persistencia)
- [Etapa 5 — Docker Compose a fondo](#etapa-5--docker-compose-a-fondo)
- [Etapa 6 — Imágenes, registries y distribución](#etapa-6--imágenes-registries-y-distribución)
- [Etapa 7 — Seguridad](#etapa-7--seguridad)
- [Etapa 8 — Observabilidad, recursos y performance](#etapa-8--observabilidad-recursos-y-performance)
- [Etapa 9 — Docker + CI/CD](#etapa-9--docker--cicd)
- [Etapa 10 — Puente Docker → Kubernetes](#etapa-10--puente-docker--kubernetes)
- [B. Plan de estudio de 8-12 semanas](#b-plan-de-estudio-de-8-12-semanas)
- [C. Laboratorios prácticos](#c-laboratorios-prácticos)
- [D. Preguntas de entrevista técnica](#d-preguntas-de-entrevista-técnica-intermedioalto)
- [E. Proyecto final de portfolio](#e-proyecto-final-de-portfolio)
- [F. Checklist de conocimientos "sin documentación"](#f-checklist-de-conocimientos-sin-documentación)
- [Qué NO necesitas aprender todavía](#qué-no-necesitas-aprender-todavía)
- [Errores comunes al aprender Docker](#errores-comunes-al-aprender-docker)
- [Qué diferencia a quien "usa" Docker de quien lo entiende](#qué-diferencia-a-quien-usa-docker-de-quien-lo-entiende)

---

## A. Roadmap visual por etapas

```
Etapa 0  Repaso y nivelación .......................... (base)
   │
Etapa 1  Arquitectura interna (CLI/daemon/containerd/runc/namespaces/cgroups)
   │
Etapa 2  Dockerfile avanzado (multi-stage, cache, .dockerignore, usuarios)
   │
Etapa 3  Networking (bridge/host/none, DNS interno, comunicación entre contenedores)
   │
Etapa 4  Storage (volumes, bind mounts, tmpfs, permisos, backup/restore)
   │
Etapa 5  Docker Compose (multi-servicio, healthchecks, profiles, dev vs prod)
   │
Etapa 6  Imágenes y registries (layers, manifests, digests, GHCR, Docker Hub)
   │
Etapa 7  Seguridad (non-root, capabilities, seccomp, secrets, supply chain)
   │
Etapa 8  Observabilidad y performance (logs, métricas, límites de recursos)
   │
Etapa 9  Docker + CI/CD (GitHub Actions, GitLab CI, DinD, cache, promoción)
   │
Etapa 10 Puente hacia Kubernetes (qué se transfiere y qué no)
   │
Proyecto final de portfolio
```

Cada flecha implica que la etapa siguiente asume dominados los criterios de la anterior, pero no es estrictamente lineal: Compose (5) y Networking/Storage (3-4) se retroalimentan, y puedes avanzar en paralelo una vez termines la Etapa 2.

---

## Niveles de conocimiento (clasificación)

Para que priorices bien el tiempo:

**FUNDAMENTAL** (no puedes decirte "sé Docker" sin esto):
Arquitectura CLI-daemon, capas de imagen, diferencia build-time/runtime, Dockerfile multi-stage, bridge networking, DNS interno, volumes vs bind mounts, Compose básico-intermedio, `.dockerignore`, cache de build, usuarios no-root, variables de entorno, logs, tags vs digests.

**IMPORTANTE** (te separa de un nivel junior):
Healthchecks, redes múltiples con aislamiento, profiles de Compose, optimización de tamaño de imagen, gestión de secrets, límites de CPU/RAM, troubleshooting de DNS y de storage, `docker system df`/limpieza, registries privados y autenticación, CI/CD con build y push de imágenes.

**AVANZADO** (nivel intermedio-alto / senior temprano):
Namespaces y cgroups a nivel de syscalls, seccomp/capabilities, entendimiento de OCI runtime spec, buildkit avanzado (cache mounts, secrets en build, multi-arquitectura), supply chain security (SBOM, firma de imágenes), Docker-in-Docker vs sidecar en runners, diseño de pipelines de promoción entre ambientes, debugging de storage drivers (overlay2).

**DÉJALO PARA KUBERNETES** (no lo profundices ahora):
Scheduling, orquestación multi-nodo, Services/Ingress, ConfigMaps/Secrets de k8s, operadores, Helm, autoescalado, políticas de red tipo NetworkPolicy, CNI plugins, etcd. Docker Compose te da la intuición de "declarar servicios", pero el modelo de orquestación de k8s es distinto y no vale la pena anticiparlo.

---

## Etapa 0 — Repaso y nivelación

**Objetivo:** confirmar que los fundamentos que ya tienes están sólidos y sin huecos silenciosos, antes de construir encima.

**Conocimientos previos:** ninguno adicional a lo que ya tienes.

**Conceptos:**
- Diferencia exacta entre **imagen** (artefacto inmutable, plantilla) y **contenedor** (instancia en ejecución de esa imagen + una capa escribible). Resuelve el problema de "reproducibilidad": la imagen es el mismo punto de partida siempre.
- Ciclo de vida de un contenedor: `created → running → paused → exited → removed`. Importa porque muchos bugs de "no arranca" son en realidad problemas de entender en qué estado quedó.
- Diferencia entre `CMD` y `ENTRYPOINT`, y por qué combinarlos es un patrón común (`ENTRYPOINT` fija el binario, `CMD` da argumentos por defecto sobreescribibles).

**Comandos** (repaso, deberías poder usarlos sin pensar):
```bash
docker run -d --name web -p 8080:80 nginx
docker ps -a
docker logs -f web
docker exec -it web sh
docker stop web && docker rm web
docker build -t myapp:1.0 .
docker images
docker rmi myapp:1.0
```
Memorizar: `run`, `ps`, `logs`, `exec`, `stop/rm`, `build`. Consultar cuando haga falta: flags menos comunes de `run` (`--restart`, `--memory`, etc.), que verás en etapas posteriores con contexto.

**Ejercicios:**
1. Levanta 3 contenedores distintos (nginx, postgres, redis) sin Compose, solo con `docker run`, y verifica que responden.
2. Entra a un contenedor corriendo con `exec -it`, modifica un archivo, sal, y explica qué pasa con ese cambio si el contenedor se elimina.
3. Explica en tus propias palabras (por escrito) la diferencia entre `docker stop` y `docker kill`.

**Laboratorio:** crea un Dockerfile simple para una app "hello world" en Go o Spring Boot, constrúyela, corre el contenedor, y mata el proceso principal desde dentro con `kill -9` vía `exec` — observa qué le pasa al contenedor.

**Errores a provocar y solucionar:**
- Confundir `CMD` y `ENTRYPOINT` de forma que el contenedor ignore los argumentos que le pasas en `docker run`.
- Intentar reconectarte a un contenedor detenido con `exec` y entender el mensaje de error.

**Criterio de dominio:** puedes explicar el ciclo de vida completo de un contenedor y la diferencia imagen/contenedor sin dudar, y sabes cuándo un cambio dentro de un contenedor se pierde.

---

## ✅ Etapa 1 — Arquitectura interna de Docker

**Objetivo:** entender qué pasa realmente cuando ejecutas `docker run`, para poder razonar sobre fallos en lugar de solo repetir comandos.

**Prerequisitos:** Etapa 0.

**Conceptos:**

| Componente | Qué es | Problema que resuelve |
|---|---|---|
| **Docker CLI** | Cliente que traduce tus comandos a peticiones HTTP contra la API del daemon | Interfaz de usuario cómoda |
| **Docker daemon (`dockerd`)** | Proceso en segundo plano que gestiona imágenes, contenedores, redes, volúmenes | Orquesta todo el ciclo de vida |
| **containerd** | Runtime de alto nivel (gestiona ciclo de vida de contenedores, pull de imágenes, snapshots) | Separa la lógica de "gestión" de la de "ejecución cruda" |
| **runc** | Runtime de bajo nivel que implementa la spec OCI: crea el contenedor real usando namespaces y cgroups | Es quien realmente aísla el proceso |
| **namespaces** (pid, net, mnt, uts, ipc, user) | Mecanismo del kernel Linux que aísla vistas: procesos, red, filesystem, hostname | Hace que un contenedor "no vea" el resto del sistema |
| **cgroups** | Mecanismo del kernel para limitar y contabilizar recursos (CPU, memoria, IO) | Evita que un contenedor consuma todo el host |
| **Union filesystem (overlay2)** | Combina capas de solo lectura (imagen) + una capa escribible (contenedor) | Reutilización de capas entre imágenes, builds rápidos |

Flujo real de `docker run nginx`:
1. CLI envía petición HTTP a `dockerd` (vía socket Unix `/var/run/docker.sock`).
2. `dockerd` delega en `containerd` la gestión del ciclo de vida.
3. `containerd` invoca `containerd-shim` + `runc`.
4. `runc` crea namespaces y cgroups, monta el filesystem (overlay2) y ejecuta el proceso.
5. El shim queda como padre del proceso para poder reportar exit codes sin mantener vivo `runc`.

**Relación entre conceptos:** namespaces = aislamiento (qué ve el proceso), cgroups = limitación (cuánto puede usar), overlay2 = de dónde viene su filesystem. Los tres juntos son "el contenedor". Esto es clave para troubleshooting: un problema de red es namespace `net`, un problema de OOM-kill es cgroups, un problema de "no encuentro un archivo" suele ser overlay2/mounts.

**Comandos:**
```bash
docker info                     # ver runtime, storage driver, cgroup version
docker system info | grep -i cgroup
ps aux | grep containerd
sudo ls /run/containerd/ # se guardan datos temporales sobre el estado del sistema desde que arrancó el equipo, organizados en un sistema de archivos en memoria RAM (tmpfs). Al igual que /proc/, su contenido es volátil y se borra por completo al apagar o reiniciar el sistema.

docker inspect <container> --format '{{.State.Pid}}'
sudo ls -la /proc/<pid>/ns/     # ver namespaces del proceso. En la carpeta /proc/ de Linux se guarda un sistema de archivos virtual (procfs) generado en la memoria RAM que funciona como una interfaz directa con el núcleo (kernel) del sistema operativo.
cat /sys/fs/cgroup/.../memory.max   # (según cgroup v1/v2)
```
Memorizar: `docker info`, `docker inspect`. Consultar: rutas exactas de `/proc` y `/sys/fs/cgroup` (cambian entre distros/versiones de cgroups).

**Ejercicios:**
1. Levanta un contenedor, obtén su PID con `docker inspect`, y desde el host explora `/proc/<pid>/ns/` para ver sus namespaces.
2. Compara `docker info` en tu Linux nativo vs en Docker Desktop/WSL — identifica diferencias (Docker Desktop corre una VM Linux ligera).
3. Explica por escrito qué pasaría si `runc` fallara al crear un namespace `net`.

**Laboratorio:** instala `strace` (si tu entorno lo permite) o usa `docker run --pid=host` para comparar la vista de procesos con y sin aislamiento pid, y documenta la diferencia.

**Errores a provocar y solucionar:**
- Correr con `--network host` y explicar qué namespace se está "compartiendo" con el host, y por qué eso rompe el aislamiento de puertos.
- Simular que el daemon no responde (`sudo systemctl stop docker`) y diagnosticar el mensaje de error del CLI.

**Criterio de dominio:** puedes dibujar de memoria el flujo CLI → dockerd → containerd → runc → namespaces/cgroups, y explicar qué mecanismo del kernel resuelve cada tipo de aislamiento.

---

## Etapa 2 — Dockerfile avanzado y build

**Objetivo:** escribir Dockerfiles de nivel profesional: pequeños, reproducibles, cacheables y seguros, para Java/Spring Boot y Go específicamente.

**Prerequisitos:** Etapa 1 (para entender por qué el cache funciona por capas).

**Conceptos:**
- ✅**Cada instrucción crea una capa.** El cache de build se invalida desde la primera instrucción que cambia hacia abajo — de ahí la regla de "poner lo que cambia menos arriba".
- 🎯**Multi-stage builds**: usar una imagen "builder" con todo el toolchain (Maven/Gradle, Go compiler) y copiar solo el artefacto final a una imagen runtime mínima. Resuelve el problema de imágenes gigantes con herramientas de compilación innecesarias en producción.
- **`.dockerignore`**: evita que contexto innecesario (`.git`, `node_modules`, `target/`) se envíe al daemon, acelerando el build y evitando invalidar cache por archivos irrelevantes.
- **Build-time vs runtime**: `ARG` solo existe durante el build; `ENV` persiste en el contenedor final. Confundirlos es una fuga de secretos común (nunca metas secretos en `ARG`/`ENV` porque quedan en el historial de capas).
- **BuildKit** (motor de build moderno, default desde Docker 23+): permite cache mounts (`--mount=type=cache`), montar secretos sin dejarlos en la imagen (`--mount=type=secret`), y builds paralelos de stages independientes.
- **Usuario no-root** en el Dockerfile: crear un usuario dedicado y usar `USER` reduce el impacto de una posible fuga del contenedor.

**Ejemplo multi-stage para Spring Boot:**
```dockerfile
# syntax=docker/dockerfile:1
FROM eclipse-temurin:21-jdk AS builder
WORKDIR /app
COPY pom.xml .
RUN --mount=type=cache,target=/root/.m2 mvn dependency:go-offline
COPY src ./src
RUN --mount=type=cache,target=/root/.m2 mvn package -DskipTests

FROM eclipse-temurin:21-jre-alpine
RUN addgroup -S app && adduser -S app -G app
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar
USER app
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

**Ejemplo multi-stage para Go:**
```dockerfile
FROM golang:1.23-alpine AS builder
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /out/app .

FROM gcr.io/distroless/static-debian12
COPY --from=builder /out/app /app
USER nonroot:nonroot
ENTRYPOINT ["/app"]
```
Nota sobre Go: al ser binario estático, distroless (sin shell, sin package manager) es viable y reduce drásticamente la superficie de ataque; esto **no** es tan directo en Java por la JVM.

**Comandos:**
```bash
docker build -t app:1.0 .
docker build --target builder -t app:builder .        # construir solo un stage
docker build --no-cache -t app:1.0 .
docker build --build-arg VERSION=1.2 -t app:1.2 .
docker history app:1.0                                 # ver capas y tamaños
docker build --progress=plain .                        # ver output detallado de BuildKit
```
Memorizar: `build`, `--target`, `--no-cache`, `history`. Consultar: sintaxis exacta de `--mount=type=cache/secret` (cambia según necesidad puntual).

**Ejercicios:**
1. Toma un Dockerfile "ingenuo" de una sola etapa para tu app Spring Boot y conviértelo a multi-stage; compara tamaños con `docker images`.
2. Reordena instrucciones para maximizar el cache: copia primero `pom.xml`/`go.mod`, instala dependencias, y luego copia el código fuente.
3. Provoca una fuga de secreto poniendo una contraseña en un `ARG`, luego usa `docker history` para "encontrarla" y entiende por qué eso es un problema real.

**Laboratorio:** construye la misma app con y sin `.dockerignore` bien configurado, mide el tiempo de build y el tamaño del contexto enviado al daemon (`docker build` te dice cuánto contexto envía).

**Errores a provocar y solucionar:**
- Cache que no invalida cuando debería (cambias código pero el build usa una capa vieja) → identificar la instrucción mal ordenada.
- Imagen final de 900MB para una app Go que debería pesar 15MB → diagnosticar con `docker history` qué capa infla el tamaño.
- Build que falla porque `COPY` no encuentra el `.jar`/binario → error típico de ruta relativa entre stages.

**Criterio de dominio:** puedes escribir de memoria un Dockerfile multi-stage optimizado para Java o Go, explicar por qué cada línea está en ese orden, y diagnosticar por qué una imagen pesa más de lo esperado usando `docker history`.

---

## Etapa 3 — Networking profundo

**Objetivo:** diagnosticar con confianza cualquier problema de conectividad entre contenedores, hacia el host, y hacia internet.

**Prerequisitos:** Etapa 1 (namespace `net`).

**Conceptos:**

| Driver | Qué hace | Cuándo se usa |
|---|---|---|
| **bridge** (default) | Crea una red virtual privada en el host; cada contenedor tiene su IP interna, NAT hacia afuera | Desarrollo local, comunicación entre contenedores de un mismo host |
| **host** | El contenedor comparte el namespace de red del host directamente | Máximo rendimiento de red, pero sin aislamiento de puertos |
| **none** | Sin interfaz de red (solo loopback) | Contenedores batch que no necesitan red |
| **overlay** | Red virtual entre múltiples hosts (Swarm) | Fuera del alcance de este roadmap (es terreno de orquestación) |

- **DNS interno de Docker**: al crear una red bridge *definida por el usuario* (no la `bridge` default), Docker levanta un servidor DNS embebido que resuelve nombres de contenedor/servicio a IPs automáticamente. Esto es lo que te permite en Compose escribir `postgres:5432` en vez de una IP.
- **La red `bridge` default NO tiene DNS embebido** — solo redes creadas por el usuario (`docker network create`) o las que crea Compose automáticamente. Es una fuente clásica de confusión.
- **Publicación de puertos (`-p`)**: crea una regla NAT (iptables) que redirige tráfico del puerto del host al puerto del contenedor. Sin publicar, el contenedor solo es alcanzable desde otros contenedores en la misma red.
- **Aislamiento de redes**: contenedores en redes distintas no se ven entre sí por defecto — es la forma nativa de segmentar, por ejemplo, "red de backend" vs "red de frontend público".

**Comandos:**
```bash
docker network ls
docker network create app-net
docker network inspect app-net
docker run -d --network app-net --name db postgres
docker run --network app-net --rm alpine ping db          # resuelve por DNS interno
docker network connect app-net otro-contenedor
docker port <container>
docker exec -it <container> sh -c "cat /etc/resolv.conf"
docker exec -it <container> sh -c "getent hosts db"
docker exec -it <container> sh -c "nslookup db"            # si está disponible
```
Memorizar: `network ls/create/inspect`, `-p`/`--network` en `run`. Consultar: sintaxis de `network connect/disconnect`, opciones avanzadas de subnetting.

**Ejercicios:**
1. Crea dos contenedores en la red `bridge` default y verifica que **no** pueden resolverse por nombre; luego repite en una red creada por ti y confirma que sí.
2. Levanta un contenedor en `--network none` e intenta hacer `ping 8.8.8.8` — documenta el error.
3. Publica el mismo puerto en dos contenedores distintos y observa el error de "address already in use".

**Laboratorio (diagnóstico dirigido):** crea un backend (Spring Boot o Go) y un Postgres en redes *distintas*, intenta que el backend se conecte, falla, y usa `docker network inspect` + `docker exec ... getent hosts` para diagnosticar y arreglarlo conectándolos a la misma red.

**Errores a provocar y solucionar (checklist de troubleshooting real):**
- "Connection refused" vs "Name or service not known" — aprende a distinguir un problema de DNS de uno de puerto/servicio caído.
- Contenedor que escucha en `127.0.0.1` dentro de sí mismo en vez de `0.0.0.0` → inalcanzable aunque el puerto esté publicado (error clásico de configuración de la app, no de Docker).
- Firewall del host (ufw/iptables) bloqueando el puerto publicado.
- Confundir el puerto interno con el externo en el mapeo `-p 8080:80`.

**Criterio de dominio:** dado un "no puedo conectar contenedor A con B", puedes seguir un proceso metódico (¿misma red? ¿DNS resuelve? ¿puerto correcto? ¿servicio escuchando en 0.0.0.0? ¿firewall?) sin necesidad de adivinar.

---

## Etapa 4 — Storage y persistencia

**Objetivo:** elegir correctamente entre volumes, bind mounts y tmpfs según el caso de uso, y diagnosticar problemas de permisos/persistencia.

**Prerequisitos:** Etapa 1 (overlay2).

**Conceptos:**

| Mecanismo | Dónde vive | Gestión | Caso de uso típico |
|---|---|---|---|
| **Writable container layer** | Dentro del filesystem del contenedor (overlay2) | Se destruye con el contenedor | Archivos temporales de la app en ejecución |
| **Volumes** | Gestionados por Docker en `/var/lib/docker/volumes/` | Docker CLI (`docker volume`) | Datos de bases de datos, persistencia entre recreaciones |
| **Bind mounts** | Ruta arbitraria del host | Tú gestionas la ruta | Desarrollo (montar tu código fuente), compartir config |
| **tmpfs** | Memoria RAM del host, nunca toca disco | Efímero, se pierde al parar el contenedor | Secretos temporales, cache que no debe persistir en disco |

- Los **volumes** son la opción recomendada para producción porque Docker los gestiona (backup, migración entre drivers, no dependen de la estructura de directorios del host).
- Los **permisos** son la causa #1 de bugs de storage: el proceso dentro del contenedor corre con un UID que puede no coincidir con el dueño del directorio en el host (especialmente con bind mounts). Esto se agrava al usar `USER` no-root en el Dockerfile.
- **Backup/restore de un volumen**: se hace montando el volumen en un contenedor auxiliar y usando `tar`.

**Comandos:**
```bash
docker volume create pgdata
docker volume ls
docker volume inspect pgdata
docker run -v pgdata:/var/lib/postgresql/data postgres
docker run -v $(pwd)/src:/app/src myapp        # bind mount
docker run --tmpfs /app/cache myapp
docker run --rm -v pgdata:/data -v $(pwd):/backup alpine tar czf /backup/pgdata.tar.gz -C /data .
docker volume prune
```
Memorizar: `volume create/ls/inspect`, `-v` en sus dos variantes (named volume vs bind mount, se distinguen porque el bind mount empieza con `/` o `./`). Consultar: sintaxis larga `--mount` (más explícita, útil en scripts de producción).

**Ejercicios:**
1. Levanta un Postgres con un volumen nombrado, inserta datos, elimina el contenedor (no el volumen), vuelve a crear el contenedor apuntando al mismo volumen y confirma que los datos persisten.
2. Repite el ejercicio anterior pero olvidando el volumen (sin `-v`) y confirma que los datos se pierden.
3. Provoca un error de permisos: usa `USER 1000` en el Dockerfile y monta un bind mount de un directorio propiedad de `root` en el host; diagnostica con `ls -la` dentro y fuera del contenedor.

**Laboratorio:** haz un backup completo de un volumen de Postgres con datos, destrúyelo, y restáuralo desde el backup usando un contenedor auxiliar con `tar`.

**Errores a provocar y solucionar:**
- "Permission denied" al escribir en un bind mount por mismatch de UID/GID.
- Confundir `docker run -v miapp:/data` (crea volumen si no existe) con un bind mount typo que crea sin querer un directorio local llamado `miapp`.
- Perder datos al hacer `docker compose down -v` sin darse cuenta de que `-v` borra volúmenes.

**Criterio de dominio:** dado un caso de uso, eliges correctamente entre volume/bind mount/tmpfs y justificas por qué; puedes diagnosticar y resolver un problema de permisos entre host y contenedor.

---

## Etapa 5 — Docker Compose a fondo

**Objetivo:** dominar Compose para levantar entornos multi-servicio realistas de desarrollo y testing, con troubleshooting incluido.

**Prerequisitos:** Etapas 2, 3, 4.

**Conceptos:**
- Compose es una capa declarativa sobre las mismas primitivas que ya conoces (crea las mismas redes, volúmenes y contenedores que harías a mano con `docker network create` + `docker run`).
- **`depends_on` no espera a que el servicio esté "listo"**, solo a que el contenedor haya arrancado — de ahí la necesidad de **healthchecks** combinados con `depends_on: condition: service_healthy`.
- **Redes en Compose**: por defecto crea una red bridge para todo el proyecto, con DNS interno automático (cada servicio es resoluble por su nombre de servicio).
- **Profiles**: permiten definir servicios opcionales (ej. herramientas de debug, seed de datos) que solo se levantan si activas el profile explícitamente — evita tener "cosas de más" corriendo siempre.
- **Variables de entorno y `.env`**: Compose lee automáticamente un archivo `.env` en el directorio del proyecto para interpolar variables (`${VAR}`) dentro del `compose.yaml`.
- **Diferencias dev vs producción**: en dev usas bind mounts para hot-reload y quizás `build:`; en producción usas imágenes ya construidas y publicadas (`image:` con tag fijo), sin bind mounts de código, con límites de recursos explícitos. Un patrón común es tener `compose.yaml` base + `compose.override.yaml` (dev, se aplica automático) + `compose.prod.yaml` (se aplica explícito con `-f`).

**Ejemplo de referencia (Spring Boot + Postgres + Redis):**
```yaml
services:
  backend:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/app
    depends_on:
      db:
        condition: service_healthy
    networks: [backend-net]

  db:
    image: postgres:16-alpine
    environment:
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 3s
      retries: 5
    networks: [backend-net]

  redis:
    image: redis:7-alpine
    profiles: ["cache"]
    networks: [backend-net]

volumes:
  pgdata:

networks:
  backend-net:
```

**Comandos:**
```bash
docker compose up -d
docker compose up --build
docker compose logs -f backend
docker compose ps
docker compose exec backend sh
docker compose down
docker compose down -v              # ¡cuidado! también borra volúmenes
docker compose --profile cache up -d
docker compose -f compose.yaml -f compose.prod.yaml up -d
docker compose config               # valida y muestra el YAML resuelto
```
Memorizar: `up/down/logs/ps/exec`, `--build`, `-f` múltiple. Consultar: sintaxis fina de `healthcheck`, `profiles`, interpolación de variables complejas.

**Ejercicios:**
1. Escribe un `compose.yaml` para tu stack real (Spring Boot o Go + Postgres) con healthcheck en la base de datos y `depends_on: condition: service_healthy` en el backend.
2. Crea un `compose.override.yaml` que monte tu código fuente como bind mount solo en desarrollo.
3. Usa `profiles` para que un servicio de administración (ej. `pgadmin`) solo se levante bajo demanda.

**Laboratorio:** provoca un `depends_on` "roto" (sin healthcheck) donde el backend arranca antes de que Postgres acepte conexiones, observa el fallo de conexión, y arréglalo con healthcheck.

**Errores a provocar y solucionar:**
- Servicios que no se ven entre sí por estar en `networks` distintas dentro del mismo `compose.yaml`.
- `docker compose down -v` accidental que borra datos de desarrollo.
- Variables de entorno no interpoladas por typo en `.env` (`docker compose config` para depurarlo).
- Puerto duplicado entre dos proyectos Compose corriendo a la vez.

**Criterio de dominio:** puedes escribir de memoria un `compose.yaml` multi-servicio con red, volumen, healthcheck y variables de entorno, y diagnosticar por qué un servicio no arranca en el orden esperado.

---

## Etapa 6 — Imágenes, registries y distribución

**Objetivo:** entender el modelo de capas/manifests a fondo y manejar registries privados y públicos con confianza.

**Prerequisitos:** Etapa 2.

**Conceptos:**
- **Layers**: cada instrucción del Dockerfile que modifica el filesystem genera una capa inmutable identificada por un hash de contenido; capas idénticas se comparten entre imágenes distintas (ahorro de espacio y de transferencia en push/pull).
- **Manifest**: documento JSON que lista las capas de una imagen y metadata (arquitectura, SO). Un **manifest list** (o "índice") agrupa manifests para distintas arquitecturas bajo un mismo tag — así `nginx:latest` sirve tanto para amd64 como arm64.
- **Tags vs digests**: un tag (`app:1.0`) es mutable — puede repuntar a otro contenido. Un **digest** (`app@sha256:...`) es inmutable y referencia exactamente ese contenido. En producción se recomienda "pinnear" por digest cuando la reproducibilidad es crítica.
- **Docker Hub vs GHCR vs registry privado**: mismo protocolo (Docker Registry HTTP API v2 / OCI Distribution Spec), difieren en límites de rate, autenticación y quién administra el acceso.
- **Autenticación**: `docker login` guarda credenciales (idealmente vía un credential helper, no en texto plano) usadas luego por `push`/`pull`.
- **Cache de pull**: capas ya presentes localmente no se vuelven a descargar — por eso compartir una imagen base entre varios servicios acelera despliegues.

**Comandos:**
```bash
docker pull postgres:16-alpine
docker tag app:1.0 ghcr.io/usuario/app:1.0
docker login ghcr.io -u usuario
docker push ghcr.io/usuario/app:1.0
docker inspect app:1.0 --format '{{.RepoDigests}}'
docker manifest inspect app:1.0
docker buildx build --platform linux/amd64,linux/arm64 -t app:1.0 --push .
docker system df                    # espacio usado por imágenes/contenedores/volúmenes
docker image prune -a               # limpieza de imágenes no usadas
```
Memorizar: `pull/tag/push/login`, `system df`, `image prune`. Consultar: sintaxis de `buildx` multi-arquitectura (la usas puntualmente).

**Ejercicios:**
1. Sube una imagen a GHCR (gratuito con cuenta de GitHub) y luego haz `pull` desde otra máquina o WSL limpio.
2. Compara el tamaño de `docker pull` de una imagen cuando ya tienes capas base compartidas vs cuando no.
3. Referencia una imagen por digest en un Dockerfile (`FROM postgres@sha256:...`) y explica la ventaja frente a `FROM postgres:latest`.

**Laboratorio:** monta un registry privado local con el contenedor oficial `registry:2`, sube y baja una imagen, e inspecciona su manifest.

**Errores a provocar y solucionar:**
- Usar `latest` en producción y que un `pull` posterior traiga una versión distinta sin que nadie lo note.
- `denied: requested access to the resource is denied` por falta de login o permisos del token.
- Confundir el nombre local de una imagen con el nombre completo que espera el registry (`usuario/app` vs `ghcr.io/usuario/app`).

**Criterio de dominio:** explicas la diferencia entre tag y digest sin dudar, sabes por qué se comparten capas entre imágenes, y puedes publicar/consumir imágenes desde un registry privado o GHCR sin ayuda.

---

## Etapa 7 — Seguridad

**Objetivo:** aplicar principios de mínimo privilegio y evitar los errores de seguridad más comunes en contenedores productivos.

**Prerequisitos:** Etapas 1, 2, 6.

**Conceptos:**
- **Root dentro del contenedor no es root del host** (gracias a namespaces), pero **sí puede ser peligroso**: si el atacante escapa del aislamiento (vulnerabilidad de kernel/runtime), heredar UID 0 agrava el impacto. Por eso `USER` no-root es buena práctica incluso sabiendo que hay aislamiento.
- **Capabilities**: Linux divide los privilegios de "root" en piezas (`CAP_NET_BIND_SERVICE`, `CAP_SYS_ADMIN`, etc.). Docker quita varias capabilities por defecto; puedes reducir aún más con `--cap-drop=ALL --cap-add=<solo lo necesario>`.
- **Seccomp**: filtra qué *syscalls* puede hacer el proceso del contenedor; Docker aplica un perfil seccomp por defecto que bloquea decenas de syscalls peligrosas.
- **User namespaces (remapping)**: opción avanzada donde el "root" del contenedor se mapea a un UID no privilegiado del host — capa extra de aislamiento, poco usada por complejidad operativa pero buena para entender qué existe.
- **Secrets**: nunca en `ENV`/`ARG` de la imagen final. Alternativas: `docker run --env-file` (fuera de la imagen), `BuildKit --mount=type=secret` (no persiste en capas), Compose `secrets:`, o gestores externos (Vault, SSM) inyectados en runtime.
- **Vulnerabilidades de imágenes / supply chain**: escanear imágenes (`docker scout`, Trivy) buscando CVEs conocidas en las dependencias del SO base y librerías. Usar imágenes oficiales o verificadas reduce superficie de ataque. Un **SBOM** (Software Bill of Materials) documenta qué contiene exactamente una imagen.
- **Pinning por digest**: evita que una imagen base cambie sin que te enteres (ver Etapa 6).

**Comandos:**
```bash
docker run --user 1000:1000 app:1.0
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE app:1.0
docker run --read-only --tmpfs /tmp app:1.0
docker scout cves app:1.0
docker scan app:1.0                 # según versión disponible
docker run --security-opt no-new-privileges app:1.0
docker run --env-file secrets.env app:1.0
```
Memorizar: `--user`, `--cap-drop/add`, `--read-only`. Consultar: perfiles seccomp/apparmor personalizados (se usan en casos puntuales de endurecimiento).

**Ejercicios:**
1. Toma una imagen que corre como root, cámbiala a no-root, y documenta qué falla (típicamente permisos de puertos <1024 o de escritura en directorios del sistema) y cómo lo arreglas.
2. Escanea una imagen base común (ej. una versión vieja de `node` o `python`) con Trivy o `docker scout` y cuenta cuántas CVEs tiene vs una versión `-alpine` reciente.
3. Ejecuta un contenedor con `--cap-drop=ALL` y observa qué operaciones dejan de funcionar.

**Laboratorio:** endurece el Dockerfile de tu proyecto Spring Boot/Go: usuario no-root, `--read-only` con `tmpfs` para directorios que sí necesitan escritura, capabilities mínimas, y verifica que la app sigue funcionando.

**Errores a provocar y solucionar:**
- App que intenta escribir logs en un filesystem `--read-only` y crashea — diagnosticar y resolver con un `tmpfs` puntual.
- Secreto filtrado en `docker history` por haberlo puesto en un `ARG` sin `--mount=type=secret`.
- Imagen base desactualizada con CVEs críticas detectadas por el scanner.

**Criterio de dominio:** puedes justificar, para una imagen dada, cada reducción de privilegio que le aplicarías (usuario, capabilities, filesystem) y explicar el impacto de cada una.

---

## Etapa 8 — Observabilidad, recursos y performance

**Objetivo:** diagnosticar problemas de consumo de CPU/RAM, logs mal gestionados, y contenedores "muertos por OOM" sin necesidad de adivinar.

**Prerequisitos:** Etapa 1 (cgroups).

**Conceptos:**
- **Límites de recursos** (`--memory`, `--cpus`) se implementan vía cgroups; superar el límite de memoria provoca un **OOM-kill** del proceso principal del contenedor (visible como exit code 137).
- **`docker stats`** da una vista en vivo de CPU%, memoria, I/O de red y disco por contenedor — primer comando ante "algo va lento".
- **Logging drivers**: por defecto Docker usa `json-file`, que si no se rota puede llenar el disco del host silenciosamente. Existen drivers alternativos (`journald`, `syslog`, drivers de terceros) según el ecosistema de observabilidad.
- **Healthchecks** no solo sirven para `depends_on` en Compose: también permiten a `docker ps` y a orquestadores saber si un contenedor "vivo" está realmente sano.
- **Exit codes** son información de diagnóstico: `0` normal, `1` error genérico de la app, `137` = SIGKILL (a menudo OOM), `143` = SIGTERM.

**Comandos:**
```bash
docker stats
docker run --memory=256m --cpus=0.5 app:1.0
docker inspect <container> --format '{{.State.OOMKilled}}'
docker inspect <container> --format '{{.State.ExitCode}}'
docker logs --tail 100 -f app
docker run --log-driver=json-file --log-opt max-size=10m --log-opt max-file=3 app
docker top <container>
docker system df -v
```
Memorizar: `stats`, `--memory/--cpus`, `logs --tail -f`, `inspect ... ExitCode`. Consultar: sintaxis de `--log-opt` según el driver.

**Ejercicios:**
1. Corre un contenedor con un límite de memoria deliberadamente bajo para una app que necesita más, provoca el OOM-kill, y confírmalo con `docker inspect ... OOMKilled`.
2. Configura rotación de logs (`max-size`/`max-file`) y genera logs masivos para verificar que efectivamente rota.
3. Compara `docker stats` de la misma app con y sin límites de CPU bajo carga (puedes usar `ab`/`hey`/`wrk` contra un endpoint simple).

**Laboratorio:** simula un memory leak controlado (script que reserva memoria en loop) dentro de un contenedor con límite fijo, observa el OOM-kill, y documenta el proceso de diagnóstico paso a paso como si fuera un incidente real.

**Errores a provocar y solucionar:**
- Disco del host lleno por logs `json-file` sin rotar.
- App que falla silenciosamente por OOM y el equipo interpreta el exit code como "bug de la app" en vez de límite de recursos.
- Confundir alto uso de CPU legítimo (carga real) con un proceso colgado en loop infinito — usar `docker top`/`exec ps` para diferenciarlos.

**Criterio de dominio:** ante un contenedor caído o lento, tu primer reflejo es `docker stats` + `docker inspect` + `docker logs`, y puedes explicar la diferencia entre un crash de aplicación y un OOM-kill por cgroups.

---

## Etapa 9 — Docker + CI/CD

**Objetivo:** automatizar build, test, tag y publicación de imágenes en pipelines de GitHub Actions y GitLab CI, con estrategia de promoción entre ambientes.

**Prerequisitos:** Etapas 2, 6, 7.

**Conceptos:**
- **Docker-in-Docker (DinD) vs socket montado vs runners con Docker nativo**: para construir imágenes dentro de un pipeline necesitas acceso a un daemon Docker. DinD levanta un daemon *dentro* del job (aislado pero con overhead y complejidad de privilegios); montar el socket del host (`/var/run/docker.sock`) es más simple pero comparte el daemon del runner (implicaciones de seguridad); runners gestionados (GitHub-hosted, GitLab SaaS) suelen dar Docker ya disponible sin que tengas que elegir.
- **Cache en CI**: reconstruir sin cache en cada pipeline es lento; se usa cache de capas (registry cache, `--cache-from`/`--cache-to` de BuildKit) o cache nativo del proveedor de CI.
- **Tagging estratégico**: tag por SHA de commit (trazabilidad exacta), tag por rama/entorno (`dev`, `staging`), y tag semántico (`1.4.2`) para releases — no dependas solo de `latest`.
- **Promoción entre ambientes**: la misma imagen (mismo digest) se prueba en `staging` y, si pasa, se re-tagea (no se reconstruye) para `production` — garantiza que lo que se probó es exactamente lo que se despliega.
- **Runners y Docker-in-Docker en GitLab** requieren configurar el executor como `docker` con el servicio `docker:dind` y variables como `DOCKER_TLS_CERTDIR`.

**Ejemplo GitHub Actions (build + push a GHCR):**
```yaml
name: build-and-push
on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - uses: actions/checkout@v4
      - uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: ghcr.io/${{ github.repository }}:${{ github.sha }}
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

**Ejemplo GitLab CI (equivalente, con DinD):**
```yaml
build:
  image: docker:24
  services:
    - docker:24-dind
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
```

**Comandos/prácticas clave:**
```bash
docker build --cache-from type=registry,ref=app:cache -t app:latest .
docker buildx build --cache-to type=registry,ref=app:cache,mode=max --push .
docker tag app:sha-abc123 app:staging
docker tag app:staging app:production   # promoción sin rebuild
```
Memorizar: patrón de login + build + push en pipeline. Consultar: sintaxis exacta de cache de BuildKit por proveedor (cambia entre GHA y GitLab).

**Ejercicios:**
1. Crea un pipeline de GitHub Actions que construya y publique tu imagen a GHCR en cada push a `main`, tageada por SHA de commit.
2. Añade cache de build al pipeline y mide la diferencia de tiempo entre el primer run y los siguientes.
3. Diseña (en papel o YAML) un flujo de promoción: build único → test en staging → re-tag a production, sin reconstruir la imagen.

**Laboratorio:** monta un pipeline completo (GitHub Actions o GitLab CI, el que uses en tu trabajo/estudio) que: build → corre tests dentro del contenedor → escanea vulnerabilidades → publica solo si todo pasa.

**Errores a provocar y solucionar:**
- Pipeline que reconstruye la imagen en cada etapa (staging y production con distinto contenido) rompiendo la garantía de "lo mismo que se probó es lo que se despliega".
- Fuga de credenciales del registry por loguearlas en texto plano en vez de usar secrets del CI.
- Cache mal configurado que nunca invalida, sirviendo builds obsoletos.

**Criterio de dominio:** puedes diseñar de punta a punta un pipeline de build-test-publish con tagging y cache correctos, y explicar la diferencia entre DinD y montar el socket del host, con sus implicaciones de seguridad.

---

## Etapa 10 — Puente Docker → Kubernetes

**Objetivo:** saber exactamente qué de todo lo anterior se traslada a Kubernetes y qué es un modelo mental distinto, para no arrastrar hábitos equivocados.

**Prerequisitos:** todas las etapas anteriores.

**Conceptos:**
- **Container runtime vs Docker**: Kubernetes no usa Docker directamente desde hace varias versiones — usa el **Container Runtime Interface (CRI)**, típicamente implementado por **containerd** o **CRI-O**. Docker Engine internamente ya usaba containerd (Etapa 1), así que el runtime de bajo nivel es el mismo mundo; lo que desaparece es la capa "Docker Engine/dockerd" como intermediario.
- **OCI (Open Container Initiative)**: especifica el formato de imagen y el runtime spec de forma neutral al proveedor — por eso una imagen construida con `docker build` funciona igual en Kubernetes sin importar qué runtime use el clúster.
- **Docker vs containerd (comparación directa)**: Docker Engine = experiencia de desarrollador completa (CLI, build, networking, volumes, Compose). containerd = solo el motor de ejecución de contenedores, sin build ni CLI amigable — es una pieza, no un producto para humanos.
- **Lo que SÍ se transfiere 1:1:** el formato de imagen, el Dockerfile y el proceso de build, el modelo de capas, el registry y su autenticación, los conceptos de namespaces/cgroups (Kubernetes los usa igual por debajo), buenas prácticas de imagen (multi-stage, no-root, tamaño), variables de entorno y secrets como concepto (aunque la implementación cambia).
- **Lo que NO se transfiere directamente:**
  - Docker Compose no es Kubernetes: Compose orquesta en *un* host; Kubernetes orquesta en un *clúster* con scheduling, autoescalado y auto-recuperación reales.
  - `docker network` bridge/host no existe como tal — Kubernetes tiene su propio modelo de red plano (cada Pod con IP propia) implementado por plugins CNI.
  - `docker volume` se convierte en el sistema de `PersistentVolume`/`PersistentVolumeClaim`, con un modelo de aprovisionamiento distinto (dinámico, vía `StorageClass`).
  - `docker run` imperativo se reemplaza por manifiestos declarativos (`Deployment`, `Pod`, `Service`) gestionados por un control plane que reconcilia el estado deseado constantemente — Compose es declarativo también, pero sin reconciliación activa ni auto-sanación.
  - Healthchecks de Docker (`HEALTHCHECK` en Dockerfile) se vuelven `livenessProbe`/`readinessProbe` de Kubernetes — el concepto es el mismo, la sintaxis y el mecanismo son distintos.

**Qué dominar de Docker ANTES de saltar a Kubernetes** (para que Kubernetes no se sienta como aprender todo desde cero):
1. Dockerfile multi-stage sólido y optimizado (Etapa 2).
2. Modelo de capas/manifests/digests (Etapa 6) — Kubernetes hereda esto tal cual.
3. Namespaces/cgroups conceptualmente (Etapa 1) — es la base de "requests/limits" en k8s.
4. Buenas prácticas de seguridad de imagen (Etapa 7) — se traduce directo a `securityContext` en k8s.
5. Networking conceptual (Etapa 3) — entender bridge/DNS interno te da la intuición para entender por qué k8s necesita su propio modelo de red distinto.

**Ejercicios:**
1. Escribe una tabla propia (sin mirar este documento) de "concepto Docker → equivalente Kubernetes (o N/A)".
2. Toma tu `compose.yaml` del proyecto final y, sin instalar Kubernetes todavía, identifica en voz alta/por escrito qué recurso de k8s reemplazaría a cada sección (esto es preparación mental, no ejecución).
3. Explica por qué una imagen bien construida en Docker (multi-stage, no-root, tamaño reducido) sigue siendo igual de valiosa en un clúster de Kubernetes.

**Criterio de dominio:** puedes explicar sin ayuda qué partes de tu conocimiento actual de Docker son directamente reutilizables en Kubernetes y cuáles requieren un modelo mental nuevo, sin subestimar ni sobrestimar la transferencia.

---

## B. Plan de estudio de 8-12 semanas

Ritmo sugerido: 6-8 horas/semana. Ajusta según tu disponibilidad real; es preferible extender a 12 semanas con consistencia que forzar 8 con huecos.

| Semana | Foco | Entregable |
|---|---|---|
| 1 | Etapa 0 + Etapa 1 | Diagrama propio de la arquitectura Docker + ejercicios de namespaces |
| 2 | Etapa 2 (Dockerfile Java) | Dockerfile multi-stage optimizado para tu app Spring Boot |
| 3 | Etapa 2 (Dockerfile Go) + repaso | Dockerfile multi-stage optimizado para tu app Go, comparación de tamaños |
| 4 | Etapa 3 (Networking) | Laboratorio de diagnóstico de red resuelto y documentado |
| 5 | Etapa 4 (Storage) | Backup/restore de volumen documentado paso a paso |
| 6 | Etapa 5 (Compose) | `compose.yaml` completo del stack backend + DB + cache con healthchecks |
| 7 | Etapa 6 (Registries) | Imagen publicada en GHCR, pinneada por digest en un proyecto |
| 8 | Etapa 7 (Seguridad) | Dockerfile + `compose.yaml` endurecidos (no-root, cap-drop, secrets) |
| 9 | Etapa 8 (Observabilidad) | Incidente simulado de OOM documentado con diagnóstico completo |
| 10 | Etapa 9 (CI/CD) | Pipeline funcional de build-test-push en GitHub Actions o GitLab CI |
| 11 | Etapa 10 (Puente a K8s) + inicio proyecto final | Tabla Docker→Kubernetes propia + arquitectura del proyecto final definida |
| 12 | Proyecto final completo | Repositorio de portfolio terminado y documentado |

Si dispones de 8 semanas en vez de 12, fusiona: semanas 2-3 en una, semana 9 dentro de la 8, y semana 11 dentro de la 10.

---

## C. Laboratorios prácticos

Organizados por dificultad creciente; cada uno debería resolverse sin copiar soluciones, solo con el roadmap como referencia conceptual.

1. **Ciclo de vida básico**: levantar, pausar, reanudar, matar y limpiar contenedores; explicar cada estado.
2. **Inspección de namespaces**: comparar `/proc/<pid>/ns/` de un proceso en el host vs dentro de un contenedor.
3. **Dockerfile ingenuo → multi-stage**: reducir el tamaño de una imagen Java o Go en al menos un 70%.
4. **Cache roto a propósito**: reordenar instrucciones para romper el cache intencionalmente, medir el impacto en tiempo de build, y luego arreglarlo.
5. **Red default vs red de usuario**: demostrar con `ping`/`getent hosts` la diferencia de resolución DNS.
6. **Aislamiento de redes**: dos grupos de contenedores en redes separadas que no pueden verse entre sí.
7. **Volumen persistente vs efímero**: demostrar pérdida y persistencia de datos según se use o no un volumen.
8. **Backup/restore real**: de un volumen de Postgres con datos de prueba.
9. **Permisos rotos**: bind mount con mismatch de UID, diagnóstico y arreglo.
10. **Compose multi-servicio con healthchecks**: backend + DB + cache, arranque ordenado y confiable.
11. **Compose dev vs prod**: mismo proyecto con `override` para desarrollo y archivo separado para producción.
12. **Publicación en GHCR**: build, tag, push, pull desde otra máquina/entorno.
13. **Pinning por digest**: reescribir un Dockerfile para referenciar su imagen base por digest en vez de tag.
14. **Endurecimiento de seguridad**: usuario no-root + cap-drop + filesystem read-only en una app real.
15. **Escaneo de vulnerabilidades**: comparar CVEs entre una imagen base vieja y una actualizada/alpine.
16. **OOM-kill provocado**: límite de memoria bajo, script que consume memoria, diagnóstico con `inspect`.
17. **Rotación de logs**: configurar `max-size`/`max-file` y verificar que efectivamente limita el disco usado.
18. **Pipeline CI/CD completo**: build, test dentro del contenedor, scan, push condicional a que todo pase.
19. **Promoción de imagen sin rebuild**: mismo digest re-tageado de staging a production.
20. **Registry privado local**: levantar `registry:2`, publicar y consumir una imagen propia.

---

## D. Preguntas de entrevista técnica (intermedio/alto)

**Arquitectura y fundamentos**
1. Explica el flujo completo desde que ejecutas `docker run` hasta que el proceso está corriendo, mencionando cada componente involucrado.
2. ¿Qué diferencia hay entre `containerd` y `runc`, y por qué existen como capas separadas?
3. ¿Qué son los namespaces y los cgroups, y qué problema resuelve cada uno por separado?
4. ¿Por qué "root dentro del contenedor" no es lo mismo que "root en el host"? ¿Es eso suficiente aislamiento por sí solo?

**Imágenes y build**
5. ¿Cómo funciona el cache de capas en Docker y qué estrategias usarías para maximizarlo en un Dockerfile de Java o Go?
6. ¿Qué es un multi-stage build y qué problema concreto resuelve?
7. ¿Cuál es la diferencia entre `ARG` y `ENV`, y por qué poner un secreto en `ARG` es un riesgo?
8. ¿Qué diferencia hay entre un tag y un digest de imagen, y cuándo usarías cada uno?

**Networking**
9. ¿Por qué dos contenedores en la red `bridge` por defecto no pueden resolverse por nombre, pero sí en una red creada por el usuario?
10. Un contenedor no responde en el puerto publicado aunque el proceso está corriendo dentro. ¿Qué pasos seguirías para diagnosticarlo?
11. ¿Qué diferencia hay entre `--network host` y la red bridge por defecto en términos de aislamiento y rendimiento?

**Storage**
12. ¿Cuándo usarías un volume en vez de un bind mount, y viceversa?
13. ¿Por qué pueden aparecer errores de permisos al usar bind mounts con un usuario no-root dentro del contenedor?

**Compose y orquestación local**
14. ¿Por qué `depends_on` no garantiza que un servicio esté realmente listo, y cómo lo solucionas?
15. ¿Cómo estructurarías archivos de Compose para diferenciar entornos de desarrollo y producción sin duplicar todo el YAML?

**Seguridad**
16. Menciona tres prácticas concretas para reducir la superficie de ataque de un contenedor en producción.
17. ¿Cómo gestionarías un secreto (por ejemplo, una contraseña de base de datos) sin que termine expuesto en la imagen final?
18. ¿Qué es el "pinning" por digest y qué problema de supply chain mitiga?

**Observabilidad y recursos**
19. Un contenedor muere inesperadamente con exit code 137. ¿Qué significa y cómo lo confirmarías?
20. ¿Qué comando usarías para diagnosticar en vivo qué contenedor está consumiendo más CPU/memoria en un host?

**CI/CD**
21. ¿Qué diferencia hay entre construir imágenes con Docker-in-Docker y montar el socket del daemon del host en un runner de CI? ¿Qué implicaciones de seguridad tiene cada uno?
22. Describe una estrategia de tagging y promoción de imágenes entre ambientes que evite reconstruir la imagen en cada etapa.

**Puente a Kubernetes**
23. ¿Kubernetes usa Docker directamente? Explica el rol de CRI, containerd y OCI en esa respuesta.
24. ¿Qué conceptos de Docker se transfieren casi directamente a Kubernetes y cuáles requieren un modelo mental distinto?

---

## E. Proyecto final de portfolio

**Nombre sugerido:** *"Order Service Platform"* — un backend de procesamiento de pedidos con persistencia, cache y CI/CD, pensado para simular un entorno profesional real, no un demo de juguete.

**Stack:**
- Backend en Spring Boot **o** Go (elige el que quieras reforzar más).
- PostgreSQL como base de datos principal.
- Redis como cache/cola simple.
- Nginx como reverse proxy delante del backend (opcional pero recomendable: te da práctica extra de networking y publicación de puertos).

**Requisitos técnicos que debe cumplir:**

1. **Dockerfiles optimizados**
   - Multi-stage para el backend.
   - Imagen final basada en `-alpine`/distroless según el lenguaje.
   - Usuario no-root.
   - `.dockerignore` correctamente configurado.

2. **Docker Compose**
   - Servicios: `backend`, `db`, `redis`, `nginx` (opcional).
   - Redes separadas: una red "pública" para nginx↔backend, otra "privada" para backend↔db/redis, sin exponer la DB directamente.
   - Volumen nombrado para persistencia de Postgres.
   - Healthchecks en `db` y `redis`, con `depends_on: condition: service_healthy` en `backend`.
   - Variables de entorno vía `.env`, sin credenciales hardcodeadas.
   - `compose.override.yaml` para desarrollo (bind mount de código) y `compose.prod.yaml` para producción (sin bind mounts, con límites de recursos).

3. **Seguridad**
   - Contraseñas gestionadas como secrets (Compose `secrets:` o `.env` fuera del control de versiones).
   - `cap-drop=ALL` con solo las capabilities estrictamente necesarias.
   - Imagen escaneada (Trivy o `docker scout`) sin CVEs críticas conocidas.

4. **Logging**
   - Configuración explícita de rotación de logs (`max-size`/`max-file`).

5. **CI/CD**
   - Pipeline (GitHub Actions o GitLab CI) que: instala dependencias, corre tests, construye la imagen, la escanea, y la publica en GHCR (o registry equivalente) tageada por SHA de commit, solo si todo lo anterior pasa.
   - Estrategia de promoción documentada (aunque sea conceptual) entre un entorno "staging" y uno "production".

6. **Documentación (README del repositorio)**
   - Diagrama de arquitectura (servicios, redes, volúmenes).
   - Instrucciones de arranque en desarrollo y en "producción" (local, simulada).
   - Explicación de las decisiones de diseño de seguridad tomadas.
   - Sección de troubleshooting con al menos 3 problemas reales que encontraste y cómo los resolviste (esto demuestra el pensamiento de debugging, no solo el resultado final).

**Por qué este proyecto sirve como portfolio:** cubre networking real (aislamiento de redes), storage real (persistencia + backup mencionado en docs), seguridad aplicada (no solo teoría), CI/CD funcional, y — crucialmente — documentación de troubleshooting, que es exactamente lo que un entrevistador técnico quiere ver para diferenciar a alguien que "levantó un tutorial" de alguien que entendió el problema.

---

## F. Checklist de conocimientos "sin documentación"

Deberías poder explicar cada uno de estos puntos en voz alta, sin buscar nada, como si se lo explicaras a un compañero:

- [ ] El flujo completo CLI → dockerd → containerd → runc → namespaces/cgroups.
- [ ] Qué son namespaces y cgroups, y qué aísla/limita cada uno.
- [ ] Por qué un Dockerfile multi-stage reduce el tamaño final de la imagen.
- [ ] Cómo funciona el cache de capas y cómo ordenar instrucciones para aprovecharlo.
- [ ] Diferencia entre `ARG` y `ENV`, y por qué un secreto en `ARG` es un riesgo.
- [ ] Diferencia entre tag y digest, y cuándo pinnear por digest.
- [ ] Por qué la red `bridge` default no resuelve nombres pero una red de usuario sí.
- [ ] Cómo diagnosticar, paso a paso, "no puedo conectar contenedor A con B".
- [ ] Diferencia entre volume, bind mount y tmpfs, con un caso de uso para cada uno.
- [ ] Por qué `depends_on` sin healthcheck no garantiza que un servicio esté listo.
- [ ] Qué significa el exit code 137 y cómo confirmar que fue un OOM-kill.
- [ ] Al menos tres prácticas de endurecimiento de seguridad de un contenedor (usuario, capabilities, filesystem).
- [ ] Cómo gestionar secretos sin que queden en la imagen o en su historial de capas.
- [ ] Diferencia entre construir imágenes con Docker-in-Docker vs montar el socket del host en CI, y el trade-off de seguridad.
- [ ] Una estrategia de tagging y promoción de imágenes entre ambientes sin reconstruir.
- [ ] Qué partes de Docker se transfieren directamente a Kubernetes (imagen, registry, capas) y cuáles no (networking, storage, orquestación).

---

## Qué NO necesitas aprender todavía

- Docker Swarm (orquestación nativa de Docker) — quedó relegado frente a Kubernetes en el ecosistema profesional; no es tiempo bien invertido para tu objetivo.
- Redes `overlay` multi-host — son terreno de orquestación (Swarm o, conceptualmente, k8s), no de Docker standalone.
- User namespace remapping en producción — es una opción avanzada real pero de nicho operativo; entiéndela conceptualmente (ya está en Etapa 7), no la implementes a fondo todavía.
- Perfiles seccomp/AppArmor personalizados desde cero — saber que existen y qué hacen (Etapa 7) es suficiente; escribir uno propio es trabajo de seguridad muy especializado.
- Optimización extrema de storage drivers (comparar overlay2 vs otros drivers en detalle) — overlay2 es el estándar de facto hoy; no gastes tiempo en drivers legacy.
- Cualquier concepto específico de Kubernetes (Services, Ingress, Helm, Operators, etc.) — se aprende mejor una vez Docker esté consolidado, como indica tu propio plan.

---

## Errores comunes al aprender Docker

1. **Memorizar comandos sin entender el modelo subyacente** — lleva a que cualquier error fuera del "camino feliz" resulte indescifrable.
2. **Usar `latest` en todo, incluso en proyectos serios** — rompe la reproducibilidad y complica el debugging ("en mi máquina funciona").
3. **No usar `.dockerignore`** — builds lentos y cache que se invalida por archivos irrelevantes (logs, `.git`, artefactos de build previos).
4. **Ignorar el tamaño de imagen** — imágenes de gigabytes por no usar multi-stage o por partir de una base innecesariamente grande.
5. **Correr todo como root "porque funciona"** — funciona hasta que hay un incidente de seguridad, y para entonces es un hábito difícil de corregir.
6. **Confundir volumes con bind mounts** y no entender por qué los datos aparecen o desaparecen según el caso.
7. **No entender `depends_on`** y asumir que garantiza orden *funcional*, no solo orden de arranque del proceso.
8. **Meter secretos en el Dockerfile o en variables de entorno de la imagen** sin saber que quedan expuestos en el historial de capas.
9. **No usar healthchecks** y depender de reinicios manuales cuando algo falla silenciosamente.
10. **Aprender Docker Compose pensando que es "Kubernetes fácil"** — genera falsas expectativas y confusión de conceptos al llegar a k8s de verdad.
11. **No practicar troubleshooting deliberado** — solo se aprende a diagnosticar problemas provocándolos intencionalmente, no evitándolos.

---

## Qué diferencia a quien "usa" Docker de quien lo entiende

- Quien "usa" Docker sabe qué comando escribir. Quien lo entiende sabe **por qué** ese comando hace lo que hace a nivel de kernel (namespaces/cgroups) y puede predecir el comportamiento en un caso nuevo que nunca vio en un tutorial.
- Quien "usa" Docker copia Dockerfiles de internet. Quien lo entiende puede **justificar cada línea**: por qué esa base, por qué ese orden, por qué ese usuario, qué pasaría si quitara una instrucción.
- Quien "usa" Docker se atasca ante un error de red o de permisos. Quien lo entiende tiene un **proceso de diagnóstico repetible** (¿misma red? ¿DNS resuelve? ¿puerto correcto? ¿usuario/UID correcto? ¿recursos agotados?) que aplica sin importar la app específica.
- Quien "usa" Docker piensa en Compose como "el archivo que levanta todo". Quien lo entiende sabe que Compose es una capa declarativa sobre las mismas primitivas de `run`/`network`/`volume`, y por eso puede razonar sobre fallos de Compose bajando al nivel de esas primitivas.
- Quien "usa" Docker ve la seguridad como un checklist opcional al final. Quien lo entiende diseña el Dockerfile y el Compose pensando en privilegio mínimo **desde el principio**, porque entiende el costo real de no hacerlo.
- Quien "usa" Docker ve Kubernetes como "otro Docker más grande". Quien realmente entiende Docker sabe distinguir con precisión qué de su conocimiento se transfiere (imágenes, capas, registries, seguridad de contenedor) y qué requiere aprender un modelo completamente nuevo (orquestación, redes de clúster, almacenamiento declarativo) — y eso es exactamente lo que te preparará bien para tu siguiente paso.

---

*Fin del roadmap. Sugerencia de uso: trátalo como documento vivo — a medida que completes cada etapa, márcala y anota tus propios hallazgos de troubleshooting en la sección de checklist; esas notas personales son, al final, más valiosas que el documento en sí.*
