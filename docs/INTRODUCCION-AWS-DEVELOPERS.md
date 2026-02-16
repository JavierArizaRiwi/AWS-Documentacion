# Introducción a AWS para Desarrolladores sin Experiencia en Cloud

**Nivel**: Principiante  
**Duración estimada**: 30 minutos  
**Objetivo**: Entender conceptos fundamentales de AWS y cómo construir arquitecturas  
**Audiencia**: Desarrolladores que nunca han usado servicios en la nube

---

## 1. Qué es Cloud Computing

Cloud computing significa usar computadoras y servicios a través de Internet, en lugar de tenerlos localmente en tu oficina.

### Analogía de la Vida Cotidiana

**Antes (On-Premises)**: Era como tener tu propio supermercado. Necesitabas:
- Construir el edificio (inversión inicial)
- Pagar arriendo del local
- Contratar empleados 24/7
- Comprar estanterías, cajas registradoras
- Mantener el edificio funcionando
- Pagar aunque no tengas clientes

**Ahora (Cloud)**: Es como comprar en línea en Amazon:
- No tienes que construir nada
- Pagas solo por lo que usas
- No te preocupas por el almacén
- Si necesitas más productos, simplemente se envían más
- Si no compras ese mes, no pagas

### Ventajas del Cloud

- **Pagas por uso**: No hay inversión inicial grande
- **Escalabilidad**: Tu aplicación crece según necesites
- **Disponibilidad global**: Acceso desde cualquier lugar del mundo
- **Menos mantenimiento**: El proveedor se encarga de la infraestructura
- **Seguridad profesional**: Equipos expertos protegen tus datos

---

## 2. Qué es AWS (Amazon Web Services)

AWS es una plataforma de computación en la nube proporcionada por Amazon. Es como una "tienda de servicios" donde puedes alquilar máquinas virtuales, bases de datos, almacenamiento, y mucho más.

### AWS vs Mercado Tradicional

Piénsalo como un mercado de consumibles:

- **Supermercado local** (Proveedor on-prem): Un solo lugar, horarios limitados, si algo se agota, esperas
- **Amazon (AWS)**: Miles de productos disponibles 24/7, envío rápido, multitud de opciones, pago flexible

### Datos de AWS

- Más de **200 servicios** disponibles
- Presencia en **30+ regiones** mundiales
- Cerca del **32% del mercado** de cloud
- Usado por empresas desde startups hasta Fortune 500

---

## 3. Regiones y Zonas de Disponibilidad

Conceptos fundamentales para entender dónde viven tus datos.

### Región (Region)

Una región es una **ubicación geográfica completamente independiente** donde AWS tiene uno o más data centers.

**Ejemplos de regiones:**
- us-east-1 (Virginia, USA)
- eu-west-1 (Irlanda)
- ap-northeast-1 (Tokyo, Japón)
- sa-east-1 (São Paulo, Brasil)

**Analogía**: Imagina que AWS es una cadena de hoteles:
- Cada región es un hotel en una ciudad diferente
- Si el hotel de Nueva York se cae, el de Japón sigue funcionando
- Datos en NY no bajan automáticamente a Japón (lo haces tú)

**¿Por qué múltiples regiones?**
1. Latencia baja (usuarios en Japón, servidor en Japón = más rápido)
2. Cumplimiento legal (datos en EU deben estar en EU por GDPR)
3. Disaster Recovery (si región A falla, usas región B)

### Zona de Disponibilidad (Availability Zone - AZ)

Dentro de cada región, hay varias Zonas de Disponibilidad. Son **data centers físicos independientes** conectados por red rápida.

**Ejemplo:**
- Región: us-east-1
  - AZ-1a (Virginia - Data center 1)
  - AZ-1b (Virginia - Data center 2)
  - AZ-1c (Virginia - Data center 3)

**Analogía**: La región es la ciudad, las AZs son distritos diferentes:
- Hotel Nueva York (región)
  - Suite en Midtown (AZ-1a)
  - Suite en Brooklyn (AZ-1b)
  - Suite en Queens (AZ-1c)
- Si la electricidad falla en Midtown, Brooklyn sigue funcionando

**Para Alta Disponibilidad**: Distribuyes tu aplicación en **múltiples AZs**:
- Base de datos en AZ-1a
- Aplicación backup en AZ-1b
- Si AZ-1a falla, AZ-1b toma el relevo automáticamente

---

## 4. IaaS, PaaS y Serverless

Tres modelos de cómo AWS te proporciona servicios. La diferencia es **quién es responsable de qué**.

### IaaS (Infrastructure as a Service)

**Tú obtienes**: Una computadora virtual en Internet (sin el hardware físico)  
**Tú eres responsable de**: Sistema operativo, aplicaciones, datos, seguridad  
**AWS es responsable de**: Hardware, red, data centers

**Servicio AWS**: EC2 (Elastic Compute Cloud)

**Analogía**: Alquilar un departamento vacío
- Tú pagas renta, tienes 4 paredes, electricidad, agua
- Tú compras muebles, pintas paredes, decides quién entra
- Dueño solo mantiene estructura del edificio

**Caso de uso**: Cuando necesitas control total (servidor web específico, SO personalizado)

```
Hardware ────────────────────────────── AWS
Sistema Operativo ──────────────────── TUYO
Aplicaciones ────────────────────────── TUYO
Datos ───────────────────────────────── TUYO
Seguridad ───────────────────────────── TUYO
```

### PaaS (Platform as a Service)

**Tú obtienes**: Una plataforma donde correr aplicaciones sin pensar en servidores  
**Tú eres responsable de**: Código, datos, aplicaciones  
**AWS es responsable de**: Infraestructura, SO, middleware, runtime

**Servicio AWS**: Elastic Beanstalk (o App Service, Cloud Run en otros)

**Analogía**: Alquilar un estudio fotográfico completamente equipado
- Tú llevas la cámara, sujeto, y tomas fotos
- Studio proporciona luces, fondos, equipamiento
- No te preocupas por el cableado o generador

**Caso de uso**: Cuando solo quieres escribir código, sin administrar servidores

```
Hardware ────────────────────────────── AWS
Sistema Operativo ──────────────────── AWS
Middleware ──────────────────────────── AWS
Runtime ─────────────────────────────── AWS
Aplicaciones ────────────────────────── TUYO
Datos ───────────────────────────────── TUYO
```

### Serverless (Sin servidores)

**Tú obtienes**: Ejecutas código sin pensar en servidores. Escribes función, la subes, escala automátic  
**Tú eres responsable de**: Código, datos  
**AWS es responsable de**: TODO lo demás (donde corre, cuándo escala, qué tan rápido)

**Servicio AWS**: Lambda

**Analogía**: Usar un servicio de "repartidor":
- Tienes un paquete (función), lo dejas en la puerta
- Repartidor lo entrega en <2 minutos
- No te preocupas por logística, rutas, transporte
- Pagas por entrega, no por "repartidor sentado esperando"

**Caso de uso**: Cuando necesitas escalar infinitamente sin pensarlo, pagar poco por uso bajo

```
Hardware ────────────────────────────── AWS
Sistema Operativo ──────────────────── AWS
Middleware ──────────────────────────── AWS
Runtime ─────────────────────────────── AWS
Scaling ─────────────────────────────── AWS (automático)
Aplicaciones ────────────────────────── TUYO
Datos ───────────────────────────────── TUYO
```

### Comparación

| Aspecto | IaaS (EC2) | PaaS (Beanstalk) | Serverless (Lambda) |
|---------|-----------|-----------------|-------------------|
| Control | Total | Medio | Bajo |
| Complejidad | Alta | Media | Baja |
| Costo para poco uso | Más caro | Medio | Más barato |
| Escala manual | Sí | Automático | Automático |
| Inicio rápido | No | Sí | Muy sí |

---

## 5. Modelo de Responsabilidad Compartida

AWS no es 100% responsable, ni tú eres 100% responsable. Es compartido.

### Regla General

**AWS es responsable de**: "Seguridad DE la nube"  
**Tú eres responsable de**: "Seguridad EN la nube"

### Ejemplos Prácticos

#### Datos (Tu responsabilidad)
- Aunque AWS encripta datos en tránsito, tú decides si usar encriptación
- AWS no puede recuperar datos que TÚ borres (eso es tu responsabilidad)
- Tú eres responsable de backups regulares

#### Credenciales (Tu responsabilidad)
- AWS proporciona autenticación segura. Pero si escribes contraseña en archivo .txt en GitHub, es tu culpa
- IAM permissions (quién accede qué): Tu responsabilidad

#### Hardware (AWS responsabilidad)
- Si el servidor físico falla, AWS lo reemplaza sin que hagas nada
- AWS actualiza BIOS, reemplaza CPUs muertas
- Tú no necesitas saber que sucede

#### Red (Compartida)
- AWS garantiza conectividad y DDoS protection
- Pero si TÚ configuras Security Group permitiendo "0.0.0.0/0" (todo el mundo), eso es tu culpa

### Matriz de Responsabilidad

```
                CLIENTE         AWS
────────────────────────────────────
Aplicaciones      100%            0%
Datos             100%            0%
Runtime           VARIABLE    VARIABLE
Database OS       VARIABLE    VARIABLE
Network Config    VARIABLE    VARIABLE
Storage           VARIABLE    VARIABLE
OS                VARIABLE    VARIABLE
Hypervisor             0%       100%
Hardware               0%       100%
Physical Data Ctr      0%       100%
Facilities             0%       100%
```

---

## 6. Servicios Principales y Cómo se Relacionan

Entender cómo se conectan los servicios es crucial para diseñar arquitecturas.

### Diagrama Mental

```
Internet
   ↓
IAM (¿Quién eres?)
   ↓
VPC (¿Dónde estás?)
   ↓
EC2 (¿Qué máquina?)
   ↓
RDS (¿Guardas datos?)
   ↓
S3 (¿Archivos?)
   ↓
Lambda (¿Código sin servidor?)
```

### 6.1 IAM (Identity and Access Management)

**¿Qué es?**: El servicio que controla **quién** tiene acceso **a qué**.

**Analogía**: Portero de un nightclub:
- Verifica tu identidad (autenticación)
- Decide qué áreas puedes entrar (autorización)
- Revoca entrada si necesita (deactivación)

**Conceptos clave**:
- **Usuario**: Persona individual (john@example.com)
- **Rol**: Conjunto de permisos (RolAdministrator)
- **Permiso**: Acción específica (s3:GetObject = leer archivo en S3)

**Relación con otros servicios**: IAM está en TODO. Antes de que EC2 ejecute código, IAM verifica permisos.

**Ejemplo real**: EC2 quiere leer de S3
```
EC2 dice: "Quiero archivo de S3"
IAM interviene: "¿Eres EC2 válida? ¿Tienes permiso s3:GetObject?"
Si SÍ → acceso permitido
Si NO → acceso denegado
```

### 6.2 VPC (Virtual Private Cloud)

**¿Qué es?**: Tu red privada en AWS. Donde viven todos tus recursos y se comunican.

**Analogía**: Tu condominio:
- Cada condominio es independiente (diferente VPC)
- Dentro hay apartamentos (servidores)
- Hay una calle principal (Internet)
- Hay guardias (Security Groups)

**Conceptos clave**:
- **Subnet**: Segmento de red (público o privado)
- **Public Subnet**: Accesible desde Internet
- **Private Subnet**: Solo accesible internamente
- **Route Table**: Reglas de tráfico
- **Security Group**: Firewall

**Ejemplo de arquitectura simple**:
```
VPC: 10.0.0.0/16 (red privada)
  ├─ Public Subnet (10.0.1.0/24)
  │  └─ EC2 servidor web (accesible desde Internet)
  └─ Private Subnet (10.0.2.0/24)
     └─ RDS base de datos (solo accesible desde EC2)
```

### 6.3 EC2 (Elastic Compute Cloud)

**¿Qué es?**: Una computadora virtual (servidor) que ejecuta tu aplicación.

**Analogía**: Alquilar una computadora en Internet:
- Puedes elegir el tamaño (procesador débil o potente)
- Pagas por hora de uso
- Puedes iniciarlo/apagarlo cuando quieras

**Conceptos clave**:
- **Instancia**: Una máquina individual
- **AMI**: Imagen del sistema (OS preinstalado)
- **Instance Type**: Tamaño (t3.micro, m5.large, etc.)
- **Key Pair**: Contraseña para acceder vía SSH

**Cómo se relaciona**:
- Vive **dentro de una VPC** (red)
- Acceso controlado por **IAM** (quién puede)
- Acceso de red controlado por **Security Group** (firewall)
- Ejecuta aplicación que conecta a **RDS** (base de datos)

**Ejemplo**:
```
Usuario en Internet → (SSH vía Key Pair) → EC2 → (consultas SQL) → RDS
                     controlado por       dentro  controlado      en Private
                     IAM                  de VPC   por Security    Subnet
```

### 6.4 RDS (Relational Database Service)

**¿Qué es?**: Base de datos administrada (MySQL, PostgreSQL, Oracle, SQL Server).

**Analogía**: Librería administrada vs. biblioteca en tu casa:
- **On-prem (en tu laptop)**: Instalas MySQL, lo mantienes, actualizas, haces backups
- **RDS (AWS)**: AWS lo hace por ti. Tú solo usas la BD.

**Conceptos clave**:
- **Multi-AZ**: Replica automática en otra zona de disponibilidad
- **Backup automático**: Diario, retenido 35 días
- **Read Replicas**: Copias de lectura en otras regiones

**Cómo se relaciona**:
- Vive **en una Private Subnet** (no expuesta a Internet)
- EC2 conecta vía **Security Group** (firewall permite puerto 3306/5432)
- Acceso autenticado con **usuario/contraseña** o **IAM Database Auth**
- **Versionado/backups** se guardan en **S3**

### 6.5 S3 (Simple Storage Service)

**¿Qué es?**: Almacenamiento de archivos masivos (ilimitado, $0.023/GB).

**Analogía**: Guardarropa gigante:
- Guardas lo que quieras
- No importa cantidad
- Recuperas cuando necesites
- Pagas por espacio

**Conceptos clave**:
- **Bucket**: Contenedor (como carpeta raíz)
- **Object**: Archivo individual
- **Versioning**: Mantiene historial de cambios
- **Lifecycle**: Mueve a almacenamiento más barato después de tiempo (Glacier)

**Cómo se relaciona**:
- EC2 sube/descarga archivos (control vía **IAM**)
- Lambda procesa archivos cuando se suben
- Backups de **RDS** se guardan en S3
- **CloudFront** (CDN) sirve desde S3 rápidamente

### 6.6 Lambda (Computación Serverless)

**¿Qué es?**: Ejecutar código sin pensar en servidores. Escribes función, la subes, AWS escala.

**Analogía**: Servicio de mensajería:
- Mandas paquete (código)
- Sistema lo entrega cuando se necesita
- Pagas solo por tiempo de ejecución

**Conceptos clave**:
- **Función**: Código que ejecuta
- **Trigger**: Evento que dispara (archivo sube a S3, mensaje en SQS, etc.)
- **Concurrency**: Cuántas funciones corren simultáneamente

**Cómo se relaciona**:
- Lambda puede estar **en VPC** (acceso a RDS privada)
- Controlado por **IAM** (permisos a qué recursos acceder)
- Se **triggeriza desde S3** (archivo sube → Lambda procesa)
- Escala automáticamente (0 a 10,000 funciones paralelas)

---

## 7. Caso de Uso: Arquitectura Completa

Entendamos cómo se conectan todos estos servicios en una aplicación real.

### Requisito: E-commerce Simple

- Sitio web estático (HTML/CSS/JS)
- Backend API (para compras)
- Base de datos (productos, usuarios, órdenes)
- Almacenamiento (imágenes de productos)

### Arquitectura

```
┌─────────────────────────────────────────────────────────┐
│                    INTERNET                             │
└────────────────────┬────────────────────────────────────┘
                     │
        Usuarios acceden a sitio web
                     │
┌────────────────────▼────────────────────────────────────┐
│        CloudFront (CDN)                                 │
│  Sirve contenido rápidamente desde 200+ ubicaciones    │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
        ┌─────────────────────┐
        │   S3 Bucket         │
        │ (HTML/CSS/images)   │
        │   Public access     │
        └─────────────────────┘

┌─────────────────────────────────────────────────────────┐
│            AWS VPC (10.0.0.0/16)                        │
│                                                          │
│  ┌─────────────────────────────────────────────────┐   │
│  │  Public Subnet (10.0.1.0/24)                    │   │
│  │  ┌────────────────────────────────────────────┐ │   │
│  │  │ ALB (Application Load Balancer)            │ │   │
│  │  │ Distribuye tráfico entre EC2s             │ │   │
│  │  └──────────────────┬───────────────────────┘ │   │
│  └─────────────────────┼─────────────────────────┘   │
│                        │                               │
│  ┌─────────────────────▼─────────────────────────┐   │
│  │  Private Subnet - Application (10.0.2.0/24)  │   │
│  │  ┌────────────────┐    ┌────────────────┐    │   │
│  │  │  EC2 Instance  │    │  EC2 Instance  │    │   │
│  │  │ (backend API)  │    │ (backend API)  │    │   │
│  │  │                │    │                │    │   │
│  │  │ Node.js + code │    │ Node.js + code │    │   │
│  │  └────────┬───────┘    └────────┬───────┘    │   │
│  │           │                      │             │   │
│  │           └──────────────┬───────┘             │   │
│  └──────────────────────────┼─────────────────────┘   │
│                             │                         │
│  ┌──────────────────────────▼──────────────────────┐  │
│  │ Private Subnet - Database (10.0.3.0/24)        │  │
│  │  ┌──────────────────────────────────────────┐  │  │
│  │  │  RDS (Multi-AZ PostgreSQL)               │  │  │
│  │  │  ├─ Persistent storage                   │  │  │
│  │  │  ├─ Automatic backups → S3               │  │  │
│  │  │  └─ Failover si falla AZ-1a              │  │  │
│  │  └──────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────┘   │
│                                                       │
└────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────┐
│ S3 Bucket - Product Images                            │
│ └─ Lifecycle: Move to Glacier after 1 year           │
│ └─ Versioning enabled (recuperar versiones antiguas)  │
└───────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────┐
│ CloudWatch (Monitoreo)                                │
│ └─ EC2 CPU > 80% → Auto Scaling dispara EC2 nuevo   │
│ └─ RDS storage > 80% → Alarma                        │
│ └─ Errores de aplicación → Alerta a equipo          │
└───────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────┐
│ IAM                                                   │
│ └─ EC2 Role: acceso a S3, RDS (solo lo necesario)   │
│ └─ Usuarios del equipo: permisos limitados           │
│ └─ Root account: MFA habilitado, poco acceso         │
└───────────────────────────────────────────────────────┘
```

### Flujo de una Compra

1. **Usuario accede sitio**
   - Navegador solicita HTML desde CloudFront
   - CloudFront obtiene de S3 (caché local)
   - Carga rápida desde ubicación geográfica cercana

2. **Usuario busca productos**
   - Navegador hace API call a ALB
   - ALB elige un EC2 disponible
   - EC2 consulta RDS por productos
   - RDS retorna datos, EC2 responde JSON
   - Navegador renderiza

3. **Usuario compra (Lambda detrás de escenas)**
   - API call a backend
   - EC2 guarda orden en RDS
   - Trigger Lambda automáticamente
   - Lambda envía email de confirmación
   - Lambda actualiza inventario

4. **Backups automáticos**
   - Cada noche, RDS snapshot → S3
   - Si BD se corrompe, recuperas snapshot en 5 minutos
   - Después de 1 año, snapshot se mueve a Glacier (costo reducido)

5. **Escalabilidad automática**
   - Traffic normal: 2 EC2 instances
   - Black Friday: CloudWatch detecta CPU >80%
   - Auto Scaling Group lanza 8 EC2s automáticamente
   - Después de Black Friday: instancias se apagan, costos bajan

### Costos Estimados Mensuales

| Servicio | Uso | Costo |
|----------|-----|-------|
| EC2 | 2 instances t3.medium | $60 |
| RDS | 1 db.t3.small Multi-AZ | $100 |
| S3 | 50GB storage | $1.15 |
| CloudFront | 100GB transferencia | $8.50 |
| Backups (S3) | 5GB | $0.12 |
| Data Transfer | Interno | $0 |
| **TOTAL** | | **~$170/mes** |

Con AWS Free Tier (primer año): -$60 (EC2) -$50 (RDS) = **~$60/mes**

---

## 8. Cómo Aprender Construyendo

La mejor forma de aprender AWS es haciendo. Aquí están los pasos:

### Paso 1: Crear Cuenta
- Ve a aws.amazon.com
- Crea cuenta (necesitas tarjeta de crédito)
- Recibe $200 crédito gratis (Free Tier)

### Paso 2: Ejercicios Progresivos
1. **Semana 1**: Crear EC2, conectarse vía SSH, instalar web server, servir página HTML
2. **Semana 2**: Crear RDS, conectar desde EC2, insertar datos, queries
3. **Semana 3**: Crear S3 bucket, subir archivo, hacer público, servir desde CloudFront
4. **Semana 4**: Crear Lambda, dispararla cuando archivo sube a S3
5. **Semana 5**: Combinar todo en arquitectura pequeña

### Paso 3: Monitorar Costos
- AWS Cost Explorer: ver exactamente qué cuesta qué
- Billing alerts: si factura > $10, recibir email
- Cleanup después de cada lab (terminar instancias, borrar buckets)

---

## 9. Errores Comunes a Evitar

1. **Usar root account para todo**
   - Crea IAM user, usa eso
   - Root account: solo cambios críticos + MFA habilitado

2. **Base de datos pública accidentalmente**
   - RDS debe estar en Private Subnet
   - Security Group solo permite desde EC2
   - Nunca 0.0.0.0/0 en puerto de BD

3. **Sin backups**
   - RDS automated backups: ON
   - S3 versioning: ON si datos críticos
   - S3 replicación cross-region: considera para DR

4. **Costos sorpresa**
   - Datos transferencia OUT: cuesta dinero
   - Snapshots viejos: borra si no necesitas
   - NAT Gateway: caro, úsalo solo si lo necesitas

5. **Sin monitoreo**
   - Configura CloudWatch alarms
   - Email si CPU >80%, storage >90%
   - Entiende factores de costo ANTES de ir a producción

---

## 10. Próximos Pasos

Este documento es introducción. Para profundizar:

1. **Servicios Específicos**: 
   - Lee documentación oficial de AWS
   - Busca tutoriales "AWS [SERVICIO] tutorial for beginners"

2. **Certificación**:
   - Objetivo inicial: AWS Certified Cloud Practitioner
   - Después: Solutions Architect Associate

3. **Comunidad**:
   - Reddit: r/aws
   - YouTube: "AWS in 100 Seconds" - explicaciones rápidas
   - Foros: AWS Developer Forums

4. **Proyecto Real**:
   - Toma proyecto personal
   - Despliégalo en AWS siguiendo arquitectura
   - Itera, aprende de errores

---

## 11. Resumen Rápido

| Concepto | Qué Es | Analogía |
|----------|--------|----------|
| Cloud | Computación vía Internet | Servicios bajo demanda |
| AWS | Plataforma de servicios en la nube | "Tienda" de infraestructura |
| Región | Ubicación geográfica | Sucursal en ciudad diferente |
| AZ | Data center en región | Almacén en barrio diferente |
| IaaS | Servidor virtual | Alquilar departamento vacío |
| PaaS | Plataforma para apps | Estudio equipado para fotógrafo |
| Serverless | Código sin servidores | Servicio de mensajería |
| IAM | Control de acceso | Portero que verifica identidad |
| VPC | Red privada | Tu condominio privado |
| EC2 | Máquina virtual | Computadora alquilada |
| RDS | Base de datos administrada | Librería administrada por otros |
| S3 | Almacenamiento de archivos | Guardarropa gigante |
| Lambda | Ejecución de código serverless | Servicio bajo demanda |

---

**Siguiente Lectura**: [01-console-iam.md](../01-basico/01-console-iam.md) - Crear tu primer usuario IAM

---

**Creado**: 16 de febrero de 2026  
**Versión**: 1.0  
**Audiencia**: Desarrolladores sin experiencia en cloud  
