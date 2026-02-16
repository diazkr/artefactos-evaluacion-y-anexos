# Artefactos de Evaluación - Trabajo Final de Maestría

## Interfaz conversacional con inteligencia artificial generativa para consultar el estado de servicios en Amazon Web Services (AWS)

**Autora:** Karen Julieth Díaz Cardozo
**Programa:** Maestría en Ingeniería de Sistemas y Computación
**Universidad Nacional de Colombia**
**Año:** 2026

---

## 📋 Descripción

Este repositorio contiene los artefactos digitales de evaluación correspondientes al **Anexo F** y **Anexo G** del Trabajo Final de Maestría. Los materiales aquí presentados documentan la evaluación exhaustiva del sistema mediante 100 pruebas funcionales y la validación de Amazon Q Business como fuente de referencia para consultas de documentación.

---

## 📁 Estructura del Repositorio

```
artefactos-evaluacion-y-anexos/
│
├── evaluacion/
│   ├── rag_evaluation.xlsx          # Anexo G: Evaluación RAG de Amazon Q
│   ├── matriz_pruebas_tfm_FINAL.xlsx # Anexo F: Matriz completa de 100 pruebas
│   ├── pruebas/                      # Evidencias visuales de casos de prueba
│   │   ├── screenshots/              # Capturas de pantalla de ejecución
│   │   └── ...
│   └── documentation_all...          # Respuestas de Amazon Q para validación
│
└── README.md                         # Este archivo
```

---

## 📊 Contenido de los Archivos

### 1. **rag_evaluation.xlsx** (Anexo G)

Evaluación de **Amazon Q Business** como referencia operativa para consultas de documentación técnica de AWS.

**Contenido:**
- **Metodología RAG Evaluation:** Basada en Evidently AI
- **Métricas obtenidas:**
  - **Precision@k:** 88% (calidad de recuperación)
  - **Groundedness:** 90% (sustentación de afirmaciones)
- **Consultas evaluadas:** 5 consultas de tipo Documentation
- **Criterio de aceptación:** ≥ 80% (✅ Cumplido)

**Hojas del archivo:**
- Matriz de evaluación de fuentes (retrieval quality)
- Matriz de evaluación de afirmaciones (groundedness/faithfulness)
- Resumen de resultados

---

### 2. **matriz_pruebas_tfm_FINAL.xlsx** (Anexo F)

Matriz completa de las **100 pruebas funcionales** ejecutadas para evaluar el sistema.

**Contenido:**
- **100 casos de prueba** (T001-T100)
- **Campos por cada caso:**
  - `id`: Identificador único (T001-T100)
  - `query_type`: Tipo de consulta (Costs, Budgets, Pricing, Documentation)
  - `complexity`: Nivel de dificultad (Básica, Intermedia, Avanzada)
  - `is_robustness`: Indica si es caso de robustez (consulta incompleta/ambigua)
  - `query_natural_language`: Consulta en lenguaje natural enviada al sistema
  - `period`: Período temporal evaluado
  - `filters_applied`: Filtros aplicados (servicio, cuenta, región)
  - `system_response`: Respuesta textual del sistema
  - `aws_reference_value`: Valor de referencia obtenido mediante APIs de AWS
  - `c2_accuracy`: Indicador binario de exactitud (0/1, N/A cuando aplica)
  - `c3_robustness`: Indicador binario de robustez (0/1, N/A cuando aplica)
  - `c4_clarity`: Indicador binario de claridad (0/1, N/A cuando aplica)
  - `observations`: Observaciones adicionales

**Distribución de pruebas:**
- **Costos:** 50 pruebas (50%)
- **Presupuestos:** 30 pruebas (30%)
- **Precios:** 15 pruebas (15%)
- **Documentación:** 5 pruebas (5%)

**Niveles de complejidad:**
- **Básicas:** 30 pruebas
- **Intermedias:** 45 pruebas
- **Avanzadas:** 25 pruebas

**Casos de robustez:** 12 pruebas (consultas incompletas o ambiguas)

---

### 3. **pruebas/** (Carpeta)

Contiene evidencias visuales (capturas de pantalla) de la ejecución de casos de prueba representativos.

**Ejemplos incluidos:**
- Consultas de costos en tiempo real
- Consultas de presupuestos con alertas
- Consultas de precios con filtros
- Consultas de documentación técnica

**Formato:** PNG/JPG
**Nomenclatura:** `T0XX_descripcion.png`

---

### 4. **documentation_all...** (Archivo)

Respuestas generadas por **Amazon Q Business** para las 5 consultas de tipo Documentation, utilizadas como fuente de referencia en la evaluación.

**Propósito:**
Dado que las consultas de documentación no se contrastan contra un valor numérico único (como en Costs o Budgets), se utilizó Amazon Q Business como asistente de IA especializado en servicios AWS para generar respuestas de referencia. Estas respuestas fueron evaluadas mediante RAG evaluation.

---

## 🎯 Resultados de la Evaluación

### Criterios evaluados:

| Criterio | Umbral | Resultado | Estado |
|----------|--------|-----------|--------|
| **C2: Exactitud vs AWS API** | ≥ 85% | **90.91%** (80/88) | ✅ Cumple |
| **C3: Robustez ante consultas incompletas** | ≥ 85% | **100%** (12/12) | ✅ Cumple |
| **C4: Claridad mínima de respuesta** | ≥ 85% | **100%** (100/100) | ✅ Cumple |

### Detalles de exactitud (C2):

- **Pruebas validables con APIs:** 88 (las 12 de robustez no aplican para C2)
- **Pruebas correctas:** 80
- **Casos fallidos:** 8 (T014, T020, T024, T036, T043, T064, T078, T098)
- **Exactitud:** 90.91%

**Distribución por tipo de consulta:**
- **Costos:** 39/44 correctas = 88.64%
- **Presupuestos:** 24/26 correctas = 92.31%
- **Precios:** 12/13 correctas = 92.31%
- **Documentación:** 5/5 correctas = 100%

---

## 📖 Nota Metodológica

### Valores de referencia:

**Para consultas de Costs, Budgets y Pricing:**
- Los valores de referencia se obtuvieron directamente de las **APIs oficiales de AWS**:
  - Cost Explorer API
  - Budgets API
  - Pricing API
- Se configuraron los mismos períodos temporales y filtros que la consulta en lenguaje natural
- Se realizó comparación numérica con tolerancia de redondeo

**Para consultas de Documentation:**
- Se utilizó **Amazon Q Business** como fuente de referencia operativa
- Amazon Q fue evaluado previamente mediante **RAG evaluation** (Anexo G)
- Obtuvo Precision@k = 88% y Groundedness = 90%, cumpliendo el criterio de ≥ 80%
- Las respuestas del sistema fueron comparadas contra las respuestas de Amazon Q por un especialista en servicios AWS

---

### Contacto:
- **Autora:** Karen Julieth Díaz Cardozo
- **Email:** [kjdiazc@unal.edu.co](mailto:kjdiazc@unal.edu.co)
- **Directores:**
  - Henry Roberto Umaña Acosta
  - Luis Fernando Ortega Melo

---

## 📝 Licencia

Este material es parte de un Trabajo Final de Maestría presentado a la Universidad Nacional de Colombia. El código fuente asociado está disponible en repositorios públicos bajo licencia MIT (donde aplique).

---


**Última actualización:** Febrero 2026
