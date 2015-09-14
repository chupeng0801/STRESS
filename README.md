#STRESS

A computationally-efficient framework for identifying potential allosteric residues at the protein surface and within the interior


###Dependency: 

Python, Perl, C compiler, MMTK

###Configuration: 

Before running the code, you need to compile the C package, type:
				```
				cd code_bundle_BL/code_pack
				make clean
				make
				cd ../../code_bundle_GN/networkTools
				make clean
				make
				```

###Usage:
```
./stress -I INPUTFILE -O OUTPUTDIR -L LOGFILE [-s|--surface] [-i|--interior] [-h|--help]
        -I | --input:       followed by the inputfile (pdb file)
        -O | --output:      followed by the output directory
        -L | --logfile:     followed by the logfile
        -s | --surface:     run the surface-critical module(optional)
        -i | --interior:    run the interior-critical module(optonal)
        -h | --help:        for help(optional)
```

To run the program, you have to at least choose one of surface/interior modules, or both.

For detailed description of the two modules, please see below.

##Identifying Surface-Critical Residues

###Overview
Inspired by the binding leverage framework first introduced by Mitternacht and Berezovsky in 2011, 
this module efficiently identifies pockets on the protein surface such that their occlusion with 
small ligands is predicted to drastically affect conformational change – such surface sites are 
thus inferred to be allosteric hotspots. Monte Carlo simulations are used to probe the all-atom 
protein surface with flexible ligands. After automated thresholding, those sites at which the 
simulated ligand strongly interferes with conformational change are given as output in the form of 
a ranked list of sites, with each site being represented by its list of constituent residues, as 
given by their residue IDs and corresponding chains. Each site usually contains 10 residues, and 
never more than 10. In the output file provided (ex: “3D3D__SURFACE_CRITICAL_residues.dat”), each 
row corresponds to a particular allosteric surface site, and columns contain the following data:

	Column 1:    Index of the binding site from Monte Carlo simulations (this is used by the 
                 STRESS developers for the purpose of thesholding the list of ranked sites)
	Column 2:    Binding leverage score for site – higher values signify that the site is 
                 such that occlusion by ligand strongly interferes with large-scale protein 
                 motions, and thus likely has a stronger allosteric affect
	Columns 3+:  The list of surface-critical residues in the given site, in the format 
                 <RESID_CHAIN>




###Notes
	
	1. If you have any difficulty generating any of the output files, output files for this example 
	are stored in the directory ./output_files/
	2. When submitting a PDB file, we suggest using the biological assembly file (not the raw 
	asymmetric unit).


###Changes to Original Code
An earlier version of this code was developed by Anurag Sethi and John Eargle of the Luthey-Schulten 
Group at The University of Illinois at Urbana-Champaign (see information in code headers). Extensive 
modifications have been introduduced by Shantao Li and Declan Clarke of the Gerstein Lab at Yale 
University. A few of the most notable modifications now integrated into the new version include:
	
	1. The printing of resID and chain information in output files
	2. The use of anisotropic network models that are generated by MMTK
	3. Modified parameters of the attractive potentials for ligand-protein interactions
	4. For the purpose of optimizing efficiency: Local searching supported by hashing is used in each 
       sampling step in the search for surface-critical residues.
	5. For the purpose of optimizing efficiency: revised data structure, workflow and cut back on some 
	   expensive operations such as square root.

##Identifying Interior-Critical Residues

###Overview
Allosteric residues within the protein interior often function as essential communication ‘bottlenecks’ 
between distinct regions. To identify such bottleneck residues, this computationally efficient module 
represents a protein structure as a  network of interacting residues, and simultaneously incorporates 
information about conformational change. The list of residues given as output are thus predicted to 
function in essential communication bottlenecks within the protein, and are thus potentially allosteric 
(see our publication for details). The output file provided (ex: “3D3D__INTERIOR_CRITICAL_residues.txt”) 
has the following format:

	Column 1:   RESIDUE (the 0-based index of the residue in the processed PDB file)
	Column 2:   ResID (as it appears in the PDB file) 
	Column 3:   Residue type (ex: “VAL”)
	Column 4:   Chain




###NOTES
	1. If you have any difficulty generating any of the output files, output files for this example 
	are stored in the directory ./output_files/
	
	2. When submitting a PDB file, we suggest using the biological assembly file (not the raw 
	asymmetric unit).
