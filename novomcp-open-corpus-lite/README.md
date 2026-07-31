# NovoMCP Open Corpus (lite): 122M compounds

**122,454,458 compounds · 59 columns · 27.4 GB · 1409 Parquet files (Snappy)**

Physicochemical + ADMET properties for the full PubChem compound space, released
under CC-BY-4.0. Derived with RDKit and TDC-benchmark ADMET models. This is the
**lite** variant (no learned embeddings).

## What's in it
- One row per PubChem compound, keyed by `cid`.
- RDKit physicochemical descriptors, drug-likeness (QED), synthetic accessibility.
- TDC-benchmark ADMET predictions (toxicity, CYP inhibition/substrate, absorption,
  distribution, clearance) as calibrated probabilities.
- PAINS structural-alert flags (the public RDKit filter catalog).

## Scope
Values here are computed descriptors and model predictions intended for research —
similarity search, library filtering, ADMET screening and triage, and as a training
substrate. The ADMET fields are model estimates, not experimental measurements, and
nothing in this dataset is a regulatory or compliance determination.

## Columns

| column | description | units | source |
|---|---|---|---|
| `cid` | PubChem Compound ID (primary key). | — | PubChem |
| `smiles` | Canonical SMILES. | — | RDKit canonicalization |
| `molecular_weight` | Molecular weight. | g/mol | RDKit Descriptors.MolWt |
| `xlogp` | Octanol–water partition coefficient (Crippen). | log10 | RDKit Crippen.MolLogP |
| `tpsa` | Topological polar surface area. | Å² | RDKit Descriptors.TPSA |
| `hbd_count` | Hbd count. | — | NovoMCP enrichment |
| `hba_count` | Hba count. | — | NovoMCP enrichment |
| `rotatable_bonds` | Rotatable bonds. | count | RDKit Descriptors.NumRotatableBonds |
| `complexity` | Molecular complexity. | — | PubChem/RDKit |
| `heavy_atom_count` | Heavy atom count. | — | NovoMCP enrichment |
| `molecular_formula` | Molecular formula. | — | NovoMCP enrichment |
| `binding_affinity_score` | Heuristic binding-affinity score. | — | NovoMCP model |
| `cardiotoxicity_1d_probability` | Predicted probability for the cardiotoxicity 1d endpoint. | 0–1 | TDC-benchmark ADMET model |
| `cardiotoxicity_5d_probability` | Predicted probability for the cardiotoxicity 5d endpoint. | 0–1 | TDC-benchmark ADMET model |
| `cardiotoxicity_10d_probability` | Predicted probability for the cardiotoxicity 10d endpoint. | 0–1 | TDC-benchmark ADMET model |
| `cardiotoxicity_30d_probability` | Predicted probability for the cardiotoxicity 30d endpoint. | 0–1 | TDC-benchmark ADMET model |
| `cyp1a2_inhibitor_probability` | Predicted probability for the cyp1a2 inhibitor endpoint. | 0–1 | TDC-benchmark ADMET model |
| `cyp2c19_inhibitor_probability` | Predicted probability for the cyp2c19 inhibitor endpoint. | 0–1 | TDC-benchmark ADMET model |
| `cyp2c9_inhibitor_probability` | Predicted probability for the cyp2c9 inhibitor endpoint. | 0–1 | TDC-benchmark ADMET model |
| `cyp2d6_inhibitor_probability` | Predicted probability for the cyp2d6 inhibitor endpoint. | 0–1 | TDC-benchmark ADMET model |
| `cyp3a4_inhibitor_probability` | Predicted probability for the cyp3a4 inhibitor endpoint. | 0–1 | TDC-benchmark ADMET model |
| `nr_ahr_agonist_probability` | Predicted probability for the nr ahr agonist endpoint. | 0–1 | TDC-benchmark ADMET model |
| `nr_ar_lbd_agonist_probability` | Predicted probability for the nr ar lbd agonist endpoint. | 0–1 | TDC-benchmark ADMET model |
| `nr_ar_agonist_probability` | Predicted probability for the nr ar agonist endpoint. | 0–1 | TDC-benchmark ADMET model |
| `nr_aromatase_inhibitor_probability` | Predicted probability for the nr aromatase inhibitor endpoint. | 0–1 | TDC-benchmark ADMET model |
| `nr_er_lbd_agonist_probability` | Predicted probability for the nr er lbd agonist endpoint. | 0–1 | TDC-benchmark ADMET model |
| `nr_er_agonist_probability` | Predicted probability for the nr er agonist endpoint. | 0–1 | TDC-benchmark ADMET model |
| `nr_ppar_gamma_agonist_probability` | Predicted probability for the nr ppar gamma agonist endpoint. | 0–1 | TDC-benchmark ADMET model |
| `sr_are_activation_probability` | Predicted probability for the sr are activation endpoint. | 0–1 | TDC-benchmark ADMET model |
| `sr_atad5_activation_probability` | Predicted probability for the sr atad5 activation endpoint. | 0–1 | TDC-benchmark ADMET model |
| `sr_hse_activation_probability` | Predicted probability for the sr hse activation endpoint. | 0–1 | TDC-benchmark ADMET model |
| `sr_mmp_activation_probability` | Predicted probability for the sr mmp activation endpoint. | 0–1 | TDC-benchmark ADMET model |
| `sr_p53_activation_probability` | Predicted probability for the sr p53 activation endpoint. | 0–1 | TDC-benchmark ADMET model |
| `ames_mutagenicity_probability` | Predicted probability for the ames mutagenicity endpoint. | 0–1 | TDC-benchmark ADMET model |
| `carcinogenicity_probability` | Predicted probability for the carcinogenicity endpoint. | 0–1 | TDC-benchmark ADMET model |
| `clinical_toxicity_probability` | Predicted probability for the clinical toxicity endpoint. | 0–1 | TDC-benchmark ADMET model |
| `developmental_toxicity_probability` | Predicted probability for the developmental toxicity endpoint. | 0–1 | TDC-benchmark ADMET model |
| `eye_corrosion_probability` | Predicted probability for the eye corrosion endpoint. | 0–1 | TDC-benchmark ADMET model |
| `eye_irritation_probability` | Predicted probability for the eye irritation endpoint. | 0–1 | TDC-benchmark ADMET model |
| `hepatotoxicity_probability` | Predicted probability for the hepatotoxicity endpoint. | 0–1 | TDC-benchmark ADMET model |
| `reproductive_toxicity_probability` | Predicted probability for the reproductive toxicity endpoint. | 0–1 | TDC-benchmark ADMET model |
| `respiratory_toxicity_probability` | Predicted probability for the respiratory toxicity endpoint. | 0–1 | TDC-benchmark ADMET model |
| `logp` | Logp. | — | NovoMCP enrichment |
| `hba` | Hydrogen-bond acceptors. | count | RDKit Lipinski.NumHAcceptors |
| `hbd` | Hydrogen-bond donors. | count | RDKit Lipinski.NumHDonors |
| `overall_toxicity_score` | Overall Toxicity Score. | — | NovoMCP-derived |
| `cardiotoxicity_max_probability` | Predicted probability for the cardiotoxicity max endpoint. | 0–1 | TDC-benchmark ADMET model |
| `nuclear_receptor_activity_score` | Nuclear Receptor Activity Score. | — | NovoMCP-derived |
| `stress_response_activity_score` | Stress Response Activity Score. | — | NovoMCP-derived |
| `cyp_inhibition_risk_score` | Cyp Inhibition Risk Score. | — | NovoMCP-derived |
| `qed` | Quantitative Estimate of Drug-likeness. | 0–1 | RDKit QED |
| `drug_likeness` | Drug-likeness composite. | 0–1 | RDKit-derived |
| `synthetic_accessibility` | Synthetic accessibility score. | 1(easy)–10(hard) | RDKit SA_Score |
| `has_pains` | Has pains. | — | NovoMCP enrichment |
| `pains_types` | Pains types. | — | NovoMCP enrichment |
| `pains_severity` | Pains severity. | — | NovoMCP enrichment |
| `aromatic_ring_count` | Aromatic rings. | count | RDKit |
| `aromatic_atom_count` | Aromatic atoms. | count | RDKit |
| `fsp3` | Fsp3. | — | NovoMCP enrichment |

## Provenance
- **Source compounds:** PubChem CIDs (snapshot 2026-05-19).
- **Descriptors:** RDKit.
- **ADMET:** models trained on public Therapeutics Data Commons (TDC) benchmark sets.
- **Partitioning:** files sharded by CID range and molecular-weight band.

## License
CC-BY-4.0 (Creative Commons Attribution 4.0). Commercial use permitted with
attribution. See `license.txt`.

## How to cite
```bibtex
@dataset{novomcp_corpus_lite_2026,
  title  = {NovoMCP Open Corpus (lite): 122M compounds},
  author = {Harrison, Ari and {NovoMCP}},
  year   = {2026},
  publisher = {Zenodo},
  doi    = {10.5281/zenodo.21710895},
  note   = {lite variant; 122,454,458 compounds}
}
```

## How to use
```python
import pyarrow.dataset as ds
d = ds.dataset("path/to/parquet", format="parquet")
t = d.to_table(filter=(ds.field("molecular_weight") < 500) & (ds.field("qed") > 0.6),
               columns=["cid", "smiles", "qed", "hepatotoxicity_probability"])
print(t.to_pandas().head())
```
