# 02 - NIVEL INTERMEDIO: Scale & Integrate

**Semanas**: 3-4  
**Objetivo**: Deployments a escala + integraciones complejas  
**Entregable**: Proyecto 2 (API Serverless + eventos)

---

## Temas (9 módulos)

| # | Tema | Duración | Proyecto |
|---|---|---|---|
| 01 | EC2 Advanced (ASG+ALB) | 4h | P2 |
| 02 | API Gateway | 4h | P2 |
| 03 | Lambda Serverless | 4h | P2 |
| 04 | EventBridge & Step Functions | 4h | P2 |
| 05 | DynamoDB Mastery | 4h | P2 |
| 06 | S3 Advanced | 4h | - |
| 07 | Networking Advanced | 4h | - |
| 08 | DevOps (CI/CD Pipelines) | 4h | - |
| 09 | Containers (ECS/Intro EKS) | 4h | - |

---

## Objetivos

Al completar este nivel:

- [OK] Auto Scaling + Load Balancers (alta disponibilidad)
- [OK] APIs REST con API Gateway
- [OK] Event-driven serverless
- [OK] DynamoDB queries avanzadas
- [OK] CI/CD pipelines funcionales
- [OK] Containers en producción
- [OK] Arquitectura escalable

---

## Arquitectura Proyecto 2

```
Cliente → CloudFront (CDN)
    ↓
    API Gateway (REST API)
    ↓
    Lambda (handler)
    ↓
    DynamoDB (data)
    ↓
    EventBridge → SNS (notificaciones)
```

---

## Checklist

- [ ] Todos los 9 temas completados
- [ ] Labs testeados
- [ ] Proyecto 2 deployado
- [ ] Observabilidad configurada
- [ ] CI/CD pipeline activo

---

**Status**: Abriendo Semana 3 (Febrero 2026)  
**Siguiente**: [03-AVANZADO](../03-avanzado/README.md)
