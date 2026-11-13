###Molecular Property Prediction with Graph Neural Networks

ENG2440 Assignment 3: predicting blood–brain barrier penetration using the MoleculeNet BBBP dataset. The project compares a Morgan-fingerprint logistic regression baseline with a bond-aware GNN under random and scaffold-based splits.



## Setup

Use **Python 3.11**. Open a terminal in the directory containing `main.ipynb` and run:

```bash
python3.11 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m pip install jupyterlab
python -m jupyter lab main.ipynb
```

On Windows, activate the environment with `.venv\Scripts\activate` instead of `source .venv/bin/activate`.

Alternatively, open the notebook in VS Code and select the Python interpreter from `.venv` as its kernel. Install the VS Code Jupyter extension if needed.

## Run the notebook

1. Start JupyterLab from the directory containing `main.ipynb`, or open that directory as your VS Code workspace.
2. Select the kernel associated with the environment where you installed the dependencies.
3. Choose **Restart Kernel and Run All Cells**.
4. Run every cell in order. Results and figures are written to `results/`.

The notebook loads and cleans the dataset, creates both split strategies, trains the models, selects checkpoints and decision thresholds using validation data, computes evaluation metrics, and displays molecular error examples.

A complete run trains six GNNs—three seeds for each split—and performs two logistic regression searches. CPU execution is supported; no GPU is required. Runtime depends on the computer. Saved notebook outputs can be viewed without rerunning training.

**Rerunning overwrites files in `results/`.** Copy the folder first if you want to preserve a particular run. The supplementary ablation tables display saved summaries rather than retraining every alternative.

## Dataset

The notebook uses `torch_geometric.datasets.MoleculeNet` to load BBBP. If the dataset is not cached locally, the loader downloads it automatically into `data/`; internet access is required for that download. No separate label file is needed.

- `1`: labelled as penetrating the blood–brain barrier.
- `0`: labelled as non-penetrating.

Invalid or empty structures are excluded. Canonical-SMILES duplicates with consistent labels are reduced to one record; all records in conflicting-label groups are removed before splitting.

## Model and evaluation settings

- **Splits:** stratified random and Bemis–Murcko scaffold splits, approximately 70% training, 15% validation and 15% test; split seed 42.
- **Baseline:** logistic regression with chiral Morgan fingerprints, radius 2 and 2,048 bits.
- **GNN:** categorical atom/bond embeddings, two GINE layers, BatchNorm, mean pooling and a binary classification head.
- **Training seeds:** 11, 22 and 33.
- **Training:** Adam, learning rate 0.001, weight decay 0.0001, batch size 64, up to 80 epochs and early-stopping patience 15.
- **Selection:** validation ROC-AUC for checkpoints and validation F1 for decision thresholds.
- **Metrics:** ROC-AUC, trapezoidal PR-AUC, accuracy, sensitivity, specificity and F1. Average precision is also exported separately.

Editable settings are grouped near the top of the notebook. See the notebook and report for interpretation and limitations.

## Files

| File or folder | Purpose |
|---|---|
| `main.ipynb` | Self-contained notebook with code, explanations and outputs |
| `requirements.txt` | Pinned direct dependencies |
| `results/` | Generated tables, figures, split indices and predictions |
| `data/` | Local dataset cache; excluded from Git |
| `ai_use_declaration.txt` | AI-assistance declaration to review and complete |


https://github.com/indiradu/Molecular-property-prediction
