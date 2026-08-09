# NovoMCP — AWS Open Data examples

Example notebooks and documentation for NovoMCP datasets in the [AWS Registry of Open Data](https://registry.opendata.aws).

## Datasets

### [NovoMCP Open Corpus (lite)](novomcp-open-corpus-lite/)
122,454,458 PubChem compounds with RDKit physicochemical descriptors, ADMET/toxicity
predictions from NovoMCP's own models (most trained on the public Therapeutics Data
Commons ADMET benchmark, MapLight / admet_ai, MIT; a few base heads on other public
toxicology data such as Tox21 — see the card for per-endpoint provenance), a
machine-learned logP, and PAINS structural-alert flags. Apache Parquet, CC-BY-4.0.

- **Data dictionary:** [novomcp-open-corpus-lite/README.md](novomcp-open-corpus-lite/README.md)
- **Get-to-know tutorial:** [novomcp-open-corpus-lite/get-to-know-a-dataset.ipynb](novomcp-open-corpus-lite/get-to-know-a-dataset.ipynb)
- **Also on:** [Kaggle](https://www.kaggle.com/datasets/novomcp/novomcp-corpus-lite-122m) · [Zenodo (concept DOI 10.5281/zenodo.21710894 — resolves to latest)](https://doi.org/10.5281/zenodo.21710894)

Produced by the open-source [NovoMCP engine](https://github.com/NovoMCP/novomcp).
