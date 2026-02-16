# S3 (Simple Storage Service) - Almacenamiento de Objetos

## 1. ¿Qué es?

S3 es un servicio de almacenamiento de objetos masivamente escalable que guarda datos en la forma de objetos organizados en contenedores llamados buckets. Un objeto es cualquier archivo: imagen, video, documento, JSON, código. Cada objeto se almacena con un nombre único (key) dentro de un bucket. S3 está diseñado para ser altamente disponible, duradero y accesible desde cualquier lugar de Internet.

Técnicamente, S3 proporciona redundancia automática (múltiples copias en múltiples data centers dentro de una región), versionado opcional, y APIs RESTful para acceso. El almacenamiento es prácticamente ilimitado en escala.

## 2. Analogía Sencilla (Explicación de la Vida Cotidiana)

S3 es como un almacén (warehouse) donde envías cajas. El almacén tiene reglas:

- Tienes uno o múltiples almacenes (buckets) bajo tu nombre
- Cada caja tiene una etiqueta (nombre de archivo) y contenidos
- Las cajas están organizadas pero accesibles desde cualquier lugar (direcciones web públicas)
- El almacén guarda múltiples copias de cada caja en locaciones diferentes (redundancia)
- Pides recuperar una caja específica (GET), envías cajas nuevas (PUT), o borras cajas (DELETE)
- No importa si envías 1 KB o 1 TB: pagas solo por lo que usas

A diferencia de un servidor (EC2) que corre aplicaciones, S3 es **almacenamiento puro**: no ejecuta código, solo guarda y recupera datos.

## 3. ¿Qué Problema Resuelve?

Antes de S3, los problemas de almacenamiento eran:

- **Escalabilidad limitada**: Discos duros tienen límite. 1 archivo de 1TB no cabe en muchos servidores
- **Redundancia manual**: Hacer backups era trabajo manual propenso a errores
- **Costo de infraestructura**: Servidores de almacenamiento eran caros
- **Acceso centralizado**: E-commerce con múltiples sitios necesitaba storage central
- **Serving de media**: Videos/imágenes pesadas degradaban el rendimiento del servidor

S3 resuelve esto ofreciendo:

- **Escalabilidad infinita**: Sube archivos de 1 byte o 5 TB, S3 escala automáticamente
- **Redundancia automática**: Múltiples copias en múltiples AZs (99.999999999% durabilidad)
- **Acceso global**: Sirve contenido rápidamente desde CloudFront ubicado cerca del usuario
- **Integración con servicios AWS**: EC2, Lambda, RDS pueden leer/escribir fácilmente
- **Versionado y retención**: Recupera versiones antiguas, cumple regulaciones

## 4. Componentes Principales

### Buckets
Contenedores de nivel superior. Cada bucket tiene:
- Nombre globalmente único (s3-bucket-names son únicos en TODO AWS)
- Región donde almacenan datos (us-east-1, eu-west-1, etc.)
- Configuración de permisos (ACLs, Bucket Policy)

Nomenclatura: nombres en minúsculas, sin espacios, sin caracteres especiales (excepto punto y guion).

### Objects (Objetos)
Los archivos reales. Cada objeto tiene:
- **Key**: Nombre/ruta única dentro del bucket (ej: "photos/vacation/beach.jpg")
- **Value**: Contenido del archivo (datos binarios)
- **Metadata**: Tags como content-type, encryption, storage class
- **Version ID**: Si bucket tiene versionado habilitado

Tamaño mínimo: 0 bytes. Tamaño máximo: 5 TB.

### Storage Classes (Clases de Almacenamiento)
Diferentes opciones de almacenamiento con trade-off de costo/acceso:

- **Standard**: Acceso frecuente, alta disponibilidad, $0.023/GB/mes (región US)
- **Standard-IA** (Infrequent Access): Acceso ocasional, costo menor pero fee por retrieval, $0.0125/GB/mes
- **Glacier**: Acceso raro, archivado, recuperación en horas, $0.004/GB/mes
- **Glacier Deep Archive**: Acceso muy raro, recuperación en 12+ horas, $0.00099/GB/mes
- **Intelligent-Tiering**: AWS automáticamente mueve entre Standard/IA basado en acceso, $0.0125/GB/mes

Usar Intelligent-Tiering para workloads de acceso impredecible.

### Permissions (Permisos)
Dos mecanismos de control de acceso:

- **Bucket Policy**: JSON que define acceso a nivel de bucket (todos objetos en "photos/" son públicos)
- **ACLs**: Access Control Lists deprecated, pero aún existe (definen si bucket/objeto es público/privado)
- **IAM**: Define qué usuarios/roles pueden hacer qué acciones
- **Object ACL**: Cada objeto puede tener ACL (público o privado)

Default: buckets y objetos son **privados**. Debes explícitamente hacer público.

### Versionado
Mantiene histórico de cambios. Si habilitas versionado:
- Cada PUT crea una versión nueva, sin borrar anterior
- Puedes recuperar versiones antiguas
- Usa más almacenamiento (guardar múltiples versiones)
- Necesario para compliance (no borrar datos históricos)

### Ciclo de Vida
Transiciones automáticas entre clases de almacenamiento o expiración:
- Documentos: Standard → Standard-IA después de 30 días → Glacier después de 90 días
- Logs: Expire después de 1 año (borrar automáticamente)

Reduce costos significativamente para data histórica.

### Encryción
- **SSE-S3**: S3 maneja las claves (default)
- **SSE-KMS**: Usa KMS para gestionar claves (más control, permite auditoría)
- **SSE-C**: Tú proporcionas la clave
- **Client-side**: Encriptar antes de enviar a S3

## 5. Casos de Uso Comunes

### Static Website Hosting
Subir HTML, CSS, JS a S3. Configurar bucket como website. Uso:
- Blogs, portafolios, documentación técnica
- Costo: solo almacenamiento (~$0.50/mes para sitio pequeño)
- Con CloudFront: contenido cacheado globalmente, página carga en <100ms desde cualquier país

### Backup y Disaster Recovery
EC2 o servidor on-prem backupea a S3 nightly:
- Uso: Glacier para backups viejos (reducir costo)
- Lifecycle: Standard 30 días, Glacier 90 días, Deep Archive 1 año
- Recuperación: Glacier standard retrieval 3-5 horas, expedited 1-5 min

### Big Data Analytics
Data Lake: millones de eventos JSON acumulados en S3:
- Athena: SQL queries directamente sobre S3 (sin cargar en BD)
- Glue: ETL jobs transforma datos en S3
- Redshift Spectrum: queries distribuidas entre Redshift + S3

### Distribución de Contenido Media
CloudFront origin S3:
- Videos/imágenes: almacenadas en S3 us-east-1
- CloudFront sirve desde 200+ edge locations
- Usuarios en Tokyo bajan video desde edge location en Tokyo (baja latencia)

### Log Storage
Aplicación EC2 escribe logs a S3:
- CloudTrail logs: auditoría de acciones AWS
- ALB access logs: quién accedió qué
- CloudFront logs: caché hits/misses
- Búsqueda/análisis con Athena

### Sharing de archivos
Link compartible con hash imposible de adivinar:
- Pre-signed URLs: URL que expira en 15 minutos, acceso temporal
- Caso: usuario descarga reporte generado

## 6. Integración con Otros Servicios AWS

### IAM (Identity and Access Management)
Define quién puede acceder qué bucket. Ejemplo:
- Grupo "backend-team" puede leer/escribir bucket "app-logs"
- Usuario "jenkins" solo puede escribir bucket "build-artifacts"
- Policy especifica: Principal (quién), Action (qué), Resource (dónde)

### EC2 (Elastic Compute Cloud)
EC2 lee/escribe S3 usando IAM Role (sin almacenar credenciales):
- Aplicación descarga config de S3 al iniciar
- Implementa backup de BD a S3 diariamente
- Sube logs de aplicación a S3

### Lambda (Funciones Serverless)
Lambda trigger: archivo sube a S3 → Lambda procesa automáticamente:
- Redimensiona imagen cuando se sube (upload 4MB JPG → S3 event → Lambda → redimensiona → guarda thumb)
- Procesa CSV: sube CSV → Lambda → inserta en BD → responde
- Valida datos: sube JSON → Lambda → valida schema → acepta o rechaza

### CloudFront (CDN)
S3 como origin para CloudFront:
- Contenido cacheado en 200+ edge locations
- Primero delivery de content al usuario desde edge más cercano
- Fallback a origin S3 si cache miss
- DDoS protection automático

### CloudWatch (Monitoreo)
Métricas de S3:
- NumberOfObjects: cuántos objetos en bucket
- BucketSizeBytes: tamaño total
- Alarms: si bucket crece demasiado rápido, alertar

### RDS (Relational Database Service)
BD exporta snapshots a S3:
- Recuperación rápida si datos corruptos
- Backup retenido indefinidamente vs RDS retiene 35 días

### Athena (SQL sobre S3)
Queries SQL sobre datos en S3 sin mover datos:
- Data warehouse: millones de logs JSON en S3
- SELECT avg(response_time) FROM logs WHERE date='2026-02-16'
- Pago: $5 por TB escaneado

### Glue (ETL)
Transformar datos en S3:
- Source: CSV en S3
- Job: Glue procesa (limpia, agrupa, transforma)
- Sink: Resultado guardado en otro S3 o Redshift

### EventBridge (Eventos)
Trigger rules basadas en eventos S3:
- Archivo sube a S3 → EventBridge rule → dispara SNS (notificación) + SQS (cola para procesar)

## 7. Buenas Prácticas

### Naming y Organización
- Usar nombres descriptivos, estructurados (bucket-environment-purpose): app-logs-prod
- Dentro del bucket, usar jerarquía (prefixes actúan como carpetas):
  ```
  app-logs/
    ├─ 2026/
    │  ├─ 02/
    │  │  ├─ 16/
    │  │  │  ├─ application-error.log
    │  │  │  └─ access.log
  ```
- Esto facilita lifecycle policies (borrar logs de >1 año)

### Seguridad
- **Default privado**: Todos los buckets comienzan privados. Solo hacer público si es necesario.
- **Block Public Access settings**: AWS permite apagar acceso público a nivel de account (defensa en profundidad)
- **Encryption**: Habilitar "SSE-KMS" para datos sensibles, añade auditoría
- **Bucket Policy review**: Revisar policy regularmente, eliminar accesos innecesarios (deuda técnica)
- **No guardar credenciales en S3**: Usar IAM Roles para EC2/Lambda

### Costos
- **Usar Storage Classes**: Logs en Standard 30 días, luego Glacier
- **Enable Intelligent-Tiering**: Automáticamente optimiza acceso (sin manual management)
- **Delete old versions**: Si versionado, borrar versiones viejas que no necesites (especialmente con grandes archivos)
- **Lifecycle policies**: Automatizar transiciones, no costo sorpresa

### Durabilidad y Disponibilidad
- S3 standard: 99.99% SLA (4 9s). Mejor que ANY hardware físico
- Replicación cross-región: para cargas críticas, habilitar Cross-Region Replication (CRR)
- Versionado + MFA Delete Protection: para datos críticos, requerir MFA para deletar

### Monitoreo
- Habilitar CloudWatch metrics (costo bajo)
- Alarms si bucket crece anormalmente
- CloudTrail: audita quién accedió qué objeto (compliance, seguridad)

## 8. Errores Comunes

### Error 1: Hacer bucket público accidentalmente
"Voy a permitir acceso público solo a index.html". Escribir policy:
```json
{
  "Effect": "Allow",
  "Principal": "*",
  "Action": "s3:*",
  "Resource": "*"
}
```
Resultado: TODO el bucket es público, incluyendo archivos privados. Breach de datos.

**Prevención**: Ser muy específico con resources. Usar S3 Block Public Access settings para evitar esto (requiere explícito override).

### Error 2: Olvidar de usar lifecycle policies
Guardar logs en S3 Standard "para siempre". 3 años después: bucket tiene millones de files de hace 2 años. Costo: $500/mes.

**Prevención**: Configurar lifecycle al crear bucket. Standard 30 días → Glacier 90 días → Delete 1 año.

### Error 3: No versionear datos críticos
Actualizar application config en S3. Por error, sobrescribir con versión mala. Aplicaciones comienzan a fallar. No hay forma de recuperar versión anterior.

**Prevención**: Versionado habilitado para cualquier data que pueda ser sobrescrita (config, templates, bundles).

### Error 4: Permisos de bucket vs objeto
Bucket es privado pero objeto tiene ACL público. Usuario intenta acceder: confusión de por qué funciona a veces.

**Prevención**: Usar solo Bucket Policies, desactivar Object ACLs (AWS permite desactivar ACLs entirely).

### Error 5: Confundir "acces denied" causas
application EC2 intenta leer de S3 bucket. Access denied. Es porque:
- IAM Role no tiene s3:GetObject permiso? (probably)
- Security group bloquea salida? (no, S3 es HTTPS)
- KMS key no da acceso? (sí, si SSE-KMS)
- Bucket policy tiene Deny? (check)

**Prevención**: Usar policy simulator, CloudTrail para debuggear.

## 9. Ejemplo Arquitectónico

### Arquitectura de Data Lake con S3

```
Datos ingieren desde múltiples fuentes:

Puente 1: Aplicación EC2
  ├─ Logs diarios → S3 bucket "raw-logs" (10GB/día)
  ├─ Compressed con Gzip
  └─ Prefijo: logs/YYYY/MM/DD/

Puente 2: EventBridge
  ├─ Eventos API → S3 "events" (JSON Lines)
  └─ Prefijo: events/YYYY/MM/DD/HH/

Puente 3: DMS (Database Migration)
  ├─ Exporta desde RDS → S3 "database-exports" (Parquet)
  └─ Nightly full export

S3 Buckets:
├─ raw-logs (250 GB, lifecycle: Glacier after 90 days, Delete after 1 year)
├─ events (500 GB, Intelligent-Tiering)
└─ database-exports (100 GB, Standard, backup importante)

Análisis:
├─ Athena: SELECT * FROM logs WHERE response_time > 5000
├─ Glue: ETL job (aggregate logs, transform a Parquet format)
├─ Redshift Spectrum: query distribuido entre Redshift + S3

Monitoreo:
├─ CloudWatch: tamaño del bucket, número de objetos
├─ CloudTrail: quién accedió qué objeto
└─ Alarms: si ingestion rate baja (posible error en fuentes)

Costos (estimado):
├─ Storage: 250GB standard + 500GB intelligent = $12 + $6 = $18/mes
├─ Glacier (data >90d): $2/mes
├─ Athena (100TB queries/month): $500/mes
└─ Total: ~$520/mes
```

### S3 Bucket Configuration (IaC)

```yaml
Bucket:
  Name: company-data-lake-prod
  Region: us-east-1
  Versioning: Enabled
  Encryption: SSE-KMS (managed key)
  BlockPublicAccess:
    BlockPublicAcls: true
    BlockPublicPolicy: true
    IgnorePublicAcls: true
    RestrictPublicBuckets: true
  
LifecyclePolicy:
  - Prefix: raw-logs/
    TransitionToGlacier: 90 days
    Expiration: 365 days
  
  - Prefix: events/
    TransitionToIntelligentTiering: 30 days
```

---

**Siguiente**: [05-RDS Basics](05-rds-basico.md)  
**Anterior**: [03-EC2 Basics](03-ec2-basico.md)
