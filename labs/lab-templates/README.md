# Lab Templates - Plantillas para Crear Labs

Usa estas plantillas como base para crear nuevos labs

---

## Plantilla: README-lab.md

```markdown
# Lab: [Nombre Descriptivo]

**Nivel**: Básico | Intermedio | Avanzado  
**Duración**: Xh (incluye setup + cleanup)  
**Costo**: ~$X  
**Requisitos**: [Módulo previo]

## Objetivo
[Una línea clara]

## Pasos
1. [Paso 1]
...
N. [Paso final]

## Evidencias
[Ver evidencias-checklist.md]

## Cleanup
[Ver cleanup.sh]
```

---

## Plantilla: evidencias-checklist.md

```markdown
# Evidencias Requeridas

## Checklist de Capturas

- [ ] Screenshot 1: [Descripción]
  Pasos: 1) ... 2) ... 3) ...
  Debe mostrar: [qué se ve]

- [ ] Screenshot 2: [Descripción]
  ...

- [ ] Terminal Output (copy-paste)
  ```
  $ comando
  output aquí
  ```
```

---

## Plantilla: cleanup.sh

```bash
#!/bin/bash
# cleanup.sh - Eliminación de recursos

set -e

AWS_PROFILE="bootcamp"
REGION="us-east-1"

echo " Limpiando recursos de [Lab Name]..."

# Step 1: Delete compute
echo "Terminando instancias EC2..."
# IFS="" read -r -a instances < <(...)
# for id in "${instances[@]}"; do
# aws ec2 terminate-instances --instance-ids "$id" ...
# done

# Step N: Verify cleanup
echo " Limpieza completada"
echo "Verifica: aws cloudtrail lookup-events --profile $AWS_PROFILE"
```

---

## Plantilla: assets/

Archivos de soporte:

```
assets/
 template.json          # CloudFormation/Terraform
 policy.json           # IAM Policy
 lambda-function.py    # Code samples
 README.md            # Documentación adicional
```

---

## Checklist Antes de Publicar Lab

- [ ] README claro y paso-a-paso
- [ ] Costo < $5 y en free tier
- [ ] Cleanup script tested
- [ ] Evidencias documentadas
- [ ] Assets incluidos
- [ ] Sin credenciales/secrets
- [ ] Links internos verificados

---

**Volver**: [Labs - Índice](README.md)
