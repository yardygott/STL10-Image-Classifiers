# STL10-Image-Classifiers

Small guide: two main steps to get the project running — install dependencies, then run training.

**Step 1 — Install dependencies (venv or system)**

- Create and activate a virtual environment (recommended):

```bash
python -m venv venv
source venv/bin/activate  # Linux / macOS
# on Windows use: .\venv\Scripts\activate
```

- Install PyTorch appropriate for your hardware, then the project requirements.

CPU-only example (Linux):

```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
pip install -r requirements.txt
```

GPU (CUDA) example: follow the instructions at https://pytorch.org/ to pick the right wheel for your CUDA version, then:

```bash
# Example (replace with the command from pytorch.org):
pip install torch torchvision --index-url https://download.pytorch.org/whl/cuXXX
pip install -r requirements.txt
```

Notes:
- `requirements.txt` is provided in the repository and includes non-PyTorch Python dependencies.
- You can also install system-wide if you prefer, but a virtual environment avoids conflicts.

**Step 2 — Run training (Train)**

- Configure the model in `config.py` by setting `MODEL_TYPE` (options: `logistic`, `fc`, `cnn`, `mobilenet_fixed`, `mobilenet_learned`) and adjust `CURRENT_PARAMS` (learning rate, epochs, etc.).
- Start training with:

```bash
python train.py
```

- Training behavior and outputs (what to expect):
  - The script writes outputs into the `plots_and_outputs` folder (created automatically).
  - Saved files include:
    - `plots_and_outputs/best_model_<MODEL_TYPE>.pth` — the best model state_dict saved during training.
    - `plots_and_outputs/training_curves_<MODEL_TYPE>.png` — combined train/validation loss and accuracy plot.

**Testing / Evaluation**

- After training, run evaluation with:

```bash
python test.py
```

- What `test.py` does and produces:
  - Loads `best_model_<MODEL_TYPE>.pth` from `plots_and_outputs` (so ensure the file exists).
  - Prints a classification report to the console.
  - Saves `plots_and_outputs/confusion_matrix_<MODEL_TYPE>.png` and displays the confusion matrix.

**Data location**

- The project expects the STL-10 data under the `data/stl10_binary/` folder in the repository root. The repository includes helper files such as `class_names.txt` and `fold_indices.txt` used by the data loader.

**Notes & tips**

- The training scripts save model weights as `state_dict()` only. To reload, ensure `config.MODEL_TYPE` matches the model used when training.
- If `test.py` reports a missing weights file, run `train.py` first or copy an existing `best_model_<MODEL_TYPE>.pth` into `plots_and_outputs`.
- For reproducibility, `train.py` seeds RNGs based on `config.RANDOM_SEED`.

## Quick file references

- See configuration: [config.py](config.py)
- Run training: [train.py](train.py)
- Run evaluation: [test.py](test.py)
- Data folder: [data/stl10_binary](data/stl10_binary)
- Outputs folder (auto-created): [plots_and_outputs](plots_and_outputs)
