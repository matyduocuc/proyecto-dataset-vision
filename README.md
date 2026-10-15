# DataOps: Ingesta Colaborativa de Dataset para Visión Artificial

Este repositorio contiene la arquitectura para la recolección de imágenes utilizando Git (código) y DVC (datos), conectados a Google Drive. Cada estudiante recolectará datos de entre 5 y 30 personas distintas (aproximadamente 10 fotos por persona).

## Pre-requisitos
- Python 3.10 o superior.
- Cuenta en GitHub y Git instalado en tu equipo.
- ID de Cliente y Secreto de Cliente de GCP (entregados por el profesor).

---

## Paso 1: Fork y Configuración del Entorno

Debes trabajar en tu propia copia del repositorio para luego proponer los cambios.

1. Ve a la página del repositorio en GitHub y haz clic en el botón **"Fork"** (esquina superior derecha).
2. Abre tu terminal y clona **tu copia** (reemplaza `TU_USUARIO` por tu usuario de GitHub):
```bash
git clone [https://github.com/TU_USUARIO/proyecto-dataset-vision.git](https://github.com/TU_USUARIO/proyecto-dataset-vision.git)
cd proyecto-dataset-vision
```

3. Crea y activa un entorno virtual. En Windows (CMD/PowerShell):
```bash
python -m venv .venv
.venv\Scripts\activate
```
*(En Mac/Linux usa `python3 -m venv .venv` y `source .venv/bin/activate`)*

4. Instala las dependencias:
```bash
pip install -r requirements.txt
pip install --upgrade pyOpenSSL cryptography
```

---

## Paso 2: Autenticación y Descarga de DVC (Pull)

1. Configura las credenciales locales para Google Drive:
```bash
dvc remote modify --local almacenamiento gdrive_client_id "ID_DEL_PROFESOR"
dvc remote modify --local almacenamiento gdrive_client_secret "SECRETO_DEL_PROFESOR"
```

2. Descarga el dataset base:
```bash
dvc pull
```
*Nota: Se abrirá tu navegador. Inicia sesión con la cuenta correspondiente, haz clic en Configuración Avanzada y acepta los permisos.*

---

## Paso 3: Aportar tus Imágenes (La Estructura)

Las imágenes y sus registros deben seguir una nomenclatura estricta para integrarse correctamente al conjunto de datos principal.

1. **Estructura de carpetas e imágenes:**
   - Ve a la ruta `data/images/`.
   - Crea una subcarpeta por cada sujeto fotografiado directamente en `images/`. El nombre de la carpeta (`id_carpeta`) debe estar formado por las iniciales del sujeto fotografiado, seguidas de `_01`. (Ejemplo: Para un sujeto llamado Carlos Gómez, la carpeta será `cg_01`).
   - *Nota:* Revisa las carpetas existentes. Si ya existe una carpeta con las mismas iniciales (ej. `cg_01`), utiliza el número correlativo siguiente para tu sujeto (`cg_02`).
   - Pega las fotos en la subcarpeta del sujeto correspondiente. Nombra cada imagen con el ID de la carpeta y un correlativo (ejemplo: `cg_01_01.jpg`, `cg_01_02.jpg`). 
   - Formatos permitidos: `.jpg`, `.jpeg` o `.png`.

2. **Registro de metadatos en el CSV:**
   - Abre el archivo **`sujetos.csv`** existente en el repositorio.
   - Ve al final del documento y agrega una fila nueva por cada sujeto fotografiado.
   - Los datos deben ir separados por comas (`,`), sin espacios adicionales.
   - Formato de las columnas: `id_carpeta,nombre_normalizado,nombre_original`
   - Ejemplo de fila: `cg_01,carlos_gomez,Carlos Gomez`

---

## Paso 4: Subir los datos (Push y Pull Request)

1. **Registra las imágenes en DVC y súbelas a Drive:**
```bash
dvc add data/images
dvc push
```
*(Este paso actualizará automáticamente el archivo de rastreo `data/images.dvc`)*

2. **Guarda los registros en Git y súbelos a tu Fork:**
```bash
git add data/images.dvc sujetos.csv
git commit -m "Agrega imágenes y metadatos de nuevos sujetos"
git push origin main
```

3. **Envía tu trabajo para revisión:**
   - Ve a tu cuenta de GitHub (a tu Fork).
   - Haz clic en el botón **"Contribute"** y luego en **"Open pull request"**.
   - Envía la solicitud para que el profesor revise e integre tu aporte al repositorio oficial.
