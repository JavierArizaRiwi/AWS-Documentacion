# Proyecto 1: Criterios de Aceptación

**Puntuación Total**: 100 pts

---

## Criterio 1: S3 Bucket Setup (20 pts)

### Novato (0-6 pts)
- Bucket creado pero con acceso público
- No hay versionado
- No tienen política clara

### Intermedio (7-13 pts)
- Bucket privado ✅
- Versionado habilitado
- Política documentada pero incompleta

### Avanzado (14-20 pts)
- Bucket privado + CloudFront OAC
- Versionado + lifecycle policy
- Política IAM restrictiva + documentada

---

## Criterio 2: CloudFront Distribution (20 pts)

### Novato (0-6 pts)
- CloudFront creado pero sin caché
- TTL default

### Intermedio (7-13 pts)
- Distribución funcional
- TTL customizado
- Compression habilitada

### Avanzado (14-20 pts)
- OAC (Origin Access Control) configurado
- Invalidación de caché
- Custom headers + security headers

---

## Criterio 3: Seguridad IAM (20 pts)

### Novato (0-6 pts)
- IAM user con acceso amplio
- Sin MFA en cuenta

### Intermedio (7-13 pts)
- Política restringida a bucket específico
- MFA habilitado
- No hay acceso innecesario

### Avanzado (14-20 pts)
- Cross-account access (si aplica)
- Temporal credentials (STS)
- IAM policy simulator tests passed

---

## Criterio 4: Performance & Optimization (15 pts)

### Novato (0-5 pts)
- Sin optimizaciones
- Static files sin compresión

### Intermedio (6-10 pts)
- Compresión gzip
- Cache headers configurados

### Avanzado (11-15 pts)
- CloudFront full optimization
- Imagen optimization
- HTTP/2 enabled

---

## Criterio 5: Documentación (15 pts)

### Novato (0-5 pts)
- README básico
- Instrucciones incompletas

### Intermedio (6-10 pts)
- README claro
- Diagrama de arquitectura
- Pasos reproducibles

### Avanzado (11-15 pts)
- Documentación completa
- Diagrama profesional (Mermaid/SVG)
- Cost analysis
- Troubleshooting guide

---

## Criterio 6: Cleanup & Budget (10 pts)

### Novato (0-3 pts)
- Sin cleanup script
- Posibles recursos abandonados

### Intermedio (4-7 pts)
- Cleanup script existe
- Presupuesto monitoreado

### Avanzado (8-10 pts)
- Cleanup script testeado
- Budget proof (screenshot)
- Cost < $5

---

## Scoring Rápido

```
100 = Experto (publication-ready)
 80 = Competente (production-ready)
 60 = Funcional (complete)
 40 = En progreso
  0 = No completado
```

---

[← Volver a Proyecto 1](./README.md)
