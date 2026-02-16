# Guía de Contribución - AWS Bootcamp Coders

¡Gracias por considerar contribuir a este proyecto! 🙌

## Cómo Contribuir

### 1. Reportar Bugs o Sugerencias

Abre un **Issue** con:
- Descripción clara del problema
- Contexto (región, servicio, paso donde falló)
- Pasos para reproducir
- Evidencias (screenshots, logs)

### 2. Mejorar Contenido

#### Para Módulos/Temas:
1. Fork del repo
2. Crea rama: `git checkout -b feat/tema-mejorado`
3. Sigue [PLANTILLA-README-TEMA.md](docs/PLANTILLA-README-TEMA.md)
4. Incluye:
   - Conceptos actualizados
   - Labs testeados en consola
   - Screenshots de evidencias
   - Preguntas de validación
5. Commit + Push + PR

#### Para Labs:
- Sigue template en [lab-templates/README-lab.md](labs/lab-templates/README-lab.md)
- Incluye cleanup script
- Testea que funciona (sin costos)

#### Para Proyectos:
- Revisa [proyectos/README.md](proyectos/README.md)
- Proporciona código runnable
- Rúbrica clara

### 3. Estilo & Formato

**Markdown**:
- Headers: `# Nivel 1`, `## Nivel 2`, etc.
- Listas con `-` para bullets, `1.` para numeradas
- `Inline code` con backticks
- Code blocks con triple backticks + language

**Comandos**:
```bash
# Usa bash, no fish/zsh
# Comenta lo que hace
aws s3 ls --profile bootcamp
```

**JSON/YAML**:
- Indenta con 2 espacios
- Comenta con `//` o `#` arriba de la sección

### 4. Convenciones de Nombres

**Archivos**:
- `NN-tema-descripcion.md` (00-onboarding, 01-iam, etc.)
- `kebab-case` para nombres
- Números secuenciales para orden

**Ramas**:
- `fix/descripcion-corta`
- `feat/descripcion-corta`
- `docs/descripcion-corta`
- `chore/descripcion-corta`

**Commits**:
```
feat: agregar lab de EC2 con Auto Scaling
fix: corregir ARN en política de S3
docs: actualizar roadmap con nuevos tópicos
chore: reorganizar estructura de carpetas
```

### 5. Checklist Antes de PR

- [ ] Contenido testeado (labs funcionan sin errores)
- [ ] Sin credenciales o datos sensibles
- [ ] Markdown bien formateado
- [ ] Links internos verificados
- [ ] Screenshots claras (sin PII)
- [ ] Cleanup scripts incluidos
- [ ] Estimación de tiempo precisa

### 6. Proceso de Review

1. Al menos 1 revisor
2. Feedback constructivo
3. Cambios solicitados = ajustas + push
4. Aprobación = merge a `main`
5. Publicación en documentación

## Estructura de Contribución por Tipo

### Agregar Nuevo Tema
```
01-basico/
  ├─ NN-tema-nuevo.md      # Contenido
  └─ labs/NN-tema-nuevo/   # Lab asociado
```

### Agregar Lab Nuevo
```
labs/
  ├─ [nivel]/
  │  └─ NN-lab-nuevo/
  │     ├─ README.md                  # Instrucciones
  │     ├─ evidencias-checklist.md   # Screenshots esperadas
  │     ├─ cleanup.sh                 # Script de limpieza
  │     └─ assets/                    # Plantillas, JSON, etc.
```

### Agregar Proyecto
```
proyectos/
  ├─ proyecto-N-nombre/
  │  ├─ README.md                  # Descripción
  │  ├─ criterios-aceptacion.md   # Rúbrica
  │  ├─ entregables.md            # Qué entregar
  │  ├─ starter-code/             # Template
  │  └─ solution/                 # Solución (privado)
```

## Versioning

Usamos **Semantic Versioning** (MAJOR.MINOR.PATCH):
- **v1.0.0**: Lanzamiento inicial (6 semanas structure)
- **v1.1.0**: Nuevos módulos o mejoras grandes
- **v1.1.1**: Bugfixes o correcciones menores

## Código de Conducta

### ✅ Esperamos:
- Respeto y profesionalismo
- Retroalimentación constructiva
- Disposición a aprender
- Documentación clara

### ❌ No toleramos:
- Spam o auto-promoción sin valor
- Comentarios ofensivos
- Compartir credenciales o datos sensibles
- Plagio sin atribución

## Preguntas?

- Abre un **Discussion** en GitHub
- Revisa [GLOSARIO.md](docs/GLOSARIO.md) para terminología
- Consulta [ROADMAP.md](docs/ROADMAP.md) para futuras mejoras

---

**¡Gracias por hacer este bootcamp cada vez mejor! 🚀**
