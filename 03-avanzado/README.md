# 03 - NIVEL AVANZADO: Production-Ready

**Semanas**: 5-6  
**Objetivo**: Architecturas resilientes + operabilidad  
**Entregable**: CAPSTONE (Microservicios + Observabilidad)

---

## Temas (8 módulos)

| # | Tema | Horas | Capstone |
|---|---|---|---|
| 01 | HA & Disaster Recovery | 4h | [Ready] |
| 02 | Observabilidad (CloudWatch+X-Ray) | 4h | [Ready] |
| 03 | IaC: CloudFormation | 4h | [Ready] |
| 04 | Messaging (SQS/SNS/Kinesis) | 4h | [Ready] |
| 05 | Security Advanced (KMS/Cognito) | 4h | [Ready] |
| 06 | Analytics (Athena/Glue) | 4h | - |
| 07 | Migraciones (DMS/SMS) | 4h | - |
| 08 | Serverless Pro | 4h | - |

---

## Objetivos

- [OK] Multi-region + Multi-AZ deployments
- [OK] EKS clusters + Helm
- [OK] Monitoreo + alertas + tracing
- [OK] Infrastructure as Code productivo
- [OK] Security enterprise-grade
- [OK] Analytics data lakes

---

## Capstone Architecture

```
Internet
    ↓
ALB (multi-AZ)
    ↓
ECS/EKS Cluster
     Service 1 (containerized)
     Service 2 (containerized)
     Service 3 (containerized)
    ↓
RDS (Multi-AZ) + ElastiCache
    ↓
CloudWatch + X-Ray (observabilidad)
    ↓
Alertas + SNS
```

---

## Checklist

- [ ] 8 temas completados
- [ ] CAPSTONE deployado
- [ ] Observabilidad full-stack
- [ ] IaC documentada
- [ ] Stress testing passed

---

**Status**: Abriendo Semana 5 (Principios Marzo 2026)  
**Objetivo Fin**: Capstone presentado + Portfolio actualizado
