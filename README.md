# 💉 MHC-I T Cell Epitope Prediction Pipeline

A Nextflow-based pipeline 🔄 for predicting **MHC-I binding T-cell epitopes** from a target protein sequence using the IEDB API.

The pipeline retrieves protein sequences from UniProt, predicts epitopes across the human reference MHC-I allele panel, filters candidates, evaluates similarity against human proteins using BLASTP, and returns top epitope candidates for downstream vaccine design.

## ✨ Pipeline Workflow

1. Accepts a UniProt Accession ID as input.
2. Retrieves the target protein sequence from UniProt.
3. Predicts MHC-I binding T-cell epitopes using the IEDB API for the 27-allele human reference panel.
4. Merges prediction results into a single TSV file.
5. Filters epitopes with prediction scores greater than 0.5.
6. Generates FASTA sequences for filtered epitopes.
7. Performs BLASTP against human protein sequences to identify potential self-peptides.
8. Appends BLAST results to the merged epitope table.
9. Selects the top n (given by user) epitopes per allele.

## ⚙️ Requirements

- Nextflow >= 24.0
- Java >= 11
- BLAST+
- Python >= 3.10
- Linux/Unix environment
- Internet connection for UniProt and IEDB API access

## 🚀 Usage

Clone the repository:

```bash
git clone https://github.com/VarshaS-37/T-Cell-Epitope-Prediction-Pipeline.git
cd T-Cell-Epitope-Prediction-Pipeline
```

Run the pipeline:

```bash
nextflow run main.nf \
    --uniprot_id P59595 \
    --top_n 4 \
    --pident_cutoff 80
```
## 🔑 Parameters

| Parameter | Description |
|------------|------------|
| `--uniprot_id` | UniProt accession of the target protein |
| `--top_n` | Number of top epitopes to retain per allele |
| `--pident_cutoff` | Maximum allowed percent identity with human proteins |

## 📂 Output

The pipeline generates a consolidated epitope file:

```text
results/
└── top_epitopes.tsv
```

Additional intermediate files generated during execution are stored in the `results/` directory.

## ⚠️ Limitations

- Supports only MHC-I epitope prediction.
- Results depend on the availability of UniProt and IEDB web services.
- Population coverage analysis, Allergenicity and Toxicity prediction is not included.
