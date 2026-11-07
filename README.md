# GMSimA (version-3)
GMSimA
(Graphical Membrane Simulation Analyzer) — a GUI suite for analyzing protein/peptide–lipid bilayer simulations from GROMACS (.tpr/.gro/.pdb + .xtc), built on MDAnalysis.

GMSimA produces publication-quality figures (300 dpi PNG), the underlying data in open formats (.xvg/.csv/.txt), representative structures (.pdb), a full run log, and a self-contained report.html per run.
Available Analyses

Intrinsic membrane properties:

    Area per lipid — per-leaflet box-area APL, plus per-species Voronoi APL
    Membrane thickness — phosphate-to-phosphate, per frame
    Order parameter (S_CD) — true C–H bond-vector NMR convention (directly comparable to experiment and gmx order), per chain and per species; chord-vector fallback for united-atom topologies
    Curvature — molecular-scale roughness radius (primary) and lipid-spacing-smoothed mesoscale undulation radius (secondary)
    Density profile — headgroup/glycerol/chains/water, plus per species
    Lateral diffusion — PBC-unwrapped, COM-corrected, FFT-based MSD with the 2D Einstein relation

Protein–membrane interaction Properties:

    Minimum distance (ξ) with contact-percentage distribution
    Penetration depth (ΔZ) vs the bilayer midplane, with membrane reference lines and a per-residue membrane-position profile
    Binding residence (τ-off) — dual-cutoff (4.5/7.0 Å hysteresis) contacts, survival-function analysis with mono/bi-exponential maximum likelihood fits, AIC model selection and bootstrap confidence intervals; both whole-peptide residence and per-lipid pooled residence (PyLipID-style), plus a per-residue residence table and plot. The anchor moiety is selectable (basic Lys/Arg, hydrophobic, aromatic, all heavy atoms, or a custom selection) with the membrane partner switching automatically between phosphates and whole-lipid atoms
    Binding sites (V2) — residue-community clustering of the residue–lipid co-contact network (Louvain) into distinct binding sites, each with occupancy, lipid composition, site-resolved τ-off with confidence interval, co-contact heat map, and representative bound-pose PDBs
    Tilt / FEL / States — tilt time series, tilt–ΔZ free-energy landscape, detection of well-separated states with populations, and export of one representative PDB structure per state
    Lipid enrichment & contacts — shell/bulk enrichment index per lipid species and per-residue membrane contact fractions

Required trajectory preprocessing

Analyze trajectories in which molecules are whole and the membrane is centered:

gmx trjconv -s run.tpr -f a.xtc -pbc mol -center -ur compact -o b.xtc

(optionally followed by a rot+trans fit for tilt/FEL work). 

Installation:

No Installation required. User can access binary by running command from the directory './gmsima'

GMSimA has been tested on Ubuntu Linux Distribution 20.04/22.04/24.04 LTS. Support to other OS will soon be released.  

Usage

    Load a topology and the trajectory (.xtc). The topology must contain exactly the atoms in the XTC (e.g. the dry system for a dry XTC):
        .tpr — preferred; exact bonds, masses and charges.
        .gro / .pdb — any GROMACS version, no GROMACS install needed. Bonds are guessed from distances and cleaned (no cross-residue H bonds, only consecutive-residue links, disulfides added); masses are guessed from atom names; charges are unavailable (no current module needs them). Use this for .tpr files newer than the installed MDAnalysis (e.g. GROMACS 2026 with MDAnalysis ≤ 2.10): gmx editconf -f system.tpr -o system.gro. Optionally pip install TprParser lets such .tpr files load directly.
    The Protein/Membrane group selectors auto-populate from the topology; any MDAnalysis selection string can be typed instead.
    Pick a stride, choose analyses, choose an output folder, Run Analysis.
    Results land in a timestamped subfolder together with GMSimA.log and report.html. The Stop button halts the run at the next module boundary and keeps everything already computed.

Multiple peptides (V2): if the protein group contains several bonded molecules, every interaction analysis runs once per peptide with suffixed outputs (deltaz_pep1.xvg, tauoff_pep2_residence.txt, ...).
