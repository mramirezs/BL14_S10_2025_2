# Elementos de la Sintaxis de Shell Scripting I

## Información del Curso
- **Programa:** Biología - Bioinformática
- **Curso:** Principios de programación
- **Docentes:** Frank Guzman Escudero, Manuel Ramírez Sáenz
- **Semestre:** 2025-02

## Logro de la Sesión
Al finalizar la sesión, el estudiante será capaz de:
- Definir y utilizar variables en shell scripting
- Diferenciar entre variables locales y globales
- Comprender el concepto de shell vs subshell
- Manipular la variable de entorno PATH
- Aplicar variables especiales en scripts bioinformáticos

**Palabras clave:** Shell Scripting, Variables, Definición de variables, Tipos de variables, Variable PATH

---

## 1. CONCEPTOS FUNDAMENTALES

### ¿Qué es una Variable?
Una **variable** es una ubicación en memoria donde se almacena un valor que puede ser:
- **Creado** durante la ejecución
- **Modificado** según las necesidades
- **Guardado** para uso posterior
- **Eliminado** cuando ya no se necesita

**Características importantes:**
- Son de naturaleza múltiple (pueden almacenar diferentes tipos de datos)
- Son dinámicas (su valor puede cambiar durante la ejecución)

### Sintaxis Básica
```bash
variable=valor
```
⚠️ **Importante:** No debe haber espacios alrededor del signo `=`

### Acceso a Variables
Para acceder al valor de una variable, se utiliza el símbolo `$`:
```bash
echo $nombre_variable
```

---

## 2. TIPOS DE VARIABLES

### Variables Locales
- **Alcance:** Solo accesibles en el shell o subshell donde se definieron
- **Sintaxis:** `variable=valor`
- **Uso:** Para operaciones temporales dentro de un script específico

```bash
nombre="Bioinformática"
echo $nombre
```

### Variables Globales (de Entorno)
- **Alcance:** Accesibles desde cualquier shell o subshell
- **Sintaxis:** `export variable=valor`
- **Uso:** Para configuraciones que deben persistir entre diferentes procesos

```bash
export LABORATORIO="Genómica"
echo $LABORATORIO
```

---

## 3. SHELL vs SUBSHELL

### Concepto de Shell Principal y Subshell
```
┌─────────────────┐
│  SHELL PADRE    │
│  (var=10)       │
│       │         │
│       ▼         │
│  ┌───────────┐  │
│  │ SUBSHELL  │  │
│  │ (var=20)  │  │
│  └───────────┘  │
└─────────────────┘
```

### Comportamiento de Variables:
- **Variables locales:** No se heredan a subshells
- **Variables globales:** Se heredan a subshells
- **Modificaciones en subshell:** No afectan al shell padre

### Comandos Importantes:
- `bash`: Crear un nuevo subshell
- `exit`: Salir del subshell actual
- `export`: Convertir variable local a global

---

## 4. VARIABLES DE ENTORNO GLOBALES

### Variables Importantes del Sistema

| Variable | Descripción |
|----------|-------------|
| `PATH` | Directorios donde el sistema busca ejecutables |
| `HOME` | Directorio personal del usuario |
| `USER` | Nombre del usuario actual |
| `SHELL` | Shell por defecto |
| `PWD` | Directorio de trabajo actual |

### Visualizar Variables de Entorno
```bash
# Ver una variable específica
echo $HOME
printenv HOME

# Ver todas las variables
printenv

# Ver primeras líneas
printenv | head
```

---

## 5. MANIPULACIÓN DE LA VARIABLE PATH

### ¿Qué es PATH?
PATH es una variable que contiene una lista de directorios separados por `:` donde el sistema busca comandos ejecutables.

### Agregar Directorios al PATH

#### Al final del PATH:
```bash
export PATH=$PATH:/nueva/ruta
```

#### Al inicio del PATH:
```bash
export PATH=/nueva/ruta:$PATH
```

### Ejemplo Práctico: Instalación de FastQC
```bash
# Crear directorio para herramientas
mkdir -p ~/Programs
cd ~/Programs

# Descargar FastQC
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.12.1.zip

# Descomprimir
unzip fastqc_v0.12.1.zip

# Agregar al PATH
export PATH=$PATH:~/Programs/FastQC

# Verificar
echo $PATH
```

---

## 6. OPERACIONES CON VARIABLES

### Comillas Simples vs Dobles

#### Comillas Dobles (`" "`)
- Permiten expansión de variables
```bash
nombre="Juan"
echo "Hola $nombre"  # Resultado: Hola Juan
```

#### Comillas Simples (`' '`)
- Texto literal, sin expansión
```bash
echo 'Hola $nombre'  # Resultado: Hola $nombre
```

### Operaciones Aritméticas
```bash
# Usando $(( ))
suma=$((13+8))
echo $suma  # Resultado: 21

# Usando $[ ]
suma=$[13+8]
echo $suma  # Resultado: 21

# Usando bc para decimales
resultado=$(echo "10.5 + 3.2" | bc)
echo $resultado  # Resultado: 13.7
```

### Expansión de Comandos
```bash
# Sintaxis moderna
usuario=$(whoami)
echo "El usuario actual es: $usuario"

# Sintaxis clásica (backticks)
usuario=`whoami`
echo "El usuario actual es: $usuario"

# Ejemplo con listado de archivos
archivos=$(ls -1h .)
echo "Archivos en el directorio: $archivos"
```

---

## 7. VARIABLES ESPECIALES

### Variables de Script

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `$0` | Nombre del script | `script.sh` |
| `$1, $2, ...` | Argumentos posicionales | `$1` = primer argumento |
| `$#` | Número total de argumentos | `3` |
| `$$` | PID del shell actual | `1234` |
| `$?` | Código de salida del último comando | `0` (éxito) |

### Ejemplo de Script con Variables Especiales
```bash
#!/bin/bash
echo "Nombre del script: $0"
echo "Primer argumento: $1"
echo "Segundo argumento: $2"
echo "Número de argumentos: $#"
echo "PID del proceso: $$"
```

---

## 8. EJERCICIOS PRÁCTICOS BIOINFORMÁTICOS

### Ejercicio 1: Script de Información de Sesión
**Objetivo:** Crear un script que muestre información de la sesión actual.

```bash
#!/bin/bash
# Archivo: info_sesion.sh

# Variable local
año=2025

# Mostrar información
echo "=== INFORMACIÓN DE SESIÓN ==="
echo "Usuario: $USER"
echo "Directorio home: $HOME"
echo "Shell actual: $SHELL"
echo "Semestre actual: ${año}_2"
echo "Número de argumentos: $#"
echo "PID del proceso: $$"
```

### Ejercicio 2: Configuración de Herramientas Bioinformáticas
**Objetivo:** Script para mostrar la configuración del entorno de trabajo.

```bash
#!/bin/bash
# Archivo: config_biotools.sh

# Definir directorios
BIOTOOLS_DIR="$HOME/biotools"
FASTQC_DIR="$BIOTOOLS_DIR/FastQC"
SAMTOOLS_DIR="$BIOTOOLS_DIR/samtools"

# Crear directorios
mkdir -p $BIOTOOLS_DIR

# Mostrar configuración actual
echo "=== CONFIGURACIÓN DE HERRAMIENTAS BIOINFORMÁTICAS ==="
echo "Directorio de herramientas: $BIOTOOLS_DIR"
echo "Directorio FastQC: $FASTQC_DIR"
echo "Directorio SAMtools: $SAMTOOLS_DIR"
echo "PATH actual:"
echo $PATH

# Agregar al PATH
export PATH="$PATH:$FASTQC_DIR:$SAMTOOLS_DIR"

echo "Nuevo PATH:"
echo $PATH
```

### Ejercicio 3: Procesamiento de Archivos FASTA
**Objetivo:** Script para analizar archivos de secuencias.

```bash
#!/bin/bash
# Archivo: analizar_fasta.sh

# Variables
archivo_fasta=$1
nombre_base=$(basename $archivo_fasta .fasta)
fecha_actual=$(date +"%Y-%m-%d")
usuario_actual=$(whoami)

# Información del archivo
echo "=== ANÁLISIS DE ARCHIVO FASTA ==="
echo "Script ejecutado por: $usuario_actual"
echo "Fecha de análisis: $fecha_actual"
echo "Archivo: $archivo_fasta"
echo "Nombre base: $nombre_base"
echo "Tamaño del archivo: $(du -h $archivo_fasta | cut -f1)"
echo "Argumentos recibidos: $#"

# Contar secuencias
num_secuencias=$(grep -c "^>" $archivo_fasta)
echo "Número de secuencias: $num_secuencias"

# Crear directorio de resultados
directorio_resultados="resultados_${nombre_base}_${fecha_actual}"
mkdir -p $directorio_resultados
echo "Directorio de resultados: $directorio_resultados"

# Crear archivo de resumen
resumen="$directorio_resultados/resumen.txt"
echo "Archivo: $archivo_fasta" > $resumen
echo "Fecha: $fecha_actual" >> $resumen
echo "Usuario: $usuario_actual" >> $resumen
echo "Secuencias: $num_secuencias" >> $resumen
echo "Resumen guardado en: $resumen"
```

---

## 9. ACTIVIDADES PRÁCTICAS

### Actividad 1: Variables Básicas (15 minutos)
1. Crear un script llamado `mi_laboratorio.sh`
2. Definir variables para:
   - Nombre del laboratorio: `laboratorio="Genómica Computacional"`
   - Tipo de análisis: `analisis="RNA-seq"`
   - Año actual: `año=2025`
3. Mostrar un mensaje usando estas variables:
```bash
echo "Laboratorio: $laboratorio"
echo "Análisis: $analisis"
echo "Año: $año"
echo "Proyecto: ${laboratorio}_${analisis}_${año}"
```

### Actividad 2: Manipulación del PATH (20 minutos)
1. Descargar SAMtools usando wget
2. Descomprimir en directorio ~/biotools/
3. Agregar SAMtools al PATH usando export
4. Verificar con `echo $PATH`
5. Probar ejecutando `samtools` (debería mostrar ayuda)

### Actividad 3: Variables Especiales (15 minutos)
1. Crear un script llamado `info_script.sh`
2. El script debe mostrar:
   - Nombre del script (`$0`)
   - Todos los argumentos (`$1`, `$2`, `$3`)
   - Número total de argumentos (`$#`)
   - PID del proceso (`$$`)
3. Ejecutar con: `bash info_script.sh datos.fasta resultados output`

### Actividad 4: Expansión de Comandos (10 minutos)
1. Crear variables usando expansión de comandos:
```bash
fecha_hoy=$(date)
directorio_actual=$(pwd)
numero_archivos=$(ls | wc -l)
usuario=$(whoami)
```
2. Mostrar toda la información en un formato organizado

---

## 10. BUENAS PRÁCTICAS

### Nomenclatura de Variables
- **Variables globales:** MAYÚSCULAS (`GENOME_DIR`, `FASTQ_PATH`)
- **Variables locales:** minúsculas (`archivo_temp`, `contador`)
- **Nombres descriptivos:** `datos_secuenciacion` en lugar de `datos`
- **Sin espacios:** usar guiones bajos `_`

### Uso de Comillas
```bash
# Siempre usar comillas para valores con espacios
directorio="Mi Proyecto de Genómica"

# Usar comillas dobles para permitir expansión
mensaje="Hola $USER, bienvenido"

# Usar comillas simples para texto literal
comando='echo $HOME'  # No expande $HOME
```

### Comentarios y Documentación
```bash
#!/bin/bash
# Propósito: Análisis de calidad de secuencias
# Autor: [Tu nombre]
# Fecha: [Fecha]
# Uso: ./script.sh archivo.fastq

# Definir variables importantes
readonly SCRIPT_NAME=$(basename $0)
readonly VERSION="1.0"
```
---

## 10. AUTOEVALUACIÓN

### Preguntas de Comprensión
1. **¿Cuál es la diferencia entre `variable=valor` y `export variable=valor`?**
   - Respuesta: La primera crea una variable local, la segunda la convierte en global

2. **¿Por qué una variable definida en un subshell no afecta al shell padre?**
   - Respuesta: Los subshells son procesos independientes con su propio espacio de memoria

3. **¿Qué hace el comando `echo $PATH | tr ':' '\n'`?**
   - Respuesta: Muestra cada directorio del PATH en una línea separada

4. **¿Cuándo usarías comillas simples vs comillas dobles?**
   - Respuesta: Dobles para permitir expansión de variables, simples para texto literal

5. **¿Qué variable especial contiene el número de argumentos pasados a un script?**
   - Respuesta: `$#`

### Ejercicio Final
Crear un script completo que:
1. Defina variables para un proyecto de análisis genómico
2. Use variables especiales para mostrar información del script
3. Utilice expansión de comandos para obtener información del sistema
4. Cree directorios usando las variables definidas
5. Muestre un resumen completo de la configuración
---

*Esta clase proporciona las bases fundamentales para el manejo de variables en shell scripting, preparando a los estudiantes para automatizar tareas básicas en bioinformática sin utilizar estructuras de control complejas.*
