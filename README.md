# ilcsoft_tutorial
Stage de M2 à l'IP2I

## Un tutorial de ilcsoft :
https://agenda.linearcollider.org/event/9272/

### Initialisation ilcsoft :
(À refaire chaque fois)
```
$ source /cvmfs/ilc.desy.de/sw/x86_64_gcc82_centos7/v02-02-03/init_ilcsoft.sh
```

Pour essayer ilcsoft sans faire toute l'installation, on peut utiliser un container docker. 
Sous linux, une fois "docker engine" installé, il suffit de tourner la commande :

```
$ docker run -it ilcsoft/ilcsoft-centos7-gcc8.2:v02-02 bash
```
Puis dans le container, on initialise ilcsoft avec :
```
$ source /home/ilc/ilcsoft/v02-02/init_ilcsoft.sh 
```

### Documentation
La documentation et le packet git du format de données LCIO 
https://github.com/iLCSoft/LCIO
et de la librairie Marlin
https://github.com/iLCSoft/Marlin


### Travail à effectuer

L'analyse à faire tourner : 
https://github.com/ggarillot/nnhAnalysis/tree/refactor

et une thèse correspondante : 
https://tel.archives-ouvertes.fr/tel-03405418


# Pour la deuxième partie du stage,
le software en développement : 
https://github.com/key4hep
et plus particulièrement l'adaptateur ilcsoft vers key4hep : 
https://github.com/key4hep/k4MarlinWrapper
