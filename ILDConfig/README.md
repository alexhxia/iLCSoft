# ILDConfig

https://agenda.linearcollider.org/event/9272/

# Section 1 Introduction à `ILCSoft`

LCIO : https://github.com/iLCSoft/LCIO

DD4hep (Detector Description for HEP) : https://dd4hep.web.cern.ch/dd4hep/

Marlin : https://ilcsoft.desy.de/Marlin/current/doc/html/index.html

## Installation depuis github
```
git clone https://github.com/iLCSoft/ILDConfig.git -b v02-02-02
```

# Section 2 Premier pas
```
cd ILDConfig
```
## Initialisation des paramètres
(À refaire à chaque fois que l'on redémarre)
```
. /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-02/init_ilcsoft.sh
```

## Tests et aides
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
Depuis le dossier `ILDConfig/StandardConfig/production/`
Muon sans angle
```
ddsim --inputFiles Examples/bbudsc_3evt/bbudsc_3evt.stdhep --outputFile=./bbudsc_3evt_SIM.slcio --compactFile $lcgeo_DIR/ILD/compact/ILD_l5_v02/ILD_l5_v02.xml --steeringFile=./ddsim_steer.py > ddsim.out 2>&1 &    
```

### Exercice 1
pi+ avec angle
Modifier le programme `ddsim_steer.py`, des lignes 186-195 :

```
SIM.gun.particle = "pi+"
SIM.gun.phiMax = 3.14

## Minimal azimuthal angle for random distribution
SIM.gun.phiMin = 0

SIM.gun.thetaMax = 2 * 3.14
SIM.gun.thetaMin = 0
```
Exécution :
```
ddsim --inputFiles Examples/bbudsc_3evt/bbudsc_3evt.stdhep --outputFile=./bbudsc_3evt_SIM.slcio --compactFile $lcgeo_DIR/ILD/compact/ILD_l5_v02/ILD_l5_v02.xml --steeringFile=./ddsim_steer.py > ddsim.out 2>&1 &    
```
NB : L'exécution s'effectue en arrière plan et le fichier de sortie sera `ddsim.out`. Pour suivre son avancement : 
```
ps
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
Depuis le dossier `ILDConfig/StandardConfig/production/`
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

### Arbre ROOT

#### Création d'un arbre ROOT :
```
Marlin --global.LCIOInputFiles=bbudsc_3evt_REC.slcio MarlinStdRecoLCTuple.xml
```
Création du fichier `StandardReco_LCTuple.root`

#### Analyse ROOT
```
cd RootMacros
```
```
root -l
```
`root [0] .x ./draw_simhits.C("../StandardReco_LCTuple.root")`
```
.x ./draw_simhits.C("../StandardReco_LCTuple.root")
```

```
gSystem->Load("$LCIO/lib/liblcioDict.so");
```
#### Lecture d'un fichier slcio avec ROOT
Depuis le dossier `ILDConfig/StandardConfig/production/`
```
touch read_slcio.C
```
Écrire dans ce fichier 
```
#ifdef __CLING__
R__LOAD_LIBRARY(liblcioDict);
#endif

#include "IO/LCReader.h"
#include "EVENT/MCParticle.h"
#include "IOIMPL/LCFactory.h"
#include "UTIL/LCIterator.h"
#include "lcio.h"

#include <iostream>

void read_slcio() {
    using namespace lcio;
    auto* lcReader = IOIMPL::LCFactory::getInstance()->createLCReader();
    lcReader->open("bbudsc_3evt_SIM.slcio");

    while(auto* evt = lcReader->readNextEvent()) {
        LCIterator<MCParticle> mcParticles(evt, "MCParticle");
        std::cout << mcParticles.size() << std::endl;
    }
}
```
Exécuter 
```
root -l read_slcio.C
```
# Section 3 Création de notre package Marlin

## Construction
Depuis le dossier `ILDConfig/StandardConfig/production/`

``` 
cp -rp $MARLIN/examples/mymarlin .
```
``` 
cd mymarlin 
```
``` 
mkdir build && cd build 
```
``` 
cmake -C $ILCSOFT/ILCSoft.cmake .. 
```
``` 
make install 
```
``` 
export MARLIN_DLL=$MARLIN_DLL:$PWD/../lib/libmymarlin.so 
```
``` 
MARLIN_DLL=$PWD/../lib/libmymarlin.so Marlin -x > mysteer.xml
```

## Creation
Depuis le dossier `ILDConfig/StandardConfig/production/`
Modifier le fichier `CMakeList.txt`, `PROJECT(mymarlin)` en `PROJECT(NewProcessorName)`

```
mv include/MyProcessor.h include/NewProcessorName.h
```
```
mv src/MyProcessor.cc src/NewProcessorName.cc
```
```
sed -i 's/MyProcessor/NewProcessorName/g' include/NewProcessorName.h
```
```
sed -i 's/MyProcessor/NewProcessorName/g' src/NewProcessorName.cc
```
``` 
cd build 
```
```
rm -R *
```
``` 
cmake -C $ILCSOFT/ILCSoft.cmake .. 
```
``` 
make install 
```
```
export MARLIN_DLL=$MARLIN_DLL:$PWD/../lib/libNewProcessorName.so
```
```
MARLIN_DLL=$PWD/../lib/libNewProcessorName.so Marlin -x > mysteer.xml
```
## Marlin processor

### Exercice 4
Se placer dans le dossier `StandardConfig/production/mymarlin`.

Décommanter la ligne `FIND PACKAGE( AIDA ) du fichier `CMakeLists.txt`

Ajouter après le ligne 114 :
```

```
