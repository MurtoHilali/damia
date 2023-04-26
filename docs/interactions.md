# Fetch Interaction Data Script

This script fetches protein interaction data from MINT or BioGRID and generates multi-FASTA files for each pair of interaction partners. **Note**: The BioGRID processing functions are still in development, and only MINT is currently working.

## Requirements

- Python 3.6 or higher
- `requests`
- `pandas`
- `biopython`

## Usage

```
python interactions.py <uniprot_ids> --source <mint|biogrid> [--output_folder <output_folder>] [--access_key <access_key>]
```

### Arguments

- `uniprot_ids`: List of UniProt IDs to fetch data for.
- `--source`: Choose the data source (mint or biogrid). Only MINT is currently working.
- `--output_folder` (optional): Output folder for interaction data (default: interactions).
- `--access_key` (optional): Access key for BioGRID API (required if source is biogrid).

## Output

- The script creates a folder with interaction data files for each UniProt ID in the specified `output_folder` (default: interactions).
- A JSON file `interaction_partners.json` is created in the `output_folder`, containing a dictionary with UniProt IDs as keys and a list of interaction partner UniProt IDs as values.
- A folder named `complexes` is created with multi-FASTA files for each pair of interaction partners. The file names are in the format `key:value[0].fa`, `key:value[1].fa`, etc.

## Limitations

- The .fa files in the `complexes` folder are automatically generated and do not account for other proteins that may be required to properly model a complex. They also assume all complexes are heterodimeric, which may not be the case.
- Some of the complexes may have exact experimental structures in the PDB, which would be preferred for most analyses.

## Functions

### `get_mint_data(output_folder, uniprot_ids)`

Fetches interaction data from MINT and saves it as TSV files in the specified output folder.

### `get_biogrid_data(output_folder, uniprot_ids, access_key)`

Fetches interaction data from BioGRID (currently under development) and saves it as TSV files in the specified output folder.

### `extract_interaction_partners_mint(uniprot_ids, output_folder)`

Extracts interaction partner UniProt IDs from MINT data and returns a dictionary with UniProt IDs as keys and a list of interaction partner UniProt IDs as values.

### `create_mfa(interaction_partners: dict)`

Generates multi-FASTA files for each pair of interaction partners and saves them in a folder named `complexes`.

### `main()`

The main function of the script. It parses command-line arguments, calls the appropriate data fetching and processing functions depending on the selected data source, and generates the JSON file with the interaction partners dictionary.

## Example Usage

To fetch MINT interaction data for UniProt IDs Q15113, P20908, and Q9BQB4, and save the output in the "interactions" folder, run the following command:

```
python interactions.py Q15113 P20908 Q9BQB4 --source mint
```

This will create the following files and folders:

- `interactions/Q15113.tsv`: MINT interaction data for Q15113
- `interactions/P20908.tsv`: MINT interaction data for P20908
- `interactions/Q9BQB4.tsv`: MINT interaction data for Q9BQB4
- `interactions/interaction_partners.json`: A JSON file containing the dictionary of interaction partners
- `complexes/`: A folder containing multi-FASTA files for each pair of interaction partners

## Notes

- The BioGRID data fetching and processing functions are still in development. Once implemented, you will be able to use the `--source biogrid` option to fetch interaction data from BioGRID.
- The script currently generates multi-FASTA files for each pair of interaction partners without accounting for other proteins that may be required to properly model a complex. The script also assumes all complexes are heterodimeric, which may not be the case.
- Some of the complexes may have exact experimental structures in the PDB, which would be preferred for most analyses.