Graphical representation

Stereochemical projections:, such as Fischer projections, Newman projections, and Wedge-Dash representations, are used to describe the three-dimensional arrangement of atoms in a molecule, particularly when there is a need to represent the stereochemistry (spatial orientation) around chiral centers or double bonds.

The opposite of a stereochemical projection would be a representation that does not provide spatial or three-dimensional information. This type of representation is typically a 2D structural formula that shows the connectivity between atoms and the types of bonds, but without any indication of the spatial orientation or stereochemistry of the atoms or bonds.

1. Lewis Structures

- Description: These are two-dimensional diagrams showing atoms connected by lines for bonds and dots for lone pairs. Lewis structures highlight bonding and lone pairs but do not depict molecular geometry.
- Use: Suitable for showing electron distribution and identifying formal charges or resonance structures.

2. Condensed Structural Formulas

- Description: These representations simplify molecular structures by grouping atoms together, typically in a line. Bonds are implied rather than explicitly shown.
- Use: Common in organic chemistry for simple molecules or repeating units, making the structure more compact.

3. Bond-Line (Skeletal) Structures

- Description: These structures use lines to represent bonds between carbons, omitting hydrogen atoms attached to carbons. Non-carbon and non-hydrogen atoms are shown explicitly.
- Use: Widely used for organic molecules, as it reduces clutter and makes complex molecules easier to interpret.

4. Three-Dimensional (3D) Models

- Description: 3D models show the spatial orientation of atoms using solid and dashed lines to represent bonds coming out of or going into the plane. Software-based 3D models (ball-and-stick or space-filling models) are common for more accurate spatial representations.
- Use: Helpful for understanding stereochemistry, molecular shape, and interactions in biological or complex chemical environments.

5. Fischer Projections

- Description: Used primarily for carbohydrates and amino acids, Fischer projections are two-dimensional representations where vertical lines denote bonds going into the plane, and horizontal lines represent bonds coming out of the plane.
- Use: Effective for depicting stereochemistry in chiral centers, particularly for biomolecules.

6. Haworth Projections

- Description: A simplified 3D structure used mainly for cyclic molecules like sugars, where the ring is drawn as a flattened structure to show stereochemistry.
- Use: Common in carbohydrate chemistry to show cyclic structures and anomeric forms.

7. Newman Projections

- Description: These show a molecule as viewed along the axis of a bond, with front and back atoms depicted as circles and lines representing substituent groups.
- Use: Useful for visualizing conformations around single bonds, particularly in alkanes and other flexible molecules.

8. Sawhorse Projections

- Description: A tilted 3D view of a molecule, showing the orientation of groups on either side of a bond.
- Use: Useful for comparing different conformations, similar to Newman projections.

9. Kekulé Structures

- Description: Similar to Lewis structures, Kekulé structures are used mainly for aromatic compounds and show alternating single and double bonds within rings.
- Use: Important for understanding resonance in benzene and other aromatic compounds, though less accurate for describing electron delocalization.

6. Ball-and-Stick Models

- Description: Physical or digital models where atoms are represented by colored balls, and bonds are represented by sticks connecting them. The angles and lengths of sticks reflect bond angles and distances.
- Use: Helps visualize molecular geometry, bond angles, and spatial arrangements of atoms in 3D, commonly used in educational and research settings.

7. Space-Filling Models

- Description: Atoms are represented by spheres with sizes proportional to their van der Waals radii, creating a more realistic view of how atoms occupy space in a molecule.
- Use: Useful for illustrating molecular size, shape, and how molecules fit together, showing spatial relationships and steric effects more accurately than ball-and-stick models.

8. Electron Cloud Models

- Description: These models represent the electron density around atoms or within molecules, often using shading or color gradients to indicate areas of high and low electron probability.
- Use: Useful for understanding the distribution of electron density, which can indicate areas of partial positive or negative charge and help predict molecular interactions.

9. NMR Spectra

- Description: NMR (Nuclear Magnetic Resonance) spectra graphically represent the response of nuclei in a magnetic field, producing peaks at frequencies that correspond to different nuclear environments.
- Use: Provides structural information, helping chemists determine functional groups, molecular environments, and, in some cases, 3D structure through coupling patterns and chemical shifts.

10. Molecular Dynamics Simulations

- Description: Computer-generated animations that model the motion of atoms and molecules over time, based on physical forces and potential energy calculations.
- Use: Useful for studying dynamic processes, molecular interactions, conformational changes, and reaction mechanisms in real-time, providing insights that static models cannot.

11. VSEPR (Valence Shell Electron Pair Repulsion) Models

- Description: These models predict molecular shapes by arranging electron pairs around a central atom to minimize repulsion, resulting in geometric structures.
- Use: Common in predicting and visualizing basic molecular geometries (e.g., linear, trigonal planar, tetrahedral) based on the number and types of electron pairs around a central atom.

15. Perspective Drawings

- Description: These are 2D drawings that use dashed, wedged, and solid lines to indicate the 3D orientation of bonds relative to the page (e.g., into or out of the plane).
- Use: Frequently used in organic chemistry to depict stereochemistry and molecular geometry, allowing quick visualization of 3D structure in a 2D format.

<<molecular_representation.pdf>>

Text-based molecular representation

1. SMILES (Simplified Molecular Input Line Entry System)

- Description: A widely-used text-based format that encodes the molecular structure in a string, using atomic symbols, bond types, and parentheses to indicate branching and rings.
- Example: CCO for ethanol, c1ccccc1 for benzene.

2. InChI (International Chemical Identifier)

- Description: A standard text representation that uniquely identifies a chemical substance, including information on atomic composition, structure, and stereochemistry.
- Example: InChI=1S/C2H6O/c1-2-3/h2-3H,1H3 for ethanol.

3. InChIKey

- Description: A fixed-length, hashed version of the InChI string, designed to be easier to search and compare in databases.
- Example: LFQSCWFLJHTTHZ-UHFFFAOYSA-N for ethanol.

4. SMARTS (SMILES Arbitrary Target Specification)

- Description: A variant of SMILES that allows for the specification of substructures, making it useful for searching molecular databases or pattern matching.
- Example: C[C@H](O)C for a structure with a chiral center.

5. IUPAC Name (Systematic Chemical Name)

- Description: The full systematic name of a molecule based on the rules set by the International Union of Pure and Applied Chemistry (IUPAC), which specifies the exact structure.
- Example: ethanol for C₂H₅OH, 2-methylbutane for isopentane.

6. Canonical SMILES

- Description: A unique SMILES string for a given molecule, used to eliminate ambiguity in how a structure might be written. It is often used in databases to ensure consistency.
- Example: The canonical SMILES for ethanol is CCO, but it is sometimes used with more detailed rules for ambiguous cases.

7. CML (Chemical Markup Language)

- Description: An XML-based standard for representing chemical structures in text form, designed for integration with web and database technologies.
- Example: <molecule><atomArray><atom id="a1" elementType="C"/><atom id="a2" elementType="C"/><atom id="a3" elementType="O"/></atomArray></molecule> for a simple molecule like ethanol.

8. PDB (Protein Data Bank) Notation

- Description: A text-based format for describing the 3D coordinates of atoms in a molecule, primarily used for macromolecular structures like proteins and nucleic acids.
- Example: The PDB file for a protein structure, e.g., ATOM 1 N MET A 1 20.154 21.331 15.425.

9. Molfile (MDL Molfile)

- Description: A text format for encoding molecular structures, including atoms, bonds, and other properties. Often used in cheminformatics and molecular modeling software.
- Example: M V3000 header followed by atom and bond information like A 1 C 0.000 0.000 0.000.

10. PubChem CIDs (Chemical Identifier)

- Description: PubChem's unique identifier for chemical substances, typically used as a reference to molecules in the PubChem database.
- Example: PubChem CID 702 refers to ethanol.

Rosdal

SLN