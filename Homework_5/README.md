# Homework 5: Named Entity Recognition (NER)

**Points:** 20 | **Due:** Sunday, March 1, 2026 @ 11pm Pacific

**Author:** Richard Young, Ph.D. | UNLV Lee Business School

**Compute:** CPU (free tier) — GPU recommended for bonus

---

## Learning Objectives

1. **Understand** Named Entity Recognition concepts
2. **Use** spaCy for entity extraction
3. **Extract** business-relevant entities from text data
4. **Analyze** entity patterns in reviews
5. **Visualize** entity distributions

---

## Grading

| Component | Points | What We're Looking For |
|-----------|--------|------------------------|
| Setup & Data | 3 | spaCy loaded, data ready |
| NER Demo | 4 | Understand entity types |
| Entity Extraction | 6 | Extract ORG, PRODUCT, GPE from reviews |
| Analysis | 4 | Identify patterns in entity mentions |
| Visualization | 3 | Clear bar charts of top entities |
| **Total** | **20** | |

---

## Entity Types

spaCy recognizes these business-relevant entities:

| Entity | Description | Example |
|--------|-------------|---------|
| ORG | Organizations | "Apple", "Amazon" |
| PRODUCT | Products | "iPhone", "Kindle" |
| GPE | Countries, cities | "New York", "USA" |
| PERSON | People names | "Tim Cook" |
| MONEY | Monetary values | "$50" |

---

## Instructions

1. Open `MIS769_HW5_NER.ipynb` in Google Colab
2. Load spaCy and understand entity types
3. Extract entities from your review dataset
4. Analyze: Which organizations/products are mentioned most?
5. Visualize the top entities by category

---

## Questions to Answer

- **Q1:** Were the extracted entities accurate? What errors did you observe?
- **Q2:** What business insights can you derive from entity mentions?
- **Q3:** How could you improve NER for your specific domain?

---

## Submission

Upload to Canvas:
- Your completed `.ipynb` notebook with all cells executed

---

\vspace{1cm}

*— Richard Young, Ph.D.*
