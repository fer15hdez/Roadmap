# Roadmap DevOps: Bash y Python

Oct 8, 2026

Este roadmap te lleva en unos 9 a 12 meses (6 a 8 horas por semana) de scripts básicos a herramientas Bash y Python que puedes mostrar en un portafolio DevOps. Cada nivel cierra con un proyecto y con criterios medibles para avanzar.

## Cómo usar el roadmap

**Entorno recomendado:** WSL2 con Ubuntu como terminal principal, Docker Desktop con integración WSL, `kind` o `minikube` para Kubernetes local, VS Code con la extensión de WSL y un repositorio en GitLab (gitlab.com es suficiente). Usa PowerShell solo para lo propio de Windows; todo el aprendizaje de Bash ocurre en WSL.

**Metodología 30% teoría, 70% práctica.** Cada semana: 2 horas de lectura y demostración, 5 de ejercicios y proyecto. Nunca leas un tema sin ejecutar al menos tres comandos o líneas de código sobre él.

**Ciclo de cada ejercicio:**

1. Lee el enunciado y escribe en una línea qué entra y qué sale.
2. Intenta resolverlo 20 minutos sin ayuda.
3. Pide la **pista 1** (solo la dirección). Intenta otros 15 minutos.
4. Pide la **pista 2** (la herramienta o función concreta). Intenta otros 15 minutos.
5. Pide la **pista 3** (esqueleto parcial).
6. Pide la **solución de referencia**, compárala con la tuya y lee las tres preguntas: por qué funciona, qué alternativa existe y cuándo fallaría.

**Reglas de calidad desde el primer día:** todo script pasa ShellCheck (Bash) o `ruff` (Python), vive en Git, tiene un README de 10 líneas y se puede ejecutar dos veces seguidas sin romper nada (idempotencia).

**Regla de avance:** pasas al siguiente nivel solo si cumples los criterios del nivel actual. Si fallas uno, repites el ejercicio asociado, no el nivel entero.

**Estructura estándar de cada nivel:** objetivo, prerrequisitos, conceptos con su caso DevOps, ejercicios (con solución de referencia después de cada uno), proyecto y criterios de avance.

## 🎯 Niveles 1 a 4: Bash

Bash ocupa los primeros cuatro niveles (unas 14 semanas) porque es la base de todo lo que haces en contenedores, CI y servidores.

### 🎯 Nivel 1: Fundamentos de Bash (semanas 1 a 3)

**Objetivo:** escribir scripts cortos, correctos y legibles que reciban parámetros y devuelvan exit codes significativos.

**Prerrequisitos:** moverte por Linux (`cd`, `ls`, `cat`, `chmod`), editar con `nano` o VS Code.

| Concepto | Caso DevOps |
| --- | --- |
| Shell, shebang, `set -euo pipefail` | Que un script de despliegue falle en el primer error en lugar de seguir roto |
| Variables y expansiones (`${var:-default}`, `$(cmd)`) | Parametrizar el entorno (dev, staging, prod) |
| Comillas simples, dobles y sin comillas | Evitar que un nombre con espacios borre el directorio equivocado |
| Condicionales (`if`, `[[ ]]`, `case`) | Validar que existe un archivo o variable antes de desplegar |
| Bucles (`for`, `while read`) | Recorrer una lista de servidores o contenedores |
| Funciones | Reutilizar `log()` y `die()` en todos tus scripts |
| Arrays y parámetros (`$1`, `$@`, `getopts`) | Aceptar `--env prod --dry-run` desde CI |
| Exit codes, stdin, stdout, stderr | Que GitLab CI marque el job como fallido cuando corresponde |
| Pipes y redirecciones (`>`, `>>`, `2>&1`, `<<<`) | Separar la salida útil de los errores en logs |

**Ejercicios:**

1. Script `hola.sh` que reciba un nombre y valide que no esté vacío.
2. Función `die()` que escriba a stderr y salga con código 1.
3. Recorrer un array de hosts y mostrar cuáles responden a `ping -c1`.
4. Script con `getopts` que acepte `-e entorno` y `-d` (dry-run).
5. Demostrar la diferencia entre `$@` y `$*` con rutas con espacios.

**Proyecto 1:** script de backup (ficha en la sección de proyectos).

**Criterios para avanzar:**

- Escribes sin ayuda un script con parámetros, validación y exit codes en menos de 30 minutos.
- ShellCheck no reporta advertencias en tus 5 ejercicios.
- Explicas con tus palabras la diferencia entre `"$var"`, `$var` y `'$var'`.
- Tu proyecto 1 se ejecuta dos veces seguidas sin errores.

### Nivel 2: Bash para administración Linux (semanas 4 a 7)

**Objetivo:** procesar texto, gestionar archivos, procesos, permisos y servicios desde scripts.

**Prerrequisitos:** nivel 1 completo.

| Concepto | Caso DevOps |
| --- | --- |
| `grep`, `sed`, `awk`, `cut`, `sort`, `uniq`, `tr` | Contar los errores 5xx por endpoint en un log de Nginx |
| `find` y `xargs` | Borrar logs de más de 14 días de forma segura |
| `jq` | Extraer campos de la salida JSON de `docker inspect` o de una API |
| Procesos y señales (`ps`, `kill`, `pgrep`, `trap`) | Limpiar archivos temporales si el script se interrumpe |
| Permisos y propiedad (`chmod`, `chown`, `umask`) | Dejar claves SSH con `600` y detectar permisos peligrosos |
| Archivos y logs (`/var/log`, `logrotate`, `journalctl`) | Diagnosticar por qué un servicio no arrancó |
| `cron` y `systemd` (units y timers) | Programar backups y reiniciar servicios caídos |

**Ejercicios:**

1. Del log de acceso de ejemplo, listar las 10 IP con más peticiones (`awk | sort | uniq -c | sort -rn | head`).
2. Reemplazar un valor en 20 archivos de configuración con `sed -i` creando antes una copia `.bak`.
3. Con `find ... -print0 | xargs -0`, comprimir archivos de más de 7 días.
4. Script que use `trap` para borrar su directorio temporal al salir o al recibir SIGINT.
5. Crear un `systemd` timer que ejecute tu backup cada noche y revisar su salida con `journalctl -u`.

**Proyecto 2:** script de monitorización.

**Criterios para avanzar:**

- Resuelves 4 de 5 ejercicios de procesamiento de texto sin consultar la solución.
- Tu script de monitorización detecta CPU, memoria, disco y servicios caídos, y guarda un log con marca de tiempo.
- Sabes explicar cuándo usar `cron` y cuándo un `systemd` timer.

### Nivel 3: Bash para automatización DevOps (semanas 8 a 11)

**Objetivo:** automatizar tareas remotas y de contenedores con manejo de errores, logging y depuración profesional.

**Prerrequisitos:** niveles 1 y 2.

| Concepto | Caso DevOps |
| --- | --- |
| SSH, SCP, `rsync` (claves, `ssh-agent`, `~/.ssh/config`) | Desplegar archivos en varios servidores sin contraseñas |
| Manejo de errores (\` |  |
| Logging con niveles y marcas de tiempo | Auditar qué hizo un script en producción |
| Debugging (`set -x`, `PS4`, `bash -n`) | Encontrar por qué un script funciona local y falla en CI |
| ShellCheck y `shfmt` | Revisar scripts en cada merge request |
| `curl` + `jq` para health checks | Esperar a que un servicio responda 200 antes de continuar |
| `docker` desde Bash | Limpiar imágenes huérfanas, reiniciar contenedores no saludables |

**Ejercicios:**

1. Función `retry N delay cmd` y úsala con `curl`.
2. Script que ejecute un comando por SSH en una lista de hosts y resuma éxitos y fallos.
3. Logger con niveles INFO, WARN y ERROR que escriba a archivo y a stderr.
4. Health check que consulte `/health` y falle si la latencia supera un umbral.
5. Script que liste contenedores con estado `unhealthy` y los reinicie, con modo `--dry-run`.

**Proyectos 3 y 4:** health checks y automatización Bash para Docker.

**Criterios para avanzar:**

- Tus scripts usan `set -euo pipefail`, validan entradas, tienen `--help` y logs con nivel.
- Puedes depurar un script roto que no has escrito en menos de 20 minutos.
- Ejecutas un health check contra un servicio real y devuelve un código de salida utilizable por CI.

### Nivel 4: Bash para CI/CD (semanas 12 a 14)

**Objetivo:** integrar scripts Bash en pipelines de GitLab CI de forma segura y reproducible.

**Prerrequisitos:** niveles 1 a 3, un repositorio en GitLab, imagen Docker propia.

| Concepto | Caso DevOps |
| --- | --- |
| Scripts en `.gitlab-ci.yml` (`script`, `before_script`, `after_script`) | Separar lógica compleja del YAML y llamarla desde `scripts/` |
| Variables de CI y variables protegidas o enmascaradas | Usar tokens sin escribirlos en el repositorio |
| Exit codes y `allow_failure` | Controlar qué falla bloquea el despliegue |
| Artefactos y caché | Pasar resultados entre jobs |
| Linting en pipeline (ShellCheck, `hadolint`, `yamllint`) | Rechazar merge requests con scripts defectuosos |
| Scripts idempotentes y `--dry-run` | Reejecutar un job sin duplicar efectos |
| Gestión básica de secretos (nunca en logs, `set +x` en secciones sensibles) | Evitar filtrar credenciales en la salida del job |

**Ejercicios:**

1. Pipeline con etapas `lint`, `test` y `build` que ejecute ShellCheck sobre `scripts/`.
2. Job que construya una imagen Docker y la suba al registry de GitLab usando variables predefinidas.
3. Job de despliegue que llame a `scripts/deploy.sh` con el entorno como parámetro.
4. Modificar un script para que no imprima un token aunque se active `set -x`.

**Criterios para avanzar:**

- Tienes un pipeline verde con lint, build y un despliegue simulado.
- Ningún secreto aparece en el repositorio ni en los logs de ningún job.
- Puedes explicar qué ocurre cuando un job falla a mitad y cómo reejecutarlo con seguridad.

## Niveles 5 a 8: Python

Python ocupa unas 18 semanas. Usa la librería estándar primero y añade paquetes externos solo cuando ahorran trabajo real en DevOps (`requests`, `PyYAML`, `pytest`, `docker`, `kubernetes`, `paramiko` o `fabric`).

### Nivel 5: Fundamentos de Python (semanas 15 a 18)

**Objetivo:** escribir programas pequeños y ordenados con funciones, módulos, excepciones y estructuras de datos.

**Prerrequisitos:** nivel 4 (Bash). Lógica de scripts, parámetros y exit codes ya dominada.

| Concepto | Caso DevOps |
| --- | --- |
| Variables y tipos (`str`, `int`, `float`, `bool`, `None`) | Leer puertos, umbrales y nombres desde configuración |
| Condicionales y bucles | Decidir si un servicio está sano y reintentar |
| Funciones, módulos y paquetes | Separar `checks.py`, `config.py` y `main.py` |
| Listas, diccionarios, sets y tuplas | Agrupar servidores por entorno; detectar duplicados con sets |
| Comprensiones | Filtrar pods o contenedores en una línea legible |
| Excepciones (`try`, `except`, `finally`, propias) | Distinguir un timeout de una respuesta 404 |
| Entornos virtuales (`venv`, `pip`, `requirements.txt`) | Reproducir el mismo entorno en tu máquina y en CI |

**Ejercicios:**

1. Función que reciba una lista de puertos y devuelva los que están fuera de 1024 a 65535.
2. Agrupar un diccionario de servidores por entorno con comprensiones.
3. Función con `try/except` que lea un archivo y falle con un mensaje claro si no existe.
4. Crear un paquete con dos módulos y un `__main__.py` ejecutable con `python -m`.
5. Reescribir en Python tu script Bash de monitorización de disco.

**Criterios para avanzar:**

- Escribes funciones con docstring y manejo de excepciones sin ayuda.
- Explicas la diferencia entre lista, tupla, set y diccionario con un ejemplo de cada uno.
- Tu entorno virtual se recrea desde `requirements.txt` en un directorio limpio.

### Nivel 6: Python para automatización (semanas 19 a 22)

**Objetivo:** reemplazar scripts Bash complejos por herramientas Python con argumentos, archivos de configuración, logs y pruebas.

**Prerrequisitos:** nivel 5.

| Concepto | Caso DevOps |
| --- | --- |
| `pathlib`, `os`, `sys`, `shutil` | Recorrer, copiar y limpiar directorios sin depender de `find` |
| Archivos, JSON, YAML y CSV | Leer `values.yaml`, manifiestos o inventarios de servidores |
| `argparse` | CLI con subcomandos (`backup`, `restore`, `check`) |
| `logging` (handlers, niveles, formato) | Logs con marca de tiempo y nivel para auditoría |
| `subprocess` (`run`, `check`, `capture_output`, sin `shell=True`) | Llamar `docker`, `kubectl` o `rsync` de forma segura |
| Expresiones regulares (`re`) | Extraer errores y direcciones IP de logs |
| Variables de entorno (`os.environ`, `.env`) | Leer tokens y URLs sin escribirlos en el código |
| `pytest` y `unittest.mock` | Probar tu lógica sin tocar servidores reales |

**Ejercicios:**

1. Convertir un YAML de configuración a JSON y validar que existan las claves obligatorias.
2. Parsear un log con `re` y contar errores por hora.
3. CLI con `argparse` y subcomandos `check` y `report`.
4. Envolver `subprocess.run(['docker','ps','--format','{{json .}}'])` y devolver una lista de diccionarios.
5. Escribir 5 tests de `pytest` para tus funciones, incluido un caso con `mock`.

**Criterios para avanzar:**

- Tu herramienta tiene `--help`, logs, manejo de errores y al menos 5 tests que pasan.
- No usas `shell=True` ni dejas secretos en el código.
- Puedes explicar cuándo Bash es mejor que Python y cuándo no.

### Nivel 7: Python para APIs y servicios (semanas 23 a 26)

**Objetivo:** consumir y exponer APIs REST con autenticación, reintentos, validación y tipado.

**Prerrequisitos:** nivel 6.

| Concepto | Caso DevOps |
| --- | --- |
| `requests` (sesiones, timeouts, cabeceras) | Llamar a las APIs de GitLab, GitHub o Docker Registry |
| APIs REST (métodos, códigos de estado, paginación) | Listar todos los pipelines fallidos de un proyecto |
| Autenticación (token, Bearer) y secretos | Usar un token de GitLab desde variables de entorno |
| Reintentos con espera exponencial y manejo de límites de tasa | No caer cuando la API responde 429 o 503 |
| `typing` y `dataclasses` | Modelar `Servicio`, `Pipeline` o `ResultadoCheck` con tipos claros |
| POO básica (clases, métodos, composición) | Clase `GitLabClient` con métodos pequeños |
| Webhooks (`http.server` o Flask mínimo) | Recibir un evento de GitLab y disparar una acción |

**Ejercicios:**

1. Consumir una API pública, paginar y guardar el resultado en JSON.
2. Cliente con sesión, timeout y reintentos en errores 5xx.
3. `dataclass` para el resultado de un health check con tipos y valores por defecto.
4. Consultar la API de GitLab y listar los últimos 10 pipelines con su estado.
5. Webhook mínimo que reciba un POST, valide un secreto y registre el evento.

**Proyecto 5:** script Python para consumir una API.

**Criterios para avanzar:**

- Tu cliente maneja timeouts, 4xx, 5xx y paginación, con tests que simulan cada caso.
- Todas las funciones públicas tienen anotaciones de tipo y pasan `mypy` sin errores graves.
- El webhook rechaza peticiones sin el secreto correcto.

### Nivel 8: Python para DevOps (semanas 27 a 32)

**Objetivo:** construir herramientas que gestionen servidores, Docker y Kubernetes, con configuración, pruebas y empaquetado.

**Prerrequisitos:** niveles 5 a 7 y base de Kubernetes (pods, deployments, services, namespaces).

| Concepto | Caso DevOps |
| --- | --- |
| SSH con `paramiko` o `fabric` | Ejecutar comandos y copiar archivos en varios servidores |
| SDK de Docker (`docker`) | Listar, arrancar, parar y limpiar contenedores e imágenes |
| Cliente de Kubernetes (`kubernetes`) | Listar pods con problemas, escalar deployments, leer eventos |
| Validación de configuración (`jsonschema`, `pydantic`) | Rechazar un YAML incorrecto antes de aplicarlo |
| Generación de configuración (`Jinja2`) | Generar manifiestos y archivos de configuración por entorno |
| Concurrencia básica (`concurrent.futures`) | Revisar 50 servidores en paralelo |
| Empaquetado (`pyproject.toml`, entry points) | Instalar tu herramienta con `pip install .` |
| Métricas y observabilidad (`prometheus_client`) | Exponer tus propias métricas para Prometheus |

**Ejercicios:**

1. Ejecutar `uptime` por SSH en 3 servidores en paralelo y consolidar la salida.
2. Listar contenedores detenidos hace más de 7 días y eliminarlos con `--dry-run` por defecto.
3. Listar pods con estado distinto de `Running` en un namespace y mostrar el motivo.
4. Plantilla Jinja2 que genere un `Deployment` desde un archivo de variables.
5. Exponer un contador de checks fallidos en `/metrics`.

**Proyectos 6, 7 y 8:** automatización de servidores, herramienta Docker y herramienta Kubernetes.

**Criterios para avanzar:**

- Tus tres herramientas se instalan con `pip`, tienen README, tests y un modo `--dry-run`.
- Las operaciones destructivas piden confirmación o un indicador explícito.
- Una prueba con un clúster local `kind` demuestra que tu herramienta funciona de principio a fin.

## Niveles 9 y 10: integración y proyecto final

### Nivel 9: integración Bash + Python (semanas 33 a 36)

**Objetivo:** decidir con criterio qué parte de una automatización va en Bash y cuál en Python, y hacer que ambas convivan en un mismo flujo.

**Prerrequisitos:** niveles 1 a 8.

| Regla de decisión | Usa |
| --- | --- |
| Encadenar comandos del sistema, preparar un entorno, lanzar procesos | Bash |
| Entrada de CI muy corta (menos de 30 líneas), pegamento entre herramientas | Bash |
| Lógica con estructuras de datos, JSON o YAML complejo, APIs, pruebas | Python |
| Reintentos, validación, concurrencia, informes | Python |

**Conceptos clave:**

- Bash llama a Python y pasa datos por argumentos, variables de entorno o stdin. Python devuelve resultados por stdout (JSON) y errores por stderr con exit codes definidos.
- Python llama a Bash con `subprocess.run([...], check=True, capture_output=True, text=True)`, sin `shell=True`.
- Contrato común: salida JSON, códigos de salida documentados (0 correcto, 1 error, 2 uso incorrecto, 3 advertencia) y logs en stderr.
- Un único `Makefile` o `justfile` como punto de entrada (`make check`, `make backup`).
- Empaquetado en una imagen Docker con ambos lenguajes.

**Ejercicios:**

1. Script Bash que llame a una herramienta Python con `--format json` y use `jq` para decidir si continúa.
2. Herramienta Python que invoque tu script Bash de backup y valide el resultado y el exit code.
3. Pasar un secreto desde una variable de CI a Python sin escribirlo en disco ni en logs.
4. Definir un `Makefile` con objetivos `lint`, `test`, `build` y `run`.
5. Crear una imagen Docker multi-etapa con Bash, Python y tus herramientas instaladas.

**Proyecto 9:** automatización integrada Bash + Python.

**Criterios para avanzar:**

- Documentas el contrato de entradas, salidas y exit codes de cada pieza.
- `make lint test` pasa en tu máquina y en el pipeline sin cambios.
- Justificas por escrito en el README por qué cada parte está en el lenguaje elegido.

### Nivel 10: proyecto final DevOps (semanas 37 a 44)

**Objetivo:** entregar una herramienta de operaciones completa, documentada, probada y desplegable, apta para mostrarla en una entrevista.

**Prerrequisitos:** niveles 1 a 9 aprobados.

**Entregables:** repositorio en GitLab con README, diagrama de arquitectura, pipeline completo, imagen publicada en el registry, manifiestos de Kubernetes, pruebas y un documento de operación (runbook).

**Ejercicios de cierre:**

1. Añadir una funcionalidad nueva a tu proyecto final sin romper las pruebas.
2. Simular un fallo (servicio caído, secreto inválido) y diagnosticarlo solo con tus logs y métricas.
3. Pedirle a otra persona que ejecute tu README desde cero y registrar cada dificultad.

**Criterios para terminar el roadmap:**

- El README permite a un tercero desplegar el proyecto en menos de 30 minutos.
- El pipeline pasa lint, pruebas, construcción y despliegue en un clúster local.
- Hay al menos 15 pruebas automáticas y cero advertencias de ShellCheck.
- Has explicado el proyecto en voz alta en 10 minutos, incluidas las decisiones de diseño y los límites.

## Ejercicios de automatización DevOps por área

Cada ejercicio se resuelve primero en Bash (niveles 1 a 4) y se reescribe en Python (niveles 6 a 8) cuando aporta valor. Esa comparación es la mejor forma de aprender cuándo usar cada lenguaje.

| Área | Ejercicio práctico | Se practica en |
| --- | --- | --- |
| Gestión de archivos | Organizar un directorio por fecha, detectar duplicados por hash (`sha256sum`) | Niveles 2 y 6 |
| Gestión de procesos | Detectar procesos zombi o con alto consumo y notificar | Niveles 2 y 6 |
| Gestión de usuarios | Crear usuarios desde un CSV, con grupos y claves SSH, de forma idempotente | Niveles 2 y 8 |
| Gestión de servicios | Verificar y reiniciar servicios `systemd` caídos con límite de reintentos | Niveles 2 y 3 |
| Gestión de logs | Rotar, comprimir y resumir errores por hora | Niveles 2 y 6 |
| Backup y restauración | Backup incremental con `rsync` y restauración verificada por checksum | Niveles 1, 3 y proyecto 1 |
| Health checks | Comprobar HTTP, puerto TCP, certificado y latencia, con exit codes | Niveles 3 y 7 |
| Monitorización | Recolectar CPU, memoria, disco y exponerlos como métricas | Niveles 2, 8 y proyecto 2 |
| Limpieza automática | Eliminar logs, imágenes Docker y temporales viejos con `--dry-run` | Niveles 3 y 8 |
| Gestión de Docker y contenedores | Reiniciar contenedores `unhealthy`, limpiar volúmenes huérfanos, inspeccionar con `jq` | Niveles 3 y 8 |
| Consumo de APIs | Listar pipelines fallidos de GitLab, crear incidencias con `requests` | Nivel 7 |
| Gestión de secretos | Leer secretos de variables de entorno, detectar secretos filtrados con `grep` o `gitleaks` | Niveles 4 y 6 |
| Validación de configuraciones | Validar YAML con `jsonschema` o `pydantic` antes de desplegar | Niveles 6 y 8 |
| Generación de configuraciones | Generar `nginx.conf` o manifiestos por entorno con Jinja2 | Nivel 8 |
| Automatización SSH y de servidores | Ejecutar comandos en varios hosts en paralelo y consolidar resultados | Niveles 3 y 8 |
| CI/CD y GitLab CI | Pipeline con lint, test, build, push al registry y despliegue | Nivel 4 y proyecto final |
| Kubernetes | Detectar pods en `CrashLoopBackOff`, escalar un deployment, leer eventos | Nivel 8 |
| Docker Registry | Listar etiquetas, borrar imágenes antiguas por política de retención | Niveles 7 y 8 |
| Webhooks | Recibir un evento de GitLab y disparar una acción segura | Nivel 7 |

**Ejercicio ejemplo con pistas progresivas (limpieza de logs):**

- **Enunciado:** borrar de `/var/log/app` los archivos `.log` de más de 14 días y mostrar cuánto espacio se liberó.
- **Pista 1:** busca los archivos por antigüedad antes de borrar nada.
- **Pista 2:** `find -mtime +14 -name '*.log'` y calcula el tamaño con `du` antes y después.
- **Pista 3:** esqueleto con un parámetro `--dry-run` que solo imprime.
- **Solución de referencia:** `find "$dir" -type f -name '*.log' -mtime +14 -print0` seguido de `xargs -0 rm -f` solo si no es dry-run. Funciona porque `-print0` y `-0` evitan problemas con espacios. Alternativa: `find ... -delete`, más corta pero sin opción de simulación. Falla si `$dir` está vacío, por eso se valida antes.

## Proyectos 1 a 5

Los proyectos crecen en complejidad. Cada uno vive en su propio repositorio y resuelve un problema que existe en equipos reales. Dificultad en escala de 1 (baja) a 5 (alta).

### Proyecto 1: script Bash de backup (dificultad 1)

- **Problema:** los datos de una aplicación se pierden si falla el disco o alguien borra un directorio.
- **Objetivo:** backup comprimido y rotativo de directorios con verificación de integridad y restauración probada.
- **Arquitectura:** un script, un archivo de configuración `backup.conf`, destino local o remoto por `rsync` y SSH, ejecución por `cron` o `systemd` timer.
- **Tecnologías:** Bash, `tar`, `gzip`, `rsync`, `sha256sum`, `cron`.
- **Estructura:** `backup.sh`, `restore.sh`, `backup.conf.example`, `tests/`, `README.md`, `.shellcheckrc`.
- **Requisitos:** `--dry-run`, retención configurable (por ejemplo 7 diarios y 4 semanales), log con marca de tiempo, exit code distinto de 0 si algo falla, bloqueo para evitar dos ejecuciones simultáneas.
- **Tareas:** leer configuración, validar rutas, crear archivo con fecha, calcular checksum, copiar al destino, aplicar retención, probar restauración en un directorio temporal.
- **Resultado esperado:** backups nocturnos que se pueden restaurar y verificar con un comando.
- **Buenas prácticas:** `set -euo pipefail`, `trap` para limpiar, comillas en todas las variables, `mktemp`, no sobrescribir backups anteriores.
- **Errores frecuentes:** no probar la restauración, borrar el backup viejo antes de confirmar el nuevo, ejecutar como `root` sin necesidad, rutas con espacios sin comillas.
- **Mejoras posteriores:** cifrado con `gpg` o `age`, notificación por webhook, versión Python (proyecto 6).

### Proyecto 2: script Bash de monitorización (dificultad 2)

- **Problema:** nadie se entera de que un disco se llena o un servicio cae hasta que los usuarios protestan.
- **Objetivo:** recolectar CPU, memoria, disco, carga y estado de servicios, y alertar cuando se superen umbrales.
- **Arquitectura:** script ejecutado cada minuto por `systemd` timer, salida a archivo CSV o JSON, alerta por webhook o correo.
- **Tecnologías:** Bash, `awk`, `df`, `free`, `systemctl`, `curl`, `jq`.
- **Estructura:** `monitor.sh`, `lib/log.sh`, `thresholds.conf`, `systemd/monitor.service`, `systemd/monitor.timer`, `README.md`.
- **Requisitos:** umbrales configurables, no repetir la misma alerta en 15 minutos, salida legible por máquina, uso de recursos mínimo.
- **Tareas:** medir métricas, comparar con umbrales, escribir métricas, enviar alerta, instalar el timer.
- **Resultado esperado:** un historial de métricas y alertas fiables, sin ruido.
- **Buenas prácticas:** funciones pequeñas, `LC_ALL=C` para parsear salida de comandos, bloqueo de ejecución, logs rotados.
- **Errores frecuentes:** parsear la salida de `top` o `ps` de forma frágil, alertar en cada ejecución, olvidar que el entorno de `cron` es distinto al de tu shell.
- **Mejoras posteriores:** exponer métricas en formato Prometheus, panel en Grafana, versión Python con `psutil`.

### Proyecto 3: script Bash de health checks (dificultad 2)

- **Problema:** hay que saber si una lista de servicios responde de verdad, no solo si el proceso existe.
- **Objetivo:** comprobar HTTP, puerto TCP, certificado TLS y latencia de varios servicios y devolver un resumen.
- **Arquitectura:** archivo `targets.txt` o `targets.yaml`, un script que itera, un resumen en terminal y en JSON, exit code usable en CI.
- **Tecnologías:** Bash, `curl`, `nc`, `openssl`, `jq`, `timeout`.
- **Estructura:** `healthcheck.sh`, `targets.txt`, `lib/checks.sh`, `tests/`, `README.md`.
- **Requisitos:** timeout por servicio, reintentos con espera, paralelismo simple (`xargs -P`), códigos 0 (todo bien), 1 (fallo) y 3 (advertencia, por ejemplo certificado a punto de caducar).
- **Tareas:** definir formato de objetivos, implementar cada tipo de check, resumir, devolver el código.
- **Resultado esperado:** un comando que dice en segundos qué falla y por qué.
- **Buenas prácticas:** siempre `--max-time` en `curl`, separar la lógica de cada check en funciones, no mezclar salida humana y de máquina.
- **Errores frecuentes:** `curl` sin timeout, considerar éxito un 200 que devuelve una página de error, ignorar certificados con `-k` sin necesidad.
- **Mejoras posteriores:** comprobaciones concurrentes más robustas en Python, integración con GitLab CI como job de verificación posterior al despliegue.

### Proyecto 4: automatización Bash para Docker (dificultad 3)

- **Problema:** los hosts Docker acumulan imágenes, volúmenes y contenedores muertos, y alguien debe reiniciar los que no están sanos.
- **Objetivo:** un conjunto de scripts que limpie recursos antiguos, reinicie contenedores no saludables y construya imágenes con etiquetas consistentes.
- **Arquitectura:** `docker` CLI, `jq` para filtrar, subcomandos (`clean`, `restart-unhealthy`, `build`, `report`), ejecución por timer.
- **Tecnologías:** Bash, Docker CLI, `jq`, `hadolint`, `systemd`.
- **Estructura:** `dockerops.sh`, `lib/`, `Dockerfile.example`, `tests/`, `README.md`.
- **Requisitos:** `--dry-run` por defecto en las acciones destructivas, lista blanca de elementos protegidos, informe del espacio recuperado, etiqueta de imagen con fecha y commit.
- **Tareas:** listar recursos, filtrar por antigüedad, aplicar acciones, medir resultados, documentar.
- **Resultado esperado:** un host Docker limpio y predecible sin intervención manual.
- **Buenas prácticas:** nunca usar `docker system prune -a` sin filtros, usar `--format` y `jq`, comprobar permisos del socket de Docker.
- **Errores frecuentes:** borrar volúmenes con datos, parsear la tabla de `docker ps` en vez de JSON, dar al usuario acceso al socket de Docker sin entender que equivale a `root`.
- **Mejoras posteriores:** reescritura con el SDK de Python (proyecto 7), alertas.

### Proyecto 5: script Python para consumir una API (dificultad 3)

- **Problema:** consultar pipelines, incidencias o imágenes a mano en la web de GitLab es lento y no se puede automatizar.
- **Objetivo:** cliente CLI en Python que consulte la API REST de GitLab y genere un informe (por ejemplo, pipelines fallidos de los últimos 7 días).
- **Arquitectura:** `Client` con sesión y reintentos, modelos con `dataclasses`, capa de informe, CLI con `argparse`.
- **Tecnologías:** Python 3.12 o superior, `requests`, `argparse`, `dataclasses`, `pytest`, `responses` o `unittest.mock`.
- **Estructura:** `src/gitlab_report/{client,models,report,cli}.py`, `tests/`, `pyproject.toml`, `README.md`.
- **Requisitos:** token por variable de entorno, paginación, timeout, reintentos en 5xx y 429, salida en tabla, JSON y CSV, tests que no usan la red.
- **Tareas:** diseñar el cliente, modelar respuestas, paginar, generar informe, escribir pruebas, empaquetar.
- **Resultado esperado:** `gitlab-report failed --days 7 --format json` funciona y está probado.
- **Buenas prácticas:** una `Session` reutilizada, tipos en todo el código, errores propios (`ApiError`), nunca registrar el token.
- **Errores frecuentes:** olvidar la paginación, no poner timeout, capturar `Exception` de forma genérica, escribir el token en el código.
- **Mejoras posteriores:** caché local, soporte de GitHub, exposición de métricas.

## Proyectos 6 a 10

### Proyecto 6: automatización Python para servidores (dificultad 3)

- **Problema:** configurar usuarios, revisar estado y ejecutar tareas en varios servidores a mano es lento y propenso a errores.
- **Objetivo:** herramienta CLI que lea un inventario YAML, se conecte por SSH en paralelo y ejecute comprobaciones y tareas idempotentes.
- **Arquitectura:** inventario YAML, módulo de conexión SSH, módulo de tareas (`check_disk`, `ensure_user`, `restart_service`), ejecutor concurrente, informe final.
- **Tecnologías:** Python, `paramiko` o `fabric`, `PyYAML`, `pydantic` o `jsonschema`, `concurrent.futures`, `pytest`.
- **Estructura:** `src/servops/{inventory,ssh,tasks,runner,cli}.py`, `inventory.example.yaml`, `tests/`, `pyproject.toml`, `README.md`.
- **Requisitos:** validación del inventario, claves SSH (nunca contraseñas en el código), timeout por host, `--dry-run`, resultados por host (ok, cambiado, fallido), exit code resumen.
- **Tareas:** definir esquema del inventario, conectar, ejecutar comando, capturar salida y código, paralelizar, resumir.
- **Resultado esperado:** `servops run check_disk --group web` devuelve una tabla por host en segundos.
- **Buenas prácticas:** tareas idempotentes, separar conexión de lógica para poder probarla con mocks, registrar cada acción con host y tarea.
- **Errores frecuentes:** aceptar cualquier clave de host sin verificar (`AutoAddPolicy`) sin entender el riesgo, no cerrar conexiones, mezclar salida de varios hosts.
- **Mejoras posteriores:** módulos de plantillas, ejecución por rol, cifrado del inventario, migración a Ansible para comparar.

### Proyecto 7: herramienta Python para gestionar Docker (dificultad 4)

- **Problema:** los equipos necesitan políticas de limpieza y de salud sobre contenedores que no cubren los comandos básicos.
- **Objetivo:** CLI que liste, filtre, limpie y reinicie contenedores e imágenes según reglas definidas en YAML, y exponga un resumen.
- **Arquitectura:** capa de acceso (SDK Docker), motor de políticas (reglas declarativas), CLI, informe en JSON y tabla, métricas opcionales.
- **Tecnologías:** Python, SDK `docker`, `PyYAML`, `pydantic`, `rich` (opcional), `pytest`.
- **Estructura:** `src/dockertool/{client,policies,actions,cli}.py`, `policies.example.yaml`, `tests/`, `Dockerfile`, `README.md`.
- **Requisitos:** políticas como `max_age_days`, `protect_labels`, `restart_unhealthy`; `--dry-run` por defecto; salida estructurada; pruebas con un cliente simulado.
- **Tareas:** conectar con el daemon, listar recursos, evaluar reglas, ejecutar acciones, generar informe.
- **Resultado esperado:** `dockertool apply --policy policies.yaml` limpia lo previsto y nada más.
- **Buenas prácticas:** acciones destructivas solo con `--apply` o confirmación, etiquetas para proteger recursos, funciones puras para evaluar reglas.
- **Errores frecuentes:** probar solo contra un Docker real, no manejar errores de red del daemon, borrar imágenes en uso.
- **Mejoras posteriores:** ejecutar como contenedor, publicar métricas, gestionar un registro privado.

### Proyecto 8: herramienta Python para interactuar con Kubernetes (dificultad 4)

- **Problema:** diagnosticar un clúster con `kubectl` repetitivo consume tiempo y se pierde el contexto.
- **Objetivo:** CLI que resuma el estado de un namespace, detecte pods en mal estado (`CrashLoopBackOff`, `ImagePullBackOff`, `Pending`), muestre eventos y logs recientes y permita escalar un deployment con seguridad.
- **Arquitectura:** cliente de Kubernetes, módulo de diagnóstico (reglas con causas probables), módulo de acciones (escalar, reiniciar con `rollout`), informe.
- **Tecnologías:** Python, cliente oficial `kubernetes`, `kind` para pruebas locales, `pytest`.
- **Estructura:** `src/k8stool/{client,diagnose,actions,cli}.py`, `manifests/` de ejemplo con pods defectuosos, `tests/`, `README.md`.
- **Requisitos:** respeta el contexto y el namespace actuales, límite de réplicas permitido, `--dry-run`, permisos mínimos de RBAC documentados.
- **Tareas:** cargar configuración (kubeconfig o in-cluster), listar pods, clasificar estados, leer eventos, implementar `scale`, probarlo en `kind`.
- **Resultado esperado:** `k8stool diagnose -n demo` explica por qué falla cada pod con una causa probable.
- **Buenas prácticas:** una `ServiceAccount` con permisos de solo lectura por defecto, confirmar antes de escalar, paginar listados grandes.
- **Errores frecuentes:** trabajar siempre con permisos de administrador, olvidar el namespace, ejecutar sobre el clúster equivocado.
- **Mejoras posteriores:** ejecutar como `CronJob`, enviar alertas, exponer métricas.

### Proyecto 9: automatización integrada Bash + Python (dificultad 4)

- **Problema:** hay tareas con una parte de sistema (Bash) y una de lógica (Python) que hoy viven en scripts mezclados e imposibles de probar.
- **Objetivo:** un flujo de verificación posterior al despliegue: Bash prepara el entorno y orquesta; Python valida, consulta APIs y genera el informe.
- **Arquitectura:** `Makefile` como entrada, `scripts/` (Bash) que llaman a `tools/` (Python) con un contrato JSON y exit codes documentados.
- **Tecnologías:** Bash, Python, `jq`, `make`, Docker, GitLab CI.
- **Estructura:** `scripts/`, `tools/`, `contracts/README.md`, `tests/` (`bats` y `pytest`), `Dockerfile`, `.gitlab-ci.yml`, `Makefile`.
- **Requisitos:** contrato de entrada y salida, una sola imagen con ambos lenguajes, pruebas de ambos lados, lint de ambos lenguajes en CI.
- **Tareas:** definir contrato, implementar piezas, probar integración, empaquetar, ejecutar en pipeline.
- **Resultado esperado:** `make verify ENV=staging` ejecuta todo y falla con un mensaje claro.
- **Buenas prácticas:** una sola fuente de configuración, logs a stderr y datos a stdout, versionar el contrato.
- **Errores frecuentes:** pasar secretos por argumentos de línea de comandos (visibles en `ps`), duplicar lógica en ambos lados, no probar el contrato.
- **Mejoras posteriores:** publicar la imagen en el registry, añadir informes en el merge request.

### Proyecto 10: proyecto final completo de automatización DevOps (dificultad 5)

- **Problema, objetivo y arquitectura:** ver la sección siguiente, donde se define el proyecto integrador completo.
- **Tecnologías:** Bash, Python, Docker, GitLab CI y Kubernetes.
- **Resultado esperado:** un repositorio de portafolio que despliegue, verifique y opere un servicio real en un clúster local.

## Matriz de competencias

Usa esta matriz para autoevaluarte cada 4 semanas. Cada celda describe qué sabes hacer en ese nivel; la última columna indica dónde se entrena.

| Competencia | Básico | Intermedio | Avanzado | Profesional | Ejercicios y proyectos |
| --- | --- | --- | --- | --- | --- |
| Scripts Bash robustos | Script con variables, condicionales y parámetros | `set -euo pipefail`, funciones, `trap`, `getopts` | Librería reutilizable, pruebas con `bats`, ShellCheck limpio | Scripts mantenidos por un equipo con estándares y revisión | Niveles 1 a 3; proyectos 1 a 4 |
| Procesamiento de texto | `grep` y `cut` | `awk`, `sed`, `sort`, `uniq`, pipelines | Pipelines con `jq`, `xargs -P`, expresiones regulares complejas | Elige la herramienta correcta y justifica | Nivel 2; proyecto 2 |
| Administración Linux | Permisos, procesos, archivos | `cron`, `systemd`, logs con `journalctl` | Timers, límites de recursos, endurecimiento básico | Diseña automatización de servidores reproducible | Nivel 2; proyectos 1 y 2 |
| Manejo de errores y logging | Revisa `$?` | Niveles de log, reintentos, `trap ERR` | Códigos de salida documentados, logs estructurados | Diagnostica incidentes desde los logs | Niveles 3 y 6; proyectos 3 y 9 |
| Automatización Docker | `docker run`, `ps`, `logs` | Limpieza y reinicio con scripts, `jq` | SDK Python con políticas y pruebas | Imágenes seguras, multi-etapa, escaneadas en CI | Proyectos 4 y 7 |
| Automatización Kubernetes | `kubectl get` y `describe` | Scripts que filtran pods y eventos | Cliente Python con diagnóstico y RBAC mínimo | Herramientas operativas con permisos y límites seguros | Proyecto 8 y final |
| APIs REST | Llamadas con `curl` | `requests` con sesión, timeout y paginación | Reintentos, límites de tasa, pruebas con simulaciones | Clientes tipados y reutilizables | Nivel 7; proyecto 5 |
| Python para herramientas | Funciones y módulos | `argparse`, `logging`, `pathlib`, `subprocess` seguro | Paquetes, `typing`, `dataclasses`, `pytest` | Herramientas instalables, documentadas, versionadas | Niveles 5 a 8; proyectos 5 a 8 |
| Integración en CI/CD | Ejecuta un script en un job | Variables protegidas, artefactos, lint | Pipelines con etapas, caché, entornos | Pipelines mantenibles, seguros y reutilizables (`include`, plantillas) | Nivel 4; proyecto final |
| Seguridad y secretos | No pone contraseñas en scripts | Variables de entorno y CI | Rotación, enmascarado, escaneo de secretos | Mínimo privilegio, auditoría y revisión | Niveles 4, 6 y 9 |
| SSH y servidores | Conexión por clave | `scp`, `rsync`, `~/.ssh/config` | Ejecución paralela con Python | Gestión de flotas con inventario y validación | Nivel 3; proyecto 6 |
| Observabilidad | Lee logs a mano | Métricas básicas y alertas con umbrales | Expone métricas Prometheus, health endpoints | Define indicadores y alertas útiles sin ruido | Proyectos 2, 3 y final |
| Diseño de herramientas | Un script para un caso | Parámetros, ayuda y `--dry-run` | Contratos de entrada y salida, idempotencia | Diseña herramientas pequeñas para operaciones de un equipo | Nivel 9; proyecto 9 |

**Cómo evaluarte:** marca cada competencia con la fecha en la que demostraste el nivel con un entregable (un script, una prueba, un pipeline), no con la sensación de saberlo. Un nivel «Profesional» exige que otra persona haya usado o revisado tu trabajo.

## Proyecto final: plataforma de verificación y operación de servicios

Construyes una herramienta que despliega un servicio en Kubernetes, verifica su salud tras cada despliegue, limpia recursos y reporta resultados, todo orquestado desde GitLab CI.

- **Problema:** tras cada despliegue, un equipo pequeño verifica a mano que el servicio funcione, revisa pods, limpia imágenes viejas y avisa del resultado. Es lento y se hace distinto cada vez.
- **Objetivo:** automatizar el ciclo completo (construir, desplegar, verificar, limpiar, informar) con herramientas propias, reproducibles y seguras.
- **Arquitectura:**
  1. Un servicio de ejemplo (API pequeña en Python con `/health` y `/metrics`) empaquetado en Docker.
  2. Un pipeline de GitLab CI con etapas `lint`, `test`, `build`, `deploy`, `verify` y `cleanup`.
  3. Scripts Bash para orquestar (`deploy.sh`, `wait_ready.sh`, `rollback.sh`) y para limpieza del host y del registry.
  4. Una herramienta Python (`opstool`) que diagnostica el clúster, valida la configuración, consulta la API de GitLab y genera un informe JSON y Markdown.
  5. Manifiestos de Kubernetes por entorno (`dev` y `staging` en un clúster `kind`), con plantillas generadas desde Python.
  6. Una notificación por webhook con el resultado y un resumen de métricas.
- **Tecnologías:** Bash, Python 3.12 o superior, Docker, GitLab CI, Kubernetes (`kind`), `kubectl`, `jq`, `pytest`, `bats`, ShellCheck, `hadolint`, `yamllint`.
- **Requisitos:**
  - Despliegue y reversión automáticos si la verificación falla.
  - Ningún secreto en el repositorio ni en los logs; variables protegidas en GitLab.
  - Operaciones destructivas con `--dry-run` por defecto.
  - RBAC de mínimo privilegio para la herramienta Python.
  - Pruebas: al menos 15 de Python y 5 de Bash, más una prueba de humo en el clúster.
  - Runbook con pasos para los 3 fallos más probables.
- **Tareas, en orden:**
  1. Servicio de ejemplo y `Dockerfile` multi-etapa.
  2. Pipeline con lint, test y build, y publicación en el registry.
  3. Manifiestos y `deploy.sh` en `kind`.
  4. `opstool diagnose` y `opstool validate`.
  5. Etapa `verify` con health checks (Bash) e informe (Python).
  6. Reversión automática y limpieza programada.
  7. Notificaciones, métricas y documentación.
- **Dificultad:** 5 de 5.
- **Resultado esperado:** un `git push` dispara el pipeline completo, despliega en `kind`, verifica, informa y, si algo falla, revierte y deja un diagnóstico legible.
- **Buenas prácticas:** imágenes fijadas por versión o digest, un solo punto de entrada (`make`), logs por stderr y datos por stdout, tests en cada merge request, README con diagrama de arquitectura.
- **Errores frecuentes:** usar la etiqueta `latest`, desplegar sin probar la reversión, dar `cluster-admin` al pipeline, no limitar los reintentos, mezclar configuración de entornos.
- **Mejoras posteriores:** Helm o Kustomize, Prometheus y Grafana, firma de imágenes, escaneo de vulnerabilidades (`trivy`), despliegue canary, migrar partes a un operador sencillo.

**Estructura de directorios:**

```
devops-platform/
  app/                 # servicio de ejemplo
  scripts/             # Bash: deploy, wait_ready, rollback, cleanup
  tools/opstool/       # Python: CLI, tests, pyproject.toml
  k8s/{base,dev,staging}/
  docker/Dockerfile
  tests/{bats,e2e}/
  docs/{architecture.md,runbook.md}
  .gitlab-ci.yml
  Makefile
  README.md
```

**Cómo presentarlo en una entrevista:** abre con el problema, muestra el pipeline verde y una reversión automática, y termina explicando una decisión de diseño (por ejemplo, por qué una parte está en Bash y otra en Python).
