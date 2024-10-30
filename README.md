# RXRX3 metadata

1. Download [metadata](https://s3.wasabisys.com/rxrx3-blinded/metadata.zip?AWSAccessKeyId=K4U6TQIYNAQX7Y34W6CS&Signature=N9kR2cz8J%2FJv8BEwqi7gtF8uPmg%3D&Expires=1730385862&u=f462c00159cf940908a0f565731b4ad8).
2. Run the script below

This filtered metadata can be viewed on datasette lite ([url](https://lite.datasette.io/?parquet=https%3A%2F%2Fraw.githubusercontent.com%2Fshntnu%2Frxrx3%2Frefs%2Fheads%2Fmain%2Fmetadata_rxrx3_trimmed.parquet)).

```
python -c "
import pandas as pd

# Read the CSV file
df = pd.read_csv('metadata_rxrx3.csv', low_memory=False)  # Replace with the actual file path

# Step 1: Remove rows where both 'gene' and 'treatment' columns contain "RXRX3"
df = df[~((df['gene'].str.contains('RXRX3', na=False)) & (df['treatment'].str.contains('RXRX3', na=False)))]

# Step 2: Keep only the specified columns
df = df[['experiment_name', 'gene', 'treatment', 'SMILES', 'concentration', 'perturbation_type', 'cell_type']]

# Step 3: Optimize data types for further compression
# Convert columns with limited unique values to categorical types
df['perturbation_type'] = df['perturbation_type'].astype('category')
df['cell_type'] = df['cell_type'].astype('category')

# Step 4: Save as Parquet with high compression
df.to_parquet('metadata_rxrx3_trimmed.parquet')
"
```
