## Jan.16 meeting

Article 1<img width="1863" height="595" alt="Image" src="https://github.com/user-attachments/assets/b7b452d9-a55e-4a4b-b59b-e79aab9845c8" />
---
## 1.Introduction
### 1.1 Research Background 
- 
### 1.2 Research Purpose
- Subject of study are 4 coacervates below
>  To ensure representative coacervate systems, we selected commonly studied PArg/PAsp coacervates driven by electrostatic interactions, rmfp-1 coacervates driven by cation−π interactions, small molecule coacervates conforming to the sticker-and-spacer model, and HBpeps coacervate systems driven by various interactions. 
- vertify the performance of Martini force field + elucidate the stability mechanisms of those coacevates 
> We conducted independent simulations of these systems using the latest Martini version 3.0 of the force field. This review not only aims to reproduce experimental phenomena using Martini 3.0 but also aims to elucidate the stability mechanisms of coacervates and benchmark Martini 3.0 through these simulations.
---
## 2. Detailed information 
### 2.1Research process of PArg/PAsp coacervates 





## Martini Tutorial 1 Lipid Self-Assembly （object is DSPC）
### 1.Generate Topology —— how to calculate the interaction & force between beads
- Force Field parameters file:
`martini_v3.0.0.itp`定义所有 Martini3 颗粒类型（bead types）和它们之间的非键相互作用
`martini_v3.0.0_ffbonded_v2.itp`通用的“带键”参数库（angles、bonds、dihedrals 等）
`martini_v3.0.0_phospholipids_PC_v2.itp`这里面包含了 DSPC 这一类 PC 头基磷脂的分子拓扑（[moleculetype]、[atoms]、[bonds]...）
- topology file:
`dspc.top`
> #include "martini_v3.0.0.itp" ; the particle definitions should be included first
> #include "martini_v3.0.0_ffbonded_v2.itp" ; the general lipid bonded definitions should be included second
> #include "martini_v3.0.0_phospholipids_PC_v2.itp" ; include water definitions here
> 
> [ system ]
> ; This title is arbitrary (but something descriptive helps)
> DSPC BILAYER SELF-ASSEMBLY
> 
> [ molecules ]
> ; Molecule types and their numbers are written in the order
> ; they appear in the structure file
> DSPC 128
> W 768  ;  
**the whole system contains 768 water molecules and 128 DSPC molecules.**

### 2.Define Box & Solvate
- position / coordinates for all molecules in the system 
> The first step is to create a simulation box containing a random configuration of 128  distearoyl-phosphatidylcholine (DSPC) lipids. This can be done by starting from a file containing a single DSPC molecule, which you can get from the [Martini 2 lipidome](https://cgmartini.nl/docs/downloads/force-field-parameters/martini2/lipidome.html#phosphatidylcholine-(pc)).

---
`DSPC-em.gro`
> DPPC sim
>    12
>     1DPPC   NC3    1   0.485   0.515   2.472
>     1DPPC   PO4    2   0.589   0.568   2.201
>     1DPPC   GL1    3   0.509   0.536   1.838
>     1DPPC   GL2    4   0.772   0.531   1.760
>     1DPPC   C1A    5   0.396   0.476   1.453
>     1DPPC   C2A    6   0.446   0.522   1.174
>     1DPPC   C3A    7   0.421   0.596   0.897
>     1DPPC   C4A    8   0.434   0.607   0.553
>     1DPPC   C1B    9   0.922   0.644   1.490
>     1DPPC   C2B   10   0.943   0.481   1.201
>     1DPPC   C3B   11   0.926   0.607   0.904
>     1DPPC   C4B   12   0.936   0.514   0.557
>    3.00000   3.00000   3.00000
---
`128_noW.gro`
```
gmx insert-molecules \
  -ci DPPC-em.gro \
  -box 7.5 7.5 7.5 \
  -nmol 128 \
  -radius 0.21 \
  -try 500 \
  -o 128_noW.gro
```

>  DPPC sim
>  1536
>     1DPPC   NC3    1   5.416   6.601   6.849
>     1DPPC   PO4    2   5.553   6.364   6.737
>     1DPPC   GL1    3   5.879   6.183   6.729
>     1DPPC   GL2    4   5.830   6.066   6.486
>     1DPPC   C1A    5   6.249   6.018   6.732
>     1DPPC   C2A    6   6.418   5.797   6.662
>     1DPPC   C3A    7   6.604   5.577   6.671
>     1DPPC   C4A    8   6.855   5.351   6.605
>     1DPPC   C1B    9   5.917   5.774   6.363
>     1DPPC   C2B   10   6.209   5.707   6.218
>     1DPPC   C3B   11   6.380   5.434   6.241
>     1DPPC   C4B   12   6.687   5.283   6.128
>     2DPPC   NC3   13   7.321   2.765   6.253
>     2DPPC   PO4   14   7.034   2.808   6.302
>     2DPPC   GL1   15   6.722   2.948   6.152
> ……
>   127DPPC   C4B 1524   7.451   0.795  -0.253
>   128DPPC   NC3 1525   4.122   0.913   6.058
>   128DPPC   PO4 1526   4.003   0.824   5.803
>   128DPPC   GL1 1527   3.680   0.790   5.619
>   128DPPC   GL2 1528   3.769   0.584   5.461
>   128DPPC   C1A 1529   3.312   0.758   5.451
>   128DPPC   C2A 1530   3.154   0.700   5.219
>   128DPPC   C3A 1531   2.966   0.715   5.001
>   128DPPC   C4A 1532   2.726   0.648   4.763
>   128DPPC   C1B 1533   3.699   0.502   5.150
>   128DPPC   C2B 1534   3.437   0.323   5.054
>   128DPPC   C3B 1535   3.259   0.364   4.787
>   128DPPC   C4B 1536   2.975   0.228   4.614
---
`water.gro`
> WATER
>   400
>     1W        W    1   0.084   3.595   3.359  0.0663  0.0148  0.2032
>     2W        W    2   0.460   2.488   2.882  0.0999 -0.0648 -0.1661
>     3W        W    3   3.165   2.218   0.652 -0.0881 -0.1237 -0.0896
>     4W        W    4   3.295   3.303   2.679 -0.1486 -0.3229  0.1120
>     5W        W    5   1.213   3.294   0.205 -0.1053 -0.1062  0.1014
>     6W        W    6   1.296   1.446   1.188  0.0877 -0.0566 -0.1282
>     7W        W    7   0.811   1.294   1.352  0.1967  0.2199  0.3289
>   ……  
>   399W        W  399   1.524   2.769   0.845 -0.5337  0.0559  0.0075
>   400W        W  400   0.745   1.509   0.104  0.1616 -0.0161  0.1005
>    3.64428   3.64428   3.64428
---
`waterbox.gro` 128 DPPC+ water as solvent 
```
gmx solvate \
  -cp 128_noW.gro \
  -cs water.gro \
  -o waterbox.gro \
  -maxsol 768 \
  -radius 0.21
```
> DPPC sim
>  2304
>     1DPPC   NC3    1   5.416   6.601   6.849
>     1DPPC   PO4    2   5.553   6.364   6.737
>     1DPPC   GL1    3   5.879   6.183   6.729
>     1DPPC   GL2    4   5.830   6.066   6.486
>     1DPPC   C1A    5   6.249   6.018   6.732
>     1DPPC   C2A    6   6.418   5.797   6.662
>     1DPPC   C3A    7   6.604   5.577   6.671
>     1DPPC   C4A    8   6.855   5.351   6.605
>   ……
>   128DPPC   GL2 1528   3.769   0.584   5.461
>   128DPPC   C1A 1529   3.312   0.758   5.451
>   128DPPC   C2A 1530   3.154   0.700   5.219
>   128DPPC   C3A 1531   2.966   0.715   5.001
>   128DPPC   C4A 1532   2.726   0.648   4.763
>   128DPPC   C1B 1533   3.699   0.502   5.150
>   128DPPC   C2B 1534   3.437   0.323   5.054
>   128DPPC   C3B 1535   3.259   0.364   4.787
>   128DPPC   C4B 1536   2.975   0.228   4.614
>   ……
>   129W        W 1537   3.295   3.303   2.679
>   130W        W 1538   0.811   1.294   1.352
>   131W        W 1539   1.802   2.109   0.786
>       ……
>   891W        W 2299   5.584   5.363   6.334
>   892W        W 2300   4.837   6.952   5.592
>   893W        W 2301   6.643   5.907   5.132
>   894W        W 2302   4.262   6.702   5.753
>   895W        W 2303   5.283   4.754   6.963
>   896W        W 2304   4.389   5.153   3.748
>    7.50000   7.50000   7.50000
> 

### 3.Energy Minimization
- purpose from martini tutorial
> Now you will perform an energy minimization of the solvated system, to get rid of high forces between beads that may have been placed too close to each other. The settings file minimization.mdp is provided for you, but you will need the topology for water and for the DSPC lipid, and to organize them as a .top file. 

`minimization.mdp` file were used for setting all parameters for energy minimization

> ; Energy minimization
> integrator  = steep
> emtol       = 100.0
> emstep      = 0.01
> nsteps      = 50000
> nstlist     = 10
> cutoff-scheme = Verlet
> coulombtype = PME
> rcoulomb    = 1.0
> rvdw        = 1.0
> energygrps  = system

---
`dspc-min-solvent.tpr`
```
gmx grompp \
  -f minimization.mdp \
  -c waterbox.gro \
  -p dspc.top \
  -o dspc-min-solvent.tpr

```
---
`minimized.gro` This files is where we start our MD.
```
gmx mdrun \
  -s dspc-min-solvent.tpr \
  -v \
  -c minimized.gro
```
- stop when Fmax is lower than some certain value（e.g. < 10 kJ/mol/nm) by steepest descents algorthm.
> Steepest Descents converged to Fmax < 10 in 6627 steps

### 4.NVT & NPT Equilibration
`martini_md.mdp`

> integrator               = md
> dt                       = 0.02
> nsteps                   = 1500000
> 
> ; Center of mass removal
> comm-mode                = Linear
> nstcomm                  = 100
> comm-grps                = System
> 
> nstxout                  = 0
> nstvout                  = 0
> nstfout                  = 0
> nstlog                   = 10000
> nstenergy                = 1000
> nstxout-compressed       = 1000
> compressed-x-precision   = 100
> compressed-x-grps        =
> energygrps               = System
> 
> cutoff-scheme            = Verlet
> nstlist                  = 20
> nsttcouple               = 20
> nstpcouple               = 20
> rlist                    = 1.35
> verlet-buffer-tolerance  = -1
> ns_type                  = grid
> pbc                      = xyz
> 
> coulombtype              = reaction-field
> rcoulomb                 = 1.1
> epsilon_r                = 15	; 2.5 (with polarizable water)
> epsilon_rf               = 0
> vdw_type                 = cutoff
> vdw-modifier             = Potential-shift-verlet
> rvdw                     = 1.1
> 
> tcoupl                   = v-rescale
> tc-grps                  = System
> tau_t                    = 1.0
> ref_t                    = 340
> 
> gen_vel                  = yes
> gen_temp                 = 340
> gen_seed                 = -1
> 
> ; Pressure coupling     
> Pcoupl                   = Berendsen
> Pcoupltype               = isotropic
> tau_p                    = 4.0 
> ref_p                    = 1.0
> compressibility          = 3e-4 
> 
> constraints              = none
> constraint_algorithm     = Lincs
> lincs_order              = 8
> lincs_warnangle          = 90
> lincs_iter               = 2

### 5.MD Production 
1 .`dspc-md.tpr`
```
gmx grompp \
  -f martini_md.mdp \
  -c minimized.gro \
  -p dspc.top \
  -o dspc-md.tpr
```
2. `gmx mdrun -deffnm dspc-md -v`

### 6. Analysis Methods
