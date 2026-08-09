# NovoMCP Open Corpus (lite): 122M compounds

**122,454,458 compounds · 59 columns · 36.8 GB · 21 Parquet files (Snappy)**

Physicochemical + ADMET properties for the full PubChem compound space, released
under CC-BY-4.0. Derived with RDKit and NovoMCP's own ADMET models. This is the
**lite** variant (no learned embeddings).

## What's in it
- One row per PubChem compound, keyed by `cid`.
- RDKit physicochemical descriptors, drug-likeness (QED), synthetic accessibility.
- ADMET predictions (CYP inhibition/substrate, absorption, distribution, clearance,
  toxicity) as screening-grade probabilities from NovoMCP's own models. Most endpoints
  are trained on the public Therapeutics Data Commons (TDC) ADMET benchmark
  (MapLight / admet_ai, MIT); a few base heads are trained on other public toxicology
  data (e.g. Tox21). Per-endpoint source is in the `source` column of the table below.
- PAINS structural-alert flags (the public RDKit filter catalog).
- Coverage note: a small fraction of ADMET endpoints are null (featurization edge cases).
- Shard coverage: a few small low-molecular-weight-band shards (e.g. `...00004`–`...00007`,
  `...00020`, ~400 KB each) carry **descriptors + PAINS only** — their ADMET columns are
  entirely null because the ADMET models don't score those very small molecules. If you
  sample the smallest shard first to read the schema, expect no ADMET there; the full
  ADMET layer lives in the larger shards.
- Data-quality pass: regression endpoints are physically bounded — non-negative
  quantities (`half_life_hr`, `clearance_hepatocyte`, `clearance_microsome`, `vdss_l_kg`)
  are `NULL` where the raw model predicted a negative value; `ppbr_percent` is clamped to
  [0, 100]; extreme out-of-domain tails on `ld50_log_mol_kg` and `lipophilicity_log_ratio`
  are set to `NULL`. `logp` is `NULL` for unparseable SMILES and out-of-domain predictions.

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
| `molecular_formula` | Molecular formula. | — | NovoMCP enrichment |
| `molecular_weight` | Molecular weight. | g/mol | RDKit Descriptors.MolWt |
| `xlogp` | Octanol–water partition coefficient, fast physics-based baseline (RDKit Crippen). | log10 | RDKit Crippen.MolLogP |
| `tpsa` | Topological polar surface area. | Å² | RDKit Descriptors.TPSA |
| `complexity` | Molecular complexity. | — | PubChem/RDKit |
| `fsp3` | Fsp3. | — | NovoMCP enrichment |
| `hba` | Hydrogen-bond acceptors. | count | RDKit Lipinski.NumHAcceptors |
| `hba_count` | Hba count. | — | NovoMCP enrichment |
| `hbd` | Hydrogen-bond donors. | count | RDKit Lipinski.NumHDonors |
| `hbd_count` | Hbd count. | — | NovoMCP enrichment |
| `heavy_atom_count` | Heavy atom count. | — | NovoMCP enrichment |
| `aromatic_atom_count` | Aromatic atoms. | count | RDKit |
| `aromatic_ring_count` | Aromatic rings. | count | RDKit |
| `rotatable_bonds` | Rotatable bonds. | count | RDKit Descriptors.NumRotatableBonds |
| `qed` | Quantitative Estimate of Drug-likeness. | 0–1 | RDKit QED |
| `drug_likeness` | Drug-likeness composite. | 0–1 | RDKit-derived |
| `synthetic_accessibility` | Synthetic accessibility score. | 1(easy)–10(hard) | RDKit SA_Score |
| `bioavailability_probability` | Predicted probability for the bioavailability endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `hia_probability` | Predicted probability for the hia endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `caco2_permeability` | Caco-2 cell apparent permeability. | log(10⁻⁶ cm/s) | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `bbb_penetration_probability` | Predicted probability for the bbb penetration endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `ppbr_percent` | Plasma protein binding rate. | % | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `vdss_l_kg` | Volume of distribution at steady state. | L/kg | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `half_life_hr` | Elimination half-life. | h | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `clearance_hepatocyte` | Intrinsic clearance (hepatocyte). | µL/min/10⁶ cells | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `clearance_microsome` | Intrinsic clearance (liver microsome). | µL/min/mg | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `pgp_inhibitor_probability` | Predicted probability for the pgp inhibitor endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `pgp_substrate_probability` | Predicted probability for the pgp substrate endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `lipophilicity_log_ratio` | Lipophilicity (octanol/water logD, pH 7.4). | log10 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `aqueous_solubility_log_mol_l` | Aqueous solubility. | log mol/L | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `cyp1a2_inhibitor_probability` | Predicted probability for the cyp1a2 inhibitor endpoint. | 0–1 | NovoMCP ADDIE model (public toxicology data, e.g. Tox21; screening-grade) |
| `cyp2c9_inhibitor_probability` | Predicted probability for the cyp2c9 inhibitor endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `cyp2c9_substrate_probability` | Predicted probability for the cyp2c9 substrate endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `cyp2c19_inhibitor_probability` | Predicted probability for the cyp2c19 inhibitor endpoint. | 0–1 | NovoMCP ADDIE model (public toxicology data, e.g. Tox21; screening-grade) |
| `cyp2d6_inhibitor_probability` | Predicted probability for the cyp2d6 inhibitor endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `cyp2d6_substrate_probability` | Predicted probability for the cyp2d6 substrate endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `cyp3a4_inhibitor_probability` | Predicted probability for the cyp3a4 inhibitor endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `cyp3a4_substrate_probability` | Predicted probability for the cyp3a4 substrate endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `cyp_inhibition_risk_score` | Cyp Inhibition Risk Score. | — | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `cyp_substrate_max_probability` | Predicted probability for the cyp substrate max endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `overall_toxicity_score` | Overall Toxicity Score. | — | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `ames_mutagenicity_probability` | Predicted probability for the ames mutagenicity endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `hepatotoxicity_probability` | Predicted probability for the hepatotoxicity endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `herg_blocker_probability` | Predicted probability for the herg blocker endpoint. | 0–1 | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `ld50_log_mol_kg` | Acute toxicity (LD50). | log mol/kg | NovoMCP model, TDC ADMET benchmark (MapLight / admet_ai, MIT) |
| `carcinogenicity_probability` | Predicted probability for the carcinogenicity endpoint. | 0–1 | NovoMCP ADDIE model (public toxicology data, e.g. Tox21; screening-grade) |
| `respiratory_toxicity_probability` | Predicted probability for the respiratory toxicity endpoint. | 0–1 | NovoMCP ADDIE model (public toxicology data, e.g. Tox21; screening-grade) |
| `eye_corrosion_probability` | Predicted probability for the eye corrosion endpoint. | 0–1 | NovoMCP ADDIE model (public toxicology data, e.g. Tox21; screening-grade) |
| `eye_irritation_probability` | Predicted probability for the eye irritation endpoint. | 0–1 | NovoMCP ADDIE model (public toxicology data, e.g. Tox21; screening-grade) |
| `cardiotoxicity_5d_probability` | Predicted probability for the cardiotoxicity 5d endpoint. | 0–1 | NovoMCP ADDIE model (public toxicology data, e.g. Tox21; screening-grade) |
| `cardiotoxicity_10d_probability` | Predicted probability for the cardiotoxicity 10d endpoint. | 0–1 | NovoMCP ADDIE model (public toxicology data, e.g. Tox21; screening-grade) |
| `cardiotoxicity_max_probability` | Predicted probability for the cardiotoxicity max endpoint. | 0–1 | NovoMCP ADDIE model (public toxicology data, e.g. Tox21; screening-grade) |
| `has_pains` | Has pains. | — | NovoMCP enrichment |
| `pains_severity` | Pains severity. | — | NovoMCP enrichment |
| `pains_types` | Pains types. | — | NovoMCP enrichment |
| `binding_affinity_score` | Heuristic binding-affinity score. | — | NovoMCP model |
| `logp` | Octanol–water partition coefficient, ML-predicted primary lipophilicity value. Held-out R²=0.76 (Prospective set), outperforming Crippen (0.58); NULL for unparseable SMILES and out-of-domain predictions (~0.03%). | log10 | NovoMCP CatBoost trained on SangsterLogP (CC-BY-4.0, Cirino et al. 2026, doi:10.1038/s41597-026-07357-2) |

## Provenance
- **Source compounds:** PubChem CIDs (snapshot 2026-05-19).
- **Descriptors:** RDKit.
- **ADMET:** NovoMCP's own models. Most endpoints trained on the public Therapeutics
  Data Commons (TDC) ADMET benchmark (MapLight / MapLight+GNN / admet_ai, all MIT-licensed);
  base heads (cyp1a2/cyp2c19 inhibition, cardiotoxicity, carcinogenicity, eye, respiratory)
  trained on other public toxicology data (e.g. Tox21). See the per-column `source`.
- **Attribution:** TDC-trained heads derive from MapLight (© MapLight Therapeutics, MIT)
  and admet_ai (© Kyle Swanson, MIT).
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
  doi    = {10.5281/zenodo.21710894},
  note   = {lite variant; 122,454,458 compounds; concept DOI resolves to the latest version}
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
