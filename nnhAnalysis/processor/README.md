# processor
Ce tuto considère que les fichiers sont en local dans le dossier `/gridgroup/ilc/nnhAnalysisFiles/AHCAL`.
```
export NNH_INPUTFILES=/gridgroup/ilc/nnhAnalysisFiles/AHCAL/
```
## Installation

Pour récupérer le projet déjà existant, télécharger la branch `refractor` du projet `nnhAnalysis` du git de `ggarillot` :
```
git clone -branch refractor https://github.com/ggarillot/nnhAnalysis.git
```
```
export NNH_HOME=~/nnhAnalysis
```
## Compilation
```
cd $NNH_HOME/processor && mkdir BUILD && cd BUILD
```
À faire obligatoirement avant le `cmake` :
```
source /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-03/init_ilcsoft.sh
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
La compilation génère une bibliotèque `libnnhProcessor` qu'il faut impérativement ajouter dans le `MARLIN_DLL`. Donc avant d'exécuter les commandes du programme, il faut donc obligatoirement :
```
export MARLIN_DLL=$MARLIN_DLL:~/nnhAnalysis/processor/lib/libnnhProcessor.so
```
## Convertir un seul fichier
```
cd $NNH_HOME/processor/script/
```
Dans le programme `NNH_steer.xml `, il faut adapter le fichier d'entrée `input.slcio` et  le fichier de sortie `output.root` par les chemins souhaités.

On peut à présent lancer ce programme qui construit un fichier `.root` à partir d'un fichier `.slcio` :
```
Marlin NNH_steer.xml 
```
NB :Vérifier le contenu du nouveau fichier .root
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
On crée un dossier pour tous les fichiers ROOTs qui seront générer :
```
mkdir $NNH_HOME/OUTPUT
```
```
export NNH_PROCESSOR_OUTPUTFILES=$NNH_HOME/OUTPUT
```
Pour rappel, les fichier d'entrée LCIO sont `/gridgroup/ilc/nnhAnalysisFiles/AHCAL/` : 
```
export NNH_PROCESSOR_INPUTFILES=/gridgroup/ilc/nnhAnalysisFiles/AHCAL/
```
### Convertir quelques processus :
Utiliser le paramètre `-p num_processus`. Par exemple pour les processus `402007` `402008` :
```
python3 launchNNHProcessor.py -n 10 -p 402007 402008 -i $NNH_PROCESSOR_INPUTFILES -o $NNH_PROCESSOR_OUTPUTFILES
```
### Convertir tous les processus
```
python3 launchNNHProcessor.py -n 100 -i $NNH_PROCESSOR_INPUTFILES -o $NNH_PROCESSOR_OUTPUTFILES
```
# Suite 
Continuer dans la partie `analysis`.

## À refaire à chaque fois que l'on ouvre le terminal ou une session `ssh` :
```
source /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-03/init_ilcsoft.sh
```
```
export  NNH_HOME=~/nnhAnalysis \
        NNH_PROCESSOR_INPUTFILES=/gridgroup/ilc/nnhAnalysisFiles/AHCAL/ \
        NNH_PROCESSOR_OUTPUTFILES=~/nnhAnalysis/OUTPUT
```
```
export MARLIN_DLL=$MARLIN_DLL:~/nnhAnalysis/processor/lib/libnnhProcessor.so
```
