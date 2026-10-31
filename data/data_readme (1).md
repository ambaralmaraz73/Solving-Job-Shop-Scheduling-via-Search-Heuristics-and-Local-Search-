# Dataset Instances & Benchmark Execution Guide

This directory contains all benchmark files and custom-generated instances used for evaluation in `notebooks/JSSP_Instances_and_Benchmark.ipynb`.

---

## Run Benchmarks and Custom Instances in Google Colab

Follow this tutorial to execute any standard benchmark or custom instance (`.txt`) on the code `JSSP_Instances_and_Benchmark.ipynb`.

### Step 1: Running in Google Colab (Recommended)

1. **Navigate to the Notebooks Folder**:
   - In this GitHub repository, click on the **`notebooks/`** folder.
   - Select and click on the file **`JSSP_Instances_and_Benchmark.ipynb`**.

2. **Open in Google Colab**:
   - At the top of the notebook preview, click the badge **`Open in Colab`**.
   - This will automatically redirect you to the Google Colab environment with the notebook loaded.

---

### Step 2: Download Benchmark Files from GitHub

1. In this GitHub repository, go into the `data/` directory.
2. Choose the dataset folder you want to use:
   - **Standard Benchmarks**: Click on `benchmarks/` (`ft10.txt`, `la36.txt`).
   - **Custom Seeded Instances**: Click on `own_instances/` (e.g., `Easy_1.txt`, `Dificil_1.txt`).
3. Click on the `.txt` file you wish to run and click the **Download** button in GitHub to save it to your local computer.

---

## Dataset Folder Structure

```text
data/
├── README.md               # Dataset documentation & execution guide
├── benchmarks/             # OR-Library standard benchmarks (ft06, ft10, ft20, la01 to la40)
└── own_instances/          # 20 custom seeded instances (random.seed(42))
    ├── Easy_1.txt ... Easy_10.txt    # 10 Easy instances (3x3 to 5x5 grids)
    ├── Medio_1.txt ... Medio_8.txt   # 8 Medium instances (6x6 to 8x8 grids)
    └── Dificil_1.txt, Dificil_2.txt  # 2 Hard instances (10x10 grids)
```

---
### Step 3: Set Up Folders and Upload Data in Colab

1. Inside your active Google Colab session, click on the **Folder icon** 📁 on the left sidebar to open the Files panel.
2. Create the required destination folder depending on your test file:
   - For standard benchmarks: Right-click in the files area, select **New folder**, and name it **`benchmarks`**.
   - For custom instances: Right-click in the files area, select **New folder**, and name it **`instances`**.
3. Drag and drop your downloaded `.txt` file directly into its corresponding folder (`benchmarks` or `instances`) in Colab.

---

### Step 4: Configure File Path and Run Code

1. Scroll down to Section 3.1, 3.2, or 3.3 in the notebook to locate the instance configuration block.
2. Set the `instance_path` variable to match the exact file location you uploaded:

```python
# Set the desired instance path
instance_path = 'benchmarks/la36.txt'  # <--- PASTE THE DESIRED INSTANCE PATH HERE

# Extract the instance name from the file path
instance_name = os.path.basename(instance_path)

# Automatically select the correct loader based on the folder path
if 'benchmarks/' in instance_path:
    jobs_data_loaded = load_or_library_instance(instance_path)
else:
    jobs_data_loaded = load_custom_instance(instance_path)
```

3. In Colab's top menu, click **Runtime > Run all**.
4. The notebook will process the execution under the strict **60.0-second protocol** and automatically log summary metrics and detailed schedules into the `results/` folder.

---

## Instances format (`.txt`) 

### 1. Custom Instances Format (`own_instances/`)

These files explicitly list each machine using the letter `M` alongside its required processing time:

* **Line 1**: Specifies the total number of Jobs and total number of Machines.
* **Following Lines**: Each line represents one Job. It lists the required sequence of `(Machine_ID, Duration)` pairs.

```text
3 3
M1 3  M2 3  M3 3   <-- Job 1: Machine 1 (3 min) -> Machine 2 (3 min) -> Machine 3 (3 min)
M1 2  M3 3  M2 4   <-- Job 2: Machine 1 (2 min) -> Machine 3 (3 min) -> Machine 2 (4 min)
M2 3  M1 2  M3 1   <-- Job 3: Machine 2 (3 min) -> Machine 1 (2 min) -> Machine 3 (1 min)
```

### 2. Standard OR-Library Format (`benchmarks/` FT10, LA36)

Literature benchmark files use the standard academic OR-Library structure where machines are **0-indexed numbers** (0, 1, 2...) instead of names:

* **Line 1**: Specifies the total number of Jobs and total number of Machines.
* **Following Lines**: Alternating pairs of `[Machine_Index] [Processing_Time]`.

```text
6 6
0 1  1 3  2 6  3 7  4 3  5 6   <-- Machine 0 (takes 1), Machine 1 (takes 3), Machine 2 (takes 6)...
1 8  2 5  4 10 5 10 0 10 3 4
```

> 💡 **Note**: As shown in Step 4, the code checks `if 'benchmarks/' in instance_path:` to call `load_or_library_instance()`, which automatically maps 0-indexed machines (`0 -> M1`, `1 -> M2`, etc.) without requiring manual file edits.
