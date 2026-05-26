<p align="center">
  <img src="./banner.png" alt="Identificación de especies a partir de audio, centrada en aves, anfibios, mamíferos e insectos del Valle Medio del Magdalena en Colombia.
" width="100%">
</p>

# Identificación de especies a partir de audio, centrada en aves, anfibios, mamíferos e insectos del Valle Medio del Magdalena en Colombia.

Este repositorio contiene el desarrollo y evaluación de un clasificador de audio basado en Machine Learning para especies clave del Magdalena Medio. El proyecto implementa y valida la arquitectura propuesta en el paper *"Animal acoustic identification, denoising and source separation using generative adversarial networks"*, evaluando su viabilidad frente a arquitecturas de alta complejidad teórica bajo restricciones computacionales.

---

## Hoja de Ruta del Proyecto

El desarrollo se estructuró en tres fases incrementales, documentadas en sus respectivos notebooks:

### 1. Fase de Exploración y Baseline (`Modelo1_CNN.ipynb`)

* **Enfoque:** Inspección profunda del dataset, análisis de distribución taxonómica y generación/visualización de espectrogramas base.
* **Hito:** Validación de la calidad del dato y diseño del cargador adaptado para archivos `.npy`.

### 2. Modelo Central: Réplica del Paper (`Modelo2_GAN.ipynb`)

* **Enfoque:** Implementación del pipeline principal basado en redes generativas adversarias (GANs) para la denotación y separación de fuentes acústicas animales.
* **Hito:** Este notebook constituye el **entregable principal del sistema**, optimizado para ejecutarse eficientemente en entornos con recursos de cómputo estándar (Colab gratuito).

### 3. Escalabilidad y Trabajo Futuro (`Modelo3_Compuesto.ipynb`)

* **Enfoque:** Diseño y desarrollo modular de una arquitectura más compleja (EfficientNet + Conformer).
* **Nota de Cómputo:** Debido a las altas demandas de memoria de los bloques de atención (Conformer) y el procesamiento *online*, este pipeline queda estructurado y listo para su entrenamiento en recursos superiores a los gratuitos.

---

## Arquitectura del Sistema Implementado

El pipeline principal se enfoca en resolver el problema del ruido en grabaciones de campo antes de la clasificación:

```
[Audio Raw (.npy)] ──> [Generación de Espectrogramas] ──> [Pipeline GAN (Denoising)] ──> [Clasificador]

```

### Componentes de la Arquitectura Avanzada (Propuesta de Escalabilidad)

Para futuros despliegues en hardware dedicado, el tercer notebook deja implementado el siguiente flujo en PyTorch:

* **Dataset loader** optimizado desde `.npy`.
* **SpecAugment online** para regularización en tiempo de ejecución.
* **Backbone EfficientNet** acoplado a un encoder **Conformer** (Attention pooling + Dual heads).
* **Loss:** Class-balanced focal loss para mitigar el desbalance de clases.

---

## Marco de Evaluación Científica

El sistema se evalúa mediante un enfoque híbrido que mide tanto la precisión de la clasificación taxonómica como la fidelidad de la reconstrucción del espectrograma (de-noising) generada por la GAN:

### 1. Métricas de Clasificación (Identificación)
* **F1 Score:** Evalúa el desempeño global del modelo de forma justa.
* **Accuracy:** Medición de la exactitud general.
* **Recall (Sensibilidad):** Crucial en bioacústica para asegurar que el modelo no pase por alto llamadas de advertencia o especies críticas en el monitoreo ambiental.

### 2. Métricas de Calidad de Espectrograma (Reconstrucción y Denoising)
Dado que el pipeline del paper utiliza Redes Generativas Adversarias (GANs) para la separación de fuentes y limpieza de ruido, la fidelidad de los espectrogramas generados se evalúa como estructuras bidimensionales (imágenes) mediante:
* **SSIM (Structural Similarity Index Measure):** Mide la conservación de la estructura matemática del espectrograma (frecuencia y tiempo) comparando luminancia, contraste y estructura frente al target limpio.
* **LPIPS (Learned Perceptual Image Patch Similarity):** Utiliza una red neuronal profunda para evaluar la distancia perceptual entre el espectrograma generado y el real. Esta métrica es significativamente más cercana a la percepción auditiva humana que los errores de pixeles tradicionales (como el MSE).
---

## Recursos y Entregables

* 🖼️ **[Presentación Entrega 1: Proyecto Rio Magdalena: EDA y Datos]([https://www.google.com/search?q=LINK_A_CANVA](https://canva.link/58wz55r3cfd3arf))**
* 🖼️ **[Presentación Entrega 2: Proyecto Rio Magdalena: : Modelo Base y Paper GAN]([https://www.google.com/search?q=LINK_A_CANVA](https://canva.link/ffc8a498j5rnhul))**
* 🖼️ **[Presentación Entrega 3: Proyecto Rio Magdalena: : Reporte Final y Escalabilidad]([https://www.google.com/search?q=LINK_A_CANVA](https://canva.link/pjv2sw8vwwkbwir))**

