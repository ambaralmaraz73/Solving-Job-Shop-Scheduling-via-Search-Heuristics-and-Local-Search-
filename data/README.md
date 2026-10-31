# 📊 Dataset Instances & Benchmark Execution Guide
This directory contains all benchmark files and custom-generated instances used for evaluation in `notebooks/JSSP_Instances_and_Benchmark.ipynb`.

---

## Run Benchmarks and Custom Instances in Google Colab

Follow this tutorial to execute any standard benchmark or custom instance (`.txt`) on the code **`JSSP_Instances_and_Benchmark.ipynb`**.

### Step 1: Running in Google Colab (Recommended)
1. Navigate to the **`notebooks/`** folder in this repository.
2. Click on **`JSSP_Instances_and_Benchmark.ipynb`**.
3. Click the **`Open in Colab`** badge at the top to launch the notebook.

### Step 2: Load the Dataset Files into Colab
Choose **one** of the following two options to make the data files accessible to Colab:

* **Method A (Automated Git Clone - Easiest)**:
  Run the setup cell at the very beginning of the notebook to clone the entire repository structure into Colab:
  ```python
  !git clone https://github.com/ambaralmaraz73/Solving-Job-Shop-Scheduling-via-Search-Heuristics-and-Local-Search-.git
  %cd Solving-Job-Shop-Scheduling-via-Search-Heuristics-and-Local-Search-
  ```

* **Method B (Manual Drag & Drop Upload)**:
  1. Click the **Folder icon** 📁 on the left sidebar of Google Colab.
  2. Drag and drop your target `.txt` file (e.g., `ft06.txt` or `Easy_1.txt`) directly into the files area.

### Step 3: Set the Target File Path in Code

Locate the configuration cell in the notebook (under Section 3.1, 3.2, or 3.3) and set the `instance_path` variable to point to your desired file:

```python
# ==========================================
# INSTANCE CONFIGURATION
# ==========================================

# Examples:
# Custom instance:    'data/own_instances/Easy_1.txt' or 'data/own_instances/Dificil_1.txt'
# Standard benchmark: 'data/benchmarks/ft06.txt' or 'data/benchmarks/la36.txt'

instance_path = 'data/benchmarks/ft06.txt'  # <--- PASTE YOUR TARGET FILE PATH HERE

# The parser automatically detects if the file is an OR-Library benchmark or a custom format
instance_name = os.path.basename(instance_path)
```

### Step 4: Execute & Export Results

1. Click **Runtime > Run all** (`Ctrl + F9`).
2. The notebook will automatically process the instance, apply all algorithms under the strict **60-second time budget**, and export individual results directly into the **`results/`** directory.

---

## 📁 Dataset Folder Structure

```text
data/
├── README.md               # This documentation file
├── benchmarks/             # OR-Library standard benchmarks (e.g., ft06, ft10, ft20, la01 to la40)
└── own_instances/          # 20 custom seeded instances generated with fixed seed (random.seed(42))
    ├── Easy_1.txt ... Easy_10.txt    # 10 Easy instances (3x3 to 5x5 grids)
    ├── Medio_1.txt ... Medio_8.txt   # 8 Medium instances (6x6 to 8x8 grids)
    └── Dificil_1.txt, Dificil_2.txt  # 2 Hard instances (10x10 grids)
```

---

## 📄 Standard File Format Specification (OR-Library vs Custom)

### Custom Instance Format (`.txt`)
```text
# Line 1: Total Jobs, Total Machines
3 3
# Lines 2+: Sequence of (Machine_ID Duration) pairs for each job
M1 3 M2 3 M3 3
M1 2 M3 3 M2 4
M2 3 M1 2 M3 1
```

### Standard OR-Library Format (e.g., FT06, LA36)
```text
# Line 1: Total Jobs, Total Machines
6 6
# Lines 2+: Alternating 0-indexed Machine_ID and Processing Time pairs per job
0 1  1 3  2 6  3 7  4 3  5 6
1 8  2 5  4 10 5 10 0 10 3 4
...
```
