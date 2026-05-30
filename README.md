# Nutri-Maker-AI
# Ecosistema Inteligente y Comunitario de Salud Preventiva

## Documento de Diseño de Producto (PRD)

Frente a la crisis global de la doble carga de la malnutrición —donde conviven la obesidad y la desnutrición, a veces en las mismas comunidades— las aplicaciones tradicionales fallan al asumir un único objetivo estándar: perder peso.

**Nutri-Maker AI** nace como una plataforma health-tech de código abierto y hardware colaborativo. Su propósito es democratizar el acceso a la salud preventiva mediante un análisis antropométrico híbrido, un motor de recomendación clínico-conductual y un ecosistema de hardware DIY (*Do It Yourself*).

---

# 1. Sistema de Análisis Corporal Híbrido mediante IA

El análisis visual no busca diagnosticar, sino ofrecer un punto de partida accesible, eliminando la barrera de adquirir básculas de bioimpedancia costosas.

## Arquitectura de Visión Computacional (Capa 1)

El sistema procesa tres fotografías (frente, perfil, espalda) utilizando un pipeline secuencial de modelos locales y en la nube para resguardar la privacidad.

### Estimación de Puntos Clave (*Pose Estimation*)

Modelos basados en MediaPipe o YOLOv8-Pose para identificar articulaciones y extraer longitudes óseas relativas. Esto permite estimar la altura aproximada mediante correlación con objetos de referencia o la calibración del giroscopio del teléfono.

### Reconstrucción de Malla 3D (*3D Body Mesh*)

Se implementan arquitecturas como SMPL (*Skinned Multi-Person Linear Model*) o HMR (*End-to-end Recovery of Human Shape and Pose*). Estas redes estiman el volumen y las proporciones corporales a partir de siluetas 2D.

### Segmentación Semántica

Redes de segmentación como DeepLabV3 aíslan la silueta del fondo y calculan la distribución corporal estimativa (cociente cintura-cadera), correlacionándolo con rangos de IMC y riesgos potenciales de grasa visceral o pérdida de masa magra.

---

## Enfoque Híbrido de Precisión

```text
[Capa 1: Visión Computacional] -> Estimación inicial basada en imágenes
              ↓
[Capa 2: Validación Manual] -> El usuario corrige peso, altura y perímetros
              ↓
[Capa 3: Aprendizaje Progresivo] -> Red neuronal local refina predicciones futuras
```

### Capa 1 — Visión Computacional

Genera la estimación base de volumen y perímetros.

### Capa 2 — Validación Manual

El usuario actúa como validador final. La interfaz despliega los datos estimados y permite corregir manualmente:

* Peso real
* Altura
* Perímetros clave
* Nivel de actividad
* Condiciones médicas preexistentes

### Capa 3 — Aprendizaje Progresivo

Un modelo de regresión liviano en el dispositivo (*On-Device ML*) aprende el sesgo sistemático del modelo general frente al cuerpo del usuario. La IA mejora progresivamente mediante chequeos semanales.

---

## Aviso de Transparencia Médica

> El análisis por fotografía es estrictamente estimativo y posee márgenes de error entre un 5% y un 12%, dependiendo de las condiciones de luz y vestimenta. No reemplaza una antropometría ISAK ni un escaneo DXA clínico.

---

## Limitaciones Técnicas y Éticas

### Técnicas

* Menor precisión con ropa holgada
* Problemas con fondos de bajo contraste
* Sensibilidad a iluminación deficiente
* Sesgos en datasets globales
* Falta de representación de diversas morfologías corporales

### Éticas

El procesamiento de imágenes corporales requiere:

* Cifrado absoluto
* No almacenamiento permanente
* Procesamiento efímero en RAM cifrada
* Destrucción inmediata tras extraer descriptores numéricos

---

# 2. Determinación Inteligente del Objetivo Corporal

El algoritmo central descarta el sesgo clásico de “adelgazar por defecto”.

```text
[Datos de Entrada: IMC, Grasa Estimada, Contexto]
                     │
 ┌───────────────────┴───────────────────┐
 ▼                                       ▼
¿Riesgo Metabólico / Alto IMC?   ¿Bajo Peso / Baja Masa Magra?
 │                                       │
 ├───────────────┐              ├───────────────┐
 ▼               ▼              ▼               ▼
Reducir Peso   Recomposición   Incrementar Peso   Ganancia Muscular
```

---

## Matriz de Priorización del Motor de IA

| Indicador Biométrico    | Contexto Clínico                    | Objetivo IA                | Justificación                         |
| ----------------------- | ----------------------------------- | -------------------------- | ------------------------------------- |
| IMC > 28 o cintura alta | Sedentarismo, riesgo cardiovascular | Reducir peso/grasa         | Reduce inflamación y presión arterial |
| IMC 21 - 24.9           | Porcentaje graso moderado-alto      | Recomposición corporal     | Sustitución de grasa por músculo      |
| IMC < 18.5              | Fatiga, baja densidad ósea          | Incrementar peso saludable | Recuperación endocrina e inmune       |
| Cualquier IMC           | Resistencia a la insulina           | Prevención metabólica      | Mejora captación de glucosa           |

---

## Inteligencia Explicable (XAI)

La IA explica sus recomendaciones mediante lenguaje natural estructurado:

> “Basado en tu relación cintura-altura y tus reportes de fatiga, tu cuerpo se beneficiará prioritariamente de un enfoque de ganancia muscular.”

---

# 3. Perfil Biométrico y Contextual del Usuario

## Datos Físicos

* Edad
* Sexo biológico
* Altura y peso verificados
* Historial antropométrico

## Contexto de Salud

* Hipertensión
* Diabetes
* Uso de fármacos
* Lesiones activas
* Calidad del sueño
* Estrés percibido

## Contexto Conductual

* Nivel deportivo
* Disponibilidad horaria
* Sedentarismo
* Adherencia histórica

## Nutrición y Economía

* Preferencias alimentarias
* Restricciones culturales
* Presupuesto alimentario
* Acceso geográfico a alimentos

---

# 4. Motor Inteligente de Entrenamiento y Nutrición

## Sistema Adaptativo de Entrenamiento

### 1. Filtrado de Seguridad y Lesiones

Si existe lesión en rodilla:

* Bloqueo de ejercicios de alta cizalla
* Sustitución por variantes seguras

### 2. Volumen de Mantenimiento Basal

Asignación automática de series semanales según experiencia.

### 3. Distribución Casa vs. Gimnasio

Adaptación según:

* Equipamiento disponible
* Hardware Maker
* Bandas de tensión
* Herramientas calisténicas

### 4. Monitoreo de Fatiga

Si el sueño es menor a 6 horas:

* Reducción automática del volumen de entrenamiento en 20%

---

## Planificación Nutricional Flexible

La IA utiliza la ecuación de Mifflin-St Jeor ajustada mediante wearables.

### Optimización Costo-Eficiente

Priorización de alimentos:

* Huevos
* Legumbres
* Atún
* Menudencias

### Adaptación Cultural

Sugerencias alimentarias basadas en:

* Región geográfica
* Temporada
* Cultura alimentaria local

---

# 5. Ecosistema Maker e Innovación Tecnológica

## Wearables DIY

### Podómetro/Pulsómetro Maker

Componentes:

* ESP32
* MAX30102
* MPU6050

Funciones:

* Conteo de pasos
* Ritmo cardíaco
* Detección de movimiento

### Báscula Open Source

Componentes:

* Celdas HX711
* AD8232
* Plataforma modular

---

## Arquitectura de Hardware

```text
[Sensores: MAX30102 + MPU6050]
                     │
                     ▼
[Microcontrolador: ESP32 (Firmware C++)]
                     │
          (BLE Cifrado)
                     ▼
[Nutri-Maker App]
```

---

## Gamificación y Comunidad

### Misiones Maker

Los usuarios desbloquean funciones premium al:

* Construir sensores
* Calibrar dispositivos
* Participar en FabLabs

### Retos Comunitarios

Los pasos colectivos generan beneficios para:

* Escuelas
* Comedores comunitarios
* Barrios vulnerables

---

# 6. Diseño Técnico del Producto

## Arquitectura Tecnológica

```text
[FRONTEND: Flutter / Kotlin]
            │
            ▼
        [API Gateway]
            │
 ┌──────────┼──────────┐
 ▼          ▼          ▼
[FastAPI] [IA Engine] [MQTT Broker]
 ▼          ▼          ▼
[PostgreSQL] [OpenCV] [ESP32]
```

---

## Stack Tecnológico

| Componente           | Tecnología         |
| -------------------- | ------------------ |
| Frontend             | Flutter            |
| Backend              | FastAPI            |
| IA                   | ONNX / PyTorch     |
| Base de datos        | PostgreSQL + Redis |
| Visión computacional | OpenCV             |
| Maker Hub            | MQTT               |

---

# Flujo UX

## 1. Onboarding

Ingreso de:

* Datos demográficos
* Presupuesto
* Preferencias
* Nivel deportivo

## 2. Captura Fotográfica

La app usa el acelerómetro para mantener alineación vertical.

## 3. Validación

El usuario ajusta:

* Peso
* Perímetros
* Datos estimados

## 4. Plan Inicial

La IA explica:

* Objetivo recomendado
* Lógica clínica
* Primer bloque de entrenamiento y nutrición

---

# 7. Diferenciación y Viabilidad

## Matriz Comparativa

| Característica   | Apps Tradicionales        | Nutri-Maker AI               |
| ---------------- | ------------------------- | ---------------------------- |
| Objetivo         | Pérdida de peso estética  | Salud preventiva             |
| Modelo económico | Pago/hardware propietario | Open source + DIY            |
| Nutrición        | Dietas genéricas          | Adaptación local y económica |
| Privacidad       | Fotos en la nube          | Procesamiento efímero        |
| Accesibilidad    | Costosa                   | Hardware de bajo costo       |
