# Tesis — Detección de residuos reciclables con visión por computadora

> Repositorio de entrenamiento de la tesis profesional: comparación de tres arquitecturas de detección de objetos —**YOLOv5, Faster R-CNN y DETR**— sobre imágenes de residuos reciclables.
> La aplicación móvil resultante está en [APP_Tesis_rec](https://github.com/FranciscoAguilarCuadra/APP_Tesis_rec).

[![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)](https://pytorch.org/)
[![YOLOv5](https://img.shields.io/badge/YOLOv5-1a73e8?style=flat-square)](https://github.com/ultralytics/yolov5)

---

## Objetivo

Comparar el desempeño de tres arquitecturas de detección de objetos entrenadas con el mismo dataset de residuos reciclables, y seleccionar la de mejor balance velocidad/precisión para su integración en una aplicación móvil en tiempo real.

**Clases:** cartón · metal · papel · pilas · plástico · vidrio

## Modelos comparados

| Modelo | Tipo | Implementación | Configuración usada |
|--------|------|----------------|---------------------|
| **YOLOv5** | Detección en una etapa (tiempo real) | [ultralytics/yolov5](https://github.com/ultralytics/yolov5) | `yolov5x.pt`, 50 épocas, img 640, batch 16, hiperparámetros en `YOLO/hyp.textura.yaml` |
| **Faster R-CNN** | Detección en dos etapas (mayor precisión) | `torchvision` (ResNet-50 FPN) | 150 épocas, lr 1e-4, batch 8, formato COCO, datos en `FASTERRCNN/texturasFast.yaml` |
| **DETR** | Transformer (atención global) | PyTorch + módulos locales | Configuración en `DETR/config.json`, métricas con TensorBoard |

## Estructura

```
Repositorio_Tesis/
├── YOLO/
│   ├── train_yolo.py        # Orquesta el entrenamiento de YOLOv5 (subprocess → yolov5/train.py)
│   └── hyp.textura.yaml     # Hiperparámetros de entrenamiento
├── FASTERRCNN/
│   ├── train_faster.py      # Entrenamiento con torchvision, métricas F1/AP/matriz de confusión
│   └── texturasFast.yaml    # Configuración de datos
└── DETR/
    ├── train_DETR.py        # Entrenamiento del transformer con TensorBoard
    └── config.json          # Configuración del modelo
```

## Requisitos

- Python ≥ 3.8
- PyTorch + torchvision (con GPU CUDA recomendada)
- `scikit-learn`, `pandas`, `matplotlib`, `tqdm`, `tensorboard`
- Para YOLOv5: clonar el repositorio oficial dentro de `YOLO/`:
  ```bash
  git clone https://github.com/ultralytics/yolov5.git YOLO/yolov5
  pip install -r YOLO/yolov5/requirements.txt
  ```

## Dataset

Las imágenes y anotaciones de los residuos reciclables **no están incluidas** en este repositorio por tamaño. Los scripts esperan:

- `YOLO/data/data.yaml` + `YOLO/data/images/` (formato YOLOv5)
- `FASTERRCNN/data/images/` + `FASTERRCNN/data/annotations/` (formato COCO)

> **Nota:** el script de DETR requiere además los módulos locales `model.py`, `dataset_loader.py` y `utils.py`.

## Resultados

YOLOv5 resultó el modelo de mejor desempeño para ejecución en tiempo real desde un dispositivo móvil, por lo que fue el integrado en la aplicación [RecyclAIDeep (APP_Tesis_rec)](https://github.com/FranciscoAguilarCuadra/APP_Tesis_rec).

## Autor

**Francisco Aguilar Cuadra** — Ingeniero Civil Informático, Universidad del Bío-Bío (2025)
[GitHub](https://github.com/FranciscoAguilarCuadra) · [LinkedIn](https://linkedin.com/in/francisco-aguilar-cuadra)
