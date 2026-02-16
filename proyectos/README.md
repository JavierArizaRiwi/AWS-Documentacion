# Proyectos - Índice

**Propósito**: Demostrar competencias en ambientes reales  
**Evaluación**: Rúbricas en [rubricas/](../rubricas/)

---

## 3 Proyectos Progresivos

### [Proyecto 1 - BÁSICO: Web Estática](./proyecto-1-fundamentos/)
**Semana**: 2  
**Stack**: S3 + CloudFront + IAM  
**Entregable**: Sitio web + documentación

### [Proyecto 2 - INTERMEDIO: API Serverless](./proyecto-2-intermedio/)
**Semana**: 4  
**Stack**: API Gateway + Lambda + DynamoDB + EventBridge  
**Entregable**: API + tests + monitoring

### [CAPSTONE - AVANZADO: Microservicios](./proyecto-3-capstone/)
**Semana**: 6  
**Stack**: ECS/EKS + Observabilidad + CI/CD + Messaging  
**Entregable**: Architecture doc + código + presentation

---

## Cada Proyecto Tiene

```
proyecto-N/
 README.md                  # Descripción + story
 criterios-aceptacion.md   # Rúbrica detail
 entregables.md            # Qué entregar
 starter-code/             # Code template (opcional)
 solution/                 # Solución completa (repo privado)
```

---

## Matriz de Evaluación

| Aspecto | P1 | P2 | Capstone |
|---------|----|----|----------|
| Arquitectura | Básica | Escalable | HA/Disaster Recovery |
| Seguridad | IAM | Encryption + Secrets | KMS + Cognito |
| Observabilidad | Logging | CloudWatch | X-Ray + Full Stack |
| CI/CD | Manual | Pipeline | Full GitOps |
| Complejidad | Star-1 | Star-3 | Star-5 |

---

## Progresión Recomendada

```
Básico         Intermedio      Avanzado
  ↓               ↓               ↓
P1: Web      → P2: API      → Capstone
(S3+CF)      (Serverless)    (Microserv)
   ↓              ↓              ↓
Portfolio    Portfolio      Production-Ready
```

---

## Entrega

```
Tu-Repo-Github/
 proyecto-1/
   README.md
   src/
   docs/
   evidencias/
 proyecto-2/
   [similar]
 capstone/
    architecture-diagram.png
    infrastructure/
    application/
    observability/
    presentation.md
```

---

**Ir a proyecto específico**:
- [→ Proyecto 1](./proyecto-1-fundamentos/)
- [→ Proyecto 2](./proyecto-2-intermedio/)
- [→ Capstone](./proyecto-3-capstone/)
