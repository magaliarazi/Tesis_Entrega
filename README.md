# MolParam — Parametrización Molecular Data-Driven para GROMOS

[![GROMACS](https://img.shields.io/badge/GROMACS-compatible-blue)](https://www.gromacs.org/)
[![Python 3.10](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/)

**Autoras:** Magali Arazi · Sofía Goldszer  
**Tutora:** Dra. Florencia Martini 
**Co- Tutor** Dr. Rodrigo Ramele  
**Institución:** ITBA 

## ¿Qué es MolParam?

MolParam es una herramienta de aprendizaje automático para asignar automáticamente **atom types** y **cargas parciales** de moléculas orgánicas pequeñas bajo el campo de fuerza **GROMOS (53A6/54A7)**, utilizado en simulaciones de dinámica molecular con [GROMACS](https://www.gromacs.org/).

La parametrización molecular es un paso necesario (y usualmente manual) antes de correr cualquier simulación MD. MolParam automatiza este proceso a partir de un archivo `.mol2`, generando como salida los parámetros compatibles con el ecosistema GROMACS/GROMOS.

### Modelos implementados

| Tarea | Representación | Modelo seleccionado |
|-------|---------------|---------------------|
| Clasificación de atom types | All-Atom (AA) | Random Forest (E3) | 
| Clasificación de atom types | United-Atom (UA) | Random Forest (E3) |
| Predicción de cargas parciales | All-Atom (AA) | TabTransformer | 
| Predicción de cargas parciales | United-Atom (UA) | Random Forest (E3) |

## Estructura del repositorio

```
Tesis_Entrega/
├── app/                  # Aplicación web Flask (MolParam)
├── version/              # Scripts de entrenamiento y análisis
├── archivos/             # Archivos .mol2 e .itp del dataset de entrenamiento
├── validation_mols/      # Moléculas de validación externa
└── environment.yml       # Entorno conda (Linux)
```

## Instalación

### Opción A: Solo correr la aplicación web

Requiere Python 3.10 y pip. Funciona en Windows, Linux y Mac.

```bash
# 1. Clonar el repositorio
git clone https://github.com/magaliarazi/Tesis_Entrega.git
cd Tesis_Entrega

# 2. Instalar dependencias
pip install flask rdkit scikit-learn pandas numpy torch
```

### Opción B (Recomendada): Reproducir el entrenamiento completo

Requiere Linux (o WSL en Windows) y [Miniconda](https://docs.conda.io/en/latest/miniconda.html).

```bash
# 1. Clonar el repositorio
git clone https://github.com/magaliarazi/Tesis_Entrega.git
cd Tesis_Entrega

# 2. Aceptar los términos de servicio de conda
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r

# 3. Crear el entorno conda con todas las dependencias
conda env create -f environment.yml

# 4. Activar el entorno
conda activate molparam
```
> **Nota:** El `environment.yml` fue generado en Linux. En Windows debe usarse [WSL](https://learn.microsoft.com/es-es/windows/wsl/install).

## Uso

### Aplicación web (recomendado)

Con el entorno activado (conda activate molparam), desde la raíz del repositorio:

```bash
cd app/
python app.py
```

Abrí el navegador en `http://localhost:5000`. Cargá tu archivo `.mol2` y obtenés los atom types y cargas parciales predichos.

### Scripts de entrenamiento

Con el entorno activado (conda activate molparam), desde la raíz del repositorio:

```bash
# Extraer el dataset desde los archivos .itp y .mol2
python version/extract_dataset_v3.py

# Entrenar modelos All-Atom
python version/model_AA.py

# Entrenar modelos United-Atom
python version/model_UA.py

# Entrenar TabTransformer (cargas parciales AA)
python version/TabTransformer.py

# Validación con moléculas externas al dataset
python version/validate_external.py
```
## Dataset

El dataset de entrenamiento fue construido a partir de **11 moléculas orgánicas** parametrizadas manualmente bajo GROMOS (53A6/54A7), extraídas de [ATB (Automated Topology Builder)](https://atb.uq.edu.au/):

- **All-Atom (AA):** 654 átomos  
- **United-Atom (UA):** 401 átomos

Las features moleculares se calcularon con [RDKit](https://www.rdkit.org/) a partir de archivos `.mol2`.

## Contacto

**Magali Arazi** — marazi@itba.edu.ar  
**Sofía Goldszer** — sgoldszer@itba.edu.ar

*Trabajo de tesis de grado — Bioingenieria, ITBA, 2026*
