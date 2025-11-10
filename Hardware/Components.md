- **NTC Thermistor**: Resistance decreases as temperature increases. Highly sensitive to small temperature changes. Used for temperature sensing and compensation. More sensitive for precise, gradual temperature changes.  
- **PTC Thermistor**: Resistance increases as temperature increases. Most sensitive near the switching point. Used for overcurrent protection and heaters. Sensitive for abrupt responses near a specific temperature.  

Here’s your text, formatted and corrected for clarity and consistency:

---

### Tool for PCB Design  
[KiCad](https://www.kicad.org)  

---

### Common PCB Terminology  

#### Integrated Design Types  
- **System-in-a-Package (SiP):** Multiple integrated circuits and components combined into a single package.  
- **Package-on-a-Package (PoP):** Stacked packages, typically used for high-density designs.  
- **System-on-a-Chip (SoC):** A single chip integrating all necessary components of a system.  
- **Computer-on-a-Module (CoM):** A single-board computer module designed for integration into larger systems.  

---

### PCB Layers and Functions  

- **F.Cu (Front Copper):** Copper traces on the front side of the PCB.  
- **In1.Cu, In2.Cu (Inner Copper):** First and second inner copper layers in multilayer PCBs.  
- **B.Cu (Back Copper):** Copper traces on the back side of the PCB.  
- **F.Adhesive/B.Adhesive (Adhesive Layers):** Bonding layers for front and back sides, respectively.  
- **F.Paste/B.Paste (Paste Layers):** Hold solder paste during component assembly on the front and back sides.  
- **F.Silkscreen/B.Silkscreen (Silkscreen Layers):** Printed text, symbols, or markings for labeling components on the front and back sides.  
- **F.Mask/B.Mask (Solder Mask):** Covers copper traces, exposing only solderable areas on the front and back sides.  
- **User.Drawings:** Custom drawings like assembly notes or outlines added by designers.  
- **User.Comments:** Additional designer comments or instructions.  
- **User.Eco1/User.Eco2 (Engineering Change Order Layers):** Document modifications or updates post-layout.  
- **Edge.Cuts:** Defines the PCB outline for physical cutting.  
- **Margin:** Additional space around the PCB edges for notes or alignment marks.  
- **F.Courtyard/B.Courtyard:** Defines space around components to avoid overlaps during assembly.  
- **F.Fab/B.Fab (Fabrication Layers):** Used for detailed component placement and mechanical dimensions for manufacturing and inspection.  

---

### Key File Types  

- **Gerber Files:** Standard files used by fabricators to define copper layers, solder masks, legends, and drill holes. Each PCB layer has its own Gerber file.  
- **BOM (Bill of Materials):** Lists all components required, including part numbers, quantities, and reference designators for assembly.  
- **CPL (Component Placement List):** Also known as Pick and Place or Centroid file, it specifies the position, orientation, and type of components for automated assembly.  
- **SCH (Schematic):** Represents the electrical connections and logic of the circuit.  
- **BRD (Board):** Represents the physical PCB layout, including traces and design rules.  
- **PcbDoc:** The main document file integrating schematic (SCH) and layout (BRD/PCB) in PCB design software.  
- **SchDoc (Schematic Document):** Manages the schematic design in PCB design software.  

---

### PCB Materials  

- **PI (Polyimide):** High-performance material with excellent thermal stability.  
- **FR-4 (Fiberglass and Epoxy):** Common material offering good strength and electrical insulation.  

---
