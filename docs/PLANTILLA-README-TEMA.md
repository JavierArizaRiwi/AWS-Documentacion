# 📝 PLANTILLA - README para Nuevos Temas

Usá esta plantilla cuando agregues un nuevo tema/módulo

---

# NN - NOMBRE DEL TEMA (Ej: "05-S3 Basics")

**Duración**: X horas (Yh concepto + Zh lab)  
**Nivel**: Básico | Intermedio | Avanzado  
**Requisitos previos**: Módulo anterior (link)  
**Objetivo**: [1 línea clara del qué aprenderás]  
**Costo estimado**: $X (incluye cleanup)

---

## 📌 Objetivo Final

Al terminar este módulo:
- ✅ [Objetivo 1 específico y medible]
- ✅ [Objetivo 2 específico y medible]
- ✅ [Objetivo 3 específico y medible]
- ✅ [Objetivo 4 específico y medible]
- ✅ [Objetivo 5 específico y medible]

---

## 🎯 Conceptos Clave (Máx 10)

1. **Concepto 1**: Descripción breve (1-2 líneas)
2. **Concepto 2**: Descripción breve
3. **Concepto 3**: Descripción breve
4. **Concepto 4**: Descripción breve
5. **Concepto 5**: Descripción breve
6. **Concepto 6**: Descripción breve
7. **Concepto 7**: Descripción breve
8. **Concepto 8**: Descripción breve
9. **Concepto 9**: Descripción breve
10. **Concepto 10**: Descripción breve

---

## 🛠️ Hands-on: [Nombre del Lab]

### Requisitos Previos
- [ ] AWS account con free tier activo
- [ ] AWS CLI v2 + credenciales configuradas
- [ ] Profile: `bootcamp` disponible
- [ ] Presupuesto budget: $XX disponible
- [ ] [Otro requisito específico]

**Tiempo estimado**: X min

### PASO 1: [Acción Descripta] (X min)

Descripción de qué harás y por qué

#### Command/Consola:
```bash
# Comando con comentario explicativo
aws service command --option value --profile bootcamp
```

#### Resultado esperado:
```
Output esperado aquí
{
  "Key": "value"
}
```

#### 🔍 Verificación:
```bash
# Comando para confirmar que funcionó
aws service describe --profile bootcamp
```

---

### PASO 2: [Segunda Acción] (X min)

[Similar estructura que PASO 1]

---

### PASO N: [Última Acción] (X min)

[Completar todos los pasos del lab]

---

## 📸 Evidencias Esperadas

Debes capturar y documentar:

**Captura #1**: [Descripción de qué se ve]
```
Pasos:
1. [Cómo capturar esta evidencia]
2. [Qué debe estar visible]

Output esperado:
[Copy-paste de terminal o screenshot]
```

**Captura #2**: [Descripción de qué se ve]
[Repetir formato]

---

## ❓ Preguntas de Validación

Responde estas preguntas para verificar comprensión:

1. **[Pregunta conceptual]**?
   - Respuesta corta esperada

2. **[Pregunta operacional]**?
   - Respuesta esperada

3. **[Pregunta de troubleshooting]**?
   - Respuesta esperada

[Continuar con 5-10 preguntas]

---

## 🎮 Mini Reto

**Objetivo**: Aplicar lo aprendido en contexto diferente

**Descripción**:
[Describe el reto con contexto diferente del lab pero mismo concepto]

**Requisitos**:
1. [Requisito 1]
2. [Requisito 2]
3. [Requisito 3]

**Validación**:
[Cómo verificas que completaste el reto exitosamente]

**Solución** (ver en `solutions/`)

---

## 🔗 Referencias & Documentación

- [AWS Official Doc - Tema](https://docs.aws.amazon.com/...)
- [Whitepaper: Nombre](https://aws.amazon.com/whitepapers/...)
- [Blog post relacionado](https://aws.amazon.com/blogs/...)
- [Video tutorial](https://www.youtube.com/watch?v=...)

---

## 📁 Archivos de Este Módulo

```
NZ-nombre-tema/
├─ README.md                    # Este archivo
├─ labs/
│  ├─ README-lab.md            # Instrucciones detalladas
│  ├─ evidencias-checklist.md  # Qué capturar
│  ├─ cleanup.sh               # Limpieza automática
│  └─ assets/
│     ├─ template.json
│     └─ [otros archivos]
└─ notes-personales.md         # Tu documentación (no commitear)
```

---

## ✅ Checklist de Cierre

Antes de pasar al próximo módulo:

- [ ] Objetivo final logrado (todos ✅)
- [ ] Conceptos clave entendidos (puedes explicar)
- [ ] Lab completado sin errores
- [ ] Todas evidencias capturadas
- [ ] Preguntas respondidas correctamente
- [ ] Mini reto completado
- [ ] Cleanup script ejecutado
- [ ] No hay recursos AWS en "stopped" state
- [ ] Documentación personal guardada
- [ ] Budget AWS dentro del límite

---

## 🧹 Cleanup (MUY IMPORTANTE)

Ejecutar al fin del módulo para evitar costos:

```bash
#!/bin/bash
# cleanup.sh - Elimina todos los recursos

echo "🧹 Limpiando recursos de [módulo]..."

# [Comando 1 para eliminar recurso]
# [Comando 2 para eliminar recurso]
# ... (ver archivo cleanup.sh completo)

echo "✅ Limpieza completada. Costo total: $X"
```

**Después de ejecutar**:
```bash
# Verifica que no hay recursos "stranded"
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --profile bootcamp
aws s3 ls --profile bootcamp
aws rds describe-db-instances --query 'DBInstances[].DBInstanceIdentifier' --profile bootcamp
```

---

## 🆘 Troubleshooting

### Problema: [Error común 1]
**Solución**: [Steps para resolver]

### Problema: [Error común 2]
**Solución**: [Steps para resolver]

---

## 📊 Resumen de Aprendizaje

### Completaste:
- ✅ [Concepto mastered]
- ✅ [Skill mastered]
- ✅ [Práctica realizada]

### Siguiente:
👉 [Link al próximo módulo]

---

## 💬 Feedback

¿Algo no funcionó? Abre un Issue:
- Describe el problema
- Incluye output de error
- Menciona tu región AWS

---

**Versión**: 1.0  
**Última actualización**: [fecha]  
**Mantenido por**: [nombre]  
**Dificultad**: ⭐⭐ de 5
