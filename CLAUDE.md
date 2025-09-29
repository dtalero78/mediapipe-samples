# CLAUDE.md

Este archivo proporciona orientación a Claude Code (claude.ai/code) cuando trabaja con código en este repositorio.

## Resumen del Repositorio

Este repositorio contiene ejemplos oficiales de MediaPipe que demuestran tareas de aprendizaje automático en múltiples plataformas (Android, iOS, JavaScript/Web, Python/Raspberry Pi). MediaPipe Solutions proporciona bloques de construcción modulares para el despliegue de ML multiplataforma.

**PROYECTO ESPECIAL**: `examen_osteomuscular_web/` - Aplicación médica completa para análisis postural y articular usando MediaPipe Pose Landmarker.

## Arquitectura y Estructura

### Organización Específica por Plataforma
- **Android**: Aplicaciones Kotlin/Java usando sistema de construcción Gradle con MediaPipe Tasks Vision SDK (`com.google.mediapipe:tasks-vision`)
- **iOS**: Aplicaciones Swift usando CocoaPods con el pod MediaPipeTasksVision
- **JavaScript/Web**: Demos HTML/JS, algunos usando TypeScript con esbuild
- **Python/Raspberry Pi**: Scripts Python usando el paquete `mediapipe` con scripts de configuración

### Categorías de Ejemplos
Cada tarea de ML tiene implementaciones en todas las plataformas:
- **Visión**: object_detection, image_classification, pose_landmarker, face_landmarker, hand_landmarker, gesture_recognizer, image_segmentation, face_detector, interactive_segmentation, image_embedder
- **Texto**: text_classification, text_embedder, language_detector
- **Audio**: audio_classifier
- **IA Generativa**: llm_inference, image_generation
- **Personalización**: ejemplos de entrenamiento y conversión de modelos
- **Médico**: examen_osteomuscular_web (aplicación médica especializada)

### Gestión de Modelos
- **Android**: Modelos descargados automáticamente via scripts Gradle (ej., `download_models.gradle`) durante la construcción
- **Python/Raspberry Pi**: Modelos descargados via scripts `setup.sh` usando wget
- **Web**: Modelos típicamente referenciados desde CDN o incluidos en el proyecto
- **Médico**: Modelos MediaPipe cargados dinámicamente desde CDN

## Comandos de Desarrollo Comunes

### Desarrollo Android
```bash
# Construir aplicación Android (desde directorio android/)
./gradlew build

# Ejecutar pruebas
./gradlew test

# Instalar en dispositivo
./gradlew installDebug
```

### Desarrollo JavaScript/TypeScript
```bash
# Servidor de desarrollo (para proyectos TypeScript como llm_chat_ts)
npm run dev

# Construcción de producción
npm run build

# Verificar código
npm run lint
```

### Desarrollo Python/Raspberry Pi
```bash
# Configurar entorno y descargar modelos
sh setup.sh

# Ejecutar ejemplo (patrón típico)
python3 classify.py
python3 detect.py
python3 recognize.py

# Con parámetros (común en ejemplos Python)
python3 script.py --model model_name.tflite --maxResults 5 --scoreThreshold 0.5
```

### Desarrollo iOS
```bash
# Instalar dependencias
pod install

# Abrir workspace (no el archivo de proyecto)
open ProjectName.xcworkspace
```

### Aplicación Médica (examen_osteomuscular_web)
```bash
# Navegar al directorio médico
cd examen_osteomuscular_web

# Servir aplicación web
python3 -m http.server 8000
# O alternativamente:
npx http-server -p 8000

# Acceder en navegador
http://localhost:8000

# Detener servidor
Ctrl+C
```

## Dependencias y Versiones Clave

### Android
- MediaPipe Tasks Vision: `0.10.29` (verificar la más reciente en archivos build.gradle)
- SDK Mínimo: 24 (Android 7.0)
- CameraX: `1.4.2`
- Corrutinas Kotlin para operaciones asíncronas

### iOS
- La versión del pod MediaPipeTasksVision varía por ejemplo (verificar Podfile)
- La versión mínima de iOS varía por tarea

### JavaScript/Web
- `@mediapipe/tasks-genai`: Para inferencia LLM
- `@mediapipe/tasks-vision`: Para tareas de visión
- Herramientas de construcción: esbuild, TypeScript

### Python
- `mediapipe`: Paquete principal para todos los ejemplos Python
- Paquetes adicionales como OpenCV típicamente instalados via requirements.txt

### Aplicación Médica
- MediaPipe Tasks Vision: `@mediapipe/tasks-vision@0.10.15` (desde CDN)
- Speech Synthesis API: Para instrucciones de voz en español
- Canvas API: Para renderizado de landmarks y esqueleto
- Web APIs: getUserMedia para acceso a cámara web

## Patrones de Integración de Modelos

### Descarga de Modelos Android
Los modelos se descargan automáticamente durante la construcción via tareas Gradle que:
1. Descargan archivos .tflite desde almacenamiento MediaPipe
2. Los colocan en directorio `src/main/assets/`
3. Configuran `androidResources.noCompress 'tflite'`

### Configuración de Modelos Python
Los scripts de configuración típicamente:
1. Instalan dependencias Python via pip
2. Descargan modelos .tflite usando wget
3. Colocan modelos en directorio del script

## Pruebas y Validación

La mayoría de ejemplos están diseñados para pruebas manuales via interacción UI en lugar de pruebas unitarias automatizadas. Al agregar características:
1. Probar en dispositivos físicos para ejemplos basados en cámara
2. Verificar carga de modelos y rendimiento de inferencia
3. Revisar uso de memoria para plataformas con recursos limitados

## Proyecto Médico Especializado: examen_osteomuscular_web

### Características Únicas
- **Aplicación médica completa**: Análisis postural y articular en tiempo real
- **Instrucciones guiadas**: Sistema de voz automático en español
- **33 landmarks corporales**: Detección precisa con MediaPipe Pose Landmarker
- **Métricas clínicas**: Ángulos articulares, simetría bilateral, evaluación postural
- **Reportes médicos**: Exportación JSON con recomendaciones clínicas
- **Validación de posición**: Verificación automática de postura del paciente

### Tipos de Examen
1. **Evaluación Postural**: Alineación cervical, inclinación pélvica, desviación lateral
2. **Rangos de Movimiento**: Flexión/extensión articular, abducción/aducción
3. **Análisis de Simetría**: Balance bilateral, comparación izquierda/derecha
4. **Examen Completo**: Evaluación integral con todas las métricas

### Arquitectura Técnica
```javascript
// Estructura principal de la aplicación médica
class MedicalPoseAnalyzer {
    - Sistema de instrucciones guiadas con voz
    - Detección MediaPipe Pose (33 landmarks)
    - Cálculos médicos especializados
    - Validación de posición automática
    - Generación de reportes clínicos
}
```

### Problemas Conocidos
- **Algoritmo cervical**: Valores irreales (>170°) - requiere calibración
- **Dependencia de iluminación**: Necesita buena luz para detección precisa
- **Calibración**: Medidas relativas, no absolutas sin calibración específica

### Uso Médico Validado
- Fisioterapia: Evaluación inicial y seguimiento
- Ortopedia: Screening de alteraciones posturales
- Medicina deportiva: Análisis de patrones de movimiento
- Medicina ocupacional: Evaluación ergonómica

## Consideraciones Multiplataforma

Al trabajar entre plataformas, tener en cuenta que:
- Los nombres de archivos de modelos pueden diferir entre plataformas
- Las interfaces API varían (Kotlin vs Swift vs JavaScript vs Python)
- Las características de rendimiento difieren significativamente entre móvil y web
- Algunas características (como aceleración GPU) son específicas de la plataforma
- **La aplicación médica es específica de web** - usa APIs del navegador no disponibles en móvil