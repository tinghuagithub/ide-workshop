# IDE Workshop: VS Code and Jupyter Notebook on HPC

Materials for a workshop introducing VS Code remote connections and Jupyter Notebook on HPC. The hands-on Higgs boson classification example runs entirely in Jupyter Notebook, with its server and Python kernel on an allocated compute node.

## Workshop example

Use XGBoost to distinguish simulated Higgs signal events from background events. Explore the data, train a model using **21 basic features**, and compare it with a second model using **seven physics-derived features**. Complete both stages in the same notebook and kernel. A GPU is not required.

## Repository contents

| File | Description |
| --- | --- |
| [Higgs_XGBoost_Workshop.ipynb](Higgs_XGBoost_Workshop.ipynb) | Exercise instructions, worked examples, model training, evaluation, and comparison plots |
| [higgs_subset.csv.gz](higgs_subset.csv.gz) | Complete workshop dataset: 550,000 simulated events, compressed without changing the CSV contents |

The notebook reads `higgs_subset.csv.gz` directly using pandas. An existing uncompressed `higgs_subset.csv` also works.

## Download the materials

Clone the repository from a terminal:

```bash
git clone https://github.com/tinghuagithub/ide-workshop.git
cd ide-workshop
```

Alternatively, select **Code > Download ZIP** on the repository page and extract the ZIP. Keep the notebook and dataset together in the same directory. If you download to your laptop, copy that directory to your HPC work/project space before starting the exercise.

## Decompress the dataset (optional)

The dataset is supplied as `higgs_subset.csv.gz`, a gzip-compressed CSV file. To create an uncompressed CSV on Linux, WSL, or DARWIN, run the following command from the directory containing the dataset:

```bash
gzip -dc higgs_subset.csv.gz > higgs_subset.csv
```

This creates `higgs_subset.csv` in the same directory while keeping the original `.gz` file. The `-d` option decompresses the file, and `-c` writes the decompressed data to standard output, which `>` saves to the CSV. It overwrites an existing `higgs_subset.csv`, so run it only when you want to create or replace that file. Allow approximately 170 MB of additional disk space for the uncompressed CSV.

Keep `higgs_subset.csv` beside `Higgs_XGBoost_Workshop.ipynb`. The notebook automatically uses the uncompressed CSV when present, otherwise it reads `higgs_subset.csv.gz` directly. You do not need to change the notebook's data path for either option.

## Python environment

The notebook requires:

```text
numpy
pandas
matplotlib
scikit-learn
xgboost
notebook
ipykernel
```

For the DARWIN workshop, follow the slides to load the instructor-selected Miniforge module and create the project environment. After obtaining a compute allocation, activate the environment before launching Jupyter:

```bash
vpkg_require miniforge
conda activate "$WORKDIR_USER/sw/ide_workshop"
```

The environment path above assumes the setup used in the workshop slides. Use your instructor's path if it differs. The notebook's first code cell prints the Python executable and XGBoost version so you can check the selected kernel.

## Run the notebook on DARWIN

1. Follow the workshop slides to request a compute allocation.
2. On the allocated compute node, activate the workshop environment and change to the directory containing the notebook and dataset.
3. Start Jupyter using the launch command in the slides.
4. Open the SSH tunnel from your local computer and keep its terminal running.
5. Open the Jupyter address in your local browser, using the token printed by your running server.
6. Open `Higgs_XGBoost_Workshop.ipynb` and run cells from top to bottom. Complete Stage 1 before continuing to Stage 2 in the same kernel.

The notebook uses `SLURM_CPUS_PER_TASK` for training threads when available, otherwise one thread. Run Jupyter and model training on the compute node assigned to your job.

To repeat the full analysis from a clean state, restart the kernel and run all cells in order.

## Results and shutdown

The notebook writes models, prediction tables, summaries, and plots under:

```text
results/higgs_notebook/
```

Both models use the same 60% training, 20% validation, and 20% test event splits. Each model's score threshold is selected using validation data before the final test comparison.

When finished, save the notebook, shut down the kernel and Jupyter server, stop the SSH tunnel, and release the compute allocation as shown in the workshop slides. Closing the browser tab alone does not stop the kernel.

## Optional local practice

On a local computer, create and activate a Python virtual environment, then install the dependencies:

```bash
python -m pip install numpy pandas matplotlib scikit-learn xgboost notebook ipykernel
jupyter notebook
```

Run these commands from the active environment and the directory containing the notebook and dataset. DARWIN module and allocation commands apply only on the cluster.

## Dataset attribution and license

The workshop subset contains the first **550,000 events** from the original 11 million-event UCI HIGGS dataset, with column names added. It retains all 28 input features and the binary label: **1 = signal**, **0 = background**. This is an introductory subset, not a random sample of the entire dataset. Gzip compression preserves the supplied CSV contents.

Whiteson, D. (2014). *HIGGS [Dataset]*. UCI Machine Learning Repository. [https://doi.org/10.24432/C5V312](https://doi.org/10.24432/C5V312).

The dataset is available under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). This license statement applies to the dataset.

Related paper: Baldi, P., Sadowski, P., and Whiteson, D. (2014). [Searching for exotic particles in high-energy physics with deep learning](https://doi.org/10.1038/ncomms5308). *Nature Communications*.
