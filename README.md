# achs-api-tests
# 🧪 PLAN DE PRUEBAS  
## Proyecto: Motor de Prestaciones – ACHS  
## Tipo de pruebas: API Testing (Funcional + Negativas + Data Driven)

---

## 1. Objetivo

Validar el correcto funcionamiento del endpoint de cálculo de beneficios del Motor de Prestaciones, asegurando que:

- El cálculo se realice correctamente según los datos de entrada.
- Se respeten las reglas de negocio definidas.
- Se gestionen adecuadamente errores por payload inválido.
- Se cumpla con la estructura esperada de respuesta.
- Se controlen correctamente los códigos HTTP.

---

## 2. Alcance

Se validará el endpoint:

POST /api/v1/beneficios/calcular

Incluyendo:

- Escenarios positivos
- Escenarios negativos
- Validaciones de estructura JSON
- Validaciones de reglas de negocio
- Iteraciones mediante Data Driven Testing (archivo CSV)

---

## 3. Estrategia de Pruebas

- Ejecución automatizada con Postman + Newman.
- Ejecución Data Driven mediante archivo CSV.
- Validación de:
  - Código HTTP
  - Propiedad `code` en respuesta
  - Estructura del body
  - Mensajes de error esperados
  - Resultado del cálculo
  - Tiempo de respuesta

---

## 4. Casos de Prueba Definidos (13)

---

### TC01 – Cálculo válido con datos completos

**Objetivo:**  
Validar que el servicio retorne cálculo correcto cuando el payload es válido.

**Resultado esperado:**
- HTTP 200
- Propiedad `code` presente
- Resultado del cálculo correcto
- Estructura JSON válida

---

### TC02 – Cálculo con incapacidad ≥ 70%

**Objetivo:**  
Validar que la pensión corresponda al 100% del SBM cuando la incapacidad es mayor o igual a 70%.

**Resultado esperado:**
- HTTP 200
- Cálculo = 100% del SBM

---

### TC03 – Cálculo con incapacidad entre 40% y 69%

**Objetivo:**  
Validar fórmula: (% incapacidad * 1.5) % del SBM.

**Resultado esperado:**
- HTTP 200
- Cálculo correcto según fórmula

---

### TC04 – Cálculo con incapacidad entre 15% y 39%

**Objetivo:**  
Validar regla correspondiente a incapacidad parcial.

**Resultado esperado:**
- HTTP 200
- Resultado conforme a regla de negocio

---

### TC05 – Payload incompleto

**Objetivo:**  
Validar manejo de error cuando faltan campos obligatorios.

**Resultado esperado:**
- HTTP 400
- Mensaje descriptivo
- Propiedad `code` presente en respuesta

---

### TC06 – Campo numérico con valor inválido

**Objetivo:**  
Validar manejo de tipo de dato incorrecto.

**Resultado esperado:**
- HTTP 400
- Mensaje de validación

---

### TC07 – Incapacidad menor a 15%

**Objetivo:**  
Validar que no se otorgue beneficio bajo el mínimo permitido.

**Resultado esperado:**
- HTTP 400 o respuesta de no elegibilidad

---

### TC08 – Incapacidad mayor a 100%

**Objetivo:**  
Validar validación de rango permitido.

**Resultado esperado:**
- HTTP 400
- Error de validación

---

### TC09 – Campos vacíos

**Objetivo:**  
Validar manejo de strings vacíos o nulos.

**Resultado esperado:**
- HTTP 400
- Mensaje de error

---

### TC10 – Estructura JSON inválida

**Objetivo:**  
Validar manejo de body mal formado.

**Resultado esperado:**
- HTTP 400
- Error de parsing

---

### TC11 – Validación de propiedad `code` en respuesta

**Objetivo:**  
Verificar que todas las respuestas incluyan la propiedad `code`.

**Resultado esperado:**
- Presencia obligatoria de `code` en todas las respuestas

---

### TC12 – Validación de tiempo de respuesta

**Objetivo:**  
Verificar que el servicio responda en tiempo aceptable.

**Resultado esperado:**
- Tiempo de respuesta menor a 2 segundos

---

### TC13 – Ejecución Data Driven (Iteraciones múltiples)

**Objetivo:**  
Validar múltiples combinaciones de datos usando archivo CSV.

**Resultado esperado:**
- Todas las iteraciones ejecutadas
- Resultados consistentes
- Sin errores inesperados
- Validación correcta en cada iteración

---

## 5. Criterios de Aceptación

- 100% de los casos ejecutados.
- 0 fallos en escenarios positivos.
- Manejo correcto de errores en escenarios negativos.
- Respuestas consistentes en todas las iteraciones del Data Driven Testing.
- Tiempo de respuesta dentro de lo esperado.

---

## 6. Herramientas Utilizadas

- Postman
- Newman
- GitHub Actions (CI)
- Archivo CSV para Data Driven Testing

---

## 7. Riesgos Identificados

- Dependencia del ambiente (localhost vs DEV).
- Variables de entorno mal configuradas.
- Inconsistencias en reglas de negocio.
- Datos incorrectos en archivo CSV.
- Cambios no controlados en el endpoint.

---
