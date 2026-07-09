A Hybrid NLP–Techno-Economic Framework for Hydrogen Policy Analysis: Integrating SWOT Text Mining with Monte Carlo LCOH Projections
Authors: İlker Mert¹, Hüseyin Yağlı², Jorge Costa³,⁴, Ana Paula Oliveira³,⁵,*
¹Osmaniye Korkut Ata University, Türkiye · ²Gaziantep University, Türkiye · ³ISEC Lisboa, Portugal · ⁴NOVA FCT, Portugal · ⁵MARE-IPSetúbal, Portugal
*Correspondence: ana.oliveira@iseclisboa.pt

https://img.shields.io/badge/License-MIT-yellow.svg

Supplementary documentation for the article "A Hybrid NLP–Techno-Economic Framework for Hydrogen Policy Analysis: Integrating SWOT Text Mining with Monte Carlo LCOH Projections."

About the Study
Text-Mining SWOT Analysis — Turkish hydrogen policy documents classified into Strength/Weakness/Opportunity/Threat categories and sub-themes using a hybrid keyword + zero-shot pipeline, validated by expert annotation (Cohen's κ = 0.81).

Stochastic LCOH Modeling — Monte Carlo simulation (N = 10,000) propagating uncertainty in CAPEX, SEC, electricity price, operating hours, OPEX, discount rate, and plant lifetime for a 20 MW PV-coupled alkaline electrolysis plant (2025–2050), with first-order Sobol sensitivity analysis.

Methodology (condensed excerpts)
The snippets below illustrate the core logic of each pipeline stage for transparency. They are summaries, not the full production codebase.

Sub-theme clustering (TF-IDF + K-Means):

python
vectorizer = TfidfVectorizer(stop_words=turkish_stopwords, max_features=100)
tfidf_matrix = vectorizer.fit_transform(texts)
clusters = KMeans(n_clusters=k, random_state=42).fit_predict(tfidf_matrix)
Hybrid SWOT classification (keyword-first, zero-shot fallback):

python
classifier = pipeline("zero-shot-classification", model="facebook/bart-large-mnli")

def classify_sentence(sentence, keyword_lexicon, keyword_threshold=1):
    hit, score = match_keywords(sentence, keyword_lexicon)   # domain lexicon + negation window
    if score >= keyword_threshold:
        return hit, score, "keyword"
    result = classifier(sentence, ["Strength", "Weakness", "Opportunity", "Threat"])
    return result["labels"][0], result["scores"][0], "zero-shot"
LCOH formulation (Section 2.4):

python
def compute_lcoh(capex, sec, price_mwh, hours, opex_share, crf):
    # p_el converted €/MWh → €/kWh ; OPEX_t = opex_share * CAPEX_t
    price_kwh = price_mwh / 1000
    opex = opex_share * capex
    return (crf * capex + opex) / (hours / sec) + sec * price_kwh
Monte Carlo / Sobol sensitivity (structure only):

python
def run_monte_carlo(sample_inputs_fn, n_iter=10_000):
    return [compute_lcoh(*sample_inputs_fn()) for _ in range(n_iter)]

# First-order Sobol indices (S1) estimated via variance decomposition
# on the Monte Carlo output across the parameter set in Table 1.
Repository Contents
Content	Description
Annotated corpus	Sentence-level SWOT-labeled Turkish policy text extracts
Sub-theme keyword lexicon	Turkish domain keyword dictionary used for automated labeling
Methodology excerpts	Condensed code illustrating clustering, classification, and LCOH/Monte Carlo logic (above)
Output tables	Summary statistics corresponding to Tables 1–5 in the manuscript
Note: Full Turkish policy source documents follow original publishers' terms; sentence-level classification outputs and document metadata are included here.

Model Validation
Expert-supervised SWOT annotation agreement: Cohen's κ = 0.81 (substantial agreement, Landis & Koch scale).

Data Sources
CAPEX/SEC/learning-curve assumptions: Frieden & Leker (2024); Rasul et al. (2024); Brändle et al. (2021)

Learning-curve rates: IEA Global Hydrogen Review (2024); IRENA (2024)

Electricity price: Turkish industrial tariff data (log-normal, μ = 67 €/MWh)

Citation
Mert, İ., Yağlı, H., Costa, J., & Oliveira, A.P. (2026). A Hybrid NLP–Techno-Economic Framework for Hydrogen Policy Analysis: Integrating SWOT Text Mining with Monte Carlo LCOH Projections. Sustainability, Volume, [Pages].

License
MIT License.

Contact
Full code, annotated corpus, and simulation scripts available upon reasonable request:

Ana Paula Oliveira — ana.oliveira@iseclisboa.pt
