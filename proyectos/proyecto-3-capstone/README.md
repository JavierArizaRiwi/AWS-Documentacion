# CAPSTONE: Microservicios Contenedorizados

**Nivel**: Avanzado  
**Duración**: 24 horas  
**Semana**: Semana 6  
**Prerequisito**: Completar todos los módulos 1-8 del nivel avanzado

---

## Contexto

Desplegar una arquitectura enterprise de microservicios con:
- Orquestación de containers
- Observabilidad completa
- Alta disponibilidad (Multi-AZ)
- CI/CD pipeline automatizado
- Security best practices

**Stack**: ECS/EKS + CloudWatch + X-Ray + CodePipeline + RDS + ElastiCache

---

## Objetivos

- [OK] EKS Cluster con 2+ node groups
- 3+ microservicios containerizados
- Full observability (logs + metrics + traces)
- CI/CD pipeline con GitHub Actions
- Disaster recovery + backups
- SECURITY: encryption + IAM + secrets

---

## Arquitectura

```
Internet
  ↓
ALB (multi-AZ)
  ↓
EKS Cluster
 Service 1 (Python/FastAPI)
 Service 2 (Node.js/Express)
 Service 3 (Go)
  ↓
RDS (PostgreSQL Multi-AZ)
ElastiCache (Redis)
  ↓
CloudWatch Logs
X-Ray Tracing
ServiceMap
```

---

## Entregables

- Architecture documentation + diagram
- Terraform/CloudFormation IaC
- Microservices code (3+ services)
- Deployment runbook
- Observability dashboard
- Presentation (15 min)
- Capstone report

---

## Presentación Final

**Formato**: 15-20 minutos

**Contenido**:
1. Problem statement
2. Architecture overview
3. Key technologies
4. Demo en vivo
5. Lecciones + mejoras futuras
6. Q&A

---

**Status**: Abriendo Semana 5+  
**Entrega**: Final de Semana 6

---

[→ Volver](../README.md)
