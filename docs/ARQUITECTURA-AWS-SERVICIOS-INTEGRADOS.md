# Arquitectura AWS: 12 Servicios Integrados en una Solución Moderna

**Nivel**: Intermedio  
**Duración estimada**: 45 minutos  
**Objetivo**: Entender cómo 12 servicios AWS trabajan juntos en una arquitectura enterprise  
**Audiencia**: Desarrolladores y arquitectos necesitando entender flujos integrados

---

## 1. Los 12 Servicios y Sus Roles

### 1.1 IAM (Identity and Access Management) - La Seguridad de Todo

**Rol**: Control de acceso centralizado. Define quién puede hacer qué en AWS.

**Responsabilidades**:
- Autenticar usuarios/servicios (verificar identidad)
- Autorizar acciones (qué recursos puede acceder)
- Auditar cambios (quién hizo qué, cuándo)
- Gestionar credenciales (access keys, MFA)

**Analogía**: Portero de edificio corporativo que verifica empleados y decide qué pisos pueden acceder.

**Relación con otros servicios**:
- Cada servicio (EC2, Lambda, RDS) funciona bajo permisos IAM
- Sin IAM correcto → acceso denegado
- IAM es **transversal** a todos los demás

### 1.2 VPC (Virtual Private Cloud) - Tu Red Privada

**Rol**: Aislamiento de red. Define dónde viven tus recursos y cómo se comunican.

**Responsabilidades**:
- Crear red privada (10.0.0.0/16)
- Segmentar en subnets públicas y privadas
- Controlar tráfico con Route Tables y NACLs
- Conectar recursos internamente
- Aislamiento de Internet

**Analogía**: Tu condominio privado. Un muro separa tu propiedad del resto del mundo.

**Relación con otros servicios**:
- EC2 vive dentro de VPC (subnet pública o privada)
- RDS debe estar en Private Subnet (nunca accesible desde Internet)
- Lambda puede vivir dentro de VPC (durante ejecución)
- ECS/EKS se desployan dentro de VPC
- Security Groups actúan como firewall interno

### 1.3 EC2 (Elastic Compute Cloud) - Máquinas Virtuales

**Rol**: Servidores procesadores de aplicaciones. Ejecuta código, aplicaciones, servicios.

**Responsabilidades**:
- Ejecutar aplicaciones (Node.js, Python, Java, etc.)
- Procesar solicitudes HTTP/HTTPS
- Gestionar conexiones a BD
- Escalar horizontalmente (múltiples instancias)

**Analogía**: Computadora virtual alquilada. Inicias, ejecutas código, pagas por hora.

**Relación con otros servicios**:
- Protegido por Security Groups (VPC)
- Acceso controlado por IAM Roles
- Se comunica con RDS en Private Subnet
- Sube/baja archivos a S3
- Reporta métricas a CloudWatch
- Puede ejecutar código que calla a Lambda

### 1.4 S3 (Simple Storage Service) - Almacenamiento Masivo

**Rol**: Repositorio de archivos. Datos, backups, assets, logs.

**Responsabilidades**:
- Guardar archivos de cualquier tamaño
- Servir contenido estático
- Preservar versiones (versioning)
- Archivar datos antiguos (Glacier)
- Retención de backups

**Analogía**: Guardarropa gigante. Guardas lo que quieras, recuperas cuando necesites.

**Relación con otros servicios**:
- EC2 sube reportes, archivos procesados
- Lambda corre cuando archivo sube (trigger)
- RDS snapshots se guardan automáticamente
- CloudFront sirve contenido desde S3 rápidamente
- Acceso protegido por IAM policies y bucket policies

### 1.5 RDS (Relational Database Service) - Base de Datos Administrada

**Rol**: Persistencia de datos. Centro de verdad para datos críticos.

**Responsabilidades**:
- Almacenar datos estructurados (MySQL, PostgreSQL, Oracle)
- Mantener integridad referencial
- Backup automático
- Failover Multi-AZ (alta disponibilidad)
- Encryption at rest y in transit

**Analogía**: Archivo seguro. Guardas documentos importantes, pueden acceder múltiples personas autorizadas.

**Relación con otros servicios**:
- EC2 conecta vía JDBC/SQL connection string
- Lambda consulta datos durante ejecución
- Backup automático va a S3
- Replicación read-only en otra región (RDS Read Replica)
- CloudWatch monitorea CPU, conexiones, almacenamiento
- IAM database auth (sin contraseña hardcodeada)

### 1.6 Lambda (Computación Serverless) - Código Sin Servidores

**Rol**: Ejecutar código bajo demanda. Procesar eventos, tareas asincrónicas.

**Responsabilidades**:
- Ejecutar funciones cuando eventos disparan
- Procesar en paralelo (escala automática)
- Integración con servicios AWS
- Logging automático
- Pricing por invocación + duración

**Analogía**: Repartidor. Recibes solicitud, la completas, desapareces. Pagas por trabajo, no por esperar.

**Relación con otros servicios**:
- API Gateway triggeriza Lambda en cada request
- SQS/SNS disparan Lambda automáticamente
- Acceso a datos en S3 durante ejecución
- Consulta RDS para información
- CloudWatch recibe logs y métricas
- IAM role define permisos

### 1.7 API Gateway - Puerta de Entrada HTTP/REST

**Rol**: Exponer APIs REST públicas. Enrutamiento elegante de solicitudes.

**Responsabilidades**:
- Crear endpoints REST (GET /users, POST /orders)
- Validar solicitudes
- Autenticación/autorización
- Rate limiting y throttling
- Transformar requests/responses
- CORS handling

**Analogía**: Recepción de hotel. Dirige huéspedes al hotel correcto (backend).

**Relación con otros servicios**:
- Recibe petición HTTP de usuario
- Valida con IAM o Cognito
- Enruta a Lambda o EC2
- Retorna respuesta transformada
- Logs en CloudWatch
- Métricas de latencia

### 1.8 CloudWatch - Observabilidad y Monitoreo

**Rol**: Ver qué está pasando en tu infraestructura. Centro de comando.

**Responsabilidades**:
- Recolectar métricas (CPU, RAM, latencia)
- Capturar logs de aplicaciones
- Crear alarmas (si CPU >80%, alertar)
- Dashboards para visualizar todo
- Event rules (disparar acciones automáticamente)

**Analogía**: "Cámara de seguridad" de tu infraestructura. Ve todo, alerta si algo anormal.

**Relación con otros servicios**:
- EC2 reporta CPU, disk, network
- RDS reporta conexiones, almacenamiento
- Lambda reporta invocaciones, errores, duración
- API Gateway reporta requests, latencies
- ECS reporta task status
- Recibe logs de todos los servicios

### 1.9 ECS (Elastic Container Service) - Orquestación de Contenedores

**Rol**: Ejecutar aplicaciones containerizadas (Docker) a escala.

**Responsabilidades**:
- Lanzar y detener contenedores
- Balancear carga entre contenedores
- Escalar basado en demanda
- Mantener tasks ejecutándose
- Integración con load balancers

**Analogía**: Gerente de restaurante que asigna mesas (contenedores) según clientes.

**Relación con otros servicios**:
- Corre dentro de VPC (EC2 instances o Fargate)
- Protegido por Security Groups
- Acceso controlado por IAM roles
- Se comunica con RDS para datos
- Carga archivos a S3
- CloudWatch monitorea task health
- Receptor de requests de ALB

### 1.10 EKS (Elastic Kubernetes Service) - Orquestación Professional

**Rol**: Ejecutar Kubernetes completo (version professional de ECS).

**Responsabilidades**:
- Orquestar pods (contenedores más pequeños)
- Auto-scaling por CPU/memoria
- Rolling updates
- Service discovery
- Ingress (HTTP routing)

**Analogía**: Kubernetes = director de orquesta. Toca todo perfectamente sincronizado.

**Relación con otros servicios**:
- Corre dentro de VPC (EC2 nodes)
- Protegido por Security Groups
- Pods acceden a RDS, S3
- CloudWatch recibe métricas de prometeo
- API Gateway redirige a Ingress
- Ingress redirige a Services

### 1.11 SQS (Simple Queue Service) - Cola de Mensajes

**Rol**: Desacoplar sistemas. Procesar trabajo de forma asincrónica.

**Responsabilidades**:
- Almacenar mensajes temporalmente
- Garantizar entrega at-least-once
- Escalabilidad automática
- Dead Letter Queue para errores
- FIFO o Standard

**Analogía**: Buzón de entrada. Alguien pone tarea, trabajador la retira cuando está libre.

**Relación con otros servicios**:
- EC2 o Lambda produce mensajes (Producer)
- Lambda o EC2 consume mensajes (Consumer)
- Si error, retira luego (retry logic)
- DLQ para mensajes fallidos
- CloudWatch monitorea tamaño de queue
- IAM controla acceso

### 1.12 SNS (Simple Notification Service) - Servicio de Notificaciones

**Rol**: Publicar eventos que múltiples servicios escuchan. Fan-out masivo.

**Responsabilidades**:
- Publicar mensajes (broadcast)
- Múltiples subscribers escuchan
- Email, SMS, HTTP, Lambda, SQS como destinos
- Filter policies
- Garantía de entrega

**Analogía**: Grito en plaza pública. Todos que estén suscritos escuchan.

**Relación con otros servicios**:
- RDS cambio importante → publish a SNS
- SNS notifica Lambda (triggeriza)
- SNS notifica email a administrador
- SNS envía a SQS (para procesamiento posterior)
- Lambda produce evento hacia SNS
- CloudWatch alarms publican a SNS

---

## 2. Matriz de Interacciones

Cómo se comunican entre sí:

| De \ A | EC2 | RDS | S3 | Lambda | SQS | SNS |
|--------|-----|-----|-----|--------|-----|-----|
| **EC2** | - | SQL | HTTP | Invoke | Send | Publish |
| **RDS** | - | - | Snapshot | - | - | - |
| **S3** | HTTP | - | - | Trigger | - | - |
| **Lambda** | HTTP | SQL | API | Invoke | Send/Recv | Publish |
| **SQS** | Poll | - | - | Auto-trigger | - | - |
| **SNS** | - | - | - | Auto-trigger | Send | - |
| **API GW** | Route | - | - | Route | - | - |
| **ECS** | - | SQL | API | - | - | - |
| **EKS** | - | SQL | API | - | - | - |

---

## 3. Arquitectura Conceptual de Aplicación Real

### Escenario: Plataforma de E-commerce (Pedidos, Pagos, Notificaciones)

```
┌─────────────────────────────────────────────────────────────────┐
│                         USUARIOS EN INTERNET                      │
│              (Navegador, Apps Mobile, Partners)                  │
└────────────────────────┬────────────────────────────────────────┘
                         │ HTTPS Request
                         ▼
            ┌────────────────────────────┐
            │   API Gateway              │
            │  (Validar entrada)         │
            │  (Rate limit: 1000 req/s)  │
            │  (Authenticate con IAM)    │
            └────────┬───────┬──────┬────┘
                     │       │      │
          GET /orders │   POST /orders   DELETE /orders/123
                     │       │      │
        ┌────────────▼┐ ┌───▼──────▼──┐ ┌───────────────┐
        │ Lambda-Get  │ │Lambda-Create│ │Lambda-Delete  │
        │(Query RDS)  │ │(Validate)   │ │(Auth check)   │
        └────────┬────┘ └──┬──────┬───┘ └───────────────┘
                 │         │      │
                 │      [DENTRO DE IAM]
                 │    - Acceso RDS checked
                 │    - Acceso S3 checked
                 │    - Acceso SQS checked
                 │
     ┌───────────▼──────────────────────────────┐
     │         VPC (10.0.0.0/16)                │
     │                                          │
     │ ┌─────────────────────────────────────┐ │
     │ │  Private Subnet 1 (10.0.1.0/24)     │ │
     │ │                                     │ │
     │ │  ┌───────────────────────────────┐ │ │
     │ │  │  RDS PostgreSQL Multi-AZ      │ │ │
     │ │  │  ├─ Master (orders table)     │ │ │
     │ │  │  ├─ Standby (auto-failover)  │ │ │
     │ │  │  └─ Replica (read-only)      │ │ │
     │ │  └───────────────┬───────────────┘ │ │
     │ │                  │ Snapshot daily   │ │
     │ │                  ▼                  │ │
     │ └─────────────────────────────────────┘ │
     │                                         │
     │ ┌─────────────────────────────────────┐ │
     │ │  Private Subnet 2 (10.0.2.0/24)     │ │
     │ │                                     │ │
     │ │  ┌───────────────────────────────┐ │ │
     │ │  │  ECS Cluster (Fargate)        │ │ │
     │ │  │  ├─ Order Processing Tasks    │ │ │
     │ │  │  ├─ Receipt PDF Generation    │ │ │
     │ │  │  └─ Fulfillment Service       │ │ │
     │ │  └───────────────┬───────────────┘ │ │
     │ │                  │ Consume from SQS │ │
     │ │                  ▼                  │ │
     │ └─────────────────────────────────────┘ │
     │                                         │
     │ ┌─────────────────────────────────────┐ │
     │ │  Public Subnet (10.0.3.0/24)        │ │
     │ │                                     │ │
     │ │  ┌───────────────────────────────┐ │ │
     │ │  │  Security Group (Firewall)    │ │ │
     │ │  │  ├─ Allow HTTPS 443 from :0   │ │ │
     │ │  │  ├─ Allow SSH 22 from Admin   │ │ │
     │ │  │  └─ Allow all outbound        │ │ │
     │ │  └───────────────┬───────────────┘ │ │
     │ │                  │                  │ │
     │ │  ┌───────────────▼───────────────┐ │ │
     │ │  │  ALB (Load Balancer)          │ │ │
     │ │  │  └─ Distribuye a Lambda/ECS  │ │ │
     │ │  └───────────────────────────────┘ │ │
     │ │                                     │ │
     │ └─────────────────────────────────────┘ │
     │                                         │
     └─────────────────────────────────────────┘

┌──────────────────────────────────────────────┐
│  SQS Queue (orders-pending)                  │
│  ├─ Mensaje: {"orderId": 123, "items": [...]}│
│  ├─ Mensaje: {"orderId": 124, "items": [...]}│
│  └─ Mensaje: {"orderId": 125, "items": [...]}│
└──────────────────────────────────────────────┘
  ▲
  │ Lambda publica cuando pedido creado
  │
  └──────────────────────────────┐
                    ┌────────────▼──────────┐
                    │  SNS (order.created)  │
                    └────────┬──────┬───────┘
                             │      │
              Email Admin ────┼─┐    │
                             │ │    └──Lambda que envía SMS
              Web Hook ───────┘ │
                             CRM
                             Update

┌──────────────────────────────────────────┐
│  S3 Buckets                              │
│  ├─ Assets: HTML/CSS/JS (servido por CF)│
│  ├─ Uploads: PDFs de recibos            │
│  ├─ Logs: CloudFront/ALB/Lambda logs   │
│  └─ Backups: RDS snapshots              │
└──────────────────────────────────────────┘

┌──────────────────────────────────────────┐
│  CloudFront (CDN)                        │
│  └─ Sirve assets S3 desde 200+ ubicaciones│
└──────────────────────────────────────────┘

┌──────────────────────────────────────────┐
│  CloudWatch                              │
│  ├─ Dashboards: EC2 CPU, RDS conn,etc  │
│  ├─ Alarms: if CPU >80%, launch EC2    │
│  ├─ Logs: Aggregated from Lambda,ECS   │
│  └─ Events: Trigger actions on changes │
└──────────────────────────────────────────┘
```

---

## 4. Flujo Detallado de una Petición Real

### Escenario: Usuario compra 3 productos

#### Paso 1: Usuario hace click "Checkout"
```
Browser HTTPS POST → api.example.com/orders
{
  "items": [
    {"productId": 101, "qty": 2},
    {"productId": 205, "qty": 1}
  ],
  "paymentMethod": "credit_card"
}
```

#### Paso 2: API Gateway recibe
```
API Gateway:
├─ Valida JSON schema
├─ Verifica HTTPS (TLS)
├─ Extrae JWT token
├─ Llama IAM: "¿Este usuario autenticado puede crear orders?"
├─ IAM responde: SÍ (tiene policy)
└─ Enruta a Lambda: POST-CreateOrder
```

#### Paso 3: Lambda procesa solicitud
```
Lambda function (dentro VPC):
├─ Lee del evento: items, user_id, payment
├─ Conecta a RDS (usa IAM role, no contraseña)
├─ Transacción SQL:
│  ├─ INSERT orders (order_id=12345, user_id=999, total=$99)
│  ├─ INSERT order_items (order_id, product_id, qty)
│  └─ COMMIT
├─ Si éxito:
│  ├─ Publica evento a SQS: queue=orders-pending
│  │  └─ Mensaje: {"orderId":12345, "status":"pending"}
│  └─ Publica a SNS: topic=order.created
│     ├─ Envía email al admin
│     ├─ Notifica CRM
│     └─ Triggeriza Lambda payment-processor
├─ Si error:
│  ├─ ROLLBACK transacción
│  ├─ Publica a SNS: topic=order.failed
│  │  └─ Alerta Slack a equipo
│  └─ Retorna error 400
├─ CloudWatch recibe:
│  ├─ Métrica: duration=145ms
│  ├─ Log: "Order 12345 created"
│  └─ Log Level: INFO
└─ Retorna JSON: {"orderId":12345, "status":"created"}
```

#### Paso 4: ECS Service consume SQS
```
ECS Task (orden processing):
├─ Poll SQS cada 5 segundos
├─ Obtiene mensaje: {"orderId":12345}
├─ Ejecuta "OrderProcessor" Docker container
├─ Conecta a RDS: SELECT * FROM orders WHERE id=12345
├─ Realiza validaciones:
│  ├─ ¿Stock disponible?
│  ├─ ¿Dirección válida?
│  └─ ¿Información de pago?
├─ Si todo OK:
│  ├─ UPDATE orders SET status='confirmed'
│  ├─ Genera PDF receipt → S3 bucket
│  ├─ DELETE mensaje de SQS (processed)
│  └─ Publica a SNS: order.confirmed
└─ Si error:
   ├─ Dead Letter Queue: ordenes-fallidas
   └─ CloudWatch alarm se dispara
```

#### Paso 5: SNS triggeriza Lambda payment
```
SNS recibe: order.confirmed
├─ Dispara Lambda: process-payment
├─ Lambda:
│  ├─ Llama servicio de pagos externo (Stripe)
│  ├─ Carga PDF receipt de S3
│  └─ Según resultado:
│     ├─ Aprobado: Publica order.paid a SNS
│     └─ Rechazado: Publica order.payment_failed a SNS
└─ Retorna status
```

#### Paso 6: SNS notifica usuario
```
SNS topic: order.paid
├─ Subscriber 1: Email service
│  └─ Envía: "Tu orden 12345 está confirmada"
├─ Subscriber 2: SMS service
│  └─ Envía: "Pedido #12345: orden confirmada"
├─ Subscriber 3: Webhook a CRM
│  └─ POST datos a CRM externo
└─ Subscriber 4: Lambda notification
   └─ Actualiza interfaz push notifications
```

#### Paso 7: CloudWatch monitorea todo
```
CloudWatch agregador:
├─ Métrica: API request_count = 1
├─ Métrica: Lambda duration = 145ms
├─ Métrica: RDS connections = 102/150 (80% threshold)
├─ Métrica: SQS queue_depth = 5 mensajes
├─ Log: "User 999 created order 12345"
├─ Log: "Payment processor latency: 342ms"
└─ Alarm check: if queue_depth > 10
   ├─ YES → Scale ECS (add more tasks)
   └─ NO → OK
```

#### Paso 8: Usuario recibe respuesta
```
Browser recibe:
{
  "orderId": 12345,
  "status": "created",
  "estimatedDelivery": "2026-02-20",
  "confirmationEmail": "Enviado a user@example.com"
}

Tiempo total: ~500ms
```

---

## 5. Diagrama de Capas (Visualización Conceptual)

```
┌───────────────────────────────────────────────────────────┐
│  CAPA 1: ENTRADA (Users & Partners)                       │
│                                                            │
│  Users (navegador) → HTTPS → CloudFront → S3 (HTML/CSS)  │
│  Mobile App        → REST API                             │
│  Partner API       → Webhooks                             │
└────────────────────┬────────────────────────────────────┘

┌────────────────────▼────────────────────────────────────┐
│  CAPA 2: ENRUTAMIENTO & AUTENTICACIÓN (Edge)            │
│                                                         │
│  API Gateway (1000 req/s, rate limit, auth)            │
│  ├─ Valida esquema                                      │
│  ├─ Verifica IAM permissions                            │
│  └─ Enruta a Lambda/ECS/EC2                             │
└────────────────────┬────────────────────────────────────┘

┌────────────────────▼────────────────────────────────────┐
│  CAPA 3: PROCESAMIENTO (Compute)                         │
│                                                         │
│  Lambda (sincrónico)  ECS (asincrónico)  EC2 (custom) │
│  ├─ Request/response   ├─ Tareas largas    ├─ Legacy   │
│  ├─ Serverless        └─ Escalable         └─ Comple   │
│  └─ <15 min timeout                                     │
└────────────────────┬────────────────────────────────────┘

┌────────────────────▼────────────────────────────────────┐
│  CAPA 4: DATOS PERSISTENTES (Data)                       │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │     RDS      │  │       S3     │  │   DynamoDB   │  │
│  │(Relacional)  │  │   (Objects)  │  │   (NoSQL)    │  │
│  │ Multi-AZ     │  │  Versioning  │  │   Scalable   │  │
│  │ Backup auto  │  │   Replicat   │  │   eventual   │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
└────────────────────┬────────────────────────────────────┘

┌────────────────────▼────────────────────────────────────┐
│  CAPA 5: EVENTOS & MENSAJERÍA (Async)                    │
│                                                         │
│  SQS (Queue)  SNS (Broadcast)  EventBridge (Rules)     │
│  ├─ FIFO     ├─ Fan-out       ├─ Complex workflows     │
│  └─ Standard └─ Multi-target   └─ Event routing        │
└────────────────────┬────────────────────────────────────┘

┌────────────────────▼────────────────────────────────────┐
│  CAPA 6: SEGURIDAD & ACCESO (Todos los niveles)         │
│                                                         │
│  IAM Roles      Security Groups      KMS Encryption    │
│  ├─ Permisos    ├─ Firewall          ├─ Keys managed  │
│  ├─ Auditoría   └─ Network layer     └─ Compliance    │
│  └─ MFA                                                 │
└────────────────────┬────────────────────────────────────┘

┌────────────────────▼────────────────────────────────────┐
│  CAPA 7: OBSERVABILIDAD (Transversal)                    │
│                                                         │
│  CloudWatch (Métricas + Logs + Alarms)                 │
│  ├─ CPU, memoria, latencia                              │
│  ├─ Logs agregados                                      │
│  ├─ Alarms → SNS → Slack/Pager                         │
│  └─ Dashboards centralizados                            │
└────────────────────────────────────────────────────────┘
```

---

## 6. Flujos de Datos Específicos

### 6.1 Procesamiento Sincrónico (Usuario espera)

```
Usuario → API Gateway → Lambda → RDS → Lambda → respuesta
         <2 segundos>
```

**Cuándo usar**: Consultas, operaciones simples, respuestas inmediatas

**Ejemplo**: GET /user/123 → Lambda consulta RDS → retorna JSON

### 6.2 Procesamiento Asincrónico (Usuario no espera)

```
Usuario → API Gateway → Lambda → SQS → ECS/Lambda → email
         <100ms>       async      procesa

Usuario recibe: {"status": "processing", "id": 12345}
```

**Cuándo usar**: Procesamiento pesado, generación PDF, envío email, análisis datos

**Ejemplo**: POST /report/generate → Lambda enqueue → ECS genera PDF → email

### 6.3 Event-Driven (Reacción en Cadena)

```
RDS update → Lambda → SNS → 
                      ├─ Lambda 1 (email)
                      ├─ Lambda 2 (CRM)
                      ├─ SQS (queue 1)
                      └─ HTTP webhook
```

**Cuándo usar**: Propagación de cambios, múltiples sistemas necesitan reaccionar

**Ejemplo**: Pedido cancelado → notificar cliente, warehouse, y finanzas

### 6.4 Streaming de Datos (Real-time)

```
IoT Device → Kinesis → Lambda → RDS/S3
  (1000s eventos/seg)
  
CloudWatch analiza:
├─ Temperatura promedio
├─ Anomalías detectadas
└─ Alertas en tiempo real
```

---

## 7. Buenas Prácticas de Seguridad

### 7.1 Control de Acceso (IAM)

```yaml
Principio: Least Privilege (mínimo acceso necesario)

EC2 Instance:
  Role: EC2-ReadS3-WriteRDS
  Policies:
    - s3:GetObject (bucket: reports-only)
    - rds:DescribeDB*
    - rds:DescribeDBInstances (resource: prod-db)
  
  PROHIBIDO:
    - s3:* (acceso a TODO S3)
    - rds:* (acceso a TODO RDS)
    - iam:* (cambiar propios permisos)
```

**Best practices**:
- Usar Roles, no Access Keys (en EC2/Lambda)
- MFA para root account (SIEMPRE)
- Auditar permissions regularmente
- CloudTrail → quién hizo qué

### 7.2 Aislamiento de Red (VPC)

```
Regla: Private Subnet para datos, Public Subnet para entrada

Public Subnet (10.0.1.0/24):
  - API Gateway (managed)
  - ALB (load balancer)
  - NAT Gateway (para salida)

Private Subnet (10.0.2.0/24):
  - EC2/Lambda (aplicaciones)
  - Security Group permite 5432 solo desde App SG

Private Subnet (10.0.3.0/24):
  - RDS (base de datos)
  - Security Group permite 5432 solo desde App SG
  - NO ACCESO DIRECTO DESDE INTERNET
```

**Best practices**:
- BD siempre en Private Subnet
- Aplicación en Private Subnet si posible
- ALB en Public Subnet
- Outbound a Internet vía NAT Gateway

### 7.3 Encriptación

```yaml
Encriptación en Tránsito:
  - API Gateway → Lambda: HTTPS (TLS 1.2+)
  - Lambda → RDS: Use SSL/TLS connection string
  - S3: Enforce SSL (bucket policy)

Encriptación en Reposo:
  - RDS: KMS encryption habilitada
  - S3: SSE-KMS (customer managed keys)
  - EBS: Encrypted volumes
```

**Best practices**:
- KMS keys para control granular
- Rotar keys regularmente
- Audit logging de acceso a keys

### 7.4 Validación y Sanitización

```
API Gateway:
  - JSON schema validation
  - Rate limiting (1000 req/s)
  - Request timeout (29 segundos)

Lambda:
  - Validar input types
  - Rechazar si no cumple schema
  - SQL prepared statements (no concatenación)

Ejemplo MALO:
  query = f"SELECT * FROM users WHERE id = {user_id}"
  
Ejemplo BUENO:
  query = "SELECT * FROM users WHERE id = %s"
  execute(query, [user_id])
```

---

## 8. Buenas Prácticas de Escalabilidad

### 8.1 Escala Automática

```yaml
Auto Scaling Group (ECS/EC2):
  Métrica: CPU > 70% por 2 minutos
  Acción: Lanzar 2 instancias más
  
  Métrica: CPU < 30% por 5 minutos
  Acción: Terminar 1 instancia
  
  Límites:
    min_size: 2 (siempre correr)
    max_size: 10 (límite de costos)

SQS/Lambda:
  Métrica: queue_depth > 100 mensajes
  Acción: Lambda escala a 1000 concurrencias
  
CloudFront:
  - No requiere escalado (manejado por AWS)
  - Automáticamente distribuye a 200+ ubicaciones
```

### 8.2 Caching Multinivel

```
Nivel 1: CloudFront (CDN)
  - Cache HTML/CSS/JS por 1 día
  - Cache API responses por 5 minutos
  - Se degrada a S3 automáticamente

Nivel 2: ElastiCache (Redis)
  - Cache sesiones usuario
  - Cache queries RDS frecuentes
  - TTL: 1 hora

Nivel 3: RDS Query Cache
  - Automático para queries simples
  - Monitor: query_cache_size

Invalidación:
  - Cuando datos cambian
  - DELETE endpoint: purga CloudFront + Redis
```

### 8.3 Desacoplamiento

```
MALO (Acoplado):
  Usuario → Aplicación → RDS
            → Email
            → Analytics
            
  Si email es lento, usuario espera más

BUENO (Desacoplado):
  Usuario → API Gateway → Lambda A (rápido 100ms)
                          ↓
                      SQS queue
                       ↙ ↓ ↘
               Lambda B  ECS  Lambda C
               (email)   (analytics) (CRM)
               
  Usuario recibe respuesta rápido
  Tareas pesadas en background
```

---

## 9. Monitoreo y Observabilidad

### 9.1 Métricas Críticas

```
API Gateway:
  - Latency (ms): P50, P99
  - 4XX errors: requests inválidas
  - 5XX errors: errores del servidor

Lambda:
  - Duration: tiempo ejecución
  - Errors: exceptions
  - Throttles: concurrencia alcanzada
  - Cold starts: primeras invocaciones

RDS:
  - CPU utilization: % de CPU
  - Database connections: activas
  - Storage used: GB
  - Read latency: ms

SQS:
  - Queue depth: mensajes pendientes
  - ApproximateAgeOfOldestMessage: qué tan viejo
  - Dead letter queue size: errores
```

### 9.2 Alarms & Alertas

```yaml
Alarms:
  - Lambda errors > 5 / minuto → Slack
  - RDS connections > 80% → Email devops
  - SQS queue_depth > 1000 → Scale ECS
  - API latency P99 > 1000ms → Page on-call
  - EC2 disk space > 85% → Alert

Escalation:
  Severity 1 (Critical):
    - Page engineer
    - Slack + Email + SMS
    
  Severity 2 (High):
    - Slack + Email
    - Review en próxima standup
    
  Severity 3 (Low):
    - Slack log only
```

---

## 10. Arquitectura de Ejemplo: Aplicación Completa

### Requisitos de Negocio
- Aplicación web con login seguro
- API REST para móvil
- Procesamiento asincrónico de reportes
- Notificaciones en tiempo real
- Escalabilidad automática
- Compliance: GDPR, datos encriptados

### Solución Integrada

```
1. ENTRADA
   └─ Usuarios → CloudFront (CDN)
                  ├─ Assets: HTML/CSS/JS desde S3
                  ├─ API requests → API Gateway
                  └─ Cognito: autenticación

2. ENRUTAMIENTO
   └─ API Gateway
      ├─ GET /profile → Lambda-profile
      ├─ POST /login → Lambda-auth (Cognito)
      ├─ POST /report → Lambda-report (async)
      └─ Todos con rate limit, validación, CORS

3. PROCESAMIENTO
   ├─ Lambda (síncrono)
   │  └─ GET requests, queries simples
   ├─ ECS (asincrónico)
   │  └─ Reportes PDF, análisis datos
   └─ EC2 (workers legados)
      └─ Integración sistemas antiguos

4. DATOS
   ├─ RDS PostgreSQL Multi-AZ
   │  ├─ Usuarios, perfiles
   │  ├─ Datos transaccionales
   │  └─ Backups automáticos a S3
   ├─ S3
   │  ├─ PDFs generados
   │  ├─ User uploads
   │  └─ Logs & backups
   └─ ElastiCache Redis
      ├─ Sesiones usuario
      ├─ Cache persistente
      └─ Rate limiting counters

5. MENSAJERÍA
   ├─ SQS: report-generation queue
   │  └─ Mensajes: {"userId": 999, "type": "monthly"}
   └─ SNS: user.events topic
      ├─ Subscriber: Email service
      ├─ Subscriber: Analytics
      └─ Subscriber: SQS → processing

6. SEGURIDAD (Transversal)
   ├─ IAM Roles restrictivas
   ├─ VPC con Private Subnets
   ├─ Security Groups por capas
   ├─ KMS para encriptación
   └─ CloudTrail para auditoría

7. OBSERVABILIDAD (Transversal)
   ├─ CloudWatch (logs + métricas)
   ├─ X-Ray (distributed tracing)
   ├─ Alarms → SNS → Slack
   └─ Dashboards: latencia, errores, recursos
```

### Flujo de Login (Caso de uso)
```
1. Usuario ingresa email/password
   └─ Cliente → HTTPS → API Gateway

2. API Gateway valida JWT (si existe)
   └─ Sin JWT → enruta a Lambda-auth

3. Lambda-auth
   ├─ Llama Cognito: "¿validar credenciales?"
   ├─ Cognito retorna JWT token
   ├─ Lambda guarda sesión en ElastiCache (1 hora TTL)
   └─ Retorna token al cliente

4. En siguientes requests
   ├─ Cliente envía: Authorization: Bearer <JWT>
   ├─ API Gateway valida token sin Lambda
   ├─ Si válido → enruta a endpoint
   └─ Si inválido → 401 Unauthorized

5. Auditoría
   ├─ CloudTrail: "user 999 logged in"
   ├─ CloudWatch: "login latency: 234ms"
   └─ Si fallo: SNS → alerta
```

---

## 11. Decisiones Arquitectónicas Clave

### 11.1 Sincrónico vs Asincrónico

| Caso | Síncrono | Asincrónico |
|------|----------|-------------|
| GET endpoint | SÍ | - |
| POST crear entidad | SÍ | - |
| Generar PDF | - | SÍ (SQS+ECS) |
| Envío email | - | SÍ (SNS+Lambda) |
| Analytics | - | SÍ (streaming) |
| Pago | SÍ (crítico) | - |
| Notificación push | - | SÍ (SNS) |

### 11.2 Lambda vs ECS vs EC2

| Aspecto | Lambda | ECS | EC2 |
|--------|--------|-----|-----|
| Duración máxima | 15 min | Ilimitado | Ilimitado |
| Startup | <1s (warm), 3-5s (cold) | 30s+ | Varios min |
| Precio | Por invocación | Por task | Por hora |
| Control SO | Nulo | Docker | Total |
| Best para | Eventos, APIs | Microservicios | Aplicaciones complejas |

### 11.3 RDS vs DynamoDB

| Aspecto | RDS | DynamoDB |
|--------|-----|----------|
| Modelo | Relacional (SQL) | NoSQL (key-value) |
| Transacciones | ACID | Limited (v1) |
| Queries | Ad-hoc | Defined schemas |
| Escalabilidad | Manual/RReplicas | Automática |
| Costo | Por instancia | Por throughput |

---

## 12. Resumen de Integraciones

### Matriz de Flujos Típicos

```
Entrada de Usuario
  ↓
API Gateway (validación, auth)
  ├─ Ruta A: Sincrónico
  │  ├─ Lambda
  │  │  ├─ Lee RDS
  │  │  ├─ Valida
  │  │  └─ Retorna 200 OK
  │  └─ Timeout: 29 segundos
  │
  └─ Ruta B: Asincrónico
     ├─ Lambda enqueue a SQS
     ├─ Retorna 202 Accepted rápido
     └─ ECS consumer procesa evento
        ├─ Accede RDS
        ├─ Genera S3 file
        └─ Publica SNS
           ├─ Email
           ├─ SMS
           └─ Webhook
```

---

## 13. Checklist de Implementación

```
[ ] IAM Roles creados (least privilege)
[ ] VPC con subnets públicas y privadas
[ ] RDS en Private Subnet, Multi-AZ
[ ] S3 buckets con versioning + encryption
[ ] Lambda con VPC + IAM roles
[ ] SQS queues configuradas (visibility timeout, DLQ)
[ ] SNS topics con subscribers
[ ] CloudWatch dashboards creados
[ ] Alarms en métricas críticas
[ ] API Gateway con rate limiting
[ ] CloudFront + S3 para assets estáticos
[ ] ElastiCache para caching
[ ] Auto Scaling Groups configuradas
[ ] CloudTrail habilitado
[ ] Encryption en tránsito (TLS)
[ ] Encryption en reposo (KMS)
[ ] Backup strategy documentada
[ ] Disaster recovery plan
[ ] Load testing: verificar limits
[ ] Cost optimization review
```

---

## 14. Conclusión

Estos 12 servicios AWS trabajan como un ecosistema integrado:

- **IAM** rige quién puede hacer qué
- **VPC** aisla tu infraestructura
- **EC2/Lambda/ECS** procesan lógica
- **RDS/S3** persisten datos
- **API Gateway** expone interfaces
- **SQS/SNS** desacoplan sistemas
- **CloudWatch** observa todo
- **CloudFront** optimiza distribución

La clave es elegir los servicios correctos para cada problema, integrándolos de forma segura, escalable y observable.

---

**Referencia**: Estos conceptos se aplican a arquitecturas en AWS de cualquier escala, desde startups hasta enterprises Fortune 500.

---

**Creado**: 16 de febrero de 2026  
**Versión**: 1.0  
**Audiencia**: Arquitectos y desarrolladores intermedio-avanzado
