# Research_Paper_Wiley_LaTeX_Project

## Wiley requirements detected
- Document class: `WileyNJD-v2`
- Layout: `AMA` reference style, `LATO1COL` one-column layout in the source manuscript
- Packages used: `amsmath`, `graphicx`, `booktabs`, `array`, `hyperref`, `url`
- Front matter: `rticletype`, dates, title, authors, affiliations, correspondence, present address
- Figures/tables: standard `figure`, `table`, `table*`, and `tabular*` environments
- Bibliography: Wiley AMA style (`wileyNJD-AMA.bst`)

## Structure
- `main.tex`: compilation entry point
- `references.bib`: extracted bibliography entries
- `sections/`: modular manuscript sections
- `figures/`, `tables/`, `supplementary/`: asset folders
- `Wiley_files/`: place Wiley class/style files here for Overleaf

## Notes
- The repository did not include the Wiley class file or supporting `.bst/.sty` files, so those must be uploaded into `Wiley_files/` for full Wiley compilation.
- Scientific content, labels, equations, tables, and citations were preserved in the section files.
