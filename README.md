# Topology-Aware Semantic BIM Integration for Building Envelope Retrofits

**Scenario Assessment and IFC Materialisation**

**Authors:** Jacopo Cassandro and Claudio Mirarchi  
**Affiliation:** Department of Architecture, Built Environment and Construction Engineering (DABC), Politecnico di Milano, Italy  
**Manuscript:** *Topology-Aware Semantic BIM Integration for Building Envelope Retrofits: Scenario Assessment and IFC Materialisation* (manuscript prepared for submission to *Sustainability*, 2026).  
**Project context:** Horizon Europe SPHEERE, Grant Agreement No. 101235544.  
**Correspondence:** jacopo.cassandro@polimi.it


## Overview

Building envelope retrofit design requires the comparison of alternative insulation products according to thermal performance, admissible thickness, material cost and environmental implications. The required information is often distributed across independently developed as-is and to-be BIM models and external product catalogues.

The research introduces a **topology-aware, ontology-based workflow** that connects independent IFC models through geometry-derived cross-model interfaces represented as `bot:Interface` resources. Linked Building Data and SPARQL queries connect the existing walls, preliminary renovation coverings and product-specific insulation attributes without first merging the original BIM models. The resulting scenarios are screened against design constraints and compared by the designer. A selected configuration can then be materialised in a separate IFC model.

The method is intended for **constraint-based feasibility screening and designer-driven comparison**, not automatic multi-objective optimisation. Thermal performance is assessed through a simplified steady-state calculation; A1–A3 GWP values do not constitute a full life-cycle assessment.

## Methodology

![Workflow of semantic BIM integration, scenario screening and IFC materialisation](manuscript/IMG/fig-1.png)

1. Represent the independent **as-is** and **preliminary to-be** IFC models and the insulation product catalogue as semantic data.
2. Derive cross-model geometric interfaces and express them as semantic relationships.
3. Retrieve building, retrofit, material and interface information through SPARQL-based views.
4. Apply thermal and dimensional constraints to available product–thickness variants.
5. Compare feasible variants using thermal transmittance, thickness, unit material price and product-stage GWP; investigate changing thickness limits.
6. Apply a designer-selected product to the relevant `IfcCovering` objects in a separately generated IFC model.

## Findings reported in the manuscript

| Analysis | Reported result |
| --- | ---: |
| Existing external walls | 54 |
| Existing walls connected by semantic interfaces | 36 |
| Proposed renovation coverings | 166 |
| Coverings connected by semantic interfaces | 162 |
| Cross-model semantic interfaces | 250 |
| Investigated insulation product variants | 101 in 7 families |
| Feasible variants, `U <= 0.28 W/(m² K)` and `d <= 200 mm` | 54 |
| Feasible variants at a maximum of 80 mm | 5 |
| Coverings affected by the demonstrative IFC materialisation | 15 |
| Selected material | Aeropan 50 mm (`M06_T050`) |
| Local intervention insulation area | 21.8033 m² |
| Indicative insulation material cost | EUR 7,387.39 |
| Indicative insulation A1–A3 GWP | 1,078.17 kg CO₂eq |

**Interpretation:** The last two figures are *derived estimates* using area-based unit indicators from the experimental catalogue. The cost covers insulation material supply, not installation or other works; the GWP covers the selected insulation material's A1–A3 stages. Product information includes documented values as well as estimates/proxies, as described in the manuscript. The local 15-covering example must not be conflated with the 36-wall feasibility study.

### Example of the generated IFC

![Selected covering and associated information in the generated IFC](manuscript/IMG/fig-8.png)

The manuscript documents a demonstration where the selected product modifies insulation geometry, links material and environmental properties and introduces native IFC cost information. The final reported IFC representation uses a consolidated `IfcCostItem` linked to 15 coverings, together with quantities and a material unit price. The original IFC files and generated IFC output must be deposited separately for independent inspection.

## Repository contents

```text
2026_Semantic-Retrofit-Decision-Support/
├── README.md
├── CITATION.cff
├── .gitignore
├── manuscript/
│   ├── template.tex
│   ├── references.bib
│   ├── IMG/                    # eight manuscript figures, original formats
│   └── Definitions/            # supplied MDPI LaTeX template assets
├── queries/
│   ├── 01_cross_model_interfaces.rq
│   ├── 02_insulation_product_properties.rq
│   └── 03_uniform_retrofit_feasibility.rq
├── results/
│   ├── semantic_integration_summary.csv
│   ├── insulation_variant_feasibility.csv
│   ├── representative_insulation_alternatives.csv
│   ├── thickness_constraint_sensitivity.csv
│   └── selected_scenario_summary.csv
├── data/
│   ├── ifc/                    # add input and generated IFC files after release approval
│   ├── catalogue/              # add insulation variant spreadsheet and provenance
│   └── rdf/                    # add RDF graphs / exports if distributable
└── docs/
    ├── REPRODUCIBILITY.md
    └── RELEASE_CHECKLIST.md
```

The `manuscript/` folder preserves the uploaded LaTeX project, including its exact original figure paths. The `queries/` folder contains examples extracted from commented LaTeX listings; they are not a complete tested query suite. The `results/` folder contains machine-readable summaries transcribed from the paper, **not** full raw results, run logs or independently verified output.

## SPARQL queries

| File | Intended output |
| --- | --- |
| [`queries/01_cross_model_interfaces.rq`](queries/01_cross_model_interfaces.rq) | Existing wall / renovation covering relationships, interface overlap and coverage metrics. |
| [`queries/02_insulation_product_properties.rq`](queries/02_insulation_product_properties.rq) | Insulation product variants, thermal conductivity and thickness, with units. |
| [`queries/03_uniform_retrofit_feasibility.rq`](queries/03_uniform_retrofit_feasibility.rq) | Feasible uniform product variants under the 0.28 W/(m² K) and 200 mm screening thresholds. |

Running the queries requires compatible RDF data and a SPARQL 1.1 endpoint. The application-specific `example.org` namespace IRIs visible in parts of the manuscript are illustrative identifiers; they should not be interpreted as publicly hosted operational services. Query execution has **not** been verified using this starter package because the source RDF datasets were not provided in the uploaded manuscript archive.

## Reproducing the analysis

Please consult [`docs/REPRODUCIBILITY.md`](docs/REPRODUCIBILITY.md) for the required input files, suggested processing sequence, and current evidence gaps. The preliminary manuscript tables are provided in [`results/`](results/) as a reference against which a later full reproduction can be checked.

## Manuscript and figures

The original uploaded LaTeX project is available in [`manuscript/`](manuscript/), with the main file [`template.tex`](manuscript/template.tex) and the bibliography [`references.bib`](manuscript/references.bib). Figure files remain under [`manuscript/IMG/`](manuscript/IMG/). Compilation generally requires a LaTeX distribution, BibTeX and `minted` support (`-shell-escape` may be required).

## Citation

Please cite the associated article once bibliographic publication details are available. Until then, the provisional citation is:

> Cassandro, J.; Mirarchi, C. *Topology-Aware Semantic BIM Integration for Building Envelope Retrofits: Scenario Assessment and IFC Materialisation*. Manuscript prepared for *Sustainability*, 2026.

See [`CITATION.cff`](CITATION.cff) for repository metadata. **No journal DOI has been assigned in this repository.**

## Data provenance and reuse

The source project reports heterogeneous material data quality and preliminary screening assumptions. Users should not interpret the experimental GWP values as uniformly verified EPD results, nor treat the indicative material prices as full renovation costs. Redistribution of original BIM files and commercial product data requires confirmation of applicable permissions.

**License:** No blanket license is asserted here. A suitable license for original research code and a separate data/figure reuse policy should be selected after confirming ownership and rights, including rights associated with the supplied publisher template.

## Acknowledgements

This work was carried out in the framework of the European Horizon SPHEERE project (Grant Agreement No. 101235544).
