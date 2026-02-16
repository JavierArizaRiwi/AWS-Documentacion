# ✅ Checklist de Revisión - Templates Internos

## Checklist para MÓDULOS/TEMAS

Usá esto como QA antes de hacer PR

### Contenido
- [ ] Objetivo claro al inicio
- [ ] 8-10 conceptos clave (máximo)
- [ ] Hands-on pasos numerados
- [ ] Screenshots/evidencias claras
- [ ] Preguntas de validación (5-10)
- [ ] Mini reto con solución
- [ ] Checklist de cierre
- [ ] Cleanup script proporcionado
- [ ] Links internos verificados

### Formato
- [ ] Headers jerarquizados (# ## ### solo)
- [ ] Markdown válido (sin errores)
- [ ] Código con syntax highlighting
- [ ] Tablas bien formateadas
- [ ] Sin URLs rotas (curl test si externa)
- [ ] Emojis apropiados (no excesivos)

### Seguridad
- [ ] Sin credenciales o secrets expuestos
- [ ] Sin datos personales (PII)
- [ ] ARNs son placeholders (123456789012)
- [ ] SSH keys no son ejemplos reales

### Pruebas
- [ ] Lab ejecutado sin errores (free tier compatible)
- [ ] Comandos protos (copy-paste lista)
- [ ] Cleanup comprobado
- [ ] Costo estimado < $5

---

## Checklist para LABS

Estructura esperada:

```
labs/[nivel]/NN-nombre-lab/
├─ README.md                    # Instrucciones
├─ evidencias-checklist.md     # Screenshots esperadas
├─ cleanup.sh                   # Script limpieza
├─ assets/                      # Plantillas, JSON, etc
│  ├─ template.json
│  ├─ policy.json
│  └─ [otros archivos]
└─ solutions/                   # (Privado, si aplica)
```

### Pre-Lab Review
- [ ] Prerequisitos claros
- [ ] Tiempo estimado preciso (±10 min)
- [ ] Costo total estimado
- [ ] Permisos IAM requeridos documentados
- [ ] Hardware mínimo requerido

### Lab Review
- [ ] Instrucciones paso-a-paso
- [ ] Salidas esperadas especificadas
- [ ] Troubleshooting section
- [ ] Alternativas si un servicio falla

### Post-Lab Review
- [ ] Evidencias esperadas documentadas
- [ ] Screenshots guía de qué capturar
- [ ] Checklist de validación
- [ ] Cleanup script testeado

---

## Checklist para PROYECTOS

```
proyectos/proyecto-N-nombre/
├─ README.md
├─ criterios-aceptacion.md
├─ entregables.md
├─ starter-code/     # Código base (opcional)
└─ solution/         # Solución completa (privado)
```

### Especificación
- [ ] Descripción ejecutiva clara
- [ ] Objetivos de aprendizaje (3-5)
- [ ] Tecnologías requeridas
- [ ] Requisitos funcionales
- [ ] Requisitos no-funcionales (seguridad, perf)
- [ ] Timeline esperado
- [ ] Budget AWS estimado

### Rúbrica
- [ ] Criterios de éxito claros (3-5)
- [ ] Niveles: Novato, Intermedio, Experto
- [ ] Puntuación total: 100 pts
- [ ] Rubric detallada en criterios-aceptacion.md

### Entregables
- [ ] Listado de qué entregar
- [ ] Formato de presentación
- [ ] Dónde hostear código (repo, gist, etc)
- [ ] Presentación requerida (video/slides)

---

## Checklist para RÚBRICAS

### Estructura General
```
rubrica-[tipo].md

Criterio 1: Descripción (X pts)
  - Novato (0-3pts): descripción
  - Intermedio (4-6pts): descripción
  - Avanzado (7-10pts): descripción

Criterio 2: ... (similar)
```

### Contenido
- [ ] 5-8 criterios por rúbrica
- [ ] Total 100 puntos
- [ ] Descripciones específicas (no vago)
- [ ] Ejemplos de "no pasa" vs "pasa"
- [ ] Pesos por criterio documentados

---

## Checklist de Calidad General

### Ortografía & Gramática
- [ ] Spell-check (usar `code-spell-checker` en VSCode)
- [ ] Consistencia: "AWS" no "aws", "EC2" no "ec2"
- [ ] Tildes correctas (español)
- [ ] Puntuación coherente

### Tono & Voz
- [ ] Profesional pero accesible
- [ ] Coaching positivo (no "no hagas esto")
- [ ] Explicativo no prescriptivo
- [ ] Inclusivo (no gendered language)

### Accesibilidad
- [ ] Imágenes con alt-text descriptivo
- [ ] Colores: no depender solo de color
- [ ] Jerarquía clara (headers)
- [ ] Párrafos cortos (máx 3-4 líneas)

---

## Template de Reviewers

Si reviewas un PR:

```markdown
## Review Checklist

- [ ] Contenido técnicamente correcto
- [ ] Markdown bien formateado
- [ ] Sin typos/gramática
- [ ] Lab es reproducible
- [ ] Cleanup script testado
- [ ] Costo dentro budget
- [ ] Aprueba ✅

**Comentarios:**
[aquí tus comentarios]

**Bloqueadores (si hay):**
[necesario para merge]
```

---

**Actualizado**: 16 Feb 2026  
**Mantenido por**: QA Team
