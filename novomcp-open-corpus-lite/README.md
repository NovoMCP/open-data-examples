# NovoMCP Open Corpus (lite): 122M compounds

**122,454,458 compounds · 59 columns · 36.8 GB · 21 Parquet files (Snappy)** · CC-BY-4.0

Physicochemical + ADMET properties for the full PubChem compound space, derived with
RDKit and NovoMCP's own ADMET models. Most ADMET endpoints are trained on the public
Therapeutics Data Commons (TDC) ADMET benchmark (MapLight / admet_ai, MIT-licensed); a
few base heads are trained on other public toxicology data (e.g. Tox21) — see the card
for per-endpoint provenance. Plus a machine-learned logP and PAINS structural-alert
flags. This is the **lite** variant (no learned embeddings). Model estimates for
research — not experimental measurements or regulatory determinations.

## Docs
- **[dataset-card.md](./dataset-card.md)** — full column dictionary, per-endpoint provenance, data-quality notes.
- **[get-to-know-a-dataset.ipynb](./get-to-know-a-dataset.ipynb)** — worked examples.

## Access
- **AWS Open Data:** `s3://novomcp-open-corpus/novomcp-open-corpus-lite/` (us-east-2, anonymous read)
- **Kaggle:** https://www.kaggle.com/datasets/novomcp/novomcp-corpus-lite-122m
- **Zenodo** (concept DOI — resolves to the latest version): https://doi.org/10.5281/zenodo.21710894

## Notes for series / trajectory work
- SMILES are **PubChem-style Kekulé** — a naive RDKit-canonical string join returns 0 rows
  for any aromatic. Match on fingerprints, or canonicalize your query to the same form.
- Shards are **partitioned by molecular-weight band, not CID** — you can't filter by CID,
  and homologous series get truncated at band edges. Pool bands before any series/trajectory
  analysis, and read scaffold groups complete (a partial band splits groups and shortens chains).
