# RDS (Relational Database Service) - Bases de Datos Administradas

## 1. ¿Qué es?

RDS es un servicio administrado de bases de datos relacionales que maneja la instalación, configuración, actualizaciones y backups automáticamente. RDS soporta múltiples motores: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server. Comparado con instalar BD manualmente en EC2, RDS automatiza tareas operacionales liberando al equipo para enfocarse en la aplicación en lugar de la infraestructura.

Técnicamente, RDS provisiona instancias con almacenamiento EBS, snapshots automáticos, failover Multi-AZ, replicación de lectura, y recuperación point-in-time.

## 2. Analogía Sencilla (Explicación de la Vida Cotidiana)

RDS vs instalar BD en EC2 es como contratar un cocinero vs comprar un restaurante:

- **EC2 + BD manual**: Compras infraestructura (servidor), instalas BD, apagas el servidor a las 2am para mantenimiento, aplicas patches, recuperas de falla de disco. TODO es tu responsabilidad. Costo inicial alto, mantenimiento constante.

- **RDS**: Contratas un servicio de catering. Especificas qué necesitas (5 conexiones simultáneas, 10GB almacenamiento), ellos manejan el resto. Si el chef se enferma, tienen backup. Si la comida expira, rotan la despensa. Pagas por uso, no por infraestructura.

RDS = administración automática de BD. La BD escala, se actualiza, y hace backup sin que hagas nada.

## 3. ¿Qué Problema Resuelve?

Problemas de instalar BD en EC2:

- **Mantenimiento constante**: Actualizar SO, aplicar security patches, repares de discos fallos
- **Backups manuales**: Fácil olvidarse o automatizar incorrectamente
- **Escalado tricky**: Si BD crece de 100GB a 500GB, ampliar volumen es riesgoso
- **Downtime durante mantenimiento**: Actualizar BD = no disponible por 1-2 horas
- **No hay replicación automática**: Si data center falla, pierdes datos (a menos que hayas configurado replicación manualmente)
- **Costos ocultos**: Servidor especial para BD, administrador especializado, respaldos en externos

RDS resuelve:

- **Mantenimiento automático**: AWS parchea, actualiza, optimiza. Tú no haces nada.
- **Backups automáticos**: Cada día, backup automático. Retención de 1-35 días.
- **Escalado sin downtime**: Cambiar clase de instancia mientras está running
- **Multi-AZ failover automático**: Falla replica → cambio automático en segundos
- **Read Replicas**: Replicas de lectura en otra región para baja latencia
- **Pricing claro**: Pagas por instance class + almacenamiento + backups

## 4. Componentes Principales

### DB Instance
Es tu servidor de BD. Define:
- **DB Instance Class**: db.t3.small (burleable, dev), db.r5.large (memory-optimized), etc.
- **Allocated Storage**: Tamaño inicial (10 GB a 65 TB)
- **Storage Autoscaling**: Crece automáticamente si se queda sin espacio

### DB Engine
Tipo de BD. Opciones:
- **MySQL**: Open source, popular, familiar
- **PostgreSQL**: Open source, advanced features (JSON, arrays)
- **MariaDB**: MySQL fork, compatible
- **Oracle Database**: Enterprise, cara
- **SQL Server**: Microsoft, integración AD
- **Aurora**: Amazon Native, compatible con MySQL/PostgreSQL, más rápido

### DB Subnet Group
Define en qué subnets puede vivir tu BD (siempre en VPC):
- Típicamente: subnets privadas (BD no accesible desde Internet)
- Multi-AZ: BD vive en 2 subnets en diferentes AZs
- Si falla Availability Zone A, automáticamente failover a Availability Zone B

### Security Groups
Firewall de BD. Define quién puede conectar:
- **Inbound**: Solo port 3306 (MySQL) desde security group de application
- **Outbound**: Típicamente no necesario para BD (no conecta a otros servicios usualmente)

Nunca "0.0.0.0/0" (de anywhere). DB debe estar oculta detrás de aplicación.

### Backup Strategy
- **Automated backups**: Cada noche, AWS captura backup completo
- **Retention period**: 1-35 días (default 7). Backups viejos se descartan
- **Point-in-time recovery**: Recuperar datos de hace 3 días, 3 horas, 3 minutos
- **Manual snapshots**: Tomar snapshot on-demand, retenido indefinidamente (hasta que borres)

### Multi-AZ Deployment
Banda-aid para failover automático:
- BD principal: Availability Zone A
- BD standby (replica): Availability Zone B, se sincroniza constantemente
- Falla AZ A → automáticamente DNS apunta a standby (failover < 1 minuto)
- Costo: ~2x (necesita 2 instancias)

### Read Replicas
Copias de lectura de BD principal:
- BD principal: soporta read + write
- Read replica 1: lectura solamente (menor latencia si aplicación en otra región)
- Read replica 2: lectura solamente
- Reduce load de BD principal:
  - Analytics queries pesadas: corren en replica, no afectan transaction processing
  - Aplicación en Japan: replica en ap-northeast-1 para baja latencia

Replicación asincrónica: pequeño lag (típicamente <1 segundo).

### Parameter Groups
Configuración de BD (MySQL max connections, log level, timeout):
- Cambios aplican sin reboot (algunos) o require reboot
- Puedes crear grupo custom, aplicar a múltiples instancias

### Enhanced Monitoring
Mejor visibilidad en OS-level metrics:
- CPU, RAM, network, disk I/O
- Vs default: solo conexiones, throughput

## 5. Casos de Uso Comunes

### Aplicación Web Transacional
- Tienda e-commerce: PostgreSQL con RDS
- Application EC2 conecta a RDS
- Multi-AZ para HA: si AZ falla, sitio permanece online
- Read replica en otra región para reportes (no afecta transacciones)

### Backend de API
- Node.js API → RDS MySQL
- Cached queries en Redis (separado)
- RDS handles: acceso concurrente, conexión pooling, backups

### OLAP (Analytics)
- Redshift > RDS para big data
- Pero: dataset pequeño <100GB → RDS es suficiente
- Corres heavy queries en read replica (no afecta BD principal)

### Compliance y Auditoría
- PCI-DSS: BD debe estar en VPC privada (no accessible de Internet) → RDS natural fit
- HIPAA: encriptado en transit (SSL) y at-rest (KMS)
- Multi-AZ: cumple High Availability requerimientos

### Migration desde On-Prem
- Base de datos legacy en servidor empresa
- Usar AWS DMS (Database Migration Service) para migrar a RDS
- Replicación contínua durante cutover
- Zero-downtime migration

## 6. Integración con Otros Servicios AWS

### VPC (Virtual Private Cloud)
RDS DEBE estar en VPC. Especificas qué subnets puede usar (DB Subnet Group). Típicamente en subnets privadas, sin acceso desde Internet.

### IAM (Identity and Access Management)
Define quién puede:
- Crear BD (PutDBInstance)
- Cambiar versión (ModifyDBInstance)
- Crear snapshots (CreateDBSnapshot)

Puedes usar IAM auth en lugar de password (más seguro para aplicaciones):
- EC2 obtiene token temporal de IAM
- Conecta a BD usando token
- Token expira en 15 minutos

### EC2 (Elastic Compute Cloud)
Aplicación en EC2 conecta a RDS:
- Connection string: `mysql://user:password@mydb.123456789.us-east-1.rds.amazonaws.com:3306/dbname`
- EC2 security group permite tráfico desde RDS security group
- No guardar password en código. Usar Secrets Manager o Parameter Store.

### Secrets Manager
Guarda credenciales de BD (username, password):
- EC2 obtiene credenciales de Secrets Manager en runtime
- Rotation automática cada 30 días
- Auditoría en CloudTrail

### CloudWatch (Monitoreo)
Métricas de RDS:
- CPU utilization, database connections, disk space, query performance
- Alarms: if CPU > 80%, notifica
- Enhanced Monitoring: OS-level disk I/O, network packets ($10/día)

### CloudTrail (Auditoría)
Registra acciones sobre RDS:
- Quién creó/modificó BD
- Cuándo
- Desde dónde
- Necesario para compliance

### S3 (Backup Archive)
RDS backups → S3 para long-term retention:
- RDS retiene 35 días. Después descartan.
- Exportar snapshot a S3 (Parquet) para análisis con Athena
- Archiva en Glacier para disaster recovery

### Lambda (Funciones)
Lambda conecta a RDS para:
- API endpoint sobre Lambda que consulta BD
- Validar data: Lambda corre al insertar en DynamoDB, consulta RDS para referential integrity
- Cuidado: cold start puede causar timeout de conexión. Usar RDS Proxy para connection pooling.

### RDS Proxy
Pooling de conexiones: entre Lambda/aplicación y RDS
- Lambda abre muchas conexiones → RDS timeout
- RDS Proxy reúsa conexiones → reduce latencia
- Costo: $0.015/ACU-hour (Approximately $10/mes)

## 7. Buenas Prácticas

### Diseño de Instancia
- Comenzar con db.t3.small/medium. Monitorear CPU/RAM.
- Si CPU constantemente >70%, escalar class.
- Si almacenamiento se agota, cambiar Allocated Storage (puede crecer on-line sin downtime).

### Backups y Disaster Recovery
- Habilitars automated backups (default 7 días)
- Crear snapshot mensual manual (para retención a largo plazo)
- Probar restauración desde snapshot regularmente (deuda técnica común: no saber si restore funciona)

### Network Segaurity
- DB Subnet Group: solo subnets privadas
- Security Group: permitir solo port 3306 desde application SG, no 0.0.0.0/0
- No exponer RDS endpoint públicamente. Usar private endpoint.

### Encryption
- Habilitadr encryption at rest (KMS)
- SSL/TLS para datos en tránsito (forzar con parameter group: require_secure_transport)
- Guardar password en Secrets Manager, no en config file o environment variable

### High Availability
- Habilitar Multi-AZ (2x costo pero critical para producción)
- Para read-heavy: crear read replicas en otras AZs
- Para HA: application connection pooling (no crear nueva conexión per request)

### Monitoring & Logging
- Habilitads Enhanced Monitoring
- Crear CloudWatch alarms: CPU >80%, freeable memory <1GB
- Habilitar slow query log (queries >1segundo aparecen en logs)
- Query Performance Insights: cuales queries gastan más recursos

### Costos
- Usar db.t3 (burstable) para dev/test
- Usar db.r5/m5 solo si workload es predecible (compra Reserved Instances para 30-50% descuento)
- Disabilitar backup para BD que no necesita (dev/test), save almacenamiento

## 8. Errores Comunes

### Error 1: BD pública accidentalmente
- Crear RDS con "publicly accessible: TRUE"
- Resultado: BD expuesta a Internet, bots atacan, datos robados
- **Prevención**: Marcar "publicly accessible: FALSE". Conectar solo a través de private network.

### Error 2: Security group demasiado permisivo
- SG que permite 3306 desde 0.0.0.0/0 (cualquiera)
- Combined con contraseña débil → breach
- **Prevención**: SG permite solo desde application SG. Usar strong passwords, rotate regularmente.

### Error 3: No dimensionar almacenamiento
- Crear BD con 50GB, sin autoscaling
- 6 meses después: storage full, aplicación erro
- **Prevención**: Habilitar autoscaling, monitor disk usage

### Error 4: Olvidar backup
- No configurar automated backups (default deshabilitado en algunas engines)
- BD corrupta, necesitas restore pero no hay backup
- **Prevención**: Automatizar backups día 1. Verificar retention policy.

### Error 5: Cambiar engine version sin testing
- Cambiar MySQL 5.7 a 8.0 directamente
- SQL incompatible break aplicación
- **Prevención**: Crear snapshot de BD actual, restaurar en env new, test aplicación con engine nuevo, verificar compatibilidad

## 9. Ejemplo Arquitectónico

### Arquitectura de E-Commerce Escalable

```
Internet → Route 53 (DNS)
    ↓
   ALB (Application Load Balancer)
    ↓
   [VPC: 10.0.0.0/16]
    
Subnet Pública (10.0.1.0/24):
  - ALB (permite 80, 443 desde 0.0.0.0/0)

Subnet Privada - Application (10.0.2.0/24):
  - EC2 x 4 (Node.js e-commerce app) en Auto Scaling Group
  - Conecta a RDS, Redis, S3
  - Security Group: permite 3000 desde ALB

Subnet Privada - Database (10.0.3.0/24):
  - RDS PostgreSQL db.r5.large (Multi-AZ)
  - Primary: 10.0.3.10
  - Standby: 10.0.3.11 (différent AZ, latent replica)
  - Security Group: permite 5432 solo desde application SG
  - Automated backups: 35 days
  - Encryption: SSE-KMS
  
Subnet Privada - Cache (10.0.4.0/24):
  - ElastiCache Redis (6-node cluster)
  - Cachea producto frecuentes, sesiones usuario

Monitoreo:
  - CloudWatch alarms:
    - Si RDS CPU >80%: scale aplicación
    - Si RDS connections >800: alert (connection pooling issue)
    - Si freeable memory <1GB: scale instancia
  - RDS Enhanced Monitoring: track I/O contention
  
Disaster Recovery:
  - Read replica en eu-west-1 (para regional failover)
  - Snapshot mensual → S3 archivado
  - RTO: <1 minuto (Multi-AZ failover)
  - RPO: 0 (síncrono replication)

Costos:
  - RDS db.r5.large Multi-AZ: 2 instancias x $1.152/horas x 730 = $1,686/mes
  - Storage: 500 GB SSD x $0.23/GB = $115/mes
  - Backup storage: $50/mes
  - Read replica (eu): +$800/mes
  - Total: ~$2,651/mes
```

### RDS Configuration (YAML/IaC)

```yaml
DBInstance:
  DBInstanceIdentifier: ecommerce-prod-db
  DBInstanceClass: db.r5.large
  Engine: postgres
  EngineVersion: "14.6"
  AllocatedStorage: 500  # GB
  StorageType: gp3
  MaxAllocatedStorage: 1000  # Autoscaling limit
  
  # High Availability
  MultiAZ: true
  
  # Network
  DBSubnetGroupName: private-db-group
  VpcSecurityGroupIds:
    - sg-db-rds-prod
  PubliclyAccessible: false
  
  # Encryption
  StorageEncrypted: true
  KmsKeyId: arn:aws:kms:us-east-1:123456789:key/xxxxx
  
  # Backup
  BackupRetentionPeriod: 35
  BackupWindow: "02:00-03:00"
  
  # Maintenance
  MaintenanceWindow: "sun:03:00-sun:04:00"
  AutoMinorVersionUpgrade: true
  
  # Monitoring
  EnableCloudwatchLogsExports:
    - postgresql
  EnableIAMDatabaseAuthentication: true
  
  # Performance Insights
  EnablePerformanceInsights: true
```

---

**Siguiente**: [Proyecto 1 Integration](06-proyecto-1-integration.md)  
**Anterior**: [04-S3 Basics](04-s3-basico.md)
