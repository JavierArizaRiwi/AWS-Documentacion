# 💰 Guía de Costos y Cleanup - Evita Sorpresas en la Factura

## 🎯 Estimación de Costos - Bootcamp Completo

### Breakdown por Servicio (6 semanas)

| Servicio | Uso Mensual | Costo/Mes | Free Tier | Costo Real |
|----------|-------------|-----------|-----------|-----------|
| EC2 | 60h/mes | $10 | 750h/mes | ✅ $0 |
| S3 | 5GB storage | $0.12 | 5GB | ✅ $0 |
| RDS | 1 instance (750h) | $30 | 750h/mes | ✅ $0 |
| DynamoDB | On-demand dev | $5 | First 25RU | ~$0-2 |
| Lambda | 1M calls | $0.20 | 1M calls | ✅ $0 |
| API Gateway | 1M requests | $3.5 | 1M/mes | ✅ $0 |
| Data Transfer | 10GB out | $1 | 1GB (limited) | ~$1 |
| NAT Gateway | Fixed | $45 | ❌ | ~$15 (si se usa) |
| **TOTAL (6 semanas)** | - | - | - | **~$20-30** |

### ✅ Con Free Tier Bien Usado: $0-10

❌ **RIESGO**: Si no limpias NAT Gateway → $45/mes extra

---

## 🚨 Servicios Costosos (Evitar en Bootcamp)

| Servicio | Razón | Costo/Hora | Alternativa |
|----------|-------|-----------|---|
| EC2 `c5.4xlarge` | Instancia grande | $0.68 | Usar `t3.micro` ($0.0104) |
| NAT Gateway | Always-on | $45/mes + datos | Usar nat-instance ($0.02/h) |
| Elastic IP unused | IP sin usar | $0.005/h | Reasignar o liberar |
| Data Transfer (In) | Más de 1GB/mes | $0.02/GB | Minimizar |
| EBS IO (Provisioned) | io1/io2 | $0.10/GB/mes | gp3 ($0.10) o gp2 ($0.05) |
| RDS Multi-AZ | Backup remoto | 2x el costo | Single-AZ para dev |

---

## ✅ Billing Alerts - Setup Automático

### En AWS Console

```markdown
Billing and Cost Management > Budgets > Create Budget

Budget Type: Spend
Period: Monthly
Budget Amount: $50
Alert Threshold: 80% ($40)
Email: tu@email.com
```

### Verificar Gastos Actuales

```bash
# Terminal
aws ce get-cost-and-usage \
  --time-period Start=2026-02-01,End=2026-02-16 \
  --granularity MONTHLY \
  --metrics "UnblendedCost" \
  --group-by Type=DIMENSION,Key=SERVICE \
  --output table --profile bootcamp
```

### Cost Anomaly Detection

```markdown
Billing > Cost Anomaly Detection > Awareness
→ Te notifica si gasto aumenta anormalmente
```

---

## 🧹 Cleanup Checklist - Por Nivel

### DIARIAMENTE (5 min)

- [ ] Revisar Cost Explorer
- [ ] Confirmar ninguna instancia EC2 en estado "stopped"

### AL FIN DE CADA LAB (10 min)

```bash
#!/bin/bash
# cleanup-lab.sh

echo "🧹 Limpiando recursos..."

# EC2
aws ec2 terminate-instances --instance-ids i-xxx --profile bootcamp

# Security Groups (wait 1 min after EC2 terminate)
sleep 60
aws ec2 delete-security-group --group-id sg-xxx --profile bootcamp

# RDS
aws rds delete-db-instance --db-instance-identifier bootcamp-db \
  --skip-final-snapshot --profile bootcamp

# S3 (vaciá primero)
aws s3 rm s3://bucketname --recursive --profile bootcamp
aws s3 rb s3://bucketname --profile bootcamp

# DynamoDB
aws dynamodb delete-table --table-name tablename --profile bootcamp

# CloudWatch Logs (opcional, costo mínimo)
aws logs delete-log-group --log-group-name /aws/lambda/function-name --profile bootcamp

# NAT Gateway (IMPORTANTE: liberar Elastic IP)
aws ec2 release-address --allocation-id eipalloc-xxx --profile bootcamp

# VPC (si creaste nueva)
aws ec2 delete-vpc --vpc-id vpc-xxx --profile bootcamp

echo "✅ Cleanup completado"
```

### AL FIN DE CADA SEMANA (30 min)

1. **Revisar recursos creados**:
```bash
aws ec2 describe-instances --query 'Reservations[].Instances[].{ID:InstanceId,State:State.Name,Type:InstanceType}' --output table --profile bootcamp
```

2. **Terminar todas las instancias**:
```bash
aws ec2 describe-instances --query 'Reservations[].Instances[].InstanceId' --output text --profile bootcamp | xargs -I {} aws ec2 terminate-instances --instance-ids {} --profile bootcamp
```

3. **Eliminar volúmenes orphaned**:
```bash
aws ec2 describe-volumes --filters "Name=status,Values=available" --query 'Volumes[].VolumeId' --output text --profile bootcamp | xargs -I {} aws ec2 delete-volume --volume-id {} --profile bootcamp
```

4. **Vaciar S3 buckets**:
```bash
# List buckets
aws s3 ls --profile bootcamp

# Empty each
aws s3 rm s3://bucket-name --recursive --profile bootcamp
aws s3 rb s3://bucket-name --profile bootcamp  # Delete
```

5. **Verificar snapshot y volumes sin usar**:
```bash
aws ec2 describe-snapshots --owner-ids self --query 'Snapshots[].{ID:SnapshotId,Size:VolumeSize,Start:StartTime}' --output table --profile bootcamp
```

### AL FINAL DEL BOOTCAMP (1 hora)

**LIMPIEZA MASIVA**:

```bash
#!/bin/bash
# full-cleanup.sh - CUIDADO: Elimina TODO

aws_profile="bootcamp"

echo "🚨 CLEANUP TOTAL - Esto elimina todo en tu account"
read -p "Continuar? (yes/no): " confirm
[[ $confirm != "yes" ]] && exit

# VPCs (excepto default)
aws ec2 describe-vpcs --query 'Vpcs[?!IsDefault].VpcId' --output text --profile $aws_profile | while read vpc; do
  # Terminar instancias
  aws ec2 describe-instances --filters "Name=vpc-id,Values=$vpc" \
    --query 'Reservations[].Instances[].InstanceId' --output text --profile $aws_profile \
    | xargs -I {} aws ec2 terminate-instances --instance-ids {} --profile $aws_profile
  
  # Esperar
  sleep 30
  
  # Eliminar VPC
  aws ec2 delete-vpc --vpc-id $vpc --profile $aws_profile
done

# S3 Buckets (excepto protected)
aws s3 ls --profile $aws_profile | while read line; do
  bucket=$(echo $line | awk '{print $NF}')
  [[ ! $bucket =~ bootcamp ]] && continue
  
  echo "Limpiando bucket: $bucket"
  aws s3 rm s3://$bucket --recursive --profile $aws_profile
  aws s3api delete-bucket --bucket $bucket --profile $aws_profile
done

# RDS (excepto default)
aws rds describe-db-instances --query 'DBInstances[].DBInstanceIdentifier' --output text --profile $aws_profile | while read db; do
  aws rds delete-db-instance --db-instance-identifier $db --skip-final-snapshot --profile $aws_profile
done

# DynamoDB Tables
aws dynamodb list-tables --query 'TableNames' --output text --profile $aws_profile | while read table; do
  aws dynamodb delete-table --table-name $table --profile $aws_profile
done

# Lambda Functions
aws lambda list-functions --query 'Functions[].FunctionName' --output text --profile $aws_profile | while read fn; do
  aws lambda delete-function --function-name $fn --profile $aws_profile
done

echo "✅ Limpieza completada"
```

---

## 📊 Monitoreo de Costos - Dashboard

### AWS Cost Explorer

```
Billing & Cost Management > Cost Explorer > Custom Reports

Filters:
- Date: Last 14 days, Daily
- Granularity: DAILY
- Metrics: UnblendedCost + UsageQuantity
- Group By: SERVICE
```

### CLI Command para Reporte Diario

```bash
# Añadir a cron para ejecutar a las 8am
0 8 * * * aws ce get-cost-and-usage \
  --time-period Start=$(date -d 'yesterday' +%Y-%m-%d),End=$(date +%Y-%m-%d) \
  --granularity DAILY \
  --metrics UnblendedCost \
  --group-by Type=DIMENSION,Key=SERVICE \
  --profile bootcamp | mail -s "AWS Daily Cost" tu@email.com
```

---

## 🎯 Presupuesto por Nivel

| Nivel | Duración | Presupuesto | Recomendado |
|-------|----------|-------------|------------|
| Básico | 2 semanas | $30 | $50 |
| Intermedio | 2 semanas | $50 | $75 |
| Avanzado | 2 semanas | $40 | $60 |
| **TOTAL** | **6 semanas** | **$120** | **$185** |

> **Con Free Tier optimizado**: $0-50 para todo

---

## 📝 Checklist de Seguridad Financiera

- [ ] AWS Budget configurado ($50 limit)
- [ ] Billing alerts activadas
- [ ] Cost Anomaly Detection ON
- [ ] Revisas Cost Explorer 2x/semana
- [ ] Cleanup scripts en tu repo
- [ ] Ejecutas cleanup al fin de cada lab
- [ ] Documentas costos reales
- [ ] Terminas instancias stopped (no just-stop)
- [ ] Liberas Elastic IPs no usadas
- [ ] Borras NAT Gateways después de usar

---

## 🆘 Si la Factura es ALTA

1. **Identifica culpable** en Cost Explorer
2. **Revisa recursos**:
```bash
# EC2 running
aws ec2 describe-instances --filters 'Name=instance-state-name,Values=running' --output table --profile bootcamp

# Data Transfer
aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name NetworkOut --start-time 2026-02-01T00:00:00Z --end-time 2026-02-16T00:00:00Z --period 86400 --statistics Sum --profile bootcamp

# NAT Gateway traffic
aws cloudwatch get-metric-statistics --namespace AWS/NatGateway --metric-name BytesOutToDestination --start-time 2026-02-01T00:00:00Z --end-time 2026-02-16T00:00:00Z --period 86400 --statistics Sum --profile bootcamp
```

3. **Contacta AWS Support** si crees que hubo error

---

**💡 Pro Tip**: Usa AWS Compute Savings Plans (1 año 32% descuento) si planeas seguir después del bootcamp
