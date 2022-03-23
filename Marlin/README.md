# ILDConfig

## Installation
```
git clone https://github.com/iLCSoft/ILDConfig.git -b v02-02-02
cd ILDConfig
. /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-02/init_ilcsoft.sh
```

## Test 
```
ddsim -h
Marlin -h
dumpevent -h 
g++ -v

find $ILCSOFT -maxdepth 2 -mindepth 2
```

## 1er simu
Muon sans angle
```
ddsim --inputFiles Examples/bbudsc_3evt/bbudsc_3evt.stdhep --outputFile=./bbudsc_3evt_SIM.slcio --compactFile $lcgeo_DIR/ILD/compact/ILD_l5_v02/ILD_l5_v02.xml --steeringFile=./ddsim_steer.py > ddsim.out 2>&1 &    
```

### Exercice 1
pi+ avec angle
```
ddsim --inputFiles Examples/bbudsc_3evt/bbudsc_3evt.stdhep --outputFile=./bbudsc_3evt_SIM.slcio --compactFile $lcgeo_DIR/ILD/compact/ILD_l5_v02/ILD_l5_v02.xml --steeringFile=./ddsim_steer.py > ddsim.out 2>&1 &    
```
