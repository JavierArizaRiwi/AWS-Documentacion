# 00 - Onboarding: Setup AWS + Fundamentos

**Duración**: 2 horas  
**Requisitos previos**: AWS account con acceso a billing  
**Objetivo**: Listar el ambiente + configurar credenciales + entender modelo de costos

---

## Objetivo Final

Al completar este módulo:
- [OK] AWS CLI v2 instalado y configurado
- [OK] Credenciales de acceso seguras (IAM user + API keys)
- [OK] MFA activado en root account
- [OK] Billing alerts configuradas
- [OK] VPC por defecto revisada
- [OK] Entendimiento básico de regiones/AZs

---

## Conceptos Clave

1. **AWS Account**: Contenedor de todos los recursos
2. **Regiones**: Áreas geográficas (us-east-1, eu-west-1, etc.)
3. **Availability Zones (AZs)**: Data centers dentro de una región
4. **IAM User vs Root**: Root = acceso total, IAM = acceso limitado
5. **Access Keys**: API key + Secret (nunca compartir)
6. **MFA**: Multi-Factor Authentication para seguridad
7. **Billing**: Modelo pay-as-you-go
8. **Free Tier**: 12 meses gratis con límites
9. **CloudTrail**: Auditoría de acciones
10. **Cost Explorer**: Ver gastos por servicio

---

## Hands-on

### TAREA 1: Crear Estructura de Carpetas Local (5 min)

```bash
# Crear carpeta principal
mkdir -p ~/AWS-Bootcamp/{credentials,scripts,labs,notes}
cd ~/AWS-Bootcamp
git init
echo "# AWS Bootcamp - Notas Personales" > notes/README.md
```

**Guardar**: `.gitignore` con credenciales

```bash
cat > .gitignore << 'EOF'
# No commitar credenciales
.aws/
*.pem
*.key
*.pfx
credentials.txt
.env
.env.local

# Dependencias
node_modules/
__pycache__/
*.pyc
venv/

# Logs
*.log
EOF
```

---

### TAREA 2: Instalar AWS CLI v2 (10 min)

#### En Linux/macOS:
```bash
# Descargar
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip

# Instalar
sudo ./aws/install

# Verificar
aws --version
```

#### En Windows:
```powershell
# Descargar MSI desde: https://awscli.amazonaws.com/AWSCLIV2.msi
```

---

### TAREA 3: Crear IAM User para Desarrollo (10 min)

**Abrir AWS Console** → IAM → Users → Create user

1. **User name**: `bootcamp-user`
2. **Enable console access**: [OK] (contraseña autogenerada)
3. **Attach policy**: `AdministratorAccess` (temporal, solo bootcamp)
4. **Enable MFA**: [OK] Autenticador (Google Authenticator, Microsoft Authenticator)

**Resultado**: Guardar:
```
User ARN: arn:aws:iam::123456789012:user/bootcamp-user
Console URL: https://123456789012.signin.aws.amazon.com/console
```

---

### TAREA 4: Generar Access Keys (5 min)

En AWS Console:
1. Login como `bootcamp-user`
2. IAM → Users → bootcamp-user → Security credentials
3. Create access key → Download CSV
4. **NUNCA compartir** estas keys

Guardar en archivo seguro (no Git):
```bash
cat > ~/.aws/credentials << 'EOF'
[default]
aws_access_key_id = AKIA...
aws_secret_access_key = ...

[bootcamp]
aws_access_key_id = AKIA...
aws_secret_access_key = ...
EOF

chmod 600 ~/.aws/credentials
```

---

### TAREA 5: Configurar AWS CLI (5 min)

```bash
aws configure --profile bootcamp
```

Responder:
```
AWS Access Key ID: [AKIA...]
AWS Secret Access Key: [...]
Default region: us-east-1
Default output format: json
```

**Verificar**:
```bash
aws sts get-caller-identity --profile bootcamp
```

Resultado esperado:
```json
{
  "UserId": "AIDA...",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/bootcamp-user"
}
```

---

### TAREA 6: Activar MFA en Root Account (5 min)

**IMPORTANTE**: Aplicar MFA al root por seguridad

1. Logout bootcamp-user
2. Login como root (email + password)
3. Account → Security credentials → MFA device → Activate
4. Escanear QR con autenticador (Google Auth, Authy, etc.)
5. Ingresar 2 códigos consecutivos
6. **Guardar códigos de backup de forma segura**

---

### TAREA 7: Configurar Billing Alerts (5 min)

AWS Console → Billing and Cost Management → Budgets

1. Create budget
2. Budget type: Spend
3. Set limit: $50 (máximo)
4. Alert threshold: 80%
5. Email de notificación: [Tu email]

**Verificar**: Console → Cost Explorer (debería mostrar $0)

---

### TAREA 8: Verificar Regiones por Defecto (3 min)

```bash
# Listar regiones habilitadas
aws ec2 describe-regions --profile bootcamp

# Setear región por defecto
export AWS_REGION=us-east-1
echo "export AWS_REGION=us-east-1" >> ~/.bashrc
```

VPC por defecto debe existir en cada región:
```bash
aws ec2 describe-vpcs --profile bootcamp \
  --filters "Name=isDefault,Values=true"
```

---

### TAREA 9: Habilitar CloudTrail (5 min)

AWS Console → CloudTrail → Trails → Create trail

1. Trail name: `bootcamp-audit`
2. S3 bucket: `bootcamp-audit-<account-id>`
3. CloudWatch Logs: [OK] Crear nuevo grupo
4. Enable for all regions: [OK]

**Propósito**: Auditar todas las acciones (cumplimiento + debugging)

---

### TAREA 10: Crear Script de Verificación (5 min)

Guardar en `scripts/verify-setup.sh`:

```bash
#!/bin/bash
set -e

echo " AWS Bootcamp - Setup Verification"
echo "======================================"
echo ""

# AWS CLI
echo " AWS CLI:"
aws --version

# Credenciales
echo ""
echo " AWS Identity:"
aws sts get-caller-identity --profile bootcamp

# Regiones disponibles
echo ""
echo " Regiones:"
aws ec2 describe-regions --query 'Regions[].RegionName' --output table --profile bootcamp

# VPC por defecto
echo ""
echo " VPC Default:"
aws ec2 describe-vpcs --filters "Name=isDefault,Values=true" \
  --query 'Vpcs[].VpcId' --output text --profile bootcamp

# Costos actuales (últimas 24h)
echo ""
echo " Gastos (últimas 24h):"
aws ce get-cost-and-usage \
  --time-period Start=$(date -d '1 day ago' +%Y-%m-%d),End=$(date +%Y-%m-%d) \
  --granularity DAILY \
  --metrics "UnblendedCost" \
  --query 'ResultsByTime[*].[TimePeriod.Start,Total.UnblendedCost.Amount]' \
  --output table --profile bootcamp || echo "N/A"

echo ""
echo " Setup completado!"
```

Ejecutar:
```bash
chmod +x scripts/verify-setup.sh
./scripts/verify-setup.sh
```

---

## Evidencias Esperadas

**Captura 1**: Output de `aws sts get-caller-identity`
```
UserId: ........
Arn: arn:aws:iam::123456789012:user/bootcamp-user
```

**Captura 2**: AWS Console → Billing Alerts (budget $50 creado)

**Captura 3**: AWS Console → CloudTrail (trail activo)

**Captura 4**: Terminal → `./scripts/verify-setup.sh` (todas verificaciones OK)

---

## Preguntas de Validación

1. ¿Cuál es la diferencia entre AWS Account root y IAM user?
2. ¿Por qué no deberías usar access keys del root?
3. ¿Cuántas AZs tiene una región típicamente?
4. ¿Qué es el Free Tier y cuánto dura?
5. ¿Cómo verificarías que tienes MFA activado?
6. ¿Qué servicio audita todas las acciones AWS?
7. ¿En qué región deberías crear recursos para lab si estás en LatAm?
8. ¿Cómo resetearías una contraseña de IAM user?

---

## Mini Reto

**Reto**: Crear un segundo IAM user `bootcamp-readonly` con acceso solo-lectura

**Pasos**:
1. Crear IAM user sin MFA por ahora
2. Attach policy: `ReadOnlyAccess`
3. Generar access keys
4. Configurar en AWS CLI como profile `bootcamp-readonly`
5. Intentar crear un EC2 security group (debe fallar por permisos)

**Validación**:
```bash
# Esto debería fallar con "UnauthorizedOperation"
aws ec2 create-security-group --group-name test \
  --description "Test" --profile bootcamp-readonly
```

---

## Checklist de Cierre

- [ ] AWS CLI v2 instalado y verificado
- [ ] IAM user `bootcamp-user` creado con console + API access
- [ ] MFA activado en root + IAM user
- [ ] Access keys guardados en `~/.aws/credentials` (NO en Git)
- [ ] `.gitignore` contiene credenciales
- [ ] AWS CLI perfiles configurados (`default` + `bootcamp`)
- [ ] Billing alerts configuradas ($50 budget)
- [ ] CloudTrail trail activo
- [ ] Script `verify-setup.sh` ejecutado exitosamente
- [ ] VPC por defecto verificada en us-east-1
- [ ] Documentación inicial en `notes/README.md`

---

## Cleanup (al final del bootcamp)

```bash
# No eliminar credenciales todavía, las usaremos todo el curso
# Pero sí apunta si cambiarías region por defecto a destruir más fácil

# Para NO completar AÚN:
# - AWS account (lo usaremos 6 semanas)
# - IAM users (los usaremos)
# - CloudTrail (lo dejaremos activo)
```

---

## Recursos

- [AWS Getting Started](https://aws.amazon.com/getting-started/)
- [AWS CLI Reference](https://docs.aws.amazon.com/cli/latest/userguide/)
- [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [AWS Free Tier](https://aws.amazon.com/free/)

---

**Siguiente**: [01-Console & IAM](/01-basico/01-console-iam.md)
