# Artefactos y Evidencias — Trabajo de Fin de Maestría

**Interfaz conversacional con inteligencia artificial generativa para consultar el estado de servicios en Amazon Web Services (AWS)**

> **Karen Julieth Diaz Cardozo**
> Universidad Nacional de Colombia — Facultad de Ingeniería
> Departamento de Ingeniería de Sistemas e Industrial
> Bogotá, Colombia · 2025

---

## Contenido del repositorio

```
artefactos-evaluacion-y-anexos/
├── evaluacion/           # Matrices de evaluación RAG (Anexo G)
└── pruebas/              # Casos de prueba y evidencias visuales (Anexo F)
```

> Los videos de entrevistas (Anexo B) y la demo del Tech Day (Anexo E) se encuentran en Google Drive por superar el límite de tamaño de GitHub. Ver enlaces en cada sección.

---

## Anexos

### Anexo A — Guía de replicación del entorno (ejecución local)

Documentación técnica completa para reproducir el sistema localmente. Cubre prerequisitos, estructura de repositorios, instalación, variables de entorno, integración de MCPs, autenticación con Keycloak y verificación del sistema.

**Repositorios del sistema:**

| Componente | Repositorio |
|---|---|
| Backend (FastAPI + LangGraph) | [github.com/diazkr/aws_agent](https://github.com/diazkr/aws_agent) |
| Frontend (Next.js + React) | [github.com/diazkr/aws-agent-front](https://github.com/diazkr/aws-agent-front) |

**Stack tecnológico:**

- **Backend:** Python 3.12+, FastAPI, LangGraph, `langchain-mcp-adapters`, Gemini 2.5 Pro
- **Frontend:** Next.js 15, React 19, TypeScript, Tailwind CSS 4, Keycloak.js
- **Infraestructura:** MongoDB Atlas, Keycloak (IdP), AWS (Cost Explorer, Pricing, Billing, Documentation)
- **MCPs integrados:** `awslabs.aws-documentation-mcp-server`, `awslabs.cost-explorer-mcp-server`, `awslabs.aws-pricing-mcp-server`, `awslabs.billing-cost-management-mcp-server`

**Ejecución rápida:**

```bash
# Backend
cd aws_agent/
uv venv && uv pip install -e .
cp .env.example .env  # completar variables
uv run uvicorn main:app --reload --port 8000

# Frontend
cd aws-agent-front/
npm install
cp .env.example .env  # completar variables
npm run dev
```

Acceder a:
- Frontend: `http://localhost:3000`
- API docs: `http://localhost:8000/docs`
- Keycloak admin: `http://localhost:8080` (requiere Docker)

---

### Anexo B — Guion de entrevista semiestructurada

Artefacto metodológico utilizado durante la fase de análisis de requerimientos. Las entrevistas fueron realizadas a profesionales del sector para comprender necesidades reales y validar la relevancia de la solución propuesta.

**Grabaciones de entrevistas:**

| # | Cargo | Enlace |
|---|---|---|
| 1 | Gerente Financiera y de Estrategia | [Ver grabación](https://drive.google.com/file/d/1VoGmp8dIojZX77UCII2zyl7jJ83heuqY/view?usp=sharing) |
| 2 | Analista de Datos | [Ver grabación](https://drive.google.com/file/d/1ZtsXypsYchd2J-cXZz39XpRs43n-jG2d/view?usp=drive_link) |
| 3 | Gerente de Tecnología | [Ver grabación](https://drive.google.com/file/d/1iILfTNhsBRz_-NDCAGN2h-cNaqTZKKbD/view?usp=sharing) |

> *Las grabaciones fueron realizadas con el consentimiento explícito de los participantes y se utilizan exclusivamente con fines académicos.*

---

### Anexo C — Versiones anteriores de los diagramas de arquitectura

Documenta la evolución arquitectónica del sistema a lo largo del proceso de diseño iterativo (Versiones 1 y 2), hasta alcanzar la arquitectura final descrita en el Capítulo 4 del documento principal.

Incluye por cada versión:
- Diagrama de descomposición funcional
- Diagrama de componentes y conectores
- Diagrama de despliegue
- Vistas de interfaz de usuario (wireframes / mockups)

---

### Anexo D — Repositorio de infraestructura AWS CDK

Infraestructura como código (IaC) desarrollada con **AWS CDK en TypeScript** para el despliegue del sistema en AWS. El repositorio es de acceso privado por contener configuraciones específicas de la infraestructura de producción.

**Stacks definidos:**

| Stack | Descripción |
|---|---|
| `controlCostosStack` | Backend: ECS Fargate, ECR, ALB, CodePipeline |
| `controlCostosStackFront` | Frontend: ECS Fargate, ECR, CodePipeline |

**Estrategia de capacidad (Fargate Spot):**
```typescript
capacityProviderStrategies: [
  { capacityProvider: "FARGATE_SPOT", weight: 4, base: 0 },
  { capacityProvider: "FARGATE",      weight: 1, base: 1 }
]
```
Esta configuración garantiza alta disponibilidad con reducción de costos de hasta 70% mediante instancias Spot.

**Auto-escalado:** 1–5 tareas, 0% downtime durante despliegues.

**CI/CD:** CodePipeline con trigger automático en push a `main` → Build Docker → Push ECR → Deploy CDK.

---

### Anexo E — Evidencias del Tech Day y despliegue en producción

Evidencias del despliegue del sistema en un entorno productivo de AWS, así como la demostración realizada durante un evento técnico interno el **14 de noviembre de 2025**.

**Video de demostración:**

[Ver video de demo en entorno de producción](https://drive.google.com/file/d/1-A3BHYOVQwudWkKBxhtXusS3f9Typ5lW/view?usp=drive_link)

La demostración evidencia:
- Consultas en lenguaje natural sobre costos de AWS
- Generación de visualizaciones de datos en tiempo real
- Mantenimiento del contexto conversacional entre interacciones

---

### Anexo F — Matriz de pruebas y evidencias de evaluación

Evaluación funcional del sistema mediante **100 casos de prueba** distribuidos en cuatro categorías, con criterios de exactitud (C2), robustez (C3) y claridad (C4).

**Distribución de casos de prueba:**

| Categoría | Descripción | Valor de referencia |
|---|---|---|
| Costs | Consultas de costos y uso por período, cuenta, servicio | Consola AWS (Cost Explorer) |
| Budgets | Estado de presupuestos y alertas de desviación | Consola AWS (AWS Budgets) |
| Pricing | Precios bajo demanda y estimaciones | Consola AWS (Pricing) |
| Documentation | Consultas conceptuales sobre servicios AWS | Amazon Q (referencia IA) |

**Resultados destacados:**

- **T017** (Costs / Intermediate): gasto total por cuenta a 6 meses → diferencia de $0.13 (0.00012%) respecto al valor de referencia.
- **T068** (Budgets / Intermediate): identificación de dos presupuestos diarios excedidos con valores exactos.
- **T091** (Pricing / Intermediate): estimación EC2 m5.large 24/7 mensual → coincidencia exacta ($70.08).
- **T054** (Documentation / Advanced): modelo de precios AWS Glue → respuesta alineada con documentación oficial.

**Recursos:**

| Recurso | Enlace |
|---|---|
| Matriz de pruebas completa (Google Sheets) | [Ver](https://docs.google.com/spreadsheets/d/1fL0rZxwG2iwlAGg7NJK8Zj9LGJPee8_6e7mWAXHii7U/edit?usp=sharing) |
| Evidencias visuales (capturas por caso) | [Ver](https://docs.google.com/document/d/1JWGXkX7wnjNzLB1OhMbFYFA168ANqUX5aqjzUS8djiM/edit?usp=sharing) |
| Archivo local (pruebas) | [`pruebas/matriz_pruebas_tfm.xlsx`](./pruebas/matriz_pruebas_tfm.xlsx) |
| Evidencias documentación | [`pruebas/documentation_all_test.docx`](./pruebas/documentation_all_test.docx) |

---

### Anexo G — Evaluación de Amazon Q Business como referencia operativa (RAG Evaluation)

Metodología de evaluación aplicada a **Amazon Q Business** como referencia para las consultas de tipo *Documentation*, basada en el estándar de *RAG evaluation* de Evidently AI.

**Dos niveles de evaluación por consulta:**

1. **Precision@k (retrieval quality):** relevancia de las fuentes citadas por Amazon Q, escala 0–2 (No relevante / Parcialmente relevante / Completamente relevante).
2. **Groundedness / Faithfulness:** proporción de afirmaciones de la respuesta sustentadas por las fuentes recuperadas.

**Alcance:** 5 consultas de la categoría *Documentation* evaluadas con matrices tabulares de fuentes y afirmaciones.

**Archivo local:** [`evaluacion/rag_evaluation.xlsx`](./evaluacion/rag_evaluation.xlsx)

---

## Descripción del sistema

Este trabajo implementa un agente conversacional que permite consultar en lenguaje natural el estado de costos, presupuestos y servicios de AWS, integrando cuatro servidores MCP oficiales de AWS Labs mediante el protocolo Model Context Protocol (MCP).

**Arquitectura:**

```
Usuario → Frontend (Next.js) → Backend (FastAPI + LangGraph)
                                        ↓
                               Agente ReAct (Gemini 2.5 Pro)
                                        ↓
              MCP Cost Explorer · MCP Pricing · MCP Billing · MCP Documentation
                                        ↓
                              AWS APIs (datos en tiempo real)
```

**Persistencia:** MongoDB Atlas
**Autenticación:** Keycloak (OpenID Connect / JWT)
**Streaming de respuestas:** Server-Sent Events (SSE)
**Observabilidad:** LangSmith (opcional)

---

## Notas

- Las grabaciones de entrevistas se publican con el consentimiento explícito de los participantes, exclusivamente con fines académicos.
- El repositorio de infraestructura CDK es privado por contener configuraciones de la infraestructura de producción.
- Los valores de referencia para Costs, Budgets y Pricing provienen directamente de la consola de AWS. Para Documentation, se utilizó Amazon Q como fuente de referencia comparativa.
