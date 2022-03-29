# analysis 

Avant d'exécuter `analysis` il faut avoir générer les fichiers ROOTs (voir la partie `processor`).

Il faut un environnement au moins sous `python 3.9` et avec `root`.

## Préparation de l'environnement
```
source /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-03/init_ilcsoft.sh
```
```
export  NNH_HOME=~/nnhAnalysis \
        NNH_INPUTFILES=/gridgroup/ilc/nnhAnalysisFiles/AHCAL/ \
        NNH_PROCESSOR_INPUTFILES=/gridgroup/ilc/nnhAnalysisFiles/AHCAL/ \
        NNH_PROCESSOR_OUTPUTFILES=$NNH_HOME/OUTPUT
        NNH_ROOTFILES=$NNH_HOME/OUTPUT
        NNH_ANALYSIS_INPUTFILES=$NNH_HOME/OUTPUT
        NNH_ANALYSIS_OUTPUTFILES=$NNH_HOME/analysis/DATA
        NNH_DATA=$NNH_HOME/analysis/DATA
```

```
mkdir $NNH_HOME/analysis/DATA $NNH_HOME/analysis/Build
```
```
hadd $NNH_HOME/analysis/DATA/DATA.root $NNH_HOME/output/*.root
```
```
source /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-03/init_ilcsoft.sh
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
cd $NNH_HOME/analysis
```
```
./bin/prepareForBDT
```
### Pré-requis 
Les paquets `joblib`, `pandas`, `numpy`, `scipy`, `sklearn`, `cppyy`, `libPyROOT` et `ROOT`
```
install joblib pandas numpy scipy sklearn cppyy
```
### Lancement
Depuis le dossier `analysis/python` :
```
cd $NNH_HOME/analysis/python
```
#### Attention redémarrer la session car la commande 
`source /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-03/init_ilcsoft.sh`
ne doit pas avoir été exécuter. 
```
python3 launchBDT_bb.py
```
```
python3 launchBDT_WW.py
```
