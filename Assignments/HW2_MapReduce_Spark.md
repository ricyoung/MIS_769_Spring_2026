# Homework 2: MapReduce Concepts & Spark Fundamentals

**Course:** MIS 769 - Big Data Analytics for Business
**Points:** 20
**Due:** Sunday, February 8, 2026 @ 11pm Pacific

---

## Learning Objectives

By completing this assignment, you will demonstrate the ability to:

1. **Set up** Apache Spark on Google Colab
2. **Understand** how Spark partitions data for parallel processing
3. **Measure and compare** processing performance with different configurations
4. **Apply** K-Means clustering and interpret business results
5. **Explain** distributed computing through your own diagram

---

## How You'll Be Graded

| Component | Points | What We're Looking For |
|-----------|--------|------------------------|
| **Spark Setup** | 3 | Successfully create a Spark session and explain configuration |
| **Data Partitioning** | 5 | Demonstrate understanding of how data is distributed |
| **Performance Experiment** | 5 | Run experiments and analyze why speedup isn't linear |
| **K-Means Clustering** | 5 | Apply clustering to Netflix data and interpret clusters |
| **Diagram** | 2 | Create a clear diagram explaining distributed processing |
| **Total** | **20** | |

---

## The Big Picture

When data gets too big for one computer, we split the work across many computers. Spark is the industry-standard tool for this.

```
YOUR LAPTOP                           SPARK CLUSTER
+----------------------+            +---------------------------+
|                      |            |     Driver Program        |
|   1 million rows     |            |         +----+            |
|   Takes: 10 minutes  |   ------>  |         |    |            |
|                      |            |    +----+----+----+       |
+----------------------+            |    |    |    |    |       |
                                    |   W1   W2   W3   W4       |
                                    |  250K 250K 250K 250K      |
                                    |  each worker in parallel  |
                                    +---------------------------+
```

---

## What to Do

1. **Open the starter notebook** in Google Colab
2. **Complete Part 1:** Install Spark and create a session
3. **Complete Part 2:** Explore data partitioning
4. **Complete Part 3:** Run the performance experiment with 1, 2, and 4 cores
5. **Complete Part 4:** Apply K-Means to Netflix data and name your clusters
6. **Complete Part 5:** Draw your own diagram of how Spark processes data

---

## Questions to Answer

- **Q1:** What does `local[2]` mean? What would `local[4]` do differently?
- **Q2:** Why doesn't 4 cores give exactly 4x speedup?
- **Q3:** What characterizes each cluster? Give them descriptive names.

---

## Submission

Upload to Canvas:
- Your completed `.ipynb` notebook with all cells executed

---

## Grading Rubric

| Criteria | Excellent (100%) | Good (85%) | Needs Work (70%) |
|----------|------------------|------------|------------------|
| **Spark Setup** | Complete with clear explanation | Setup works, minimal explanation | Setup issues |
| **Partitioning** | Shows clear understanding | Basic understanding | Confusion about concepts |
| **Performance** | Analyzes results with insight | Reports results only | Incomplete experiment |
| **Clustering** | Meaningful cluster names with justification | Names given but weak reasoning | Generic or missing names |
| **Diagram** | Clear, accurate, shows understanding | Mostly accurate | Incomplete or inaccurate |

---

## Resources

- **Starter Notebook:** [MIS769_HW2_MapReduce_Spark.ipynb](Notebooks/MIS769_HW2_MapReduce_Spark.ipynb)
- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)
- [How Netflix Uses Spark](https://netflixtechblog.com/spark-and-spark-streaming-at-netflix-21e9e5e3cd44)
