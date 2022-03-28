# processor

## Installation

Télécharger le git du programme de la branch `refractor`
```
git clone -branch refractor https://github.com/ggarillot/nnhAnalysis.git
```
```
cd nnhAnalysis/processor
```
## Compilation

```
mkdir Build && cd Build
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
## Préparation de l'environnement (À refaire à chaque fois)
```
source /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-03/init_ilcsoft.sh
```
```
export NNH_HOME=/path/to/nnhAnalysis
```
```
export NNH_ANALYSIS_INPUTFILES=/gridgroup/ilc/nnhAnalysisFiles/AHCAL/
```
```
export NNH_ANALYSIS_OUTFILES=~/nnhAnalysisFiles/
```
```
export MARLIN_DLL=$MARLIN_DLL:$PWD/lib/libnnhProcessor.so
```


## Avec un fichier
```
cd script/
```
Dans le programme `NNH_steer.xml ` emplacer les fichiers `input.slcio` et `output.root` par les noms souhaités.

On lance le programme qui transforme un fichier .slcio et fichier .root
```
Marlin NNH_steer.xml 
```
Vérifier le contenu du nouveau fichier .root
```
root -l
TFile *f = TFile::open("XXX.root")
f->ls()
tree->Scan()
```
## Sur plusieurs fichiers
