# 01 - NIVEL BÁSICO: Fundamentos AWS

**Semanas**: 1-2  
**Objetivo**: Entender servicios core + crear base sólida  
**Entregable**: Proyecto-1 (Web estática en S3)

---

## Temas del Nivel

| # | Tema | Duración | Lab | Estado |
|---|---|---|---|---|
| 01 | Console & IAM | 4h | [Ready] | Ready |
| 02 | Redes Básicas (VPC) | 4h | [Ready] | Ready |
| 03 | EC2 Essentials | 4h | [Ready] | Ready |
| 04 | S3 Basics | 4h | [Ready] | Ready |
| 05 | RDS Basics | 4h | [Ready] | Ready |
| 06 | Proyecto 1 Integration | 8h | [Pending] | Pending |

---

## Archivos por Tema

### [01-Console & IAM](01-console-iam.md) [Ready]
- AWS Console navigation
- IAM users, roles, policies
- MFA setup
- Access keys generation
- Policy simulator

### [02-Redes Básicas](02-redes-basicas.md) [Ready]
- VPC creation
- Subnets (public/private)
- Route tables
- Internet Gateway
- Network ACLs

### [03-EC2 Essentials](03-ec2-basico.md) [Ready]
- Instancias
- Key pairs y SSH
- Security groups
- Elastic IPs
- EBS volumes y snapshots

### [04-S3 Basics](04-s3-basico.md) [Ready]
- Buckets y objetos
- Policies y versioning
- Server-side encryption
- Lifecycle policies
- Static hosting

### [05-RDS Basics](05-rds-basico.md) [Ready]
- DB instance creation
- Engine selection
- Multi-AZ deployments
- Backups
- Security groups para DB

### 06-Proyecto-1 Integration
- Combinar todos los servicios
- Web estática + CloudFront
- Documentación de arquitectura

---

## Objetivos de Aprendizaje

Al completar el nivel básico:

- [OK] Navegar AWS Console sin miedo
- [OK] Crear y gestionar usuarios IAM seguros
- [OK] Diseñar VPC simple (public + private)
- [OK] Lanzar y configurar EC2 instances
- [OK] Usar S3 para almacenamiento
- [OK] Configurar base de datos RDS
- [OK] Entender costos de cada servicio
- [OK] Crear arquitectura simple (web + DB)

---

## Timeline

| Semana | Tópicos | Horas | Entregable |
|--------|---------|-------|-----------|
|--------|---------|-------|-----------|
| **W1** | Onboarding + IAM + VPC | 8h | VPC architecture dia |
| **W2** | EC2 + S3 + RDS | 8h | Proyecto 1 código |

---

## Quickstart

```bash
# 1. Completar onboarding primero
cd ../00-onboarding && cat README.md

# 2. Empezar con IAM
cat 01-console-iam.md

# 3. Ejecutar lab
cd labs/basico/01-iam-lab && bash setup.sh

# 4. Capturar evidencias
# (ver en evidencias-checklist.md)

# 5. Cleanup
bash cleanup.sh
```

---

## Checklist de Cierre

Para pasar a Intermedio, verifica:

- [ ] IAM users creados + MFA activo
- [ ] VPC diagrama documentado
- [ ] EC2 instance launched + SSH access
- [ ] S3 bucket creado + policy aplicada
- [ ] RDS instance running
- [ ] Proyecto 1 deployado
- [ ] Todos los labs completados
- [ ] Cleanup ejecutado (0 recursos stranded)
- [ ] Costos dentro presupuesto ($30-50)
- [ ] Documentación personal guardada

---

## Estructura de Carpetas

```
01-basico/
 README.md              # Este archivo
 01-console-iam.md
 02-redes-basicas.md
 03-ec2-basico.md
 04-s3-basico.md
 05-rds-basico.md
 labs/
    01-iam-lab/
    02-vpc-lab/
    03-ec2-lab/
    04-s3-lab/
    05-rds-lab/
```

---

**Siguiente**: [02-INTERMEDIO](../02-intermedio/README.md) (después de Proyecto 1)

**O ir a tema específico**:
- [01-Console & IAM](01-console-iam.md)
