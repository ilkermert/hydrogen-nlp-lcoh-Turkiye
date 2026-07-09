# A Hybrid NLP–Techno-Economic Framework for Hydrogen Policy Analysis: Integrating SWOT Text Mining with Monte Carlo LCOH Projections

**Authors**

İlker Mert¹, Hüseyin Yağlı², Jorge Costa³⁴, Ana Paula Oliveira³⁵*

¹ Osmaniye Korkut Ata University, Türkiye  
² Gaziantep University, Türkiye  
³ ISEC Lisboa, Portugal  
⁴ NOVA School of Science and Technology (NOVA FCT), Portugal  
⁵ MARE-IPSetúbal, Portugal  

*Corresponding Author: ana.oliveira@iseclisboa.pt*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## Overview

This repository provides supplementary materials associated with the article:

> **A Hybrid NLP–Techno-Economic Framework for Hydrogen Policy Analysis: Integrating SWOT Text Mining with Monte Carlo LCOH Projections**

The study combines Natural Language Processing (NLP) and techno-economic modeling to evaluate hydrogen policy frameworks and their implications for green hydrogen production costs.

The framework consists of two integrated components:

1. **Policy Text Analysis**
   - SWOT-based classification of hydrogen policy documents.
   - Hybrid keyword and zero-shot NLP pipeline.
   - Expert validation using manually annotated samples.

2. **Techno-Economic Assessment**
   - Levelized Cost of Hydrogen (LCOH) modeling.
   - Monte Carlo uncertainty analysis.
   - Sensitivity assessment using Sobol indices.
   - Long-term projections for green hydrogen production systems.

---

## Methodological Framework

### Policy Analysis Pipeline

The NLP workflow includes:

- Text preprocessing
- TF-IDF vectorization
- K-Means clustering for sub-theme identification
- Hybrid SWOT classification
- Expert validation

#### Example: TF-IDF and K-Means Clustering

```python
vectorizer = TfidfVectorizer(
    stop_words=turkish_stopwords,
    max_features=100
)

tfidf_matrix = vectorizer.fit_transform(texts)

clusters = KMeans(
    n_clusters=k,
    random_state=42
).fit_predict(tfidf_matrix)
```

#### Example: Hybrid SWOT Classification

```python
classifier = pipeline(
    "zero-shot-classification",
    model="facebook/bart-large-mnli"
)

def classify_sentence(
    sentence,
    keyword_lexicon,
    keyword_threshold=1
):
    hit, score = match_keywords(
        sentence,
        keyword_lexicon
    )

    if score >= keyword_threshold:
        return hit, score, "keyword"

    result = classifier(
        sentence,
        [
            "Strength",
            "Weakness",
            "Opportunity",
            "Threat"
        ]
    )

    return (
        result["labels"][0],
        result["scores"][0],
        "zero-shot"
    )
```

---

### LCOH Modeling

The Levelized Cost of Hydrogen was calculated using discounted annualized capital costs, operating expenditures, and electricity consumption.

#### Example: LCOH Function

```python
def compute_lcoh(
    capex,
    sec,
    price_mwh,
    hours,
    opex_share,
    crf
):
    price_kwh = price_mwh / 1000
    opex = opex_share * capex

    return (
        (crf * capex + opex)
        / (hours / sec)
        + sec * price_kwh
    )
```

where:

- CAPEX = capital expenditure (€ kW⁻¹)
- SEC = specific energy consumption (kWh kgH₂⁻¹)
- CRF = capital recovery factor
- OPEX = annual operating expenditure
- pₑₗ = electricity price

---

### Monte Carlo Simulation

Uncertainty propagation was performed using 10,000 Monte Carlo iterations.

```python
def run_monte_carlo(
    sample_inputs_fn,
    n_iter=10000
):
    return [
        compute_lcoh(*sample_inputs_fn())
        for _ in range(n_iter)
    ]
```

First-order Sobol indices were estimated through variance decomposition of simulation outputs.

---

## Validation Results

The SWOT classification framework was validated using expert-labelled samples.

Cohen's Kappa (κ): 0.81

According to the Landis and Koch interpretation scale, this value indicates substantial agreement between automated and expert classifications.

---

## Citation

If you use this repository, please cite:

```text
Mert, İ., Yağlı, H., Costa, J., & Oliveira, A. P. (2026).
A Hybrid NLP–Techno-Economic Framework for Hydrogen Policy Analysis:
Integrating SWOT Text Mining with Monte Carlo LCOH Projections.
Sustainability.
```

---

## License

This project is distributed under the MIT License.

---

## Contact

Ana Paula Oliveira  
ISEC Lisboa / MARE-IPSetúbal  
Email: ana.oliveira@iseclisboa.pt
