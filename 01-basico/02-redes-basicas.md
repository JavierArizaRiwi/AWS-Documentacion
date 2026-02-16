# VPC (Virtual Private Cloud) - Redes Básicas en AWS

## 1. ¿Qué es?

Una VPC es un ambiente de red aislado e independiente que creas en AWS. Es tu propio espacio virtual privado dentro del cloud público de Amazon donde puedes lanzar recursos (EC2, RDS, etc.) con control total sobre la configuración de red. Cada VPC tiene su propio rango de direcciones IP, subredes, tablas de enrutamiento y gateways.

En términos técnicos, una VPC es una representación virtual de una red física tradicional. Proporciona aislamiento en la capa de red, control de flujo de tráfico, y la capacidad de conectar múltiples segmentos de red dentro de AWS o hacia tu infraestructura on-premises.

## 2. Analogía Sencilla (Explicación de la Vida Cotidiana)

Imagina un condominio privado en la ciudad. La manzana completa es AWS (la ciudad entera), y tu VPC es tu condominio. Dentro del condominio tienes:

- **Puertas con seguridad** (Security Groups): Controlan quién entra a cada apartamento
- **Muros de seguridad** (Network ACLs): Protegen todo el perímetro del condominio
- **Avenidas internas** (Subnets): Diferentes secciones dentro del condominio
- **Portería principal** (Internet Gateway): La entrada/salida del condominio hacia la calle (Internet)
- **Portería lateral privada** (NAT Gateway): Permite que los residentes salgan al mundo sin que el mundo los vea

Otros residentes no pueden entrar a tu condominio sin permiso, y tú controlas exactamente qué entra y qué sale.

## 3. ¿Qué Problema Resuelve?

En AWS, si no tuvieras VPC, todos los usuarios compartirían la misma red, lo cual sería un caos de seguridad. La VPC resuelve varios problemas críticos:

- **Aislamiento de seguridad**: Tu infraestructura está completamente separada de otros clientes de AWS
- **Control de acceso granular**: Defines exactamente qué tráfico es permitido
- **Cumplimiento normativo**: Muchas regulaciones (HIPAA, PCI-DSS) requieren aislamiento de red
- **Separación de ambientes**: Puedes tener VPCs separadas para desarrollo, testing y producción
- **Conectividad híbrida**: Puedes conectar tu VPC a tu office on-premises de forma segura

Antes de VPC, los clientes de cloud público tenían exposición a vulnerabilidades de "vecinos". AWS resolvió esto con VPC.

## 4. Componentes Principales

### Subnets
Son divisiones dentro de una VPC asociadas a una Availability Zone específica. Pueden ser:
- **Públicas**: Tienen acceso directo a Internet (con Internet Gateway)
- **Privadas**: No tienen acceso directo a Internet (requieren NAT Gateway para salir)

### Internet Gateway (IGW)
Es el puerto de entrada/salida de tu VPC a Internet. Sin él, tu VPC es completamente aislada. Es redundante por defecto en AWS (no tiene punto único de fallo).

### NAT Gateway / NAT Instance
Permite que instancias en subnets privadas inicien conexiones salientes a Internet sin recibir conexiones entrantes. Es útil para servidores que necesitan descargar actualizaciones pero no deben exponer puertos.

### Route Tables
Son conjuntos de reglas (rutas) que determinan a dónde se envía el tráfico de red según la dirección IP de destino. Cada subnet está asociada a una tabla de rutas.

### Security Groups
Son firewalls stateful a nivel de instancia. Definen inbound rules (qué puede entrar) y outbound rules (qué puede salir). Son stateful: si permites una conexión entrante, la salida correspondiente se permite automáticamente.

### Network ACLs (NACLs)
Son firewalls stateless a nivel de subnet. Son una capa adicional de seguridad por encima de Security Groups. Procesan reglas en orden numérico.

### VPC Endpoints
Permiten que tus instancias privadas se comuniquen con servicios AWS públicos (S3, DynamoDB, etc.) sin pasar por Internet.

## 5. Casos de Uso Comunes

### Arquitectura de Aplicación Web Tradicional
- Subnet pública con ALB (Application Load Balancer)
- Subnet privada con servidores de aplicación
- Subnet privada con base de datos
- NAT Gateway en la subnet pública para que la capa de aplicación pueda descargar dependencias

### Entorno de Desarrollo Aislado
- VPC separada para desarrollo
- Sin Internet Gateway (completamente privada)
- Conectable a través de VPN desde la oficina
- Bajo costo, para experimentación

### Multi-Región con Replicación
- VPC en us-east-1 (principal)
- VPC en eu-west-1 (replica para DR)
- VPC Peering o Transit Gateway para comunicación entre regiones

### Compliance y PCI-DSS
- Subnet pública para DMZ
- Subnet privada para procesamiento de datos sensibles
- Sin acceso entre ellas sin pasar por firewall (WAF)

## 6. Integración con Otros Servicios AWS

### IAM (Identity and Access Management)
Define QUIÉN puede crear, modificar o eliminar VPCs, subnets, security groups. Un usuario puede tener permisos para crear VPCs pero no para cambiar route tables. VPC + IAM = políticas de seguridad multinivel.

### EC2 (Elastic Compute Cloud)
Las instancias EC2 SIEMPRE se lanzan dentro de una VPC. La VPC determina si la instancia es accesible desde Internet. Sin VPC configurada correctamente, no puedes acceder a tus servidores.

### RDS (Relational Database Service)
Las bases de datos se lanzan en subnets privadas dentro de una VPC. Especificas un DB Subnet Group que define en qué subnets vivirá tu BD. Security Group de RDS solo puede recibir tráfico de EC2s en la misma VPC.

### S3 (Simple Storage Service)
Mientras que S3 es un servicio global, puedes usar VPC Endpoints para acceso privado a S3 desde subnets privadas sin exposición a Internet. Reduce latencia y aumenta seguridad.

### CloudWatch (Monitoreo)
VPC Flow Logs envían información a CloudWatch sobre el tráfico que entra/sale de tu VPC. Útil para debugging de conectividad y seguridad.

### ELB (Elastic Load Balancer)
Los load balancers se distribuyen entre múltiples subnets dentro de tu VPC. El ALB requiere al menos 2 subnets en diferentes AZs para alta disponibilidad.

### Auto Scaling
Cuando Auto Scaling lanza nuevas instancias, las coloca automáticamente en subnets que especificaste. VPC debe estar correctamente configurada para que el escalado sea efectivo.

## 7. Buenas Prácticas

### Diseño de Subnets
- Siempre usar al menos 2 AZs para alta disponibilidad. Distribuir tus subnets entre AZs diferentes.
- Usar CIDR blocks que no se sobrepongan entre ambientes. Ejemplo: VPC Dev usa 10.0.0.0/16, VPC Prod usa 10.1.0.0/16.
- Dimensionar subredes con suficientes IPs. Una subnet /27 solo tiene 32 IPs. Para producción, usar /24 o más amplio.

### Seguridad en Capas
- Usar Security Groups para controlar acceso a nivel de instancia
- Usar NACLs como capa defensiva adicional a nivel de subnet
- Cambiar las reglas por defecto: por defecto permiten todo. Ser explícito: permitir solo lo necesario.

### Acceso a Internet Seguro
- Para recursos que necesitan Internet saliente, usar NAT Gateway (no NAT Instance) en subnet pública
- NAT Gateway es redundante por AZ. Para HA, lanzar NAT Gateway en cada AZ.
- El costo de NAT Gateway es mayor ($0.045/hora), considerarlo en el budget.

### Naming y Tagging
- Nombrar subnets con referencia a su función: "web-subnet-1a", "db-subnet-1b"
- Usar tags para identificar propósito, ambiente, equipo responsable

### Connectivity
- Documentar exactamente qué puertos y protocolos son necesarios entre capas
- Usar security groups específicos por función, no un security group "allow all"
- Hacer cleanup de security group rules sin usar (técnica deuda técnica)

## 8. Errores Comunes

### Error 1: VPC sin Internet Gateway
Crear una VPC pública pero no adjuntar IGW. Resultado: instancias con IPs públicas pero sin acceso a Internet. La instancia no puede recibir solicitudes desde Internet.

**Prevención**: Checkear que cada VPC que intente acceder a Internet tenga un IGW adjunto.

### Error 2: Subnet completamente pública cuando debería ser privada
Accidentalmente asignar direcciones IP públicas automáticas a instancias en subnets que deberían ser privadas. Resultado: servidores sensibles expuestos a Internet.

**Prevención**: Desactivar "Auto-assign IPv4" en subnets privadas.

### Error 3: Security Group demasiado permisivo
Crear un security group que permita "0.0.0.0/0" (toda Internet) en puertos como SSH (22) o RDP (3389). Esto se llama "administrative access to the world".

**Prevención**: Usar security groups restrictivos. Si necesitas SSH, solo desde tu IP actual o desde un bastion host.

### Error 4: No tener NAT Gateway para salida
Instancia privada que necesita descargar software pero no hay NAT Gateway. Resultado: falla silenciosa de deployments.

**Prevención**: Planificar NAT Gateway desde el inicio si tienes subnets privadas que necesiten Internet saliente.

### Error 5: Olvidar de actualizar route tables
Crear una subnet nueva pero no actualizarse en la route table correspondiente. Resultado: subnet aparentemente desconectada.

**Prevención**: Cada subnet DEBE estar asociada a una route table (por defecto, la VPC tiene una).

## 9. Ejemplo Arquitectónico

### Arquitectura Web de Tres Capas (Típica en Producción)

```
VPC: 10.0.0.0/16

Availability Zone 1a:
  Subnet Pública (10.0.1.0/24):
    - ALB (Application Load Balancer)
    - Dirección secundaria para Internet Gateway
  
  Subnet Privada - Aplicación (10.0.2.0/24):
    - 2-3 Instancias EC2 (aplicación web)
    - Security Group: Solo tráfico del ALB
  
  Subnet Privada - Base de Datos (10.0.3.0/24):
    - RDS PostgreSQL
    - Security Group: Solo SQL (5432) desde subnet de aplicación

Availability Zone 1b:
  [Identical setup para HA]

Internet Gateway (IGW):
  - Conecta subnet pública a Internet
  - ALB recibe tráfico de usuarios
  
NAT Gateway:
  - Ubicado en subnet pública
  - Permite que EC2 en subnets privadas descargue updates
  - Salida a Internet sin exposición de puertos
```

### Flujo de Tráfico

1. Usuario en Internet envía request a www.miapp.com (DNS resuelve a IP del ALB)
2. Request llega a ALB en subnet pública
3. ALB rutea request a EC2 en subnet privada (según route table)
4. EC2 procesa request, conecta a RDS
5. RDS devuelve datos solo a EC2 (security group restringe)
6. EC2 envía respuesta al ALB
7. ALB devuelve respuesta al usuario
8. Si EC2 necesita actualización de software, conecta a NAT Gateway
9. NAT Gateway se comunica con Internet, asegurando que EC2 nunca recibe conexiones entrantes

### Costos Estimados

- VPC: Gratis
- Subnet: Gratis
- Route Table: Gratis
- Security Group: Gratis
- Internet Gateway: Gratis
- NAT Gateway: $0.045/hora (~$32/mes)
- VPC Endpoints: $0.01/hora (~$7/mes)

El costo principal es NAT Gateway. Para MVP, considerar usar NAT Instance (máquina EC2 que actúa como NAT) para ahorrar, aunque es menos confiable.

---

**Siguiente**: [03-EC2 Essentials](03-ec2-basico.md)  
**Anterior**: [01-Console & IAM](01-console-iam.md)
