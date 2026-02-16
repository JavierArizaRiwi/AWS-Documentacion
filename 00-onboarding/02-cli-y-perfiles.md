# 02 - CLI y Perfiles AWS: Configuración Finales

**Duración**: 1.5 horas  
**Requisitos**: Completar 01-Cuenta-y-Seguridad  
**Objetivo**: CLI funcional + múltiples perfiles para diferentes usos

---

##  Lo Que Harás

- Instalar AWS CLI v2
- Crear 3 perfiles diferentes (principal, dev, readonly)
- Testear acceso
- Guardar config en `.aws/`

---

##  Setup CLI

Ver: [02-cli-y-perfiles.md - Completo](README.md) (en desarrollo)

**Resumen**:
1. `aws configure --profile bootcamp`
2. `aws sts get-caller-identity --profile bootcamp`
3. Crear `.aws/config` con perfiles
4. Testear cada uno

---

##  Verificación

```bash
aws sts get-caller-identity --profile bootcamp
# {
#   "UserId": "AIDA...",
#   "Account": "123456789012",
#   "Arn": "arn:aws:iam::123456789012:user/bootcamp-user"
# }
```

 Si ves UserId → Listo

---

**Siguiente**: [03-Buenas Prácticas](03-buenas-practicas-costos.md)
