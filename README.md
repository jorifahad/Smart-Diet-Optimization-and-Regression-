# Smart-Diet-Optimization-and-Regression-

A nutritional planning project that compares **exact** and **approximate** optimization methods for constructing low-cost diet plans that satisfy minimum nutritional requirements.

The system uses food cost and nutrient information to determine suitable food quantities while meeting targets for:

- Protein
- Vitamins
- Carbohydrates

## Project Overview

Designing a balanced diet involves two competing goals:

1. Meeting nutritional requirements.
2. Minimizing the total cost of selected food items.

This project formulates the task as a constrained optimization problem and evaluates two approaches:

- **Linear Programming (PuLP):** Produces an exact minimum-cost solution for the linear formulation.
- **Differential Evolution (SciPy):** Searches for an approximate solution using a population-based evolutionary algorithm.

Multiple nutritional scenarios are evaluated to compare cost, execution time, and solution behavior.

## Mathematical Formulation

### Objective Function

The objective is to minimize the total diet cost:

```text
Minimize:  Z = Σ cᵢxᵢ
```

Where:

- `cᵢ` is the cost per unit of food item `i`.
- `xᵢ` is the selected quantity of food item `i`.
- `Z` is the total cost of the diet plan.

### Nutritional Constraints

For each required nutrient:

```text
Σ aᵢⱼxᵢ ≥ bⱼ
```

Where:

- `aᵢⱼ` is the amount of nutrient `j` in food item `i`.
- `bⱼ` is the minimum required amount of nutrient `j`.

The implementation includes minimum requirements for protein, vitamins, and carbohydrates. Some scenarios also include priority foods that must appear in the solution.

## Methods

### 1. Linear Programming with PuLP

The exact model:

- Creates one continuous decision variable for each food item.
- Minimizes total food cost.
- Enforces all nutritional constraints.
- Adds minimum quantities for priority foods when required.
- Returns an optimal solution when the problem is feasible.

### 2. Differential Evolution

The approximate model:

- Uses SciPy's `differential_evolution`.
- Searches within bounded food quantities.
- Adds a large penalty when nutritional requirements are violated.
- Produces a feasible or near-feasible approximate solution depending on the search configuration.

## Dataset

The dataset contains the following columns:

| Column | Description |
|---|---|
| `Food Item` | Name and category of the food |
| `Cost/unit ($)` | Cost per unit |
| `Protein (g/unit)` | Protein supplied per unit |
| `Vitamins (units/unit)` | Vitamin value per unit |
| `Carbohydrates (g/unit)` | Carbohydrates supplied per unit |

## Experimental Scenarios

The notebook evaluates multiple nutritional requirement settings. The final comparison file contains three scenarios with different minimum nutrient targets and optional priority foods.

## Results

The recorded comparison results are:

| Scenario | Exact Cost (PuLP) | Approx. Cost (DE) | Exact Time (s) | Approx. Time (s) | Cost Difference |
|---|---:|---:|---:|---:|---:|
| Scenario 1 | 170.35 | 2241.07 | 0.0093 | 784.54 | 2070.71 |
| Scenario 2 | 258.07 | 2352.09 | 0.0102 | 783.46 | 2094.02 |
| Scenario 3 | 276.62 | 2251.45 | 0.0093 | 786.67 | 1974.83 |

### Key Findings

- PuLP achieved substantially lower costs in all recorded scenarios.
- PuLP also completed much faster in the saved comparison results.
- Differential Evolution distributed quantities across more food items, while PuLP concentrated on a smaller set of cost-effective foods.
- The approximate method was highly sensitive to bounds, penalties, population size, and iteration settings.
- For this linear formulation, Linear Programming was the more suitable method.

## Project Files

| File | Description |
|---|---|
| `Diet_Optimization_Code.ipynb` | Complete implementation and visualizations |
| `Diet_Optimization_Dataset.csv` | Food cost and nutrient dataset |
| `Diet_Optimization_Comparison_results.csv` | Cost and execution-time comparison |
| `Diet_Optimization_Report.pdf` | Full project report |

## Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- PuLP
- SciPy
- Jupyter Notebook

## How to Run

### 1. Install the required packages

```bash
pip install pandas numpy matplotlib pulp scipy
```

### 2. Place the dataset beside the notebook

Make sure the notebook reads the correct local filename:

```python
data = pd.read_csv("Diet_Optimization_Dataset.csv")
```

### 3. Run the notebook

Open:

```text
Diet_Optimization_Code.ipynb
```

Then run the cells in order.

## Example Workflow

```text
Food and nutrient data
        ↓
Define nutritional requirements
        ↓
Solve with PuLP
        ↓
Solve with Differential Evolution
        ↓
Compare cost and execution time
        ↓
Visualize results
```

## Limitations

- The model currently includes protein, vitamins, and carbohydrates only.
- Food quantities are continuous rather than restricted to practical serving sizes.
- Differential Evolution requires careful tuning of bounds and penalty terms.
- The approximate method may return costly solutions when the search space is large.
- Dietary preferences, allergies, age, medical conditions, and cultural restrictions are not included.

## Future Improvements

- Add fats, fiber, minerals, and micronutrients.
- Use realistic serving-size and integer constraints.
- Support allergies, dietary preferences, and food exclusions.
- Add calorie limits and upper nutrient bounds.
- Improve Differential Evolution through tuned penalties and stopping criteria.
- Build an interactive interface for personalized diet generation.
- Extend the system for hospitals, schools, and humanitarian food-planning programs.
