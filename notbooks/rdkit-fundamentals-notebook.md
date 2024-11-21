
### 2.2 Creating Molecules from Molecular Formulas
```python
# Create molecules from molecular formulas
water = Chem.MolFromFormula('H2O')
carbon_dioxide = Chem.MolFromFormula('CO2')
```

### 2.3 Creating 3D Molecules
```python
# Generate 3D coordinates for a molecule
mol = Chem.MolFromSmiles('CCO')
mol_3d = Chem.AddHs(mol)  # Add hydrogen atoms
AllChem.EmbedMolecule(mol_3d)  # Generate 3D coordinates
AllChem.MMFFOptimizeMolecule(mol_3d)  # Energy minimization
```

## 3. Molecule Properties
```python
# Basic molecule properties
mol = Chem.MolFromSmiles('CCO')

# Molecular weight
mol_weight = Descriptors.ExactMolWt(mol)

# Atom count
atom_count = mol.GetNumAtoms()

# Bond count
bond_count = mol.GetNumBonds()

# Formal charge
formal_charge = Chem.GetFormalCharge(mol)

print(f"Molecular Weight: {mol_weight:.2f}")
print(f"Atom Count: {atom_count}")
print(f"Bond Count: {bond_count}")
print(f"Formal Charge: {formal_charge}")
```

## 4. Molecular Manipulation
### 4.1 Adding and Removing Atoms/Hydrogens
```python
# Add hydrogens
mol_with_h = Chem.AddHs(mol)

# Remove hydrogens
mol_without_h = Chem.RemoveHs(mol_with_h)
```

### 4.2 Modifying Molecules
```python
# Clone a molecule
mol_copy = Chem.MolFromSmiles(Chem.MolToSmiles(mol))

# Sanitize molecule (correct valence, aromaticity)
Chem.SanitizeMol(mol)
```

## 5. Structural Analysis
```python
# Ring information
mol = Chem.MolFromSmiles('c1ccccc1')
ring_info = mol.GetRingInfo()

# Check if molecule contains rings
has_rings = ring_info.NumRings() > 0

# Aromatic atom detection
aromatic_atoms = [atom.GetIdx() for atom in mol.GetAtoms() if atom.GetIsAromatic()]

print(f"Number of Rings: {ring_info.NumRings()}")
print(f"Aromatic Atoms: {aromatic_atoms}")
```

## 6. Molecular Descriptors
```python
mol = Chem.MolFromSmiles('CCO')

# Calculate various molecular descriptors
descriptors = {
    'Molecular Weight': Descriptors.ExactMolWt(mol),
    'LogP': Descriptors.MolLogP(mol),
    'Num H-Bond Donors': Descriptors.NumHDonors(mol),
    'Num H-Bond Acceptors': Descriptors.NumHAcceptors(mol),
    'Topological Polar Surface Area': Descriptors.TPSA(mol)
}

for name, value in descriptors.items():
    print(f"{name}: {value}")
```

## 7. Chemical Reactions
```python
# Define a reaction SMARTS
reaction_smarts = '[C:1](=O)[OH:2]>>[C:1](=O)Cl'

# Create a reaction object
rxn = AllChem.ReactionFromSmarts(reaction_smarts)

# Apply reaction to a molecule (convert carboxylic acid to acid chloride)
reactant = Chem.MolFromSmiles('CC(=O)O')
products = rxn.RunReactants((reactant,))

# Convert products to SMILES
product_smiles = [Chem.MolToSmiles(prod[0]) for prod in products]
print("Reaction Products:", product_smiles)
```

## 8. Substructure Searching
```python
# Create a molecule and a substructure
mol = Chem.MolFromSmiles('c1ccccc1CO')
substructure = Chem.MolFromSmarts('c1ccccc1')

# Check for substructure match
has_substructure = mol.HasSubstructMatch(substructure)
print("Has Substructure:", has_substructure)

# Get substructure matches
matches = mol.GetSubstructMatches(substructure)
print("Substructure Matches:", matches)
```

## 9. Molecular Visualization
```python
# Generate 2D depiction of molecules
molecules = [
    Chem.MolFromSmiles('CCO'),
    Chem.MolFromSmiles('c1ccccc1'),
    Chem.MolFromSmiles('CC(=O)O')
]

# Create a grid image of molecules
img = Draw.MolsToGridImage(molecules, molsPerRow=3, legends=['Ethanol', 'Benzene', 'Acetic Acid'])
img.save('molecule_grid.png')
```

## 10. Advanced Techniques
### 10.1 Fingerprint Generation
```python
from rdkit.Chem import AllChem
from rdkit import DataStructs

# Generate Morgan Fingerprints (Extended Connectivity Fingerprints)
mol1 = Chem.MolFromSmiles('CCO')
mol2 = Chem.MolFromSmiles('CCO')
mol3 = Chem.MolFromSmiles('c1ccccc1')

# Generate fingerprints
fp1 = AllChem.GetMorganFingerprintAsBitVect(mol1, 2)
fp2 = AllChem.GetMorganFingerprintAsBitVect(mol2, 2)
fp3 = AllChem.GetMorganFingerprintAsBitVect(mol3, 2)

# Calculate similarity
similarity_1_2 = DataStructs.TanimotoSimilarity(fp1, fp2)
similarity_1_3 = DataStructs.TanimotoSimilarity(fp1, fp3)

print(f"Similarity between mol1 and mol2: {similarity_1_2}")
print(f"Similarity between mol1 and mol3: {similarity_1_3}")
```

### 10.2 Conformer Generation
```python
# Generate multiple conformers
mol = Chem.MolFromSmiles('CCO')
mol = Chem.AddHs(mol)

# Generate multiple conformers
conformers = AllChem.EmbedMultipleConfs(mol, numConfs=10)

# Energy minimize conformers
for conf in conformers:
    AllChem.MMFFOptimizeMolecule(mol, confId=conf)
```

## Conclusion
This notebook provides a comprehensive overview of RDKit's fundamental capabilities. RDKit is an incredibly powerful library for computational chemistry, offering tools for molecular representation, manipulation, analysis, and visualization.

## Recommended Resources
- [RDKit Documentation](https://www.rdkit.org/docs/index.html)
- [RDKit GitHub Repository](https://github.com/rdkit/rdkit)
- [RDKit Tutorials](https://www.rdkit.org/docs/Tutorial.html)

## Additional Notes
- Always install RDKit in a conda environment for the best compatibility
- Keep RDKit and its dependencies updated
- Explore the extensive documentation for more advanced features
