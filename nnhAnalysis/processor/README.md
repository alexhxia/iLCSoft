# processor

## Avec un fichier

Télécharger le git du programme de la branch `refractor`
```
git clone -branch refractor https://github.com/ggarillot/nnhAnalysis.git
```

```
cd nnhAnalysis/processor
```
On compile le projet :
```
mkdir Build && cd Build
cmake -C $ILCSOFT/ILCSoft.cmake ..
make
make install
export MARLIN_DLL=$MARLIN_DLL:$PWD/lib/libnnhProcessor.so
```

```
cd script/
```
Remplacer les fichiers input.slcio et output.root par les noms souhaité.

On lance le programme qui transforme un fichier .slcio et fichier .root
```
Marlin NNH_steer.xml 
```
