# 🔄 MHC 1 binding T Cell Epitope Prediction Pipeline

A Nextflow-based pipeline for predicting T cell epitopes binding to MHC 1 for a given protein sequence using the IEDB API.

## Pipeline Flow

- Accepts a UniProt Accession ID as input
- Retrieves the target protein sequence from Uniprot
- Retrieves MHC 1 binding T cell epitopes of the sequence for the 27 Human Allele Panel
- Merges 27 retrieved files into one tsv
- Filters epitopes with score greater than 0.5
- Retrieves fasta sequences of the filtered epitopes
- Checks similarituy with humann genome to avoid auto immune risk by using blastp 
- Blast results are merged into the existing merged tsv
- Top n epitopes given by user is slected from each allelel n pident score given by user is ude to slect the top epitopes

## Requirements

- Nextflow >= 24.0
- Java >= 11
- BLAST+
- Python >= 3.10
- Linux/Unix environment

## Repository Structure

```text
.
├── main.nf
├── nextflow.config
├── modules/
│   ├── fetch_sequence.nf
│   ├── blast_search.nf
│   ├── retrieve_iedb.nf
│   ├── clean_tsv.nf
│   └── merge_tsv.nf
├── bin/
├── README.md
└── .gitignore
```

## Usage
Download all the files
Run the pipeline with a UniProt accession:

```bash
nextflow run main.nf \
    --uniprot_id P59595 \
    --top_n 4 \
    --pident_cutoff 80
```

### Parameters

| Parameter | Description |
|-----------|-------------|
| `--uniprot_id` | UniProt accession of the target protein |
| `--top_n` | Number of top homologous sequences to retain from each allele|
| `--pident_cutoff` | Minimum percent identity threshold with human sequnece|

## Output

The pipeline generates a consolidated epitope file:

```text
results/
└── top_epitopes.tsv
```

Additional intermediate files are generated during execution for sequence retrieval, BLAST analysis, and epitope processing and stored in results folder.

## Example

```bash
nextflow run main.nf \
    --uniprot_id P59595 \
    --top_n 10 \
    --pident_cutoff 80
```

## Notes

- Internet access is required for sequence and epitope retrieval.

## Limitation
- this pipleine only predicts epitopes for mhc 1
- furhetr these epitopes can be checkde for allerginicity toxocoty and poulation covergafe using other tools to create vaccine candidtaes


