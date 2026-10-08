# 🗺️ RoadMap 

## Elementos base que todo DevOps debe dominar
 - Antes de entrar en los niveles, estos son los pilares transversales que atraviesan toda la carrera: 
    - sistemas operativos Linux, 
    - redes TCP/IP, control de versiones con Git, 
    - al menos un lenguaje de scripting (Bash + Python), 
    - fundamentos de seguridad, y 
    - pensamiento de automatización como filosofía de trabajo. Sin estos, el resto no tiene dónde sostenerse.

## 🟢 Nivel inicial — Fundamentos esenciales
 - Duración estimada: 4 a 6 meses  
    - `Objetivo`: Entender el ciclo de vida completo del software, trabajar cómodamente en entornos Linux, versionar código con Git, y construir tu primer pipeline funcional.  
    - `Conceptos teóricos clave`: DevOps lifecycle (`plan → build → test → release → deploy → operate → monitor`), cultura de colaboración Dev+Ops, principios de "shift left" en testing, el modelo de ramas (GitFlow vs trunk-based development), fundamentos de contenedores vs máquinas virtuales.  
    - Herramientas a dominar:
       - ✅Linux CLI: navegación, permisos, procesos, cron, SSH
       - ✅Git: commits, branches, merge/rebase, pull requests, resolución de conflictos
       - 🎯Docker: Dockerfile, docker build/run/push, `volumes`, redes de contenedores, Docker Compose
       - 🎯GitHub `Actions` o GitLab CI: workflows básicos, triggers, jobs y steps
       - `Bash` y Python básico: scripts de automatización, lectura de variables de entorno, manejo de archivos

### Proyecto hands-on: 
  Tomar una aplicación web simple (puede ser un "Hello World" en Node.js o Python Flask), crear su Dockerfile, publicarla en Docker Hub, y armar un pipeline en GitHub Actions que corra los tests automáticamente y construya la imagen al hacer push a main.
    
### Buenas prácticas del nivel:
  -  Un Dockerfile por imagen, imágenes small (usar Alpine o Distroless)
  -  Nunca commitear credenciales al repositorio (usar .gitignore y variables de entorno)
  -  Escribir mensajes de commit descriptivos siguiendo Conventional Commits
  -  Siempre tener tests antes de automatizar el deploy

### Errores y antipatterns comunes:
 - Usar root como usuario dentro de los contenedores
 - Pipelines sin manejo de errores (set -e omitido en Bash)
 - Trabajar directamente en la rama main sin pull requests
 - Ignorar los logs de build cuando fallan

### Lab local con recursos limitados: 
 - Docker Desktop (o Podman en Linux sin daemon) en cualquier laptop de 8GB RAM es suficiente. Usar GitHub free tier para los pipelines. No hace falta cloud todavía.

## Nivel intermedio — Herramientas y prácticas aplicadas
- Duración estimada: 6 a 10 meses
- Prerequisito: Dominar el nivel inicial
- Objetivo: Operar stacks reales en Kubernetes, gestionar infraestructura como código, implementar observabilidad básica y comenzar a pensar en seguridad desde el pipeline.
- Conceptos teóricos clave: Orquestación de contenedores, reconciliation loops, principio de inmutabilidad de la infraestructura, GitOps como modelo operativo, SLI/SLO básicos, el modelo de seguridad del pipeline (supply chain attacks, SBOM), estrategias de despliegue (blue-green, canary, rolling update).

### Herramientas a dominar:

- **Kubernetes**: Pods, Deployments, Services, ConfigMaps, Secrets, Ingress, RBAC, namespaces, HPA
- **Helm**: templates, values.yaml, repositorios de charts, upgrades y rollbacks
- **ArgoCD**: Application CRDs, sincronización automática, app-of-apps pattern
- **Terraform**: providers, resources, state backend remoto (S3 + DynamoDB), módulos reutilizables, terraform plan/apply/destroy
- **AWS o GCP**: VPC, subnets, IAM roles/policies, EC2/Compute Engine, S3/GCS, RDS/Cloud SQL, Load Balancers, EKS/GKE
- **Prometheus + Grafana**: exporters, PromQL básico, dashboards, alertmanager
- HashiCorp Vault o AWS Secrets Manager para gestión de secretos

- `Proyecto hands-on`: Desplegar una aplicación de microservicios (al menos 2 servicios: API + base de datos) en un cluster Kubernetes (puede ser Kind o k3s localmente, o EKS/GKE en la nube). El pipeline en GitHub Actions debe hacer: lint → test → build de imagen → push a registry → apply del chart Helm vía ArgoCD. La infraestructura (VPC, cluster, nodos) debe estar descrita en Terraform. Agregar un dashboard en Grafana con las 4 golden signals (latencia, tráfico, errores, saturación).

### Buenas prácticas del nivel:

- Nunca aplicar kubectl apply -f manual en producción; todo a través de GitOps
- State de Terraform siempre en remoto, nunca en local ni commiteado
- RBAC de Kubernetes siguiendo el principio de mínimo privilegio
- Separar entornos (dev/staging/prod) con namespaces o clusters distintos
- Resource requests y limits definidos en todos los Deployments

### Errores y antipatterns comunes:

- Usar latest como tag de imagen en producción (rompe la trazabilidad)
- Guardar el terraform.tfstate en Git
- No configurar probes de liveness y readiness en los Pods
- Monolitos de Terraform: un solo main.tf con todo; en su lugar, separar por módulos y workspaces
- Aplicar cambios de infraestructura sin revisar el plan primero

`Lab local con recursos limitados`: Kind (Kubernetes in Docker) o k3s en una VM de 4GB RAM es suficiente para practicar Kubernetes. Para cloud, las cuentas free tier de AWS o GCP permiten un cluster pequeño por horas sin costo significativo. Usar tfenv para manejar versiones de Terraform.

## Nivel avanzado — Arquitectura, escalabilidad y especialización
Duración estimada: 8 a 12 meses
Prerequisito: Dominar el nivel intermedio
Objetivo: Diseñar plataformas internas para equipos de desarrollo, liderar decisiones de arquitectura, implementar observabilidad de nivel producción, y especializarse en una rama (SRE, Platform Engineering o DevSecOps).
Conceptos teóricos clave: Internal Developer Platforms (IDP), error budgets y SLO contractuales, chaos engineering y diseño para fallos, FinOps y optimización de costos cloud, policy as code y compliance as code, multi-tenancy en Kubernetes, progressive delivery, supply chain security (SLSA framework).
Herramientas a dominar:

OpenTelemetry: instrumentación de trazas distribuidas, integración con Jaeger o Tempo
OPA (Open Policy Agent) y Kyverno: políticas de seguridad como código en Kubernetes
Backstage: catálogo de servicios e Internal Developer Portal
Crossplane o Pulumi: infraestructura compuesta orientada a la experiencia del desarrollador
Chaos Mesh o LitmusChaos: experimentos de ingeniería del caos
Tekton o Argo Workflows: pipelines nativos en Kubernetes
Service Mesh: Istio o Linkerd para observabilidad, mTLS y traffic management
GitHub Advanced Security o Snyk para análisis de vulnerabilidades en el pipeline

Proyecto hands-on (proyecto final): Construir una plataforma interna completa: un portal en Backstage que exponga plantillas de "golden path" (scaffolding de nuevos servicios), integrado con un pipeline GitOps que aprovisiona infraestructura con Crossplane, despliega en Kubernetes con ArgoCD, y expone métricas, trazas y logs unificados en Grafana con OpenTelemetry. Incluir políticas de seguridad con OPA que bloqueen imágenes sin firma o sin SBOM. Ejecutar un experiment de chaos contra el sistema y documentar el error budget consumido.
Rutas de especialización:
Site Reliability Engineering (SRE): Profundizar en SLO/SLA/SLI, incident management, toil reduction, capacity planning, blameless postmortems. Herramientas: PagerDuty, Runbook automation, Spanner/Bigtable para stateful workloads.
Platform Engineering: Foco en developer experience, Internal Developer Platforms, golden paths, self-service infrastructure. Comunidades clave: CNCF, Platform Engineering Slack, Team Topologies como marco teórico.
DevSecOps: Supply chain security, SAST/DAST integrado en CI, gestión de vulnerabilidades, SLSA levels, firma de imágenes con Cosign/Sigstore, threat modeling.
Buenas prácticas del nivel:

Todo cambio de plataforma tiene un SLO de deploy frequency y change failure rate
Los errores son métricas, no logs: estructurar todos los eventos como datos consultables
Infraestructura multi-región con failover automático probado regularmente
Las políticas de seguridad se testean igual que el código de aplicación

Errores y antipatterns comunes:

Construir una plataforma interna sin hablar con los equipos de desarrollo que la usarán
Implementar Service Mesh sin entender el overhead de performance
Confundir disponibilidad alta con resiliencia (son cosas distintas)
No tener runbooks para cada alerta de Prometheus
Asumir que más métricas equivale a mejor observabilidad; la cardinalidad excesiva destruye el rendimiento de Prometheus

Lab local con recursos limitados: Un cluster k3s en un servidor de segunda mano (o una instancia EC2 t3.medium) es suficiente para la mayoría de los experimentos avanzados. Backstage puede correrse localmente con Node.js. Para chaos engineering, Kind con 3 nodos alcanza para los experimentos básicos.

Certificaciones recomendadas por nivel
Para el nivel inicial, la Docker Certified Associate es un buen punto de partida. En el nivel intermedio, la Certified Kubernetes Administrator (CKA) es la más reconocida en la industria, seguida de la AWS Solutions Architect Associate o su equivalente en GCP. En el nivel avanzado, la Certified Kubernetes Security Specialist (CKS) diferencia a quienes quieren especializarse en seguridad, y la CKAD complementa el perfil para quienes trabajan cerca de los equipos de desarrollo.

Orden recomendado de aprendizaje y dependencias
Linux/Redes → Git → Bash/Python → Docker → CI/CD básico → Kubernetes → Helm → Cloud fundamentals → Terraform → Observabilidad (Prometheus/Grafana) → ArgoCD/GitOps → DevSecOps básico → OpenTelemetry → OPA/Policy as code → Platform Engineering/SRE → Chaos Engineering.
Cada etapa depende de la anterior: no tiene sentido aprender Kubernetes sin entender Docker, ni ArgoCD sin entender Kubernetes, ni Crossplane sin dominar Terraform. La disciplina de respetar este orden ahorra meses de frustración.Has dicho: Explícame qué debo aprender de Linux y redes para DevOpsExplícame qué debo aprender de Linux y redes para DevOps13:35Sonnet 4.6