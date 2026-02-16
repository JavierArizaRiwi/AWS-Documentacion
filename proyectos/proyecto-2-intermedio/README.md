# Proyecto 2: API Serverless + Eventos

**Nivel**: Intermedio  
**Duración**: 16 horas  
**Semana**: Semana 4  
**Prerequisito**: Completar hasta módulo 8 (DevOps) del nivel intermedio

---

## Contexto

Necesitas crear una API para un e-commerce que:
- Maneja múltiples operaciones (CRUD)
- Escala automáticamente
- Procesa eventos asíncrónos (notificaciones, analytics)
- Costo bajo (serverless)

**Stack AWS**: API Gateway + Lambda + DynamoDB + EventBridge

---

## Objetivo

Construir una API REST completa:
- [OK] Endpoints: POST/GET/PUT/DELETE
- Base de datos NoSQL
- Event-driven notifications
- Monitoring + logs
- CI/CD pipeline

---

## Arquitectura

```
Cliente
  ↓
API Gateway (REST)
  ↓
Lambda (handler)
  ↓
DynamoDB (data)
  ↓
EventBridge (async)
  ↓
SNS/SQS (notifications)
```

---

## Entregables

Similar a Proyecto 1 pero con:
- API endpoints documentados
- Test cases (Postman/curl)
- Lambda code + layers
- Deployment pipeline
- Monitoring dashboard

---

## Empezar

```bash
cd ~/proyecto-2-api
cat README.md  # (aún en desarrollo)
```

---

**Status**: En Desarrollo (Semana 3)  
[→ Volver](../README.md)
