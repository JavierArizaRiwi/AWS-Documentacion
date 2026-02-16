# 📖 Glosario AWS - Términos Esenciales

## A

**Account ID**: Identificador único de 12 dígitos para tu cuenta AWS
- Ejemplo: `123456789012`
- Necesario para ARNs y políticas

**Access Key**: Credencial para programmatic access (CLI, SDK, API)
- Compuesta por: Access Key ID + Secret Access Key
- ⚠️ Mantener en secreto como contraseña

**AMI** (Amazon Machine Image): Plantilla precargada con SO y software
- Base para lanzar instancias EC2
- Tipos: AWS managed, custom, community, marketplace

**AZ** (Availability Zone): Data center individual dentro de una región
- Típicamente 3 por región
- Usar múltiples AZs para high availability

**ARN** (Amazon Resource Name): Identificador único de recurso AWS
- Formato: `arn:aws:service:region:account-id:resourcetype/resource`
- Ejemplo: `arn:aws:s3:::my-bucket/my-object`

---

## B

**Bucket**: Contenedor de almacenamiento en S3 (nivel top)
- Nombre global único
- Acceso vía HTTP/HTTPS URLs

**Billing Alert**: Notificación cuando gastos exceden límite
- Configurar en AWS Billing Console
- Recomendado: $50-100 para bootcamp

---

## C

**CloudFormation**: Infrastructure as Code (IaC) para AWS
- Formato: JSON o YAML templates
- Maneja stacks (grupos de recursos)

**CloudTrail**: Auditoría de todas las llamadas API en tu cuenta
- Log histórico de quién hizo qué cuándo
- Essencial para compliance

**CloudWatch**: Monitoring y logging centralizado
- Métricas, logs, alarmas
- Base para observabilidad

**Cognito**: Autenticación y autorización para aplicaciones
- User pools (autenticación)
- Identity pools (autorización)

**CVE** (Common Vulnerabilities and Exposures): Bug de seguridad conocido
- AWS actualiza parches constantemente
- Revisar security bulletins

---

## D

**DDB** (DynamoDB): Base de datos NoSQL serverless
- Modelo: Key-value + document
- Escalado automático

**DMS** (Database Migration Service): Migra BDs on-prem → AWS
- Homogénea (MySQL → MySQL) o heterogénea (Oracle → PostgreSQL)

**DNS**: Domain Name System, traduce dominio → IP
- Route 53 = DNS de AWS

---

## E

**EBS** (Elastic Block Storage): Almacenamiento en bloque para EC2
- Persiste después de terminar instancia
- Snapshots para backups

**EC2** (Elastic Compute Cloud): Máquinas virtuales en AWS
- Tipología: On-demand, Reserved, Spot, Dedicated
- Grupos de seguridad = firewall

**ECR** (Elastic Container Registry): Almacén de imágenes Docker
- Similar a Docker Hub pero privado
- Integración con ECS/EKS

**ECS** (Elastic Container Service): Orquestación de contenedores
- Launch types: EC2, Fargate (serverless)
- Task definition = "docker-compose"

**EFS** (Elastic File System): NFS compartido entre EC2s
- Multi-AZ natively
- Más caro que EBS pero flexible

**EKS** (Elastic Kubernetes Service): Kubernetes manejado
- Clusters, node groups, networking
- Integración con ON-prem Kubernetes

**Endpoint**: URL para acceder a un servicio AWS
- VPC endpoints = acceso privado
- Regional endpoint = arn:aws:region:service

---

## F

**Fargate**: Serverless compute para containers
- Sin gestionar EC2 instances
- Pay-per-use

**Free Tier**: AWS ofrece servicios gratis por 12 meses
- Límites: 750h/mes EC2, 5GB/mes S3, etc.
- Algunos servicios free para siempre

---

## G

**GSI** (Global Secondary Index): Índice alternativo en DynamoDB
- Permite queries con otra partition key
- Consume RUs independientes

---

## I

**IAM** (Identity and Access Management): Sistema de permisos AWS
- Users (personas), Roles (servicios), Policies (permisos)
- Least privilege = minimizar permisos

**IaC** (Infrastructure as Code): Definir infra en código
- Herramientas: CloudFormation, Terraform, CDK
- Version control + reproducibilidad

---

## K

**KMS** (Key Management Service): Cifrado y gestión de claves
- Customer master key (CMK)
- Integración con S3, EBS, RDS, etc.

---

## L

**Lambda**: Serverless compute (funciones)
- Triggers: API, S3, DynamoDB, SNS, etc.
- Pricing: por invocaciones + duración
- Timeout: 15 minutos máximo

**LSI** (Local Secondary Index): Índice alternativo en DynamoDB
- Misma partition key que tabla
- Límite: 10GB por partition key value

---

## M

**MFA** (Multi-Factor Authentication): 2 factores de autenticación
- TOTP (Google Authenticator): código de 6 dígitos
- Hardware MFA: dispositivo físico

**Microservices**: Arquitectura con múltiples servicios pequeños
- Cada servicio: independiente, deployable, escalable
- AWS ideal para esto (Lambda, ECS, etc.)

---

## N

**NAT Gateway**: Permite EC2 privadas conectar a internet
- NO enrutadas desde internet
- Ubicado en subnet pública

**NLB** (Network Load Balancer): Balanceador L4 (protocolo)
- Ultra-high performance
- Casos: Gaming, IoT, no-HTTP protocols

---

## O

**On-demand**: Instancias pagadas por hora/minuto
- Sin compromiso a largo plazo
- Más caro que Reserved

---

## P

**Policy**: Documento JSON que define permisos
- Estructura: Effect, Action, Resource, Condition
- Attached a Users, Groups, Roles

**Principal**: Entidad que toma acción (User, Role, Service)

---

## R

**RDS** (Relational Database Service): BD manejada
- Engines: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server
- Multi-AZ, Read Replicas, Backups automáticos

**Region**: Área geográfica com 3+ AZs
- Ejemplo: us-east-1 (N. Virginia), eu-west-1 (Irlanda)
- Elegir cercano al usuario para baja latencia

**Reserved**: Instancias con commitments 1-3 años
- 30-60% más barato que On-demand
- Inflexible si cancelaste

**Route Table**: Reglas de enrutamiento en VPC
- Define cómo packets van dentro/fuera subnet
- Destino → Target (IGW, NAT, Peering, etc.)

**Role**: Set de permisos para servicios/usuarios temporales
- EC2 role = permisos para EC2 instance
- Assume role = temporal token (STS)

---

## S

**S3** (Simple Storage Service): Object storage escalable
- Buckets (top-level), Objects (archivos)
- Replicación, versioning, lifecycle policies

**Secrets Manager**: Almacén centralizado de passwords/API keys
- Rotación automática
- Acceso auditado via CloudTrail

**SG** (Security Group): Firewall a nivel de instancia
- Reglas: Inbound (entrada), Outbound (salida)
- Stateful (si permites entrada, salida automática)

**SNS** (Simple Notification Service): Pub/Sub messaging
- Topics (canales)
- Subscribers: SQS, Lambda, Email, HTTP, etc.

**Spot**: Instancias con descuento 90% (capacidad ociosa)
- Interrumpibles sin aviso
- Ideal: batch, análisis, dev/test

**SQS** (Simple Queue Service): Cola de mensajes
- Asíncrono, decoupled processing
- FIFO vs estándar

**SSM** (Systems Manager): Gestión centralizada de instancias
- Parameter Store: variables de config
- Session Manager: acceso sin SSH

**State Machine** (Step Functions): Workflow serverless
- Define pasos (estados)
- Manejo de errores, reintentos, parallelización

---

## T

**Tag**: Label clave-valor para recursos AWS
- Organización, billing, automation
- Ejemplo: Environment=prod, Owner=team-a

**Terraform**: IaC alternativo a CloudFormation
- Agnóstico (AWS, Azure, GCP, etc.)
- HCL language

**TTL** (Time To Live): Expiración automática de datos
- DynamoDB: auto-delete items
- S3: auto-delete objects (lifecycle)

---

## V

**VPC** (Virtual Private Cloud): Red aislada en AWS
- Subnets (divisiones dentro VPC)
- Acceso controlado vía route tables + NACLs

**VPN**: Conexión encriptada entre on-prem y AWS
- Site-to-site VPN: oficina ↔ AWS
- Client VPN: single PC ↔ AWS

---

## W

**Whitelist**: Permitir listaexplícita (allow-only)
- Opuesto a blacklist (deny-only)
- Security best practice

---

## X

**X-Ray**: Tracing distribuido de aplicaciones
- Visualiza llamadas entre servicios
- Identifica bottlenecks, errores

---

## Z

**Zone**: Abbreviation para Availability Zone (AZ)

---

## 🔗 Acrónimos Comunes

| Sigla | Significado | Categoría |
|-------|-------------|-----------|
| AMI | Amazon Machine Image | EC2 |
| AZ | Availability Zone | Network |
| ARN | Amazon Resource Name | General |
| AWS | Amazon Web Services | General |
| CDN | Content Delivery Network | Performance |
| CI/CD | Continuous Integration/Deployment | DevOps |
| CVE | Common Vulnerabilities | Security |
| DDB | DynamoDB | Database |
| DMS | Database Migration Service | Migration |
| EBS | Elastic Block Store | Storage |
| EC2 | Elastic Compute Cloud | Compute |
| ECR | Elastic Container Registry | Container |
| ECS | Elastic Container Service | Container |
| EFS | Elastic File System | Storage |
| EKS | Elastic Kubernetes Service | Container |
| FaaS | Function as a Service | Serverless |
| HA | High Availability | Architecture |
| IAM | Identity & Access Management | Security |
| IaC | Infrastructure as Code | DevOps |
| KMS | Key Management Service | Security |
| LSI | Local Secondary Index | DynamoDB |
| NAT | Network Address Translation | Network |
| NLB | Network Load Balancer | Networking |
| PII | Personally Identifiable Info | Security |
| RDS | Relational Database Service | Database |
| RTO | Recovery Time Objective | DR |
| SG | Security Group | Security |
| SNS | Simple Notification Service | Messaging |
| SQS | Simple Queue Service | Messaging |
| SSM | Systems Manager | Management |
| VPC | Virtual Private Cloud | Network |

---

**Last Updated**: 16 Feb 2026
**Contribuciones**: Abre PR si encuentras términos faltantes
