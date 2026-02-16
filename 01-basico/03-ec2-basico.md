# EC2 (Elastic Compute Cloud) - Servidores en la Nube

## 1. ¿Qué es?

EC2 es el servicio de computación en la nube de AWS que proporciona instancias virtuales (servidores) escalables bajo demanda. Una instancia EC2 es una máquina virtual configurable que corre en los servidores físicos de AWS. Puedes elegir CPU, memoria, almacenamiento, sistema operativo y software preinstalado. Pagas solo por las horas que la instancia está en ejecución.

Técnicamente, EC2 utiliza tecnología de virtualización (Xen Hypervisor) para crear máquinas virtuales aisladas que comparten el hardware físico subyacente, pero con garantías de aislamiento y rendimiento.

## 2. Analogía Sencilla (Explicación de la Vida Cotidiana)

EC2 es como alquilar una computadora en lugar de comprarla. Con hardware tradicional, compras una computadora con especificaciones fijas y la tienes en tu casa. Con EC2:

- Eliges las especificaciones que necesitas (CPU, RAM, almacenamiento)
- Enciendes la máquina (la renta comienza)
- Usas la máquina
- Apagas la máquina cuando no la necesitas (dejas de pagar)
- Si necesitas una mejor máquina, no compras una nueva: solo pagas por una más potente

Si tienes un sitio web con tráfico variable, EC2 te permite tener 1 servidor en horarios bajos y 10 servidores durante el Black Friday, sin deuda de hardware.

## 3. ¿Qué Problema Resuelve?

Antes de la nube, los problemas eran:

- **Inversión inicial alta**: Comprar servidores es costoso y lento
- **Rigidez**: Si compras un servidor con 8 CPU y necesitas 4, desperdicias dinero durante 3 años
- **Mantenimiento**: Refrigeración, electricidad, reemplazo de disco duro roto
- **Escalado impredecible**: Black Friday sorpresa = sin capacidad
- **Data center limitado**: Si tu data center explota, pierdes todo

EC2 resuelve esto ofreciendo:

- **Escalado bajo demanda**: Compra potencia cuando la necesites
- **Flexibilidad de especificaciones**: Cambia de tipo de instancia fácilmente
- **Sin mantenimiento**: AWS maneja el hardware físico
- **Disponibilidad global**: Elige en qué región/AZ correr tu servidor
- **Integración con otros servicios**: Auto Scaling, Load Balancing, Monitoring automático

## 4. Componentes Principales

### Tipos de Instancia
Diferentes categorías optimizadas para diferentes cargas:

- **General Purpose (t3, m5)**: Balance de CPU, memoria, networking. Aplicaciones web, pequeñas BDs.
- **Compute Optimized (c5, c6i)**: Alto rendimiento CPU. Batch processing, científico, gaming.
- **Memory Optimized (r5, r6i)**: Mucha RAM. Bases de datos en memoria (Redis), big data analytics.
- **Storage Optimized (i3, d2)**: Alto IOPS. NoSQL databases, data warehousing.
- **GPU (p3, g4)**: Procesamiento gráfico. Machine Learning, rendering de video.

Cada tipo tiene subtamaños: t3.micro (1 vCPU, 1 GB RAM) a t3.2xlarge (8 vCPU, 32 GB RAM).

### AMI (Amazon Machine Image)
Es la plantilla que define qué sistema operativo e software preinstalado tiene la instancia. Cuando lanzas una instancia, especificas qué AMI usar:
- AMI pública: Ubuntu, Amazon Linux 2, Windows Server (gratis para el SO)
- AMI personalizada: Creas una imagen con tu aplicación preinstalada
- AMI del marketplace: Imágenes de terceros (a menudo con costo adicional)

### Instance Lifecycle
Una instancia pasa por estados:
- **pending**: Inicializando
- **running**: Ejecutándose (se cobra por tiempo)
- **stopping**: Apagándose
- **stopped**: Apagada (no se cobra por compute, pero sí por almacenamiento)
- **terminating**: Eliminándose
- **terminated**: Eliminada (no se puede recuperar)

Es crítico entender: **stopped ≠ terminated**. Una instancia stopped pueden reiniciarla después. Una terminada se pierde.

### EBS (Elastic Block Storage) - Volúmenes
El almacenamiento adjunto a tu instancia EC2:
- **Root volume**: Donde vive el sistema operativo
- **Additional volumes**: Almacenamiento extra que puedes adjuntar
- Tipos: gp2 (general, recomendado), io1 (alta IOPS), st1 (throughput alto), sc1 (arqueológico/barato)

### Key Pairs - Acceso SSH
Para conectarte a tu instancia Linux, necesitas una clave privada. AWS genera o importas un key pair. Sin la clave correcta, no puedes acceder a la instancia.

### Security Groups
Definen qué tráfico de red es permitido. Sin permitir puerto 22 (SSH) en el security group, no puedes conectarte, incluso si tienes la clave privada.

### Elastic IP (EIP)
Dirección IP pública estática. Normalmente, si terminas una instancia, su IP pública se libera. Con EIP, mantienes la misma IP incluso después de terminar/reiniciar.

## 5. Casos de Uso Comunes

### Hosting de Sitio Web
- t3.micro con Ubuntu, Nginx, aplicación Node.js
- EBS gp2 para almacenar código
- Security group permite HTTP (80) y HTTPS (443)
- Detrás de un ALB para múltiples instancias

### Servidor de Base de Datos
- r5.large (memory optimized) para PostgreSQL
- EBS io1 (alto IOPS) para rendimiento de BD
- Security group solo permite acceso desde aplicación (puerto 5432)
- Multi-AZ: 2 instancias, 1 como replica

### CI/CD Worker
- c5.xlarge (compute optimized) para compilaciones rápidas
- Instancia spot (descuento 70%, pero puede terminar)
- Corre Jenkins agent
- Almacenamiento en S3 para artefactos

### Machine Learning Training
- p3.8xlarge con GPU
- Corre PyTorch/TensorFlow
- EBS gp2 para datasets
- Termina la instancia cuando termina el entrenamiento

### Development/Testing
- t3.small para ambiente dev
- Apenas usada, muy económica
- Apagada fuera de horario laboral

## 6. Integración con Otros Servicios AWS

### VPC (Virtual Private Cloud)
EC2 SIEMPRE vive dentro de una VPC. No puedes lanzar una instancia sin especificar VPC y subnet. La subnet determina si la instancia es accesible desde Internet.

### IAM (Identity and Access Management)
Define permisos: quién puede lanzar instancias, terminar instancias, cambiar tamaño. Además, EC2 puede tener un IAM Role adjunto que le permite acceder otros servicios AWS (ej: leer desde S3, escribir logs a CloudWatch).

### Auto Scaling (ASG)
Lanza/termina instancias automáticamente basado en métricas. ASG necesita Launch Template (configuración de instancia) + Scaling Policies (cuándo escalar).

### Elastic Load Balancer (ELB)
Distribuye tráfico entre múltiples instancias EC2. El Load Balancer revisa la salud de las instancias: si una falla, deja de enviar tráfico a ella. Requiere que instancias estén en el mismo security group del ALB.

### CloudWatch (Monitoreo)
Recibe métricas de EC2: CPU, networking, disk. Puedes crear alarms: si CPU > 80% durante 5 minutos, dispara SNS o dispara Auto Scaling.

### S3 (Simple Storage Service)
EC2 puede leer/escribir a S3 usando IAM Role, sin necesidad de almacenar credenciales en la instancia. Típicamente: código en S3 → descarga en EC2 → ejecuta.

### RDS (Relational Database Service)
EC2 conecta a RDS especificando hostname, puerto, credenciales. RDS tiene security group que solo permite tráfico desde security group de EC2.

### Route 53 (DNS)
Puedes apuntar tu dominio a la IP de una instancia EC2, pero típicamente apuntas a un ALB que tiene instancias EC2 detrás.

### CloudTrail (Auditoría)
Registra todas las acciones sobre EC2: quién lanzó qué instancia, en qué momento, desde qué IP.

## 7. Buenas Prácticas

### Selección de Tipo de Instancia
- Empezar con t3.small/medium. Si está sobre-utilizada (CPU siempre > 80%), cambiar a familia diferente.
- Para cargas impredecibles, usar burstable instances (t3) que ofrecen CPU bajo demanda.
- No usar instancias "huge" si no tienes métrica que lo justifique (diferencia de costo es exponencial).

### Gestión del Ciclo de Vida
- Apagar instancias de desarrollo/testing fuera de horario (usar EventBridge + Lambda para automatizar)
- Terminar, no dejar stopped: stopped still charges for storage, pero use case es raro
- Usar tags para identificar instancias por propósito (Environment=prod, Team=backend, Cost-Center=12345)

### Seguridad
- Cambiar security group default: por defecto permite todo. Ser restrictivo: solo puertos necesarios
- Usar Systems Manager Session Manager en lugar de SSH directo (más seguro, auditado, sin claves en GitHub)
- Actualizar el SO regularmente: `apt update && apt upgrade` en Linux
- Usar key pairs de 4096-bit RSA, no guardar claves privadas en repositorios

### Almacenamiento
- No guardar datos importantes en el root volume. Si la instancia se termina, los datos se pierden.
- Usar EBS volúmenes separados para datos persistentes, snapshot regularmente
- Para datos temporales (logs), usar ephemeral storage (gratuito, pertenece a instancia, se pierde si termina)

### Monitoreo
- Adjuntar CloudWatch agent a las instancias para ver memoria y disk (CPU es default)
- Crear alarms: CPU > 80%, Disk > 90%, Network errs > 100 packets/min
- Usar CloudWatch Logs para aplicación logs, no solo syslog

### Costos
- Usar Spot Instances para cargas tolerantes a interrupciones (descuento 70%)
- Usar Reserved Instances si workload es predecible (descuento 30-50% por 1-3 años)
- Terminar instancias no usadas (deuda técnica común)
- Usar VPC endpoints para acceso a S3/DynamoDB sin NAT Gateway

## 8. Errores Comunes

### Error 1: Guardar datos en root volume
Crear instancia, instalar aplicación, almacenar datos en /home. Luego terminar instancia para "pasar a versión nueva". Resultado: todos los datos se pierden.

**Prevención**: Arquitecturar aplicación: datos en EBS volume separado (o mejor, en RDS/S3), código en repositorio (no en instancia).

### Error 2: No actualizar security group
Lanzar instancia con security group que permite SSH (22) desde 0.0.0.0/0. Bots automáticamente atacan puerto 22. En una hora, instancia está comprometida.

**Prevención**: SSH solo desde tu IP o Bastion host. Usar Systems Manager Session Manager.

### Error 3: Instancia stopped olvidada
"Voy a pausar instancia para no pagar." Olvida que existe. Meses después: discover que tiene EBS volume cuyo costo se acumula. Sorpresa de factura.

**Prevención**: Terminar, no pausar. Si la necesitas después, usar AMI baked.

### Error 4: Cambio de tipo de instancia sin parar
Ir a https://console.aws.amazon.com, cambiar tipo de instancia (t3.micro a m5.large) sin parar primero. Resultado: la instancia se para durante cambio, luego se inicia. Posible data corruption.

**Prevención**: Siempre parar, cambiar tipo, iniciar. O mejor: usar Auto Scaling, dejar que reemplace instancia.

### Error 5: No compartir AMI
Crear instancia, instalar software "perfecto", dejar que se ejecute. Cuando la instancia falla, recrear manualmente (error-prone). 

**Prevención**: Después de configurar instancia correctamente, crear AMI personalizado. Usar ese AMI para futuras instancias.

## 9. Ejemplo Arquitectónico

### Arquitectura de Aplicación Web Escalable

```
Internet → Route 53 (DNS)
     ↓
   ALB (Application Load Balancer)
     ↓
   [VPC: 10.0.0.0/16]
     ↓
   Subnet Pública (10.0.1.0/24):
     - ALB security group: permite 80, 443 desde 0.0.0.0/0
     
   Subnet Privada (10.0.2.0/24):
     - EC2 Instance 1: t3.medium, Ubuntu, Nginx, app Node.js
     - EC2 Instance 2: t3.medium (replica para HA)
     - Security group: permite TCP 3000 desde ALB
     
   Auto Scaling Group:
     - Launch Template: AMI personalizado con app
     - Min: 2, Desired: 4, Max: 10 instancias
     - CloudWatch Metric: CPU
     - Scaling Policies: Si avg CPU > 70%, agregar 2 instancias
     
   Subnet Privada - Data (10.0.3.0/24):
     - RDS PostgreSQL (Multi-AZ)
     - Security group: permite 5432 desde app security group
     
   Monitoring:
     - CloudWatch alarms: si cualquier instancia CPU > 90%
     - SNS: notifica al equipo
     - CloudWatch Logs: app logs se envían a CloudWatch
```

### Desglose de Costos (Producción, 24/7)

- ALB: $15/mes (fijo) + $0.006/GB processed
- EC2 t3.medium x 4 (average): 4 instancias × $0.0416/hora × 730 horas/mes = $121/mes
- EBS gp2 x 4 (30GB cada): 120GB × $0.10/GB/mes = $12/mes
- RDS db.t3.small Multi-AZ: $0.35/hora × 730 = $255/mes
- NAT Gateway (si aplicación sale a Internet): $0.045/hora × 730 = $32/mes + datos

**Total aproximado: $435/mes** (sin exceso de uso de NAT)

Si usas Spot Instances + Reserved (1 year): baja a $250-300/mes

---

**Siguiente**: [04-S3 Basics](04-s3-basico.md)  
**Anterior**: [02-Redes Básicas](02-redes-basicas.md)
