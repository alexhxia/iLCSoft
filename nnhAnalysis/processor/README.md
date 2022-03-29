# processor
Si les fichiers sont dans le dossier `/gridgroup/ilc/nnhAnalysisFiles/AHCAL`.

## Installation

Télécharger le git du programme de la branch `refractor`
```
git clone -branch refractor https://github.com/ggarillot/nnhAnalysis.git
```
## Compilation
```
cd nnhAnalysis/processor && mkdir BUILD && cd BUILD
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
export NNH_HOME=~/nnhAnalysis
```
```
export NNH_PROCESSOR_INPUTFILES=/gridgroup/ilc/nnhAnalysisFiles/AHCAL/
```
```
export NNH_PROCESSOR_OUTPUTFILES=$NNH_HOME/OUTPUT
```
```
export MARLIN_DLL=$MARLIN_DLL:$NNH_HOME/processor/lib/libnnhProcessor.so
```
## Convertir un seul fichier
```
cd $NNH_HOME/processor/script/
```
Dans le programme `NNH_steer.xml ` modifier `input.slcio` et `output.root` par les chemins souhaités, soit le fichier des données et le fichier de sortie.

On peut à présent lancer le programme qui construit un fichier `.root` à partir d'un fichier `.slcio` :
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
## Convertir plusieurs processus
```
cd $NNH_HOME/processor/script
```
```
mkdir $NNH_HOME/OUTPUT
```
### Liste de quelques processus :
Exemple pour les processus `402007` `402008` :
```
python3 launchNNHProcessor.py -n 10 -p 402007 402008 -i $NNH_PROCESSOR_INPUTFILES -o NNH_PROCESSOR_OUTFILES
```
### Convertir tous les processus
```
python3 launchNNHProcessor.py -n 10 -i $NNH_PROCESSOR_INPUTFILES -o NNH_PROCESSOR_OUTFILES
```
# Suite 
Continuer dans la partie `analysis`.
