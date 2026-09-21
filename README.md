# Zero-Shot GPR-Based Subsurface Sensing with Resolution- and Domain-Invariant Fourier Neural Operators

This repository provides the source code and experimental data associated with the manuscript

**"Zero-Shot GPR-Based Subsurface Sensing with Resolution- and Domain-Invariant Fourier Neural Operators"**

by Zixin Wang, Ishfaq Aziz, and Mohamad Alipour.

The study develops a zero-shot ground-penetrating radar (GPR)-based subsurface sensing framework using Fourier neural operators (FNOs). The proposed FNO is trained exclusively on synthetic GPR data generated through finite-difference time-domain (FDTD) simulations and is subsequently evaluated on real-world GPR measurements without using target-domain data during training.

By learning spectral weights in the Fourier domain and emphasizing dominant low-frequency modes associated with physical wave propagation, the FNO provides temporal resolution invariance and robustness to discrepancies between synthetic and real GPR data. The framework is evaluated using laboratory and field experiments involving both single-layer and two-layer subsurface material configurations.

## Framework Overview

![Framework Overview](Framework_Overview.jpg)

The proposed framework consists of four main stages:

1. **Data collection:** Synthetic GPR signals are generated using FDTD simulations implemented in gprMax, while real-world GPR measurements are collected from laboratory and field experiments.

2. **Data preprocessing:** The amplitude envelope of each GPR A-scan is obtained using the Hilbert transform and normalized to the range [0, 1]. Material-property labels are independently normalized using min–max normalization.

3. **Zero-shot FNO learning:** The FNO is trained exclusively on synthetic GPR data to estimate subsurface material properties, including relative permittivity, electrical conductivity, and layer depth.

4. **Performance evaluation:** The trained FNO is directly evaluated on real-world GPR measurements. Performance is assessed using the Pearson correlation coefficient (R), bias, root mean squared error (RMSE), and unbiased root mean squared error (ubRMSE). The framework is also evaluated in terms of resolution invariance, robustness to domain discrepancies, and computational efficiency.

For the field experiments, soil moisture variation is additionally evaluated in terms of volumetric water content (VWC).

## Experimental Configurations

The study considers four representative experimental scenarios:

- **Laboratory single-layer material:** A single soil layer with varying moisture conditions. Relative permittivity, electrical conductivity, and layer depth are estimated.

- **Laboratory two-layer material:** A layer of wood shavings placed over soil. Material properties of both the soil and wood-shavings layers are investigated.

- **Field single-layer material:** Bare soil monitored under realistic environmental conditions, including rainfall-induced variations in soil moisture.

- **Field two-layer material:** Soil covered by an upper layer consisting of natural leaves or wood chips. Soil moisture variations are estimated under realistic field conditions.

Real-world GPR measurements are collected using a GSSI StructureScan MiniXT system equipped with a 2,700 MHz antenna. Synthetic source-domain GPR signals are generated using gprMax.

## Repository Contents

The repository is organized according to the four experimental configurations investigated in the study:

- `Laboratory_Single_Layer_Material/`
  - Synthetic data generation, experimental GPR data, and model training/evaluation notebooks for the laboratory single-layer configuration.

- `Laboratory_Two_Layer_Material/`
  - Synthetic data generation, experimental GPR data, and model training/evaluation notebooks for the laboratory two-layer configuration.

- `Field_Single_Layer_Material/`
  - Synthetic data generation, experimental field GPR data, and model training/evaluation notebooks for the field single-layer configuration.

- `Field_Two_Layer_Material/`
  - Synthetic data generation, experimental field GPR data, and model training/evaluation notebooks for the field two-layer configurations, including soil–leaves and soil–wood chips cases.

- `Framework_Overview.jpg`
  - Overview of the proposed zero-shot FNO framework.

- `README.md`
  - Description of the repository, methodology, dependencies, and usage.

## Models

The study investigates the following data-driven approaches:

- **1D CNN:** A one-dimensional convolutional neural network used as a supervised baseline. The model is trained using labeled synthetic GPR data and evaluated on experimental measurements.

- **DANN:** A domain adversarial neural network used as an unsupervised domain-adaptation baseline. DANN uses labeled synthetic source-domain data together with unlabeled real target-domain data to learn domain-invariant representations.

- **MiTSformer:** A Transformer-based time-series regression baseline. MiTSformer is trained exclusively on synthetic GPR data and subsequently evaluated on experimental measurements under the same zero-shot evaluation setting used for the FNO.

- **FNO:** The proposed Fourier Neural Operator model. The FNO is trained exclusively on synthetic GPR data and directly applied to real-world GPR measurements without using real-world data during model training. Spectral convolution and Fourier-mode truncation enable the model to capture global wave interactions while facilitating resolution- and domain-invariant generalization.

## Fourier Neural Operator

The FNO learns mappings between functions using spectral convolution in the Fourier domain. The input GPR waveform is treated as a discretized realization of an underlying continuous function.

The model transforms the signal into the Fourier domain using the fast Fourier transform (FFT), applies learnable transformations to a truncated set of Fourier modes, and then transforms the representation back to the original domain using the inverse fast Fourier transform (IFFT).

By retaining the dominant Fourier modes, the FNO emphasizes low-frequency spectral information associated with global wave behavior while reducing sensitivity to high-frequency discrepancies caused by measurement noise, discretization differences, and modeling uncertainties.

This formulation also allows the learned operator to be evaluated on temporal discretizations different from those used during training.

## Synthetic Data Generation

Synthetic GPR signals are generated using the finite-difference time-domain (FDTD) method implemented in **gprMax**, which numerically solves Maxwell's equations to simulate electromagnetic wave propagation.

The simulations specify both intrinsic radar parameters and extrinsic material parameters.

Material parameters considered in the simulations include:

- Relative permittivity
- Electrical conductivity
- Layer depth

Synthetic datasets are generated for the laboratory and field configurations and are used as source-domain data for model training.

## Experimental Data

Experimental GPR measurements are collected for four configurations:

- Laboratory single-layer soil
- Laboratory two-layer soil–wood shavings
- Field single-layer bare soil
- Field two-layer soil–leaves and soil–wood chips

The experimental measurements serve as the real-world target domain for evaluating the generalization capability of models trained on synthetic data.

For the field experiments, reference soil properties are measured using an in situ TEROS-12 capacitance sensor. Soil moisture is additionally characterized in terms of volumetric water content (VWC).

## Data Preprocessing

Each GPR A-scan is converted to its amplitude envelope using the Hilbert transform.

The amplitude envelope is normalized to the range [0, 1] according to its maximum amplitude. Material-property labels are independently normalized using min–max normalization.

The preprocessing procedure is applied consistently to the synthetic and experimental GPR signals prior to model training and evaluation.

## Resolution- and Domain-Invariant Evaluation

In addition to material-property estimation accuracy, the study investigates two important generalization capabilities of the proposed FNO.

### Resolution Invariance

The spectral-convolution formulation of the FNO enables the learned operator to be evaluated on GPR signals with temporal resolutions different from those used during training.

This capability is particularly useful in zero-shot radar sensing because the temporal resolution of experimental measurements may differ from that of synthetic training data.

### Domain Invariance

Simulation-to-reality discrepancies can arise from measurement noise, discretization errors, modeling assumptions, boundary conditions, material uncertainties, and imperfect radar-model calibration.

Fourier-mode truncation emphasizes the dominant low-frequency spectral components while suppressing high-frequency discrepancies, thereby improving robustness to differences between synthetic and real GPR signals.

The robustness of the proposed FNO is further evaluated under increased simulation-to-reality discrepancies.

## Evaluation Metrics

Model performance is evaluated using the following metrics:

- Pearson correlation coefficient (`R`)
- Bias
- Root mean squared error (`RMSE`)
- Unbiased root mean squared error (`ubRMSE`)
- Standard deviation of predictions
- Training time
- Inference time

The Pearson correlation coefficient is used to evaluate the capability of each model to capture the underlying physical variability of the target material properties, while bias, RMSE, and ubRMSE provide complementary measures of estimation accuracy.

## Requirements

The code is implemented in Python. Major dependencies include:

- PyTorch
- neuraloperator
- NumPy
- pandas
- SciPy
- scikit-learn
- Matplotlib
- gprMax

Additional dependencies may be required by individual notebooks.

## Installation

Install the required Python packages as needed.

The FNO implementation uses the **NeuralOperator** library. It can be installed using:

```bash
pip install neuraloperator
```

The FNO model can then be imported in Python using:

```python
from neuralop.models import FNO
```

Other commonly required packages can be installed using:

```bash
pip install numpy pandas scipy scikit-learn matplotlib
```

PyTorch should be installed according to the user's operating system and computing environment.

For synthetic GPR data generation, install and configure **gprMax** following its official installation instructions.

## Usage

The provided notebooks contain the main workflows for:

1. Synthetic GPR data generation
2. Experimental GPR data loading
3. GPR signal preprocessing
4. Model training
5. Zero-shot evaluation on experimental GPR measurements
6. Material-property estimation
7. Field soil-moisture and VWC evaluation
8. Resolution-invariance analysis
9. Domain-invariance analysis
10. Performance comparison with baseline models

Users should update local file paths in the notebooks as needed before execution.

For the proposed FNO model, the NeuralOperator implementation is imported using:

```python
from neuralop.models import FNO
```

The FNO is trained using only synthetic source-domain GPR signals and their corresponding material-property labels. After training, the model is directly evaluated on experimental GPR measurements without additional training or fine-tuning using the experimental data.

## Citation

If you use the code or data in this repository, please cite:

**Zixin Wang, Ishfaq Aziz, and Mohamad Alipour,  
"Zero-Shot GPR-Based Subsurface Sensing with Resolution- and Domain-Invariant Fourier Neural Operators."**

Full bibliographic information will be added upon publication.