# Proyecto 1: Web Estática + CDN + IAM

**Nivel**: Básico  
**Duración**: 8 horas (lab + proyecto)  
**Semana**: Semana 2  
**PREREQUISITO**: Completar módulos 01-05 del nivel básico

---

## Contexto

Imagina que trabajas en una agencia y necesitas hostear un sitio web estático de un cliente con:
- Acceso rápido globalmente (CDN)
- Seguridad (sin acceso directo a bucket)
- Escalabilidad automática
- Bajo costo

**Solución AWS**: S3 + CloudFront + IAM + Route 53

---

## Objetivos

Al terminar, lograrás:
- [OK] Crear S3 bucket para almacenar HTML/CSS/JS
- Configurar hosting estático
- Usar CloudFront para CDN
- IAM policy para restringir acceso
- Conectar dominio personalizado (opcional)
- Documentar arquitectura

---

## Arquitectura Esperada

```
Dominio (Route 53 - opcional)
    ↓
CloudFront (CDN - caché global)
    ↓
S3 Bucket (origen privado)
    ↓
HTML/CSS/JS/Assets
```

---

## Criterios de Aceptación

Ver [criterios-aceptacion.md](criterios-aceptacion.md)

---

## Qué Entregar

Ver [entregables.md](entregables.md)

---

## Empezar

```bash
# 1. Crear directorio
mkdir -p ~/proyecto-1-web && cd ~/proyecto-1-web

# 2. Crear estructura
mkdir -p src/{html,css,js} docs

# 3. Crear archivo base
cat > src/html/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head><title>Mi Sitio</title></head>
<body><h1>Hola AWS!</h1></body>
</html>
EOF

# 4. Seguir instrucciones en criterios-aceptacion.md
```

---

## Recursos

- [S3 Website Hosting Docs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
- [CloudFront Distribution](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)
- [Route 53 DNS](https://docs.aws.amazon.com/route53/)

---

[ Volver al Índice de Proyectos](../README.md)
