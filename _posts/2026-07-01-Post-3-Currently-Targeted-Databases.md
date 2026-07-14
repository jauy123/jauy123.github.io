## Post 3 — Currently Targeted Databases

In Post 1, I mentioned that there are two overarching types of code planned to be added: **Fetcher Classes** and **Convenience Functions**.

At the moment, there are plans for four Convenience Functions:

1. `from_pdb()` — already implemented pre-GSoC -- need to be refactored to use new **Fetcher Classes**
   REST API: (https://files.wwpdb.org/download/)

2. `from_alphafold()` — targets the AlphaFold database  
   REST API: [AlphaFold prediction API](https://alphafold.ebi.ac.uk/api/prediction/)

3. `from_mddb()` — targets the Molecular Dynamics Data Bank  
   REST API: [MDDB REST API documentation](https://mdposit.mddbr.eu/api/rest/docs/)

4. `from_doi()` — targets supplementary information from any DOI  
   Implementation: internal `pooch` implementation

A more tentative Convenience Function is `from_materials_project()`, since it would require users to register for their own API keys.

Materials Project API: [Materials Project API documentation](https://next-gen.materialsproject.org/api#accessing-data)