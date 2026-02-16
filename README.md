# AWS Upskilling for Coders - Ruta Completa (Básico → Intermedio → Avanzado)

**Duración**: 6 semanas  
**Nivel**: Desde cero hasta AWS Solutions Architect Associate  
**Formato**: Hands-on labs + 3 Proyectos progresivos + Rúbricas  
**Costo**: $0-50 (con Free Tier optimizado)

---

## Estructura del Repositorio

```
aws-Upskilling-coders/
 00-onboarding/              # Setup inicial (4 módulos)
 01-basico/                  # Semanas 1-2 (6 temas)
 02-intermedio/              # Semanas 3-4 (9 temas)
 03-avanzado/                # Semanas 5-6 (8 temas)
 labs/                       # 23 labs hands-on
    lab-templates/          # Plantillas para crear labs
    basico/
    intermedio/
    avanzado/
 proyectos/                  # 3 proyectos progresivos
    proyecto-1-fundamentos/
    proyecto-2-intermedio/
    proyecto-3-capstone/
 rubricas/                   # Criterios de evaluación
 docs/                       # Documentación global
    ROADMAP.md
    GLOSARIO.md
    COSTOS-Y-CLEANUP.md
    CHECKLIST-REVISION.md
    PLANTILLA-README-TEMA.md
 README.md                   # Este archivo
 CONTRIBUTING.md             # Cómo contribuir
 LICENSE                     # MIT License
```

---

## Quick Start (5 min)

```bash
# 1. Clone este repo
git clone https://github.com/JavierArizaRiwi/AWS-Documentacion
cd aws-bootcamp-coders

# 2. Lee el README del nivel donde estés
cat 00-onboarding/README.md      # ← AQUÍ EMPEZAR

# 3. Configura tu perfil AWS
aws configure --profile bootcamp
aws sts get-caller-identity --profile bootcamp
```

---

## Niveles de Aprendizaje

### NIVEL BÁSICO (Semanas 1-2)
| Tema | Duración | Lab | Proyecto |
|------|----------|-----|----------|
| 01-Console & IAM | 4h | [Ready] | - |
| 02-VPC & Networking | 4h | [Ready] | - |
| 03-EC2 Essentials | 4h | [Ready] | - |
| 04-S3 Basics | 4h | [Ready] | - |
| 05-RDS Basics | 4h | [Ready] | - |
| **Proyecto 1** | **8h** | - | **Web Estática** [Ready] |

[Empezar: Nivel Básico →](01-basico/README.md)

---

### NIVEL INTERMEDIO (Semanas 3-4)
**9 temas**: EC2 ASG+ALB, API Gateway, Lambda, EventBridge, DynamoDB, S3 Advanced, Networking Advanced, DevOps CI/CD, ECS/Fargate

**Proyecto 2**: API Serverless + Eventos (16h)

[Ver: Nivel Intermedio →](02-intermedio/README.md)

---

### NIVEL AVANZADO (Semanas 5-6)
**8 temas**: HA/DR, Observability, CloudFormation IaC, Messaging, Security Advanced, Analytics, Migraciones, Serverless Pro

**CAPSTONE**: Microservicios + EKS + Full Stack (24h)

[Ver: Nivel Avanzado →](03-avanzado/README.md)

---

## Cronograma (6 semanas)

| Semana | Tópicos | Horas | Entregable |
|--------|---------|-------|----------|
| 1-2 | IAM, VPC, EC2, S3, RDS | 20h | Proyecto 1 [Ready] |
| 3-4 | EC2 ASG, Lambda, API GW, DDB, CI/CD, ECS | 24h | Proyecto 2 [Ready] |
| 5-6 | EKS, Observability, IaC, Security, Analytics | 24h | Capstone [Ready] |
| **Total** | 23 Labs + 3 Proyectos | **68h** | **Portfolio Ready** |

---

## Metodología por Tema

Cada módulo sigue esta estructura:

```
Objetivo (qué lograrás)
Conceptos Clave (máx 10)
Hands-on Lab (paso a paso)
Evidencias (screenshots)
Preguntas Validación (5-10)
Mini Reto (profundización)
Checklist Cierre
Cleanup Scripts
```

 [Ver Plantilla →](docs/PLANTILLA-README-TEMA.md)

---

## 3 Proyectos Progresivos

### Proyecto 1: Web Estática (Básico - Semana 2)
**Stack**: S3 + CloudFront + IAM  
**Entregable**: Sitio web en producción  
**Rúbrica**: 100 pts, 6 criterios  
[Ir a Proyecto 1 →](proyectos/proyecto-1-fundamentos/)

### Proyecto 2: API Serverless (Intermedio - Semana 4)
**Stack**: API Gateway + Lambda + DynamoDB + EventBridge  
**Entregable**: API REST + Monitoring  
**Rúbrica**: 100 pts, 6 criterios  
[Ir a Proyecto 2 →](proyectos/proyecto-2-intermedio/)

### CAPSTONE: Microservicios (Avanzado - Semana 6)
**Stack**: EKS + Observability + CI/CD + IaC  
**Entregable**: Production-ready architecture  
**Rúbrica**: 100 pts, 7 criterios  
[Ir a Capstone →](proyectos/proyecto-3-capstone/)

---

## Documentación Global

- **[ROADMAP.md](docs/ROADMAP.md)** - Futuras mejoras + contribuciones
- **[GLOSARIO.md](docs/GLOSARIO.md)** - 100+ términos AWS explicados
- **[COSTOS-Y-CLEANUP.md](docs/COSTOS-Y-CLEANUP.md)** - Evita sorpresas en factura
- **[CHECKLIST-REVISION.md](docs/CHECKLIST-REVISION.md)** - QA para PRs
- **[PLANTILLA-README-TEMA.md](docs/PLANTILLA-README-TEMA.md)** - Cómo crear nuevos temas

---

## Labs Organizados

### Por Nivel
- **[Labs Básicos](labs/basico/)** - 6 labs (IAM, VPC, EC2, S3, RDS, etc)
- **[Labs Intermedios](labs/intermedio/)** - 9 labs (ASG, Lambda, DDB, ECS, etc)
- **[Labs Avanzados](labs/avanzado/)** - 8 labs (EKS, CFN, X-Ray, KMS, etc)

### Templates para Crear Labs
[Lab Templates →](labs/lab-templates/)

Cada lab incluye:
```
 README.md                    # Instrucciones
 evidencias-checklist.md     # Qué capturar
 cleanup.sh                   # Script limpieza
 assets/                      # Templates JSON/YAML
```

---

## Rúbricas de Evaluación

- **[Rúbrica Labs](rubricas/rubrica-labs.md)** - 23 labs evaluados
- **[Rúbrica Proyectos](rubricas/rubrica-proyectos.md)** - P1, P2, Capstone
- **[Rúbrica Presentaciones](rubricas/rubrica-presentaciones.md)** - Demos + Pitches

Escala: Novato (0-33%) → Intermedio (34-66%) → Avanzado (67-100%)

---

## Costos Estimados

| Nivel | Presupuesto | Con Free Tier |
|-------|-------------|---|
| Básico (2 sem) | $30 | $0 (Free Tier) |
| Intermedio (2 sem) | $50 | ~$10 |
| Avanzado (2 sem) | $40 | ~$20 |
| **TOTAL (6 sem)** | **$120** | **~$30** |

 [Guía Completa de Costos →](docs/COSTOS-Y-CLEANUP.md)

---

## Cómo Empezar (Paso a Paso)

### Paso 1: Prerequisites (30 min)
```bash
# AWS CLI v2
aws --version  # v2.x.x

# Git
git --version  # v2.x

# Acceso AWS
aws sts get-caller-identity
```

### Paso 2: Onboarding (2-3 horas)
1. Leer: [00-Onboarding/README.md](00-onboarding/README.md)
2. Crear: AWS account + IAM user + MFA
3. Configurar: AWS CLI + 3 profiles
4. Setup: Billing alerts + CloudTrail

### Paso 3: Nivel Básico (2 semanas)
1. Completar: 5 temas + 5 labs
2. Hacer: Proyecto 1 (Web Estática)
3. Presentar: Demostración

### Paso 4: Siguientes Niveles
Repetir ciclo: Tema → Lab → Proyecto

---

## Checklist de Éxito

### Antes de Empezar
- [ ] AWS account + Free Tier activo
- [ ] AWS CLI instalado + configurado
- [ ] Budget de $50-100 establecido
- [ ] CloudTrail habilitado
- [ ] MFA en root account

### Durante el Bootcamp
- [ ] Ejecutar cleanup al final de cada lab
- [ ] Monitorear costos (Cost Explorer)
- [ ] Documentar lecciones personales
- [ ] Hacer preguntas en Issues

### Al Final (6 semanas)
- [ ] 3 proyectos completados
- [ ] Portfolio actualizado
- [ ] Presentaciones hechas
- [ ] GitHub públicamente visible

---

## Comunidad & Soporte

### Issues & Discussions
- **Bug**: Algo no funciona → Abre Issue
- **Feature**: Sugerencia → Abre Discussion
- **Pregunta**: Conceptual → Abre Discussion

### Contribuir
- Agregando labs nuevos
- Mejorando documentación
- Reportando bugs
- Traduciendo contenido

[Ver CONTRIBUTING.md](CONTRIBUTING.md)

---

## Recursos Oficiales

- [AWS Official Docs](https://docs.aws.amazon.com)
- [AWS Training](https://aws.amazon.com/training/)
- [AWS Whitepapers](https://aws.amazon.com/whitepapers/)
- [AWS Well-Architected Framework](https://aws.amazon.com/blogs/aws/well-architected-framework/)

---

## Estadísticas del Proyecto

- **Contenido**: 23 labs + 3 proyectos + 60+ horas
- **Cobertura**: 22 servicios AWS core
- **Nivel**: Certificación SAA-ready
- **Versión**: 1.0 (Feb 2026)
- **Status**: Full course boot [Ready]

---

## Licencia & Disclaimer

**Licencia**: MIT (Libre para uso educativo)

**Disclaimer**: 
- No somos afiliados con AWS
- Los autores NO son responsables por costos incurridos
- Usar siempre Free Tier + Cleanup scripts
- Verificar saldo de cuenta regularmente

---

## Autores & Contribuidores

- **Creador**: AWS Bootcamp Team
- **Versión**: v1.0 (16 Feb 2026)
- **Contribuidores**: [Ver aquí](CONTRIBUTING.md#como-contribuir)
- **Especial**: Gracias por ser parte de la comunidad

---

## Proyecto Abierto a Contribuciones

**¿Falta un tema?** → Abre un Issue o PR  
**¿Encontraste un bug?** → Reportalo  
**¿Mejora la documentación?** → Contribuye  

El objetivo es hacer el bootcamp AWS más accesible para hispanohablantes

---

## Siguiente Paso

**[EMPEZAR: 00-Onboarding →](00-onboarding/README.md)**

---

**¡Bienvenido al AWS Bootcamp para Coders!**
