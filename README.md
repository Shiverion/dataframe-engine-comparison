# dataframe-engine-comparison

# DuckDB vs Polars vs Pandas — Performance Benchmark

A practical performance and memory benchmark comparing **Pandas**, **Polars**, and **DuckDB** on large-scale CSV analytics.

This repository focuses on **real-world workloads**, not microbenchmarks, using a 13.1M-row dataset to evaluate how modern data tools behave under memory pressure and different execution models.

---

## Why This Benchmark Exists

Pandas is the default tool for data analysis in Python, but it relies on eager execution and full in-memory materialization.

Modern alternatives such as **Polars** and **DuckDB** introduce:
- Lazy evaluation
- Query optimization
- Streaming execution
- Lower memory overhead

This benchmark answers a simple question:

> *Which tool should you use when your data no longer fits comfortably in memory?*

---

## Dataset

- **Source**: Indonesian National Socioeconomic Survey (Susenas 2024)
- **Size**: 13.1 million rows × 25 columns
- **Format**: CSV (~5 GB on disk)

---

## Tools Compared

| Tool    | Execution Model | Primary Use Case |
|--------|-----------------|------------------|
| Pandas | Eager, in-memory | Exploratory analysis, ecosystem compatibility |
| Polars | Lazy + eager     | High-performance DataFrame pipelines |
| DuckDB | SQL, streaming   | Analytical queries on large files |

---

## Benchmarks

### 1. Full Analytical Benchmark
Measures end-to-end performance and memory usage across realistic analytical workloads:
- Filtering
- Grouped aggregations
- Window functions
- Advanced self-joins

### 2. CSV Read Benchmark
Evaluates the cost of reading large CSV files into memory:
- Pandas `read_csv`
- Polars eager and lazy reads
- DuckDB query-only reads
- DuckDB → Pandas DataFrame conversion

---

## Key Findings

- **Pandas exhibits significant memory amplification** for complex operations.
- **Polars achieves 50–100× lower memory usage** through lazy evaluation and optimized execution.
- **DuckDB can process large CSV files with near-zero memory usage** using streaming queries.
- Converting DuckDB results to Pandas DataFrames removes most performance benefits.
- The fastest tool depends heavily on available RAM and workload type.

---

## Environments Tested

- **Local**: MacBook Pro, 16 GB RAM
- **Cloud**: Google Colab, 47 GB RAM

Each benchmark was run multiple times with warm-up runs and peak memory tracking.

---


## Repository Structure

```
.
├── benchmarks/
│   ├── csv_read_benchmark.ipynb
│   └── analytical_benchmark.ipynb
├── results/
│   ├── benchmark_summary.csv
│   └── plots/
├── figures/
├── README.md
```



---

## Important Notes

- DuckDB → DataFrame benchmarks may use row limits to avoid out-of-memory crashes.
- Memory measurements reflect **peak usage**, not final resident size.
- Results may vary depending on hardware and filesystem performance.

---

## When to Use Each Tool

- **Pandas**: Small datasets, prototyping, ecosystem compatibility
- **Polars**: High-performance DataFrame transformations with sufficient RAM
- **DuckDB**: SQL-style analytics on datasets larger than memory

---

## Article & Write-up

A detailed explanation of the results and methodology is available here:

👉 *When Pandas Crashed My Laptop: A 13-Million-Row Wake-Up Call*  
(Linked from Medium)

---

## Reproducibility

All benchmarks were executed with:
- Python 3.10
- Pandas 2.x
- Polars 0.20.x
- DuckDB 0.10.x

See notebooks for exact versions and configuration.

---

## System Design

### Requirements

- Compare Pandas, Polars, and DuckDB on CSV reads and analytical workloads.
- Record elapsed time and memory observations for the configured input file.

### Pipeline

```text
Google Drive mount → configured CSV path → repeated dataframe workloads
                  → timing and memory measurements → notebook outputs
```

`Benchmark.ipynb` is the only benchmark runner. Its dataset path is a placeholder and the repository does not contain the benchmark CSV or a results directory, so the README's dataset size and performance claims are not reproduced by this checkout alone.

### Trade-offs

| Decision | Benefit | Cost |
| --- | --- | --- |
| Notebook-based local measurements | Easy to inspect and adapt workloads | Results depend on the runtime, machine, and input path |
| Compare eager, lazy, and SQL paths | Shows different execution models | Equivalent operations can require different setup |

### What to Revisit

- Provide the dataset source and a documented path configuration.
- Save raw run measurements and hardware/software versions with each result.
- Separate warm-up runs from measured repetitions.

## License

MIT License


