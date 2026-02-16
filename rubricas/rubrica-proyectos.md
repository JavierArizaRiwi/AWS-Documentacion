# Rúbrica Proyectos - Evaluación Proyectos Finales

**Escala**: 100 puntos por proyecto  
**Criterios**: 6-7 dimensiones por nivel

---

## PROYECTO 1: Web Estática (100 pts)

Ver: [Proyecto 1 - Criterios Aceptación](../proyectos/proyecto-1-fundamentos/criterios-aceptacion.md)

---

## PROYECTO 2: API Serverless (100 pts)

### Criterio 1: API Functionality (25 pts)
- CRUD operations work
- All endpoints respond
- Error handling

### Criterio 2: Database Design (20 pts)
- DynamoDB schema optimal
- GSI/LSI if needed
- Queries efficient

### Criterio 3: Event Processing (20 pts)
- EventBridge rules active
- Events log correctly
- No data loss

### Criterio 4: Monitoring (15 pts)
- CloudWatch dashboard
- Alarms configured
- Logs aggregated

### Criterio 5: Code Quality (10 pts)
- Readable + documented
- Follows AWS patterns
- Error handling

### Criterio 6: Documentation (10 pts)
- API docs (Swagger/OpenAPI)
- Deployment guide
- Troubleshooting

---

## CAPSTONE: Microservicios (100 pts)

### Criterio 1: Infrastructure (20 pts)
- EKS cluster healthy
- Multi-AZ deployment
- Security groups correct

### Criterio 2: Microservices Architecture (20 pts)
- 3+ services deployed
- Services decouple properly
- Communication patterns clear

### Criterio 3: Observability (15 pts)
- Full logging enabled
- Metrics collected
- X-Ray tracing active
- Dashboard functional

### Criterio 4: CI/CD Pipeline (15 pts)
- Pipeline automated
- Tests pass
- Deployment rollbacks work
- Secrets management correct

### Criterio 5: Security & Compliance (15 pts)
- RBAC applied
- Network policies
- Encryption enabled
- No secrets in code

### Criterio 6: Disaster Recovery (10 pts)
- Backup strategy documented
- RTO/RPO defined
- Tested recovery

### Criterio 7: Documentation & Presentation (5 pts)
- Architecture diagram
- Deployment runbook
- Live demo successful

---

## Scoring por Proyecto

```
P1: 60-100 = Pass
P2: 60-100 = Pass
Capstone: 70-100 = Pass (más riguroso)

Nota Final = (P1 + P2 + Capstone) / 3
```

---

[↩️ Volver](./README.md)
