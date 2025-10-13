# 🏙️ Explicador de Decisiones del Agente Urbano RL

Sistema interactivo para explicar decisiones de agentes de Reinforcement Learning aplicados a planeación urbana, con adaptación automática del vocabulario técnico según la audiencia.

## 📋 Descripción

Este portal web permite explicar las decisiones de un agente de RL urbano en **tres niveles técnicos diferentes**:

- **Nivel 1 - Lenguaje Común**: Para público general sin conocimientos técnicos
- **Nivel 2 - Profesional**: Para arquitectos y urbanistas con terminología especializada
- **Nivel 3 - Técnico RL**: Para científicos de datos e investigadores en RL/ML

## ✨ Características Principales

### 🎚️ Sistema de Niveles Técnicos

- [ ] Adaptación automática del vocabulario según la audiencia
- [ ] Tres niveles de complejidad con prompts especializados
- [ ] Modo comparación para ver las tres respuestas simultáneamente

### 💾 Sistema de Caché Inteligente

- Cache MD5-based para evitar consultas duplicadas
- Respuestas instantáneas para queries repetidas
- Reducción de costos y tiempo de respuesta

### 📊 Métricas y Análisis

- Tiempo de generación por respuesta
- Conteo de tokens (entrada/salida)
- Indicador de uso de caché
- Historial completo de conversaciones

### 🔧 Configuración Flexible

- Variables de entorno configurables desde la interfaz
- Personalización avanzada del system prompt
- Presets predefinidos (sencillo y técnico)
- Alertas automáticas de configuración faltante

### 📜 Historial y Trazabilidad

- Registro completo de todas las consultas
- Timestamps y métricas por conversación
- Capacidad de limpiar historial y caché

## 🚀 Instalación y Uso

### Requisitos Previos

- Python 3.11 o superior
- Acceso a una API compatible con OpenAI (OpenAI, Azure, etc.)

### Instalación Local

1. **Clonar el repositorio**

```bash
git clone https://github.com/enriquegomeztagle/urban-rl-explainer.git
```

2. **Crear entorno virtual**

```bash
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
```

3. **Instalar dependencias**

```bash
pip install -r requirements.txt
```

4. **Configurar variables de entorno**

Crear archivo `.env` en la raíz del proyecto:

```env
OPENAI_API_KEY=tu_api_key_aqui
OPENAI_BASE_URL=https://api.openai.com
OPENAI_MODEL=gpt-4
```

5. **Ejecutar la aplicación**

```bash
streamlit run app.py
```

La aplicación estará disponible en `http://localhost:8501`

### 🐳 Instalación con Docker

1. **Construir la imagen**

```bash
docker build -t rl-urbanism-explainer .
```

2. **Ejecutar el contenedor**

```bash
docker run -p 8501:8501 \
  -e OPENAI_API_KEY=tu_api_key_aqui \
  -e OPENAI_BASE_URL=https://api.openai.com \
  -e OPENAI_MODEL=gpt-4 \
  rl-urbanism-explainer
```

O usando un archivo `.env`:

```bash
docker run -p 8501:8501 --env-file .env rl-urbanism-explainer
```

3. **Acceder a la aplicación**

Abrir navegador en `http://localhost:8501`

### 1. Configuración Inicial

- Verificar que las variables de entorno estén configuradas (sidebar izquierdo)
- Seleccionar el nivel técnico deseado con el slider

### 2. Cargar Datos

- Usar el botón "Cargar ejemplo" para presets predefinidos
- O ingresar manualmente:
  - **Objetivo del agente**: Qué busca optimizar
  - **Reglas**: Restricciones y políticas
  - **Cálculos**: Métricas y evaluaciones realizadas
  - **Pregunta**: La consulta específica sobre la decisión

### 3. Generar Respuesta

- **Modo Individual**: Genera respuesta en el nivel técnico seleccionado
- **Modo Comparación**: Genera las 3 respuestas simultáneamente

### 4. Revisar Resultados

- Ver métricas de generación (tiempo, tokens, caché)
- Revisar la explicación adaptada al nivel seleccionado
- Consultar historial de conversaciones previas

## 🏗️ Arquitectura Técnica

### Componentes Principales

```
app.py
├── Configuración
│   ├── Variables de entorno (OPENAI_API_KEY, BASE_URL, MODEL)
│   ├── Session State (historial, caché, métricas)
│   └── Presets (ejemplos técnicos y sencillos)
│
├── Sistema de Prompts
│   ├── BASE_CRITICAL_RULES (reglas compartidas)
│   ├── SYSTEM_PROMPT_LEVEL_CONFIG (configuraciones por nivel)
│   └── build_system_prompt() (composición dinámica)
│
├── Generación de Respuestas
│   ├── generate_response_from_inputs() (con caché MD5)
│   ├── LangChain + ChatOpenAI
│   └── Manejo de errores y métricas
│
└── Interfaz Streamlit
    ├── Sidebar (configuración de env vars)
    ├── Selector de nivel técnico
    ├── Formulario de entrada
    ├── Tabs (individual vs comparación)
    └── Expanders (historial y caché)
```

### Sistema de Caché

- **Clave**: MD5(objetivo + reglas + calculos + pregunta + nivel_tecnico)
- **Almacenamiento**: st.session_state (en memoria)
- **Beneficios**: Respuestas instantáneas, reducción de costos

### Arquitectura de Prompts

1. **Base compartida**: Reglas críticas comunes a todos los niveles
2. **Configuración por nivel**: Rol, tarea, reglas extra, formato, ejemplos
3. **Composición dinámica**: `build_system_prompt(level)` ensambla el prompt final

## 🔍 Características Avanzadas

### Anti-Hallucination System

El sistema incluye múltiples salvaguardas para prevenir que el LLM invente información:

- Reglas críticas explícitas en el prompt
- Validación de contexto proporcionado
- Instrucciones para responder "no sé" cuando falta información
- Separación clara entre ejemplos de formato y datos reales

### Progress Tracking

- Barras de progreso multi-etapa
- Estados de procesamiento en tiempo real
- Feedback visual de caché vs nueva generación

### Error Handling

- Manejo de errores de conexión
- Timeout handling
- Mensajes de error descriptivos con soluciones sugeridas

## 📊 Métricas Disponibles

- **Tiempo de generación**: Duración total de la consulta
- **Nivel técnico**: Nivel usado para la respuesta
- **Estado de caché**: Si la respuesta proviene de caché
- **Tokens**: Conteo de tokens de entrada/salida (cuando disponible)
- **Timestamp**: Marca de tiempo de cada conversación

## 🛠️ Tecnologías Utilizadas

- **Streamlit**: Framework web interactivo
- **LangChain**: Integración con LLMs
- **OpenAI API**: Generación de lenguaje natural
- **Loguru**: Sistema de logging
- **Python-dotenv**: Gestión de variables de entorno

## 📝 Dependencias

Ver `requirements.txt` para la lista completa de dependencias .

Principales:

- `streamlit`
- `langchain-openai`
- `loguru`

## 🔐 Seguridad

- Las API keys se almacenan en variables de entorno
- Input type="password" para campos sensibles en la UI
- No se almacenan credenciales en código fuente
- Recomendado usar `.env` file

## 🐛 Troubleshooting

### Error: Variables de entorno faltantes

**Solución**: Verificar que `.env` contenga todas las variables requeridas o configurarlas desde el sidebar.

### Error: Connection timeout

**Solución**: Verificar conectividad a internet y validez de OPENAI_BASE_URL.

### Error: Invalid API key

**Solución**: Revisar que OPENAI_API_KEY sea válida y tenga permisos necesarios.

### Respuestas inconsistentes

**Solución**: Limpiar caché desde el expander "💾 Estadísticas de Caché".

## 🚧 Limitaciones Conocidas

- El caché es volátil (se pierde al cerrar la sesión)
- Máximo de tokens por respuesta: 1024 (configurable en código)
- Requiere conexión a internet para consultas al LLM

## 📄 Licencia

Este proyecto fue desarrollado para propósitos de investigación en planificación urbana utilizando agentes de Aprendizaje por Refuerzo y está destinado únicamente para evaluación académica.

### Aviso de Copyright

© 2025 Enrique Ulises Baez Gomez Tagle. Todos los derechos reservados.

### Términos de Uso

**Solo Investigación y Evaluación**: Este código base está creado específicamente para investigación académica en planificación urbana utilizando técnicas de Aprendizaje por Refuerzo.

**Sin Uso Comercial**: Este proyecto no puede ser utilizado para propósitos comerciales sin el permiso escrito explícito del autor.

**Sin Redistribución**: El código no puede ser redistribuido, copiado o modificado sin autorización del autor.

**Atribución Requerida**: Cualquier referencia a este trabajo debe incluir la atribución adecuada al autor.

### Propiedad Intelectual

Este proyecto representa trabajo original desarrollado independientemente para investigación en planificación urbana y Aprendizaje por Refuerzo. La arquitectura, implementación y decisiones de diseño son propiedad intelectual del autor.

### Contacto

Para preguntas sobre este proyecto o licencias, por favor contactar:

- **Autor**: Enrique Ulises Baez Gomez Tagle
- **GitHub**: [@enriquegomeztagle](https://github.com/enriquegomeztagle)
- **Propósito**: Proyecto de Investigación RL en Planificación Urbana

---

## 👨‍💻 Autor

**Enrique Ulises Baez Gomez Tagle**

GitHub: [@enriquegomeztagle](https://github.com/enriquegomeztagle)

---

**Hecho con ❤️ para investigación en planificación urbana y explicabilidad de IA**
