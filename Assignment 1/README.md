# Mixed-Mode Dissimilarity Matrix (Python / pandas)

Assignment: compute a dissimilarity matrix for a dataset whose attributes are of **mixed types**
(nominal, binary, ordinal, numeric) using pandas and NumPy.

- **Name / ID:** _your name and ID here_
- **Course:** _course name here_
- **Notebook:** `mixed_mode_dissimilarity.ipynb` (run on Kaggle)

---

## 1. Dataset

10 objects, 11 attributes (`mixed_mode_data.csv`).

| Attribute | Type | Notes |
|---|---|---|
| M_Status | Nominal | Single, Married, Divorced, Widowed |
| Blood_Group | Nominal | AB+, A+, B+, O-, ... (each value is a separate category) |
| Soft_Drink | Nominal | Coke, Fanta, Pepsi |
| Gender | Symmetric binary | M / F |
| iPhone | Asymmetric binary | Y = positive |
| Test1 | Asymmetric binary | Y = positive |
| Test2 | Asymmetric binary | P = positive, N = negative |
| Credit_Rating | Ordinal | Fair < Good < Excellent |
| Designation | Ordinal | Lecturer < Asst. Professor < Asso. Professor < Professor |
| Height | Numeric | |
| Weight | Numeric | |

## 2. Method

Each attribute `f` gives a distance `d_f(i, j)` in [0, 1] and an indicator `delta_f(i, j)`
(1 = attribute is used for this pair, 0 = attribute is ignored).

| Type | Distance `d_f` | `delta_f` |
|---|---|---|
| Nominal, symmetric binary | 0 if values match, otherwise 1 | 1 |
| Asymmetric binary | 0 if values match, otherwise 1 | **0 if both values are negative**, otherwise 1 |
| Ordinal | `abs(z_i - z_j)`, where `z = (rank - 1) / (M - 1)` | 1 |
| Numeric | `abs(x_i - x_j) / (max - min)` | 1 |

The final mixed-mode dissimilarity is

```
d(i, j) = sum_f( delta_f * d_f ) / sum_f( delta_f )
```

Why `delta`: for asymmetric binary attributes a (negative, negative) match carries no information,
so it is left out of both the numerator and the denominator.

## 3. Steps in the notebook

1. Load the CSV into a pandas DataFrame and clean text (strip spaces, fix spelling).
2. Declare which columns are nominal, binary, ordinal or numeric.
3. Define one small function per attribute type (`nominal_d`, `asym_binary_d`, `ordinal_d`, `numeric_d`), each returning `(d, delta)`.
4. `build_matrix(col, func)` loops over every pair `(i, j)` and fills a distance matrix `D` and a delta matrix `W`.
5. Build `D` and `W` for every attribute.
6. Combine: `d_mixed = sum(W * D) / sum(W)`.
7. Find the most similar and most dissimilar pair and highlight them.

## 4. Results

Mixed-mode dissimilarity matrix (lower triangle, rounded to 3 decimals):

| Obj | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|
| **1** | 0.000 |  |  |  |  |  |  |  |  |  |
| **2** | 0.671 | 0.000 |  |  |  |  |  |  |  |  |
| **3** | 0.613 | 0.595 | 0.000 |  |  |  |  |  |  |  |
| **4** | 0.862 | 0.783 | 0.653 | 0.000 |  |  |  |  |  |  |
| **5** | 0.773 | 0.723 | 0.800 | 0.561 | 0.000 |  |  |  |  |  |
| **6** | 0.506 | 0.627 | 0.832 | 0.577 | 0.634 | 0.000 |  |  |  |  |
| **7** | 0.600 | 0.572 | 0.334 | 0.777 | 0.704 | 0.851 | 0.000 |  |  |  |
| **8** | 0.491 | 0.633 | 0.722 | 0.744 | 0.442 | 0.651 | 0.631 | 0.000 |  |  |
| **9** | 0.759 | 0.656 | 0.512 | 0.467 | 0.539 | 0.501 | 0.772 | 0.560 | 0.000 |  |
| **10** | 0.537 | 0.557 | 0.437 | 0.771 | 0.682 | 0.674 | 0.617 | 0.759 | 0.676 | 0.000 |

- **Most similar pair:** objects **7 and 3**, d = **0.334**
- **Most dissimilar pair:** objects **4 and 1**, d = **0.862**

A value close to 0 means two objects are very similar; a value close to 1 means they are very different.

## 5. How to run

1. Upload `mixed_mode_data.csv` as a Kaggle dataset and add it to the notebook.
2. Update `folder` in the first cell to the dataset path.
3. Run all cells in order. Requirements: Python 3, `pandas`, `numpy`.

## 6. Files

```
README.md
mixed_mode_data.csv
mixed_mode_dissimilarity.ipynb
```

## 7. Reference

Han, J., Kamber, M., Pei, J. *Data Mining: Concepts and Techniques*, Chapter 2 (Measuring data similarity and dissimilarity, attributes of mixed types).
