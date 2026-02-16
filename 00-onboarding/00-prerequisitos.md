# 00 - Onboarding: Setup AWS (Índice de Módulos)

**Semana**: Semana 0 (Pre-bootcamp)  
**Duración total**: ~6 horas distribuidas  
**Objetivo**: Ambiente listo + conceptos fundamentales

---

## Módulos del Onboarding

Este nivel tiene 4 submódulos secuenciales:

### [00-Prerequisitos](00-prerrequisitos.md) 
- Verificar requisitos técnicos
- Hardware mínimo
- Software requerido (Git, terminal, etc)
- Free trial límites

### [01-Cuenta y Seguridad](01-cuenta-y-seguridad.md) 
- Crear AWS account
- Root user security
- MFA setup
- CloudTrail habilitado

### [02-CLI y Perfiles](02-cli-y-perfiles.md) 
- Instalar AWS CLI v2
- Configurar credentials
- Crear profiles (bootcamp, dev, readonly)
- Verificar permisos

### [03-Buenas Prácticas y Costos](03-buenas-practicas-costos.md) 
- AWS Well-Architected Framework intro
- Billing alerts
- Cost optimization basics
- Cleanup rituals

---

##  Checklist Onboarding Completo

Al terminar esta sección:

- [ ] AWS CLI v2 instalado + `aws --version`
- [ ] Credenciales configuradas + `aws sts get-caller-identity`
- [ ] IAM user creado con MFA
- [ ] 3 profiles en `~/.aws/` (default, bootcamp, readonly)
- [ ] Billing alert configurada ($50 límite)
- [ ] CloudTrail trail activo
- [ ] VPC default verificada
- [ ] Cleanup script en tu máquina
- [ ] Documentación personal creada
- [ ] Presupuesto AWS entendido

---

##  Orden Recomendado

1. Lee: **00-Prerequisitos**
2. Ejecuta: **01-Cuenta y Seguridad** (laborioso pero crítico)
3. Configura: **02-CLI y Perfiles** (necesario para todo)
4. Planifica: **03-Buenas Prácticas y Costos**

**Timing**: ~1 día para completar todo

---

##  Tips

- No saltear MFA, es core delpracticum
- Usar AWS Free Tier, pero monitorear gastos
- Guardar cloudtrail logs (auditoría)
- Crear documentación personal mientras aprendes

---

**Siguiente**: [README.md principal](../README.md) para ver estructura general

**O empezar ya**: [00-Prerequisitos](00-prerrequisitos.md)
