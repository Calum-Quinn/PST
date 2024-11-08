# Laboratoire 1 PST

## Exercice 1

### cpus
```
cpus <- scan("cpus.txt",numeric(0)," ")
```

### examen
```
examen <- read.table("examen.txt",TRUE)
```

## Exercice 2

### a)

1. Quels sont les effets de la commande `boxplot(cpus, xlab="performance relative", col="darkslategray4", horizontal=T)` ?
	Cette commande à pour effet de générer un boxplot horizontal des données avec comme légende de l'axe x `performance relative` et avec la couleur du graphique en gris.
2. Quel est l'effet de la fonction `rug()` ?
	Cette commande ajoute des graduations à l'axe horizontal pour représenter la densité des valeurs (diagramme en aiguilles).
	
### b) Commenter la distribution des valeurs observées en se basant sur les graphiques de la Figure 5 : valeur(s) atypique(s), asymétrie
Comme on peut le voir dans les deux graphiques, la majorité des valeurs se trouvent au début de l'échelle donnée (~0-100). Ceci nous amène à dire qu'il y a un coefficient de corélation positif et qu'il y a des valeurs atypiques entre environ 90 et 900.

### c) Calculer la performance relative médiane et la performance relative moyenne des valeurs observées en utilisant les fonctions de R adéquates. Est-il plus approprié d’utiliser la médiane ou la moyenne ?
```
> mean(cpus)
[1] 86.88
> median(cpus)
[1] 42
```

En sachant que la grande majorité des valeurs se trouvent entre 0 et 100, la moyenne et la médiane sont assez écarté. Ceci vient très problement des valeurs atypiques qui tire beaucoup la moyenne vers le haut.
Dans ce cas il est plus logique d'utiliser la médiane car elle est moins affectée par les quelques valeurs atypiques très grandes.

### d) Déterminer le(s) mode(s) des valeurs observées à l’aide des commandes suivantes :
```
n.cpus<-table(cpus)
as.numeric(names(n.cpus)[n.cpus==max(n.cpus)])
```

```
> n.cpus<-table(cpus)
> as.numeric(names(n.cpus)[n.cpus==max(n.cpus)])
[1] 24 36 66
```

### e) Que fait la commande `summary(cpus)` ?
```
> summary(cpus)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   7.00   24.00   42.00   86.88   73.50  915.00 
```

La fonction nous rend une vision globale de l'objet, dans ce cas cpus.txt. Il nous donne les informations:
- Valeur minimum
- Premier Quartile (25% des données sont plus petites que cette valeur)
- Médiane
- Moyenne
- Troisième Quartile (25% des données sont plus grande que cette valeur)
- Valeur maximum

### f) Décrire l’effet sur la moyenne et sur la médiane des trois interventions suivantes :

1. ajouter un processeur de performance relative 43;
```
> cpus<-c(cpus,43)
> summary(cpus)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   7.00   24.00   43.00   86.02   71.00  915.00 
```

La médiane augmente puisque la valeur ajoutée est plus grande que la médiane précédente.
A l'inverse, la moyenne descend puisque la valeur ajoutée est plus petite.

2. soustraire 9 à chaque valeur observée;
```
> cpus<- cpus - 9
> summary(cpus)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  -2.00   15.00   34.00   77.02   62.00  906.00
```

On voit que soustraire 9 à chaque valeur à eu comme effet de soustraire exactement 9 à toutes les valeurs calculées (médiane, moyenne, ...).

3. diviser chaque observation par 3.
J'ai ajouté à nouveau les 9 à chaque valeur avant de faire ça pour éviter d'avoir des calculs particulier au niveau des valeurs négatives.
```
> cpus<- cpus + 9
> cpus<- cpus / 3
> summary(cpus)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  2.333   8.000  14.333  28.673  23.667 305.000
```

On remarque que diviser toutes les valeurs par une constane (dans ce cas 3), divise également la médiane et la moyenne par la même constante.

### g) Calculer l’écart-type des performances relatives une fois avec les valeurs atypiques et une fois sans en utilisant la fonction sd(). Les valeurs atypiques peuvent être déterminées à l’aide de la fonction boxplot() avec plot=FALSE comme argument. Que constate-t-on ? L’écart-type est-il un indicateur robuste ?
```
> ecartTypeAvec<-sd(cpus)
> print(ecartTypeAvec)
[1] 49.02201

> valeursAtypiques<-boxplot(cpus,plot = FALSE)$out
> print(valeursAtypiques)
[1]  48.00000 305.00000 123.33333  61.66667 170.00000
> ecartTypeSans <- sd(cpus[!cpus %in% valeursAtypiques])
> print(ecartTypeSans)
[1] 11.99519
```

L'écart-type calculé avec les valeurs atypique et largement plus grand que sans. Ceci est normal mais montre qu'il faut faire attention lors de l'utilisation de celui-ci. C'est donc un indicateur non robuste car il est fortement influencé par les valeurs extrêmes.