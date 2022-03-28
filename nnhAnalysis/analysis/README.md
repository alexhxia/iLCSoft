# Analysis 

## Préparation de l'environnement
```
source /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-03/init_ilcsoft.sh
```
```
export NNH_HOME=~/nnhAnalysis
```
```
mkdir $NNH_HOME/analysis/DATA $NNH_HOME/analysis/Build
```
```
hadd $NNH_HOME/analysis/DATA/DATA.root $NNH_HOME/output/*.root
```

## Compilation
```
cd $NNH_HOME/analysis/Build
```
```
cmake -C $ILCSOFT/ILCSoft.cmake ..
```
```
make
```
```
make install
```
## Exécution 
`exec/prepareForBDT.cxx` :
```
./bin/prepareForBDT
```
### Pré-requis 
Les paquets `joblib`, `pandas`, `numpy`, `scipy`, `sklearn`, `cppyy`, `libPyROOT` et `ROOT`

### Cas de ROOT

### Lancement
Depuis le dossier `analysis/python` :
```
cd $NNH_HOME/analysis/python
```
```
python3 launchBDT_bb.py
```
```
python3 launchBDT_WW.py
```
