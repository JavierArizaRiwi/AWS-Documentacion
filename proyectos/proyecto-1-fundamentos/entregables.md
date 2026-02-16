# Proyecto 1: Entregables Requeridos

---

## 📦 Qué Entregar

Crear un repositorio (GitHub, GitLab, etc) con:

```
mi-proyecto-1/
├─ README.md
│  ├─ Descripción ejecutiva
│  ├─ Arquitectura diagram
│  ├─ Setup instructions
│  └─ Cost analysis
├─ src/
│  ├─ html/
│  │  └─ index.html (+ otros)
│  ├─ css/
│  │  └─ style.css
│  └─ assets/
│     └─ logo.png
├─ infrastructure/
│  ├─ s3-setup.sh
│  ├─ cloudfront-policy.json
│  └─ iam-policy.json
├─ docs/
│  ├─ ARCHITECTURE.md
│  ├─ DEPLOYMENT.md
│  ├─ SECURITY.md
│  └─ TROUBLESHOOTING.md
└─ evidencias/
   ├─ screenshot-1-s3.png
   ├─ screenshot-2-cloudfront.png
   └─ cost-breakdown.png
```

---

## ✅ Archivos Clave

### 1. README.md
```markdown
# Mi Proyecto Web AWS

## 🎯 Descripción
[1 párrafo sobre qué es]

## 🏗️ Arquitectura
[Diagrama or descripción]

## 🚀 Cómo Deployar
1. Paso 1
2. Paso 2
...

## 💰 Costo
- S3: $X/mes
- CloudFront: $Y/mes
- Total: ~$Z

## 📚 Links
- [MyWebsite](https://example.com)
- [CloudFront Distribution](https://...)
```

### 2. Diagrama de Arquitectura (ARCHITECTURE.md)

```markdown
# Arquitectura

```
┌─────────────────┐
│  Internet Users │
└────────┬────────┘
         │
    ┌────▼─────┐
    │CloudFront│ (CDN - Global)
    └────┬─────┘
         │
    ┌────▼────────┐
    │ S3 Bucket   │ (Private)
    │ index.html  │
    │ style.css   │
    └─────────────┘
```
```

### 3. deployment-script.sh

```bash
#!/bin/bash
# Deploy website to S3 + CloudFront

# Variables
BUCKET_NAME="my-website-$(date +%s)"
REGION="us-east-1"
PROFILE="bootcamp"

# 1. Create bucket
aws s3 mb s3://$BUCKET_NAME --region $REGION --profile $PROFILE

# 2. Enable versioning
aws s3api put-bucket-versioning \
  --bucket $BUCKET_NAME \
  --versioning-configuration Status=Enabled \
  --profile $PROFILE

# 3. Upload files
aws s3 sync src/ s3://$BUCKET_NAME/ --profile $PROFILE

# 4. Create CloudFront distribution
# (ver infrastructure/cloudfront-policy.json)
```

### 4. Evidencias (Screenshots)

Capturar:
- [ ] S3 bucket properties (versioning ON)
- [ ] CloudFront distribution status
- [ ] IAM policy attached
- [ ] Website URL funcionando
- [ ] Network tab mostrando cache hits
- [ ] Cost breakdown en AWS Console

---

## 🎬 Presentación (Opcional pero Recomendado)

**Formato**: 5 minutos video o 3-slide deck

**Contenido**:
1. "Qué problema resuelve"
2. Diagrama + funcionamiento
3. Lecciones aprendidas + mejoras futuras

---

## 📝 Checklist Final

Antes de submitir:

- [ ] Repo público o shared (revisor puede clonar)
- [ ] README tiene instrucciones claras
- [ ] Scripts son reproducibles
- [ ] Website URL funciona
- [ ] Cleanup script está presente
- [ ] No hay secrets en repo
- [ ] Costo total documentado
- [ ] Evidencias incluidas

---

## 🚀 Cómo Submitir

1. Pushear a GitHub/GitLab
2. Copiar URL del repo
3. Enviar a mentor/revisor
4. Incluir link en perfil/portfolio

---

[← Volver a Proyecto 1](./README.md)
