# Parallel Computing with Dask and Ray
## PowerPoint Presentation Outline

**Course**: MIS 769 - Big Data Fundamentals (Part 1)
**Type**: Supplementary / Optional Demo

---

## Slide 1: Title Slide

**Parallel Computing in Python: Dask & Ray**

*Scaling from Laptop to Cluster*

MIS 769 - Advanced Data Analytics

---

## Slide 2: The Problem

**Why Can't We Just Use Pandas?**

- Pandas loads everything into memory
- Single-threaded by default
- 10GB dataset + 8GB RAM = crash
- 100M row groupby = waiting... waiting...

**Real-world scenarios:**
- Processing log files (terabytes)
- Analyzing sensor data (millions of records/day)
- Training ML models on large datasets

---

## Slide 3: The Solution - Parallel Computing

**Two Approaches:**

1. **Scale Up** - Bigger machine (expensive, has limits)
2. **Scale Out** - Multiple machines/cores working together

**Key Insight:**
> "If a task takes 8 hours on 1 core, can we do it in 1 hour on 8 cores?"

**The answer: Often yes, with the right tools**

---

## Slide 4: Meet the Tools

| Tool | Created By | Year | Focus |
|------|-----------|------|-------|
| **Dask** | Anaconda | 2015 | Data analytics |
| **Ray** | UC Berkeley | 2017 | General distributed computing |

Both are:
- Open source
- Python native
- Scale from laptop to cluster
- Free to use

---

## Slide 5: Why These Tools Exist

**Before Dask/Ray:**
- Hadoop/Spark = Java/Scala (steep learning curve)
- MPI = Complex, low-level
- Multiprocessing = Manual, error-prone

**With Dask/Ray:**
- Stay in Python
- Familiar APIs (NumPy, Pandas)
- Minimal code changes
- Same code: laptop → cluster

---

## Slide 6: Dask Overview

**"Parallel computing for analytics"**

**Core Components:**
- **Dask Array** → Parallel NumPy
- **Dask DataFrame** → Parallel Pandas
- **Dask Delayed** → Parallelize any function
- **Dask Bag** → Parallel lists (JSON, text)

**Key Concept: Lazy Evaluation**
- Build a task graph first
- Execute only when you call `.compute()`

---

## Slide 7: Dask - How It Works

```
[Chunk 1] [Chunk 2] [Chunk 3] [Chunk 4]
    ↓         ↓         ↓         ↓
 [Task 1] [Task 2]  [Task 3] [Task 4]  ← Parallel
    ↓         ↓         ↓         ↓
    └────────────┬────────────────┘
                 ↓
           [Aggregate]
                 ↓
             [Result]
```

**Chunks + Task Graph = Parallelism**

---

## Slide 8: Dask DataFrame Example

**Pandas (Sequential):**
```python
import pandas as pd
df = pd.read_csv('huge_file.csv')  # Loads ALL into RAM
result = df.groupby('category').mean()
```

**Dask (Parallel):**
```python
import dask.dataframe as dd
df = dd.read_csv('huge_file.csv')  # Lazy, chunked
result = df.groupby('category').mean().compute()
```

**Almost identical API!**

---

## Slide 9: Dask Pros & Cons

**PROS:**
- Familiar Pandas/NumPy API
- Handles larger-than-memory data
- Built-in dashboard for monitoring
- Lazy evaluation (optimize before running)
- Great for ETL and data pipelines

**CONS:**
- Not all Pandas functions supported
- Overhead for small datasets
- Limited for stateful computations
- Debugging can be tricky (lazy evaluation)

---

## Slide 10: Ray Overview

**"A universal framework for distributed computing"**

**Core Concepts:**
- **Tasks** → Stateless functions (`@ray.remote`)
- **Actors** → Stateful objects
- **Object Store** → Shared memory

**Key Concept: Futures**
- Launch tasks immediately
- Get results when ready

---

## Slide 11: Ray - How It Works

```python
# Regular function
def slow_task(x):
    return x * x

# Ray remote function
@ray.remote
def slow_task(x):
    return x * x

# Launch 4 tasks in parallel
futures = [slow_task.remote(i) for i in range(4)]
results = ray.get(futures)  # Wait for all
```

**One decorator = distributed!**

---

## Slide 12: Ray Actors (Stateful)

```python
@ray.remote
class Counter:
    def __init__(self):
        self.value = 0

    def increment(self):
        self.value += 1
        return self.value

# Create distributed counter
counter = Counter.remote()
ray.get(counter.increment.remote())  # Returns 1
ray.get(counter.increment.remote())  # Returns 2
```

**State persists across calls**

---

## Slide 13: Ray Pros & Cons

**PROS:**
- Very flexible (any Python code)
- Excellent for ML/AI workflows
- Stateful actors for complex logic
- Growing ecosystem (Ray Train, Ray Serve)
- Used by OpenAI, Uber, Netflix

**CONS:**
- Steeper learning curve than Dask
- No native DataFrame (use with Modin)
- More boilerplate for simple tasks
- Overhead for small computations

---

## Slide 14: Head-to-Head Comparison

| Feature | Dask | Ray |
|---------|------|-----|
| **Learning curve** | Easy | Moderate |
| **DataFrame API** | Native | Via Modin |
| **NumPy API** | Native | Manual |
| **Custom functions** | `delayed()` | `@ray.remote` |
| **Stateful compute** | Limited | Actors |
| **ML Integration** | Scikit-learn | PyTorch, TF, HF |
| **Task visualization** | Built-in | Limited |
| **Best for** | Data ETL | ML pipelines |

---

## Slide 15: Performance - Real Numbers

**Test: 5 Million Row DataFrame GroupBy**

| Method | Time |
|--------|------|
| Pandas (sequential) | 2.5 sec |
| Dask (4 workers) | 0.8 sec |
| **Speedup** | **3.1x** |

**Test: 8 Independent Tasks (1 sec each)**

| Method | Time |
|--------|------|
| Sequential | 8.0 sec |
| Ray (4 workers) | 2.5 sec |
| **Speedup** | **3.2x** |

---

## Slide 16: When to Use What

**Use Dask when:**
- Working with large CSV/Parquet files
- Scaling Pandas workflows
- Building ETL pipelines
- Need familiar DataFrame API

**Use Ray when:**
- Building ML training pipelines
- Need stateful distributed objects
- Custom parallel algorithms
- Integrating with PyTorch/TensorFlow

**Use Both:**
- Dask-on-Ray combines both worlds

---

## Slide 17: The "Over-Stacking Compute" Anti-Pattern

**A common (expensive) mistake:**

```
"I have 81 GB of data to process..."
        ↓
"Let me spin up a 256 GB RAM cluster on Databricks"
        ↓
df = pd.read_parquet("huge_file.parquet")
result = df.groupby('category').mean()  ← SINGLE CORE!
        ↓
"Why is only 1 of my 64 cores at 100%?"
        ↓
"Taking 4 hours... let me get a BIGGER cluster"
        ↓
💸💸💸 (Wasted cloud spend)
```

**The problem:** Pandas is single-threaded. More cores don't help.

---

## Slide 18: Real-World Example - RAPIDS vs Pandas

**Problem Gambling Research Dataset:**
- 10 million banking transactions
- 342,000 users
- 30 engineered features

| Operation | Pandas (CPU) | RAPIDS (GPU) |
|-----------|-------------|--------------|
| Load 10M rows | ~30 sec | ~3 sec |
| Feature engineering | Minutes | Seconds |
| **KMeans clustering** | 60+ sec | **0.43 sec** |
| **t-SNE (50K points)** | 10+ min | **1.62 sec** |

**Full dataset: 81 GB** - Pandas can't even load it!

---

## Slide 19: The Bigger Picture

**Python Parallelization Landscape:**

```
                    ┌─────────────────┐
                    │   Your Code     │
                    └────────┬────────┘
         ┌──────────────────┼──────────────────┐
         ▼                  ▼                  ▼
    ┌─────────┐       ┌──────────┐       ┌─────────┐
    │  Dask   │       │   Ray    │       │ RAPIDS  │
    │ (CPU)   │       │ (CPU)    │       │ (GPU)   │
    └─────────┘       └──────────┘       └─────────┘
         │                  │                  │
         └──────────────────┼──────────────────┘
                            ▼
                 ┌─────────────────────┐
                 │  Cluster / Cloud    │
                 └─────────────────────┘
```

**Choose based on your bottleneck:**
- CPU-bound, need parallelism → **Dask/Ray**
- Data doesn't fit in memory → **Dask**
- Have GPU, need speed → **RAPIDS**

---

## Slide 20: Also Worth Knowing

**Other Tools in This Space:**

| Tool | Description |
|------|-------------|
| **RAPIDS (cuDF)** | GPU-accelerated Pandas (NVIDIA) |
| **Modin** | Drop-in Pandas replacement (uses Ray/Dask) |
| **Polars** | Fast DataFrame library (Rust-based) |
| **Vaex** | Out-of-core DataFrames |
| **PySpark** | Python API for Apache Spark |

**Trend:** Python is becoming the language for big data

---

## Slide 21: Getting Started

**Installation:**
```bash
pip install dask[complete]
pip install ray[default]
```

**Google Colab:**
- Both work out of the box
- Free GPU/TPU for RAPIDS

**Resources:**
- docs.dask.org
- docs.ray.io
- Demo notebook in Week 3/optional

---

## Slide 22: Key Takeaways

1. **Pandas is single-threaded** - Bigger clusters won't help!

2. **Don't over-stack compute** - Use tools that parallelize

3. **Dask = Parallel Pandas** - Multi-core CPU utilization

4. **Ray = Distributed Python** - Custom parallel workflows

5. **RAPIDS = GPU acceleration** - 100x speedups possible

6. **Match tool to bottleneck:**
   - CPU-bound → Dask/Ray
   - Need GPU → RAPIDS
   - Memory-bound → Dask

**Demo Time!** → See notebook

---

## Slide 23: Questions?

**Resources:**
- Demo: `Week_03/optional/MIS769_Demo_Dask_Ray_Parallelization.ipynb`
- Dask: https://docs.dask.org
- Ray: https://docs.ray.io

---

## Speaker Notes

### For the Demo:

1. **Start with the problem** - Show a Pandas operation that's slow or crashes
2. **Show Dask DataFrame** - Same API, faster results
3. **Show Ray remote functions** - Parallelize custom code
4. **Compare timings** - Let students see the speedup
5. **Mention RAPIDS** - If they have GPU access, even faster

### Key Points to Emphasize:

- This is about **scaling** your existing Python skills
- You don't need to learn Spark/Java to do big data
- Start on laptop, move to cluster when needed
- The ecosystem is maturing rapidly (2025 is a great time to learn)

### The "Over-Stacking" Story (Use This!):

Tell students: "I've seen this happen countless times in industry..."

1. Company has 50GB of transaction data
2. Data scientist writes Pandas code, takes 3 hours
3. "Let me get a bigger machine" - spins up 64-core cluster
4. Still takes 3 hours (Pandas uses 1 core!)
5. "Must need even MORE compute" - upgrades to 128 cores
6. Still 3 hours, but now 10x the cloud bill
7. Solution: Switch to Dask/RAPIDS, same code, 10 minutes

**The punchline:** They were paying for 64 cores but using 1. That's like renting a 64-car garage but only parking one car.

**How to check:** Open Activity Monitor (Mac) / htop (Linux) / Task Manager (Windows) while running Pandas. You'll see ONE core at 100%, the rest idle. With Dask/Ray, you'll see ALL cores working.

### Common Student Questions:

**Q: When is Pandas enough?**
A: If your data fits in memory and processing is fast enough, stick with Pandas. These tools add complexity.

**Q: Can I use both together?**
A: Yes! Dask-on-Ray runs Dask computations on a Ray cluster.

**Q: What about PySpark?**
A: PySpark is great for existing Spark infrastructure. Dask/Ray are more Pythonic and easier to start with.
