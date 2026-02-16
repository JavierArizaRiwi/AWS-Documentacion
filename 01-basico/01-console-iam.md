# 01-Básico: AWS Console & IAM (Identity and Access Management)

**Duración**: 4 horas (2h concepto + 2h lab)  
**Requisitos**: Completar [00-Onboarding](../00-onboarding/README.md)  
**Objetivo**: Dominar IAM para seguridad + acceso en AWS

---

##  Objetivo Final

Al terminar:
-  Entender modelo de IAM (Users, Roles, Policies)
-  Crear usuarios con permisos específicos
-  Aplicar principio de least privilege
-  Activar MFA en usuarios
-  Crear roles para servicios (EC2, Lambda)
-  Usar policy simulator para tester permisos

---

##  Conceptos Clave

1. **Root Account**: Acceso total, nunca para desarrollo
2. **IAM User**: Persona o aplicación que accede AWS
3. **IAM Role**: Set de permisos asignables a usuarios/servicios
4. **IAM Policy**: Documento JSON que define permisos (Allow/Deny)
5. **Policy Document**: 
   - Effect: Allow o Deny
   - Action: qué se permite (s3:GetObject, ec2:RunInstances)
   - Resource: sobre qué (arn:aws:s3:::bucket/*)
6. **Least Privilege**: Usuario/role = mínimos permisos necesarios
7. **MFA**: Factor adicional de autenticación
8. **Access Keys**: Credenciales programáticas (API/CLI)
9. **Temporary Security Credentials**: STS roles (expiran)
10. **Principal**: Usuario, role, o servicio que realiza acción

---

## 🛠️ Hands-on: IAM Lab

### PASO 1: Explorar AWS Console (20 min)

#### Navegación por Consola
1. Login como `bootcamp-user`
2. Visitar **IAM Dashboard**:
   - Security status: Revisar items (debe haber MFA ya )
   - Users list
   - Roles
   - Policies

#### Estructura de IAM
```
AWS Account (ROOT)
├── Users
│   ├── bootcamp-user (ya existe)
│   ├── dev-engineer (crear aquí)
│   └── qa-engineer (crear aquí)
├── Roles
│   ├── EC2DefaultRole
│   ├── LambdaExecutionRole
│   └── S3AccessRole
├── Policies (Managed)
│   ├── AWS Managed (AmazonS3ReadOnlyAccess)
│   └── Customer Managed (crear aquí)
└── Groups (organizador de usuarios)
```

---

### PASO 2: Crear IAM User (15 min)

**Crear usuario para desarrollo**

En AWS Console → IAM → Users → Create user

```
User name: dev-engineer
Console access:  Autogenerada
Require password change: 
MFA requirement:  (después)
```

**Asignar Permissions** (seleccionar una):
- [ ] AdministratorAccess (NO para prod)
- [x] PowerUserAccess (sin IAM changes)
- [ ] Custom policy (más específico)

**Result**: Guardar en archivo seguro
```
Console sign-in URL: https://123456789012.signin.aws.amazon.com/console
Temporary password: [...]
Username: dev-engineer
```

---

### PASO 3: Crear Custom Policy (20 min)

**Política personalizada** para restringir S3 a bucket específico

En AWS Console → IAM → Policies → Create policy

**Usar JSON editor**:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3ListBucket",
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetBucketLocation"
      ],
      "Resource": "arn:aws:s3:::bootcamp-lab-*"
    },
    {
      "Sid": "S3GetPutObject",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::bootcamp-lab-*/*"
    },
    {
      "Sid": "CloudWatchLogs",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:*:*:*"
    },
    {
      "Sid": "EC2ReadOnly",
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*"
      ],
      "Resource": "*"
    },
    {
      "Sid": "DenyDeleteBucket",
      "Effect": "Deny",
      "Action": "s3:DeleteBucket",
      "Resource": "*"
    }
  ]
}
```

**Guardar as**: `S3-Dev-Policy`

---

### PASO 4: Crear IAM Group (10 min)

**Organizar múltiples usuarios**

IAM → Groups → Create group

```
Group name: developers
Attach policy: S3-Dev-Policy (creada arriba)
Add users: dev-engineer
```

Ventaja: Cambiar política en grupo = todos los users actualizados

---

### PASO 5: Crear IAM Role para EC2 (15 min)

**Los roles son para SERVICIOS, no usuarios**

IAM → Roles → Create role

```
Use case: EC2
Policy: S3-Dev-Policy
Role name: EC2-S3-Access
```

Resultado:
```
Role ARN: arn:aws:iam::123456789012:role/EC2-S3-Access
Trust relationship: 
{
  "Principal": {
    "Service": "ec2.amazonaws.com"
  },
  "Effect": "Allow"
}
```

---

### PASO 6: Activar MFA para Usuario (10 min)

**Asegurar dev-engineer con MFA**

IAM → Users → dev-engineer → Security credentials

1. Assigned MFA device: Assign MFA
2. Device type: Autenticador (Google Authenticator/Authy)
3. Escanear QR
4. Ingresar 2 códigos consecutivos

**Validación**:
- Intentar login sin código MFA → falla
- Con código → éxito

---

### PASO 7: Generar Access Keys (5 min)

**Para CLI/API access**

IAM → Users → dev-engineer → Security credentials

```
Access key ID: AKIA...
Secret access key: [descarga CSV]
```

**CLI CONFIG**:
```bash
aws configure --profile dev

AWS Access Key ID: AKIA...
AWS Secret Access Key: ...
Default region: us-east-1
Default output format: json
```

**Test**:
```bash
# Esto debería funcionar
aws s3 ls --profile dev

# Esto debería fallar (no permiso EC2)
aws ec2 describe-instances --profile dev  # Error: UnauthorizedOperation
```

---

### PASO 8: Policy Simulator (10 min)

**Testear políticas sin riesgo**

IAM → Policies → [S3-Dev-Policy] → Policy simulator

**Test Case 1**: ¿Puede dev-engineer acceder s3:GetObject?
```
User: dev-engineer
Action: s3:GetObject
Resource: arn:aws:s3:::bootcamp-lab-images/photo.jpg
```
Resultado:  ALLOWED

**Test Case 2**: ¿Puede dev-engineer borrar bucket?
```
User: dev-engineer
Action: s3:DeleteBucket
Resource: arn:aws:s3:::bootcamp-lab-*
```
Resultado:  DENIED (por Deny explícito)

---

### PASO 9: AWS IAM Policy Variables (10 min)

**Políticas dinámicas usando variables**

Crear policy con variables:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3BucketUserHome",
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetBucketLocation"
      ],
      "Resource": "arn:aws:s3:::bootcamp-home-${aws:username}",
      "Condition": {
        "StringEquals": {
          "s3:prefix": [
            "${aws:username}/",
            "${aws:username}/*"
          ]
        }
      }
    }
  ]
}
```

Resultado: Usuario `dev-engineer` solo ve su carpeta `dev-engineer/` en S3

---

### PASO 10: Crear Usuario de Solo-Lectura (5 min)

**Usuario para QA/Monitoring**

IAM → Users → Create user

```
Username: qa-engineer
Console access: 
MFA: 
Policies: ReadOnlyAccess (AWS managed)
```

**Validación**:
```bash
# Puede ver, no puede modificar
aws ec2 describe-instances --profile qa

# Esto falla
aws ec2 terminate-instances --instance-ids i-123 --profile qa  # Error
```

---

##  Evidencias Esperadas

**Screenshot 1**: IAM Dashboard mostrando 3 usuarios
- bootcamp-user
- dev-engineer  
- qa-engineer

**Screenshot 2**: IAM Group "developers" con policy S3-Dev-Policy

**Screenshot 3**: Role "EC2-S3-Access" creado

**Screenshot 4**: Policy Simulator resultado de test

**Screenshot 5**: Terminal mostrando:
```bash
$ aws s3 ls --profile dev
2026-02-16 10:30:00 bootcamp-lab-images

$ aws ec2 describe-instances --profile dev
An error occurred (UnauthorizedOperation)
```

---

##  Preguntas de Validación

1. ¿Cuál es la diferencia entre usuario IAM y rol IAM?
2. ¿Qué principio de seguridad aplicas al crear políticas IAM?
3. ¿Por qué NO debe un usuario tener AdministratorAccess en desarrollo?
4. ¿Cómo especificas un recurso en una policy IAM usando ARN?
5. ¿Qué es `${aws:username}` en una política y por qué es útil?
6. ¿En qué caso usarías STS AssumeRole?
7. ¿Cómo verificarías si un usuario tiene permisos suficientes?
8. ¿Qué pasa si tienes Allow + Deny explícitos en la misma acción?

---

##  Mini Reto

**Reto**: Crear política IAM restrictiva para DynamoDB

**Requisitos**:
- Usuario `ddb-developer` puede hacer CRUD en tabla `users`
- NO puede acceder otras tablas
- NO puede borrar tabla
- NO puede modificar backups
- SÍ puede ver CloudWatch logs

**Pasos**:
1. Crear policy con DynamoDB actions limitadas
2. Crear usuario ddb-developer
3. Attach policy
4. Testear con Policy Simulator

**Policy base**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetItem",
        "dynamodb:Query",
        "dynamodb:Scan",
        "dynamodb:PutItem",
        "dynamodb:UpdateItem",
        "dynamodb:BatchGetItem",
        "dynamodb:BatchWriteItem"
      ],
      "Resource": "arn:aws:dynamodb:*:*:table/users"
    },
    {
      "Effect": "Deny",
      "Action": [
        "dynamodb:DeleteTable",
        "dynamodb:UpdateTable",
        "dynamodb:CreateBackup"
      ],
      "Resource": "*"
    }
  ]
}
```

---

##  Checklist de Cierre

- [ ] Acceso AWS Console con bootcamp-user + MFA funcionando
- [ ] 3 usuarios IAM creados: bootcamp-user, dev-engineer, qa-engineer
- [ ] Grupo "developers" creado y políticas aplicadas
- [ ] Policy personalizada "S3-Dev-Policy" creada
- [ ] Role "EC2-S3-Access" creado y confianza EC2 verificada
- [ ] MFA activado en dev-engineer
- [ ] Access keys generadas y CLI configurado con profiles
- [ ] Policy Simulator usado y tests pasados
- [ ] CLI tests ejecutados (s3 ls éxito, ec2 fail esperado)
- [ ] Política con variables `${aws:username}` entendida
- [ ] Mini reto DynamoDB completado
- [ ] Documentación guardada en `notes/iam-learnings.md`

---

##  Cleanup (NO hacer aún - los usaremos)

```bash
# NO borrar todavía:
# - Usuarios (los usaremos en labs)
# - Roles (necesarios para EC2)
# - Políticas (base para otros módulos)

# Al final del BOOTCAMP:
# - IAM → Users → Delete dev-engineer, qa-engineer
# - IAM → Roles → Delete EC2-S3-Access
# - IAM → Policies → Delete custom policies
# - IAM → Groups → Delete developers
```

---

##  Recursos

- [IAM User Guide](https://docs.aws.amazon.com/iam/)
- [IAM Policy Examples](https://docs.aws.amazon.com/iam/latest/userguide/access_policies_examples.html)
- [ARN Reference](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)
- [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [Policy Simulator](https://policysim.aws.amazon.com/)

---

## Archivos del Lab

```
01-basico/
├── labs/
│   ├── 01-iam-lab/
│   │   ├── iam-setup.sh           # Script para setup
│   │   ├── s3-dev-policy.json     # Política S3
│   │   ├── ddb-policy.json        # Reto DynamoDB
│   │   └── README.md              # Este archivo
│   └── ...
```

---

**Siguiente**: [02-Redes Básicas (VPC)](/01-basico/02-redes-basicas.md)
