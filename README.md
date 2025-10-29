# Elementos de la Sintaxis de Shell Scripting I
## Aplicación de Variables a Proyectos Bioinformáticos

## Información del Curso
- **Programa:** Biología - Bioinformática
- **Curso:** Principios de programación
- **Docentes:** Frank Guzman Escudero, Manuel Ramírez Sáenz
- **Semestre:** 2025-02

## Logro de la Sesión
Al finalizar la sesión, el estudiante será capaz de:
- Aplicar variables en la automatización de proyectos bioinformáticos reales
- Diseñar la estructura estándar de un proyecto de análisis genómico
- Crear scripts que organicen automáticamente espacios de trabajo
- Desarrollar scripts que analicen y generen reportes de datos genómicos
- Implementar buenas prácticas en documentación de proyectos

**Palabras clave:** Estructura de Proyecto, Automatización, Análisis Genómico, Scripts Productivos, Documentación

---

## 1. ESTRUCTURA ESTÁNDAR DE PROYECTO DE ANÁLISIS DE GENOMAS BACTERIANOS

### ¿Por qué Organizar el Proyecto?

La **organización** de un proyecto bioinformático es crucial para:
- **Reproducibilidad:** Otros investigadores pueden entender y repetir el análisis
- **Mantenibilidad:** Es fácil encontrar archivos meses o años después
- **Escalabilidad:** Agregar nuevas muestras o análisis sin confusión
- **Documentación:** Claridad en el flujo de trabajo

### Estructura Recomendada para Análisis de Genomas Bacterianos

```
proyecto_genoma_bacteriano/
│
├── README.md                 # Descripción general del proyecto
├── metadata.txt              # Información de muestras y experimento
│
├── datos_crudos/
│   ├── genomas_ensamblados/  # Archivos FASTA de genomas
│   │   ├── bacteria_001.fasta
│   │   ├── bacteria_002.fasta
│   │   └── ...
│   ├── secuencias_raw/       # Lecturas crudas (FASTQ)
│   │   ├── muestra_001_R1.fastq
│   │   ├── muestra_001_R2.fastq
│   │   └── ...
│   └── referencias/          # Genomas de referencia
│       └── reference.fasta
│
├── datos_procesados/
│   ├── ensamblajes/          # Genomas procesados/ensamblados
│   ├── alineamientos/        # Archivos BAM/SAM
│   ├── variantes/            # Archivos VCF
│   └── anotaciones/          # Archivos GFF/GTF
│
├── resultados/
│   ├── analisis_filogenetico/    # Árboles y análisis filogenéticos
│   ├── pangenoma/                # Análisis de pangenoma
│   ├── comparativo/              # Análisis comparativos
│   └── reportes/                 # Reportes finales (PDF, HTML)
│
├── logs/
│   ├── ejecucion_2025-02-15.log  # Registros de ejecución
│   └── errores_2025-02-15.log    # Errores ocurridos
│
├── scripts/
│   ├── setup_proyecto.sh          # Script de inicialización
│   ├── analizar_genoma.sh         # Script de análisis
│   └── utils/
│       └── funciones_comunes.sh
│
└── documentacion/
    ├── manual_usuario.md           # Instrucciones de uso
    └── parametros_usados.txt       # Parámetros de ejecución
```

### Descripción de Cada Directorio

| Directorio | Propósito | Contenido Típico |
|------------|-----------|------------------|
| `datos_crudos/` | Datos originales (nunca modificados) | FASTA, FASTQ, referencias |
| `datos_procesados/` | Datos intermedios tras procesamiento | BAM, VCF, genomas anotados |
| `resultados/` | Análisis finales y gráficos | Árboles filogenéticos, reportes |
| `logs/` | Registros de ejecución | Mensajes de error, timestamps |
| `scripts/` | Scripts de automatización | Scripts bash del proyecto |
| `documentacion/` | Información del proyecto | Notas, métodos, decisiones |

---

## 2. VARIABLES CLAVE PARA PROYECTOS GENÓMICOS

### Variables Estándar a Definir

```bash
#!/bin/bash
# Variables del Proyecto
PROJECT_NAME="Analisis_Genomas_Bacterianos_2025"
PROJECT_DIR="$HOME/proyectos/$PROJECT_NAME"
FECHA_CREACION=$(date +"%Y-%m-%d")
USUARIO=$(whoami)
VERSION_PROYECTO="1.0"

# Directorios Principales
DATA_RAW="$PROJECT_DIR/datos_crudos"
DATA_PROCESSED="$PROJECT_DIR/datos_procesados"
RESULTS="$PROJECT_DIR/resultados"
LOGS="$PROJECT_DIR/logs"
SCRIPTS="$PROJECT_DIR/scripts"
DOCS="$PROJECT_DIR/documentacion"

# Subdirectorios
GENOMAS_RAW="$DATA_RAW/genomas_ensamblados"
SECUENCIAS_RAW="$DATA_RAW/secuencias_raw"
REFERENCIAS="$DATA_RAW/referencias"
ENSAMBLAJES="$DATA_PROCESSED/ensamblajes"
ALINEAMIENTOS="$DATA_PROCESSED/alineamientos"

# Archivo de Log
LOG_FILE="$LOGS/ejecucion_$(date +%Y-%m-%d_%H-%M-%S).log"
```

---

## 3. CASO PRÁCTICO 1: CREACIÓN AUTOMÁTICA DE ESTRUCTURA DE PROYECTO

### Objetivo
Crear un script que **automatice la creación** de la estructura de carpetas estándar para un nuevo proyecto de análisis genómico, progresando desde simple a complejo.

---

### Versión 1: Muy Simple (Solo Comandos Básicos)

```bash
#!/bin/bash
# Script: setup_proyecto_v1.sh
# Descripción: Crea estructura básica de proyecto
# Autor: [Tu nombre]
# Fecha: 2025-02-XX

# Crear directorio principal
mkdir proyecto_genoma

# Crear subdirectorios principales
mkdir proyecto_genoma/datos_crudos
mkdir proyecto_genoma/datos_procesados
mkdir proyecto_genoma/resultados

echo "Estructura básica creada."
```

**Limitaciones:** 
- ❌ No es flexible (nombre fijo)
- ❌ Difícil de reutilizar
- ❌ Sin información de logging

**Ejecución:**
```bash
bash setup_proyecto_v1.sh
```

---

### Versión 2: Con Variables (MEJORA IMPORTANTE)

Aquí vemos el **poder de las variables** - el script ahora es reutilizable:

```bash
#!/bin/bash
# Script: setup_proyecto_v2.sh
# Descripción: Crea estructura de proyecto usando variables
# Autor: [Tu nombre]
# Fecha: 2025-02-XX
# Uso: bash setup_proyecto_v2.sh

# ========== DEFINIR VARIABLES ==========
NOMBRE_PROYECTO="Genoma_Bacteria_XYZ"
DIRECTORIO_BASE="$HOME/proyectos"
PROYECTO_DIR="$DIRECTORIO_BASE/$NOMBRE_PROYECTO"

# Subdirectorios principales
DATOS_CRUDOS="$PROYECTO_DIR/datos_crudos"
DATOS_PROCESADOS="$PROYECTO_DIR/datos_procesados"
RESULTADOS="$PROYECTO_DIR/resultados"
LOGS="$PROYECTO_DIR/logs"
SCRIPTS="$PROYECTO_DIR/scripts"

# ========== CREAR ESTRUCTURA ==========
# Crear directorio principal
mkdir -p $PROYECTO_DIR

# Crear subdirectorios principales
mkdir -p $DATOS_CRUDOS
mkdir -p $DATOS_PROCESADOS
mkdir -p $RESULTADOS
mkdir -p $LOGS
mkdir -p $SCRIPTS

# ========== MOSTRAR INFORMACIÓN ==========
echo "======================================="
echo "PROYECTO CREADO EXITOSAMENTE"
echo "======================================="
echo "Nombre: $NOMBRE_PROYECTO"
echo "Ubicación: $PROYECTO_DIR"
echo "Directorios creados:"
echo "  - Datos crudos: $DATOS_CRUDOS"
echo "  - Datos procesados: $DATOS_PROCESADOS"
echo "  - Resultados: $RESULTADOS"
echo "  - Logs: $LOGS"
echo "  - Scripts: $SCRIPTS"
echo "======================================="
```

**Ventajas:**
- ✅ Variables reutilizables
- ✅ Fácil de modificar
- ✅ Mensaje de confirmación claro

**Ejecución:**
```bash
bash setup_proyecto_v2.sh
```

---

### Versión 3: Estructura Completa (Production-Ready)

Esta es la versión profesional que incluye subdirectorios completos, archivos de configuración y logging:

```bash
#!/bin/bash
# Script: setup_proyecto.sh
# Descripción: Crea estructura completa de proyecto de análisis genómico
# Autor: [Tu nombre]
# Fecha: 2025-02-XX
# Uso: bash setup_proyecto.sh [nombre_proyecto]

# ========== VARIABLES DEL SCRIPT ==========
# Si el usuario proporciona nombre, usarlo; si no, usar valor por defecto
NOMBRE_PROYECTO="${1:-Genoma_Bacteria_Default}"
DIRECTORIO_BASE="$HOME/proyectos"
PROYECTO_DIR="$DIRECTORIO_BASE/$NOMBRE_PROYECTO"

# Variables de tiempo y usuario
FECHA=$(date +"%Y-%m-%d")
HORA=$(date +"%H:%M:%S")
USUARIO=$(whoami)
VERSION_PROYECTO="1.0"

# Directorios principales
DATOS_CRUDOS="$PROYECTO_DIR/datos_crudos"
DATOS_PROCESADOS="$PROYECTO_DIR/datos_procesados"
RESULTADOS="$PROYECTO_DIR/resultados"
LOGS="$PROYECTO_DIR/logs"
SCRIPTS="$PROYECTO_DIR/scripts"
DOCS="$PROYECTO_DIR/documentacion"

# Subdirectorios de datos crudos
GENOMAS_RAW="$DATOS_CRUDOS/genomas_ensamblados"
SECUENCIAS_RAW="$DATOS_CRUDOS/secuencias_raw"
REFERENCIAS="$DATOS_CRUDOS/referencias"

# Subdirectorios de datos procesados
ENSAMBLAJES="$DATOS_PROCESADOS/ensamblajes"
ALINEAMIENTOS="$DATOS_PROCESADOS/alineamientos"
VARIANTES="$DATOS_PROCESADOS/variantes"
ANOTACIONES="$DATOS_PROCESADOS/anotaciones"

# Subdirectorios de resultados
FILOGENIA="$RESULTADOS/analisis_filogenetico"
PANGENOMA="$RESULTADOS/pangenoma"
COMPARATIVO="$RESULTADOS/comparativo"
REPORTES="$RESULTADOS/reportes"

# ========== CREAR ESTRUCTURA DE DIRECTORIOS ==========
echo "[INFO] Creando estructura de proyecto..."

# Directorio principal
mkdir -p $PROYECTO_DIR

# Directorios principales
mkdir -p $DATOS_CRUDOS $DATOS_PROCESADOS $RESULTADOS $LOGS $SCRIPTS $DOCS

# Subdirectorios de datos crudos
mkdir -p $GENOMAS_RAW $SECUENCIAS_RAW $REFERENCIAS

# Subdirectorios de datos procesados
mkdir -p $ENSAMBLAJES $ALINEAMIENTOS $VARIANTES $ANOTACIONES

# Subdirectorios de resultados
mkdir -p $FILOGENIA $PANGENOMA $COMPARATIVO $REPORTES

# ========== CREAR ARCHIVOS DE CONFIGURACIÓN ==========

# Crear README.md
cat > $PROYECTO_DIR/README.md << 'EOF'
# Análisis de Genomas Bacterianos

## Información General
- **Nombre del Proyecto:** [Ingresar aquí]
- **Fecha de Creación:** [Fecha]
- **Investigador Responsable:** [Nombre]
- **Institución:** Facultad de Ciencias de la Salud - Programa de Biología

## Descripción del Proyecto
[Descripción del análisis y objetivos]

## Estructura de Directorios
- `datos_crudos/`: Datos originales sin procesar
  - `genomas_ensamblados/`: Archivos FASTA
  - `secuencias_raw/`: Archivos FASTQ
  - `referencias/`: Genomas de referencia
- `datos_procesados/`: Datos intermedios tras procesamiento
  - `ensamblajes/`: Genomas ensamblados
  - `alineamientos/`: Archivos BAM/SAM
  - `variantes/`: Archivos VCF
  - `anotaciones/`: Archivos GFF/GTF
- `resultados/`: Análisis finales
  - `analisis_filogenetico/`: Árboles filogenéticos
  - `pangenoma/`: Análisis de pangenoma
  - `comparativo/`: Análisis comparativos
  - `reportes/`: Reportes y visualizaciones
- `logs/`: Registros de ejecución
- `scripts/`: Scripts de automatización
- `documentacion/`: Información del proyecto

## Instrucciones de Uso
1. Agregar datos en `datos_crudos/`
2. Ejecutar scripts de análisis desde `scripts/`
3. Revisar resultados en `resultados/`
4. Consultar logs en `logs/` si hay errores
EOF

# Crear metadata.txt
cat > $PROYECTO_DIR/metadata.txt << EOF
==============================================
INFORMACIÓN DEL PROYECTO
==============================================
Nombre: $NOMBRE_PROYECTO
Fecha de creación: $FECHA a las $HORA
Usuario: $USUARIO
Directorio base: $PROYECTO_DIR
Versión del proyecto: $VERSION_PROYECTO

==============================================
INFORMACIÓN DEL SISTEMA
==============================================
Sistema operativo: $(uname -s)
Versión del kernel: $(uname -r)
Shell: $SHELL
Bash versión: $BASH_VERSION

==============================================
DIRECTORIOS PRINCIPALES
==============================================
Datos crudos: $DATOS_CRUDOS
Datos procesados: $DATOS_PROCESADOS
Resultados: $RESULTADOS
Logs: $LOGS
Scripts: $SCRIPTS
Documentación: $DOCS

==============================================
SUBDIRECTORIOS ESPECIALIZADOS
==============================================
Genomas ensamblados: $GENOMAS_RAW
Secuencias raw: $SECUENCIAS_RAW
Referencias: $REFERENCIAS
Ensamblajes: $ENSAMBLAJES
Alineamientos: $ALINEAMIENTOS
Variantes: $VARIANTES
Anotaciones: $ANOTACIONES
EOF

# Crear archivo de log inicial
cat > $LOGS/inicio_proyecto.log << EOF
[$(date +"%Y-%m-%d %H:%M:%S")] === PROYECTO INICIALIZADO ===
Usuario: $USUARIO
Directorio: $PROYECTO_DIR
Estructura de directorios creada exitosamente.
Versión del proyecto: $VERSION_PROYECTO
EOF

# ========== MOSTRAR RESUMEN ==========
echo ""
echo "=========================================="
echo "✓ PROYECTO CREADO EXITOSAMENTE"
echo "=========================================="
echo ""
echo "INFORMACIÓN DEL PROYECTO:"
echo "  Nombre: $NOMBRE_PROYECTO"
echo "  Ubicación: $PROYECTO_DIR"
echo "  Usuario: $USUARIO"
echo "  Fecha: $FECHA a las $HORA"
echo ""
echo "ESTRUCTURA DE DIRECTORIOS CREADA:"
echo "├── datos_crudos/"
echo "│   ├── genomas_ensamblados/"
echo "│   ├── secuencias_raw/"
echo "│   └── referencias/"
echo "├── datos_procesados/"
echo "│   ├── ensamblajes/"
echo "│   ├── alineamientos/"
echo "│   ├── variantes/"
echo "│   └── anotaciones/"
echo "├── resultados/"
echo "│   ├── analisis_filogenetico/"
echo "│   ├── pangenoma/"
echo "│   ├── comparativo/"
echo "│   └── reportes/"
echo "├── logs/"
echo "├── scripts/"
echo "└── documentacion/"
echo ""
echo "ARCHIVOS CREADOS AUTOMÁTICAMENTE:"
echo "  ✓ README.md (descripción del proyecto)"
echo "  ✓ metadata.txt (información de configuración)"
echo "  ✓ logs/inicio_proyecto.log (registro inicial)"
echo ""
echo "PRÓXIMOS PASOS:"
echo "1. cd $PROYECTO_DIR"
echo "2. Agregar archivos en datos_crudos/"
echo "3. Crear scripts de análisis en scripts/"
echo "4. Documentar avances en documentacion/"
echo ""
echo "=========================================="
```

**Características de la Versión 3:**
- ✅ Acepta nombre de proyecto como argumento (`$1`)
- ✅ Crea estructura completa con 15+ subdirectorios
- ✅ Genera archivos de configuración automáticos
- ✅ Registra información en log
- ✅ Muestra resumen visual detallado
- ✅ Usa variables para todo (fácil mantener)

**Ejecución:**
```bash
# Crear proyecto con nombre personalizado
bash setup_proyecto.sh "Mi_Estudio_Genoma_2025"

# O usar nombre por defecto
bash setup_proyecto.sh
```

---

## 4. CASO PRÁCTICO 2: ANÁLISIS DE GENOMAS Y GENERACIÓN DE REPORTES

### Objetivo
Crear scripts que **analicen archivos genómicos** (FASTA) y generen reportes automáticos, con progresión de complejidad.

---

### Versión 1: Análisis Muy Simple

```bash
#!/bin/bash
# Script: analizar_genoma_v1.sh
# Descripción: Análisis básico de un archivo FASTA
# Autor: [Tu nombre]
# Fecha: 2025-02-XX
# Uso: bash analizar_genoma_v1.sh archivo.fasta

# Variables
ARCHIVO_FASTA="$1"
NOMBRE_MUESTRA=$(basename $ARCHIVO_FASTA .fasta)

# Información básica
echo "=== ANÁLISIS BÁSICO DE FASTA ==="
echo "Archivo: $ARCHIVO_FASTA"
echo "Tamaño: $(du -h $ARCHIVO_FASTA | cut -f1)"
echo "Número de secuencias: $(grep -c '^>' $ARCHIVO_FASTA)"
echo "Nombre de muestra: $NOMBRE_MUESTRA"
```

**Ejecución:**
```bash
bash analizar_genoma_v1.sh bacteria_001.fasta
```

---

### Versión 2: Análisis Ampliado con Múltiples Métricas

```bash
#!/bin/bash
# Script: analizar_genoma_v2.sh
# Descripción: Análisis completo de archivo FASTA genómico
# Autor: [Tu nombre]
# Fecha: 2025-02-XX
# Uso: bash analizar_genoma_v2.sh archivo.fasta

# ========== VARIABLES DE ENTRADA ==========
ARCHIVO_FASTA="$1"
NOMBRE_MUESTRA=$(basename $ARCHIVO_FASTA .fasta)

# ========== VARIABLES DE FECHA Y USUARIO ==========
FECHA=$(date +"%Y-%m-%d")
HORA=$(date +"%H:%M:%S")
USUARIO=$(whoami)
DIRECTORIO_TRABAJO=$(pwd)

# ========== VARIABLES DE CÁLCULOS ==========
TAMAÑO_BYTES=$(wc -c < $ARCHIVO_FASTA)
TAMAÑO_MB=$(echo "scale=2; $TAMAÑO_BYTES / 1048576" | bc)
NUM_SECUENCIAS=$(grep -c "^>" $ARCHIVO_FASTA)
NUM_LINEAS=$(wc -l < $ARCHIVO_FASTA)

# Calcular longitud total del genoma
LONGITUD_TOTAL=0
while IFS= read -r linea; do
    if [[ ! "$linea" =~ ^">" ]]; then
        LONGITUD_TOTAL=$((LONGITUD_TOTAL + ${#linea}))
    fi
done < $ARCHIVO_FASTA

# ========== CREAR DIRECTORIO DE RESULTADOS ==========
DIRECTORIO_RESULTADOS="resultados_${NOMBRE_MUESTRA}_${FECHA}"
mkdir -p $DIRECTORIO_RESULTADOS

# ========== GENERAR REPORTE ==========
ARCHIVO_REPORTE="$DIRECTORIO_RESULTADOS/reporte_analisis.txt"

cat > $ARCHIVO_REPORTE << EOF
==============================================
REPORTE DE ANÁLISIS DE GENOMA BACTERIANO
==============================================

INFORMACIÓN DEL ANÁLISIS
------------------------
Fecha de análisis: $FECHA a las $HORA
Usuario que ejecutó: $USUARIO
Directorio de trabajo: $DIRECTORIO_TRABAJO

INFORMACIÓN DEL ARCHIVO
------------------------
Nombre de archivo: $ARCHIVO_FASTA
Nombre de muestra: $NOMBRE_MUESTRA
Ruta completa: $(readlink -f $ARCHIVO_FASTA)

ESTADÍSTICAS DEL ARCHIVO
------------------------
Tamaño en bytes: $TAMAÑO_BYTES
Tamaño en MB: $TAMAÑO_MB MB
Número de líneas: $NUM_LINEAS

ESTADÍSTICAS GENÓMICAS
------------------------
Número de secuencias: $NUM_SECUENCIAS
Longitud total del genoma: $LONGITUD_TOTAL pb
Longitud promedio por secuencia: $((LONGITUD_TOTAL / NUM_SECUENCIAS)) pb

==============================================
FIN DEL REPORTE
==============================================
EOF

# ========== MOSTRAR RESUMEN EN PANTALLA ==========
echo ""
echo "=========================================="
echo "✓ ANÁLISIS COMPLETADO"
echo "=========================================="
echo ""
echo "ARCHIVO ANALIZADO: $ARCHIVO_FASTA"
echo ""
echo "RESULTADOS GENERALES:"
echo "  Número de secuencias: $NUM_SECUENCIAS"
echo "  Longitud total: $LONGITUD_TOTAL pb"
echo "  Longitud promedio: $((LONGITUD_TOTAL / NUM_SECUENCIAS)) pb"
echo "  Tamaño del archivo: $TAMAÑO_MB MB"
echo ""
echo "REPORTE GUARDADO EN:"
echo "  $ARCHIVO_REPORTE"
echo ""
echo "=========================================="
```

**Características:**
- ✅ Calcula múltiples métricas (tamaño, número de secuencias, longitud total)
- ✅ Crea directorio de resultados dinámicamente
- ✅ Genera reporte en archivo de texto
- ✅ Muestra resumen en pantalla
- ✅ Usa expansión de comandos (`$()`)
- ✅ Buena documentación

**Ejecución:**
```bash
bash analizar_genoma_v2.sh bacteria_001.fasta
```

**Output esperado:**
```
==========================================
✓ ANÁLISIS COMPLETADO
==========================================

ARCHIVO ANALIZADO: bacteria_001.fasta

RESULTADOS GENERALES:
  Número de secuencias: 5
  Longitud total: 4850 pb
  Longitud promedio: 970 pb
  Tamaño del archivo: 0.00 MB

REPORTE GUARDADO EN:
  resultados_bacteria_001_2025-02-15/reporte_analisis.txt

==========================================
```

---

### Versión 3: Análisis Profesional con Directorios de Proyecto

La versión final integra variables de proyecto completo:

```bash
#!/bin/bash
# Script: analizar_genoma.sh
# Descripción: Análisis profesional de genomas bacterianos
# Autor: [Tu nombre]
# Fecha: 2025-02-XX
# Uso: bash analizar_genoma.sh archivo.fasta [directorio_proyecto]

# ========== VALIDAR ARGUMENTOS ==========
if [ -z "$1" ]; then
    echo "Uso: bash $0 archivo.fasta [directorio_proyecto]"
    exit 1
fi

# ========== VARIABLES DE ENTRADA ==========
ARCHIVO_FASTA="$1"
DIRECTORIO_PROYECTO="${2:-.}"

# Validar que el archivo existe
if [ ! -f "$ARCHIVO_FASTA" ]; then
    echo "ERROR: Archivo no encontrado: $ARCHIVO_FASTA"
    exit 1
fi

# ========== VARIABLES DE IDENTIFICACIÓN ==========
NOMBRE_MUESTRA=$(basename $ARCHIVO_FASTA .fasta)
FECHA=$(date +"%Y-%m-%d")
HORA=$(date +"%H:%M:%S")
USUARIO=$(whoami)
HOSTNAME=$(hostname)

# ========== VARIABLES DE DIRECTORIOS DEL PROYECTO ==========
DATA_PROCESSED="$DIRECTORIO_PROYECTO/datos_procesados"
RESULTS="$DIRECTORIO_PROYECTO/resultados"
LOGS="$DIRECTORIO_PROYECTO/logs"

# Crear directorios si no existen
mkdir -p $DATA_PROCESSED
mkdir -p $RESULTS
mkdir -p $LOGS

# ========== VARIABLES DE SALIDA ==========
ARCHIVO_PROCESADO="$DATA_PROCESSED/${NOMBRE_MUESTRA}_analizado.fasta"
DIRECTORIO_RESULTADOS="$RESULTS/${NOMBRE_MUESTRA}"
ARCHIVO_REPORTE="$DIRECTORIO_RESULTADOS/reporte_completo.txt"
ARCHIVO_LOG="$LOGS/analisis_${NOMBRE_MUESTRA}_${FECHA}.log"
ARCHIVO_ESTADISTICAS="$DIRECTORIO_RESULTADOS/estadisticas.csv"

# Crear directorio de resultados específico
mkdir -p $DIRECTORIO_RESULTADOS

# ========== CALCULAR ESTADÍSTICAS ==========
# Información general del archivo
TAMAÑO_BYTES=$(wc -c < $ARCHIVO_FASTA)
TAMAÑO_MB=$(echo "scale=3; $TAMAÑO_BYTES / 1048576" | bc)
NUM_SECUENCIAS=$(grep -c "^>" $ARCHIVO_FASTA)
NUM_LINEAS=$(wc -l < $ARCHIVO_FASTA)

# Calcular longitud total y estadísticas GC
LONGITUD_TOTAL=0
BASES_G=0
BASES_C=0

while IFS= read -r linea; do
    if [[ ! "$linea" =~ ^">" ]]; then
        LONGITUD_TOTAL=$((LONGITUD_TOTAL + ${#linea}))
        BASES_G=$((BASES_G + $(echo $linea | grep -o 'G' | wc -l)))
        BASES_G=$((BASES_G + $(echo $linea | grep -o 'g' | wc -l)))
        BASES_C=$((BASES_C + $(echo $linea | grep -o 'C' | wc -l)))
        BASES_C=$((BASES_C + $(echo $linea | grep -o 'c' | wc -l)))
    fi
done < $ARCHIVO_FASTA

GC_TOTAL=$((BASES_G + BASES_C))
PORCENTAJE_GC=$(echo "scale=2; ($GC_TOTAL * 100) / $LONGITUD_TOTAL" | bc)

# ========== REGISTRAR EN LOG ==========
cat > $ARCHIVO_LOG << EOF
[$(date +"%Y-%m-%d %H:%M:%S")] Inicio de análisis
Usuario: $USUARIO
Hostname: $HOSTNAME
Archivo: $ARCHIVO_FASTA
Nombre de muestra: $NOMBRE_MUESTRA

[$(date +"%Y-%m-%d %H:%M:%S")] Análisis completado exitosamente
Secuencias: $NUM_SECUENCIAS
Longitud total: $LONGITUD_TOTAL pb
Contenido GC: $PORCENTAJE_GC%
EOF

# ========== GENERAR REPORTE COMPLETO ==========
cat > $ARCHIVO_REPORTE << EOF
==============================================
REPORTE COMPLETO DE ANÁLISIS DE GENOMA
==============================================

INFORMACIÓN DEL ANÁLISIS
------------------------
Fecha: $FECHA a las $HORA
Usuario: $USUARIO
Computadora: $HOSTNAME
Nombre de muestra: $NOMBRE_MUESTRA

UBICACIÓN DE ARCHIVOS
------------------------
Archivo original: $ARCHIVO_FASTA
Resultados: $DIRECTORIO_RESULTADOS
Log de ejecución: $ARCHIVO_LOG

INFORMACIÓN DEL ARCHIVO
------------------------
Tamaño: $TAMAÑO_BYTES bytes ($TAMAÑO_MB MB)
Número de líneas: $NUM_LINEAS

ESTADÍSTICAS GENÓMICAS
------------------------
Número de secuencias: $NUM_SECUENCIAS
Longitud total: $LONGITUD_TOTAL pb
Longitud promedio por secuencia: $((LONGITUD_TOTAL / NUM_SECUENCIAS)) pb

Bases G: $BASES_G
Bases C: $BASES_C
Contenido GC total: $PORCENTAJE_GC%

==============================================
EOF

# ========== GENERAR ARCHIVO CSV DE ESTADÍSTICAS ==========
cat > $ARCHIVO_ESTADISTICAS << EOF
Parámetro,Valor
Nombre de muestra,$NOMBRE_MUESTRA
Fecha de análisis,$FECHA
Número de secuencias,$NUM_SECUENCIAS
Longitud total (pb),$LONGITUD_TOTAL
Longitud promedio (pb),$((LONGITUD_TOTAL / NUM_SECUENCIAS))
Contenido GC (%),$PORCENTAJE_GC
Tamaño del archivo (MB),$TAMAÑO_MB
EOF

# ========== MOSTRAR RESUMEN FINAL ==========
echo ""
echo "╔══════════════════════════════════════════╗"
echo "║ ✓ ANÁLISIS COMPLETADO EXITOSAMENTE      ║"
echo "╚══════════════════════════════════════════╝"
echo ""
echo "📊 INFORMACIÓN GENERAL:"
echo "   Muestra: $NOMBRE_MUESTRA"
echo "   Archivo: $ARCHIVO_FASTA"
echo ""
echo "📈 ESTADÍSTICAS GENÓMICAS:"
echo "   Secuencias: $NUM_SECUENCIAS"
echo "   Longitud total: $LONGITUD_TOTAL pb"
echo "   Longitud promedio: $((LONGITUD_TOTAL / NUM_SECUENCIAS)) pb"
echo "   Contenido GC: $PORCENTAJE_GC%"
echo ""
echo "💾 ARCHIVOS GENERADOS:"
echo "   ✓ Reporte: $ARCHIVO_REPORTE"
echo "   ✓ Estadísticas CSV: $ARCHIVO_ESTADISTICAS"
echo "   ✓ Log: $ARCHIVO_LOG"
echo ""
echo "╚══════════════════════════════════════════╝"
```

**Características profesionales:**
- ✅ Validación de argumentos
- ✅ Integración con estructura de proyecto
- ✅ Calcula contenido GC (relevante en bioinformática)
- ✅ Genera múltiples formatos de salida (TXT, CSV)
- ✅ Registro detallado en log
- ✅ Interfaz visual mejorada con emojis

**Ejecución:**
```bash
bash analizar_genoma.sh bacteria_001.fasta ~/proyectos/Mi_Proyecto_2025
```

---

## 5. ACTIVIDADES PRÁCTICAS

### Actividad 1: Crear Estructura de Proyecto (30 minutos)
1. Ejecutar `setup_proyecto.sh` con nombre personalizado
2. Verificar que se crearon todos los directorios:
   ```bash
   tree proyecto_genoma_bacteriano/
   # o
   find proyecto_genoma_bacteriano/ -type d
   ```
3. Examinar contenido de `README.md` y `metadata.txt`
4. Consultar el log en `logs/inicio_proyecto.log`

### Actividad 2: Analizar un Archivo FASTA (25 minutos)
1. Crear archivo FASTA de prueba:
   ```bash
   cat > bacteria_test.fasta << 'EOF'
   >secuencia_1
   ATGCGATCGATCGATCGATCG
   >secuencia_2
   GCTAGCTAGCTAGCTAGCTA
   >secuencia_3
   TTAATTAATTAATTAATTAA
   EOF
   ```
2. Ejecutar `analizar_genoma_v2.sh bacteria_test.fasta`
3. Revisar reporte generado

### Actividad 3: Integración Completa (20 minutos)
1. Crear un nuevo proyecto con `setup_proyecto.sh`
2. Copiar archivo FASTA a `datos_crudos/genomas_ensamblados/`
3. Ejecutar `analizar_genoma.sh` proporcionando ruta del proyecto
4. Verificar que se crearon archivos en `datos_procesados/` y `resultados/`

---

## 6. RESUMEN DE CONCEPTOS

### De Versión 1 a Versión 3: Progresión de Complejidad

| Aspecto | V1 | V2 | V3 |
|---------|----|----|-----|
| Flexibilidad | Fija | Variables | Completa |
| Directorios | 3 | 5 | 15+ |
| Archivos generados | 0 | 1 | 4 |
| Validación | No | No | Sí |
| Integración de proyecto | No | Parcial | Completa |

---

## 7. PALABRAS CLAVE

- **Automatización:** Usar scripts para ejecutar tareas repetitivas
- **Estructura de Proyecto:** Organización estándar de directorios
- **Variables:** Contenedores reutilizables de información
- **Escalabilidad:** Capacidad de manejar múltiples proyectos/muestras
- **Documentación:** Registros de lo realizado (logs, reportes)

---

## 8. REFERENCIAS BIBLIOGRÁFICAS

1. Hausenblas, M. (2022). *Learning Modern Linux*. O'Reilly Media, Inc.
2. Blum, R., & Bresnahan, C. (2021). *Linux command line and Shell scripting bible*. Wiley.
3. Hsu, J. (2020). *Bioinformatics Data Skills*. O'Reilly Media, Inc.
