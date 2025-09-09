```mermaid
flowchart TB
  %% LAYERS
  subgraph "UX[Experiencia (Front)]"
    A1["Portal Web Marca Blanca\nNext.js/Flutter Web"]
    A2["App Móvil Ligera\nFlutter"]
    A3["Widget Embebible\npara sitios de clientes"]
    A4["Centro de Ayuda / Chat\nBot + FAQ"]
  end

  subgraph "INTEG[Integración & Mensajería]"
    I1["API Gateway / BFF\nGraphQL + REST"]
    I2["Webhooks & Event Bridge\n(Stripe/MercadoPago/WhatsApp)"]
    I3["Mensajería Externa\nWhatsApp/Telegram/Email/SMS"]
    I4["Firma Digital / ID\nOpcional"]
  end

  subgraph APP[Servicios de Aplicación (SaaS Multi-Tenant)]
    direction TB
    S1[Auth & Tenancy\nOIDC + RBAC + Auditoría]
    S2[Mini CRM\nContactos, Historial, Segmentos]
    S3[Operaciones & Archivos\nTareas, Hitos, Repo Docs]
    S4[Monetización & Facturación\nPlanes, Invoices, Pagos]
    S5[Portal de Cliente\nAutoservicio 24/7]
    S6[Notificaciones\norquestador (email/push/WA)]
    S7[Catálogo de Módulos Nicho\nFitness/Salud/Belleza/Técnicos]
  end

  subgraph DATA[Capa de Datos]
    D1[DB Transaccional Multi-Tenant\n(PostgreSQL/Cloud SQL)]
    D2[Almacenamiento de Archivos\nS3/GCS - cifrado]
    D3[Cola/Event Streaming\nKafka/PubSub]
    D4[Data Lake / Warehouse\nParquet + BigQuery/Snowflake]
    D5[Metadatos & Catálogo]
  end

  subgraph AI[IA & Automatización]
    M1[Recomendaciones\n(recordatorios, precios, upsell)]
    M2[Automatización de Cobros\nreintentos, dunning, reminders]
    M3[Asistente Conversacional\nFAQ, onboarding guiado]
    M4[Motor de Reglas\nworkflows sin-código]
  end

  subgraph SEC[Seguridad & Cumplimiento]
    P1[Cifrado en tránsito/repouso\nTLS, KMS]
    P2[Gestión de secretos\nVault/KMS]
    P3[Compliance\nGDPR/LGPD/Logs de auditoría]
    P4[Rate Limiting & WAF]
  end

  subgraph OPS[DevOps, Observabilidad & Entornos]
    O1[CI/CD\nTests, build, IaC (Terraform)]
    O2[Contenedores & Orquestación\nDocker + Kubernetes]
    O3[Monitorización & Trazas\nPrometheus + OpenTelemetry + Grafana]
    O4[Entornos\nDev / Staging / Prod]
    O5[Feature Flags & Experimentos]
  end

  %% FLOWS
  A1 --> I1
  A2 --> I1
  A3 --> I1
  A4 --> I1

  I1 --> S1
  I1 --> S2
  I1 --> S3
  I1 --> S4
  I1 --> S5
  I1 --> S6
  I1 --> S7

  S2 --> D1
  S3 --> D1
  S4 --> D1
  S5 --> D1
  S6 --> D3
  S3 --> D2

  D1 <--> D4
  D3 --> D4
  D4 --> M1
  D4 --> M2
  D4 --> M3
  D4 --> M4

  M1 --> S6
  M2 --> S4
  M3 --> A4
  M4 --> S3

  %% EXTERNALS
  I2 --- S4
  I3 --- S6
  I4 --- S1

  %% GUARDS
  SEC -.protege.-> UX
  SEC -.protege.-> INTEG
  SEC -.protege.-> APP
  SEC -.protege.-> DATA
  SEC -.protege.-> AI
  SEC -.protege.-> OPS

  OPS --- UX
  OPS --- INTEG
  OPS --- APP
  OPS --- DATA
  OPS --- AI
  OPS --- SEC
```
