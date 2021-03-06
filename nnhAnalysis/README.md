# nnhAnalysis

## Données initiales
Les données LCIO sont stockées localement dans le dossier :
```
/gridgroup/ilc/nnhAnalysisFiles/AHCAL/
```
Puis chaque fichier est trié dans des sous-dossiers en fonction de leur numéro de processus.

Un exemple de nom d'un fichier avec son chemin :
``` 
/gridgroup/ilc/nnhAnalysisFiles/402001/rv02-02.sv02-02.mILD_l5_o1_v02.E250-SetA.I402001.Pe1e1h.eL.pR.n000.d_dstm_15089_0_MINI.slcio 
```

## `processus`
On traite une première fois les fichiers LCIO dans la partie `processor` afin obtenir un fichier ROOT par processus (cf `processor/README`), qui sera placer dans un dossier `RESULTS`.

NB : les commandes pour avoir un environnement opérationnel, à refaire à chaque ouverture :
```
source /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-03/init_ilcsoft.sh
```
```
export  NNH_INPUTFILES=/gridgroup/ilc/nnhAnalysisFiles/AHCAL \
        NNH_OUTPUTFILES=/gridgroup/ilc/nnhAnalysisFiles/result \
        NNH_HOME=~/nnhAnalysis
```
```     
export  NNH_PROCESSOR_INPUTFILES=$NNH_INPUTFILES \
        NNH_PROCESSOR_OUTPUTFILES=$NNH_HOME/processor/RESULTS 
```
```
export MARLIN_DLL=$MARLIN_DLL:$NNH_HOME/processor/lib/libnnhProcessor.so
```
## `analysis`
On traite une première fois les fichiers LCIO dans la partie `processor` afin obtenir un fichier ROOT par processus (cf `processor/README`), qui sera placer dans un dossier `RESULTS`.

NB : les commandes pour avoir un environnement opérationnel, à refaire à chaque ouverture :
```
source /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-03/init_ilcsoft.sh
```
```
export  NNH_INPUTFILES=/gridgroup/ilc/nnhAnalysisFiles/AHCAL \
        NNH_OUTPUTFILES=/gridgroup/ilc/nnhAnalysisFiles/result \
        NNH_HOME=~/nnhAnalysis
```
```     
export  NNH_ANALYSIS_INPUTFILES=$NNH_HOME/processor/RESULTS \
        NNH_ANALYSIS_OUTPUTFILES=$NNH_HOME/analysis/DATA
```
