# ILDConfig

https://agenda.linearcollider.org/event/9272/

## Installation
```
git clone https://github.com/iLCSoft/ILDConfig.git -b v02-02-02
```
```
cd ILDConfig
```
```
. /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-02/init_ilcsoft.sh
```

## Test 
```
ddsim -h
```
```
Marlin -h
```
```
dumpevent -h 
```
```
g++ -v
```
```
find $ILCSOFT -maxdepth 2 -mindepth 2
```

## Run simulation
Muon sans angle
```
ddsim --inputFiles Examples/bbudsc_3evt/bbudsc_3evt.stdhep --outputFile=./bbudsc_3evt_SIM.slcio --compactFile $lcgeo_DIR/ILD/compact/ILD_l5_v02/ILD_l5_v02.xml --steeringFile=./ddsim_steer.py > ddsim.out 2>&1 &    
```

### Exercice 1
pi+ avec angle
```
ddsim --inputFiles Examples/bbudsc_3evt/bbudsc_3evt.stdhep --outputFile=./bbudsc_3evt_SIM.slcio --compactFile $lcgeo_DIR/ILD/compact/ILD_l5_v02/ILD_l5_v02.xml --steeringFile=./ddsim_steer.py > ddsim.out 2>&1 &    
```

## Fichiers LCIO
```
anajob bbudsc_3evt_SIM.slcio
```
```
dumpevent bbudsc_3evt_SIM.slcio 2 | less
```

### Exercice 2
``` 
LCIO_READ_COL_NAMES="HCalBarrelRPCHits HCalECRingRPCHits HCalEndcapRPCHits"; dumpevent bbudsc_3evt_SIM.slcio 2 | less
```
```
export LCIO_READ_COL_NAMES="HCalBarrelRPCHits HCalECRingRPCHits HCalEndcapRPCHits"
```
```
dumpevent bbudsc_3evt_SIM.slcio 2 | less
```
## Lire LCIO avec Python
```
touch dumplcio.py
```
Y copier
```
from pyLCIO import UTIL, EVENT, IMPL, IO, IOIMPL
import sys
infile = sys.argv[1]
rdr = IOIMPL.LCFactory.getInstance().createLCReader( )
rdr.open( infile )
for evt in rdr:
    col = evt.getCollection("MCParticle")
    for p in col:
        print(p.getEnergy())
```
Exécuter :
```
python dumplcio.py bbudsc_3evt_SIM.slcio
```
### Exercice 3
Modifier pour afficher les énergies totales par évènement.
```
from pyLCIO import UTIL, EVENT, IMPL, IO, IOIMPL
import sys

infile = sys.argv[1]
rdr = IOIMPL.LCFactory.getInstance().createLCReader( )
rdr.open(infile)

for evt in rdr:
    col = evt.getCollection("MCParticle")
    s = 0.
    for p in col:
        s = s + p.getEnergy()
    print(s)
```
Exécuter :
```
python dumplcio.py bbudsc_3evt_SIM.slcio
```
## Reconstruction
```
Marlin MarlinStdReco.xml \
    --constant.lcgeo_DIR=$lcgeo_DIR \
    --constant.DetectorModel=ILD_l5_o1_v02 \
    --constant.OutputBaseName=bbudsc_3evt \
    --global.LCIOInputFiles=bbudsc_3evt_SIM.slcio \
    > marlin.out 2>&1 &
```

### Analyse ROOT
Création d'un arbre ROOT
`Marlin --global.LCIOInputFiles=bbudsc_3evt_REC.slcio \
MarlinStdRecoLCTuple.xml
`
