# MolParam — Parametrización Molecular Data-Driven para GROMOS 54A7

MolParam es una herramienta de aprendizaje automático para asignar automáticamente **atom types** y **cargas parciales** de moléculas orgánicas pequeñas bajo el campo de fuerza **GROMOS 54A7**, utilizado en simulaciones de dinámica molecular con [GROMACS](https://www.gromacs.org/).

Este trabajo es una contribución al ecosistema abierto de GROMACS, orientada a automatizar parcialmente la etapa de parametrización molecular —uno de los pasos más costosos y especializados en la preparación de simulaciones de dinámica molecular.

> Proyecto Final de Carrera — Bioingeniería, ITBA (2026)  
> Autoras: Magali Arazi · Sofía Goldszer  
> Tutora: Dra. Florencia Martini · Co-tutor: Dr. Rodrigo Ramele

---

## ¿Qué es?

Dado un archivo `.mol2` con la estructura 3D de una molécula, el sistema predice átomo por átomo:

- **Atomtype** (tipo de átomo GROMOS 54A7): clasificación multiclase con Random Forest
- **Carga parcial** (en unidades de carga elemental, *e*): regresión con TabTransformer (esquema AA) y Random Forest (esquema UA)

El pipeline opera en dos esquemas de representación atómica:
- **AA (All-Atom)**: incluye todos los átomos, con hidrógenos explícitos
- **UA (United-Atom)**: los hidrógenos no polares se fusionan con su carbono vecino

### Modelos implementados

| Tarea | Representación | Modelo |
|-------|---------------|--------|
| Clasificación de atom types | All-Atom (AA) | Random Forest |
| Clasificación de atom types | United-Atom (UA) | Random Forest | 
| Predicción de cargas parciales | All-Atom (AA) | TabTransformer | 
| Predicción de cargas parciales | United-Atom (UA) | Random Forest | 

---

## Estructura del repositorio

```
Tesis_Entrega/
├── app/                  # Aplicación web Flask (MolParam)
├── version/              # Scripts de entrenamiento y análisis
├── archivos/             # Archivos .mol2 e .itp del dataset de entrenamiento
├── validation_mols/      # Moléculas de validación externa
└── environment.yml       # Entorno conda reproducible
```

---

## Instalación

### Requisitos previos
- [Anaconda](https://www.anaconda.com/) o [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
- GROMACS instalado (para simulaciones MD posteriores)

### Pasos

```bash
# 1. Clonar el repositorio
git clone https://github.com/magaliarazi/Tesis_Entrega.git
cd Tesis_Entrega

# 2. Crear el entorno conda con todas las dependencias
conda env create -f environment.yml

# 3. Activar el entorno
conda activate molparam
```

---

## Uso

### Aplicación web (recomendado)

```bash
cd app/
python app.py
```

Abre el navegador en `http://localhost:5000`. Cargá tu archivo `.mol2` y obtenés los atom types y cargas parciales predichos.

### Scripts de entrenamiento

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

---

## Dataset

El dataset de entrenamiento fue construido a partir de **11 moléculas orgánicas** parametrizadas manualmente bajo GROMOS 54A7, extraídas de [ATB (Automated Topology Builder)](https://atb.uq.edu.au/):

- **All-Atom (AA):** 654 átomos  
- **United-Atom (UA):** 401 átomos

Las features moleculares se calcularon con [RDKit](https://www.rdkit.org/) a partir de archivos `.mol2`.

---

## Contacto

**Magali Arazi** — marazi@itba.edu.ar  
**Sofía Goldszer** — sgoldszer@itba.edu.ar

*Trabajo de tesis de grado — Ingeniería Biomédica, ITBA, 2025*
