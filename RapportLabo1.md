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

## Exercice 3

### a) Tracer les boîtes à moustaches en parallèle en utilisant les commandes suivantes :
```
lblue<-"#528B8B"
par(pty="s")
boxplot(note~groupe, data=examen, ylim=c(1,6), xlab="groupe", varwidth=T, col=lblue, main="examen")
abline(h=4, lty=2)
```

### b) Rajouter les bâtonnets des notes des étudiants des deux classes, sur le côté gauche des boîtes à moustaches pour la classe 𝐴 (side=2 comme argument de la fonction rug()) et sur le côté droite pour la classe 𝐵 (side=4 comme argument de la fonction rug()).
```
> note.A<-split(examen$note, examen$groupe)$A
> note.B<-split(examen$note, examen$groupe)$B
> rug(note.A,side=2)
> rug(note.B,side=4)
```

### c) En se basant sur la Figure 6, existe-t-il une différence significative entre les deux groupes à l’examen de fin d’unité ?
Pas particulièrement, les différences sont les suivantes:
- Le groupe B à deux personnes ayant fait une note hors norme (1-2).
- Le groupe B à une dispersion légèrement plus grande mais généralement c'est similaire au groupe A.
- La médiane du groupe B est légèrement plus élevée.
- Le groupe A n'a que 25% en dessous du 4, le groupe B en a plus.

### d) Observe-t-on sur les boîtes à moustaches une différence entre les dispersions des deux groupes ?
Oui, comme cité à la question précédente le groupe B est légèrement plus dispersé.

### e) Calculer les écarts-types des deux groupes à l’aide des fonctions by() et sd().
```
> ecartsTypes <- by(examen$note, examen$groupe, sd)
> print(ecartsTypes)
examen$groupe: A
[1] 0.7503156
------------------------------------------------------------------------------------------ 
examen$groupe: B
[1] NA
> ecartsTypes <- by(examen$note, examen$groupe, function(x) sd(x, na.rm = TRUE))
> print(ecartsTypes)
examen$groupe: A
[1] 0.7503156
------------------------------------------------------------------------------------------ 
examen$groupe: B
[1] 1.026574
```

Comme on le voit à la première tentative, le groupe B possède des valeurs nulles et donc il faut les enlever pour calculer l'écart-type.

### f) Que peut-on déduire en comparant les conclusions établies en c., d. et e. ?
Qu'en effet le groupe B à un plus grand écart entre les résultats des étudiants. Par contre puisque la médiane est plus élevée que le groupe A, ceci montre qu'il y a probablement des différences plus significatives entre les élèves.

### À votre avis, entre les boîtes à moustaches en parallèle et le graphique tracé ci-dessus, lequel est le plus approprié ?
Le graphique semble plus adapté car même s'il ne montre pas autant clairement les pourcentiles, il démontre plus de granularité.
Nous pouvons par exemple voir que dans les deux groupes il y a un creux dans la grande masse de valeurs, c'est donc une répartition bimodale pour les deux.
Le graphique donne une meilleure vision globale du niveau des étudiants.

## Exercice 4

### a)
```
install.packages("ggplot2","arules", type = "source")
library(ggplot2)
library(arules)
```

1. Pourquoi ce changement de nom de variable ?
	Car R peu comprendre les "-" comme des moins, il est donc plus sage de mettre des "_" dans les nom de variables.

### b) Calculer la proportion d’observations contenant des valeurs manquantes en utilisant les commandes ci-dessous.
```
dim(AdultUCI)
nrows<-nrow(AdultUCI)
n.missing<-rowSums(is.na(AdultUCI))
sum(n.missing>0)/nrows
```

```
> dim(AdultUCI)
[1] 48842    15
> nrows<-nrow(AdultUCI)
> n.missing<-rowSums(is.na(AdultUCI))
> sum(n.missing>0)/nrows
[1] 0.3824577
```

### c) En se basant sur les boîtes à moustaches en parallèle de la Figure 8, pour quel type de formation observe-t-on la plus grande dispersion du temps de travail ? Existe-t-il une différence entre les médianes des types de formation ? En donner brièvement la raison.
Le type de formation présentant la plus grande dispersion semble être `11th`.

En ce qui concerne les médianes, seulement `Doctorate` et `Prof-school` sont différentes et supérieurs à 40h par semaine.
Ceux-ci sont des types de travail qui sont reconnus pour être difficile mais aussi très occupant en terme de temps de travail.


### d) Pour chaque type de formation, on peut déterminer puis afficher à l’écran le temps maximal de travail hebdomadaire à l’aide des commandes
```
nx<-by(dframe$hours_per_week, dframe$education, max, na.rm=T)
nx
```

```
> nx<-by(dframe$hours_per_week, dframe$education, max, na.rm=T)
> nx
dframe$education: Preschool
[1] 75
------------------------------------------------------------------------------------------ 
dframe$education: 1st-4th
[1] 96
------------------------------------------------------------------------------------------ 
dframe$education: 5th-6th
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: 7th-8th
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: 9th
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: 10th
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: 11th
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: 12th
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: HS-grad
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: Prof-school
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: Assoc-acdm
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: Assoc-voc
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: Some-college
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: Bachelors
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: Masters
[1] 99
------------------------------------------------------------------------------------------ 
dframe$education: Doctorate
[1] 99
```

### La formation pour laquelle un temps maximal a été observé se détermine par les commandes suivantes, est-ce surprenant?
```
max(nx)
names(nx)[nx==max(nx)]
```

```
> max(nx)
[1] 99
> names(nx[nx==max(nx)])
 [1] "5th-6th"      "7th-8th"      "9th"          "10th"         "11th"         "12th"         "HS-grad"     
 [8] "Prof-school"  "Assoc-acdm"   "Assoc-voc"    "Some-college" "Bachelors"    "Masters"      "Doctorate"
```

Non ce n'est pas particulièrement surprenant car dans tout domaine il y a une ou plusieurs personnes qui travail un nombre d'heures aberrant.
Ce qui est plus surprenant c'est qu'il y aie une formation avec autant de différence, c'est à dire `Preschool` avec "seulement" 75h par semaine.

### e) En s’inspirant des commandes utilisées ci-dessus, déterminer la formation pour laquelle la distribution des temps de travail se caractérise par le plus petit écart-type.
```
> MinSd<-by(dframe$hours_per_week, dframe$education, sd, na.rm=T)
> MinSd
dframe$education: Preschool
[1] 11.434
------------------------------------------------------------------------------------------ 
dframe$education: 1st-4th
[1] 12.22667
------------------------------------------------------------------------------------------ 
dframe$education: 5th-6th
[1] 11.37219
------------------------------------------------------------------------------------------ 
dframe$education: 7th-8th
[1] 14.56277
------------------------------------------------------------------------------------------ 
dframe$education: 9th
[1] 11.46478
------------------------------------------------------------------------------------------ 
dframe$education: 10th
[1] 13.91801
------------------------------------------------------------------------------------------ 
dframe$education: 11th
[1] 13.9968
------------------------------------------------------------------------------------------ 
dframe$education: 12th
[1] 12.62027
------------------------------------------------------------------------------------------ 
dframe$education: HS-grad
[1] 11.42384
------------------------------------------------------------------------------------------ 
dframe$education: Prof-school
[1] 14.98344
------------------------------------------------------------------------------------------ 
dframe$education: Assoc-acdm
[1] 12.1991
------------------------------------------------------------------------------------------ 
dframe$education: Assoc-voc
[1] 10.94331
------------------------------------------------------------------------------------------ 
dframe$education: Some-college
[1] 12.79618
------------------------------------------------------------------------------------------ 
dframe$education: Bachelors
[1] 11.42306
------------------------------------------------------------------------------------------ 
dframe$education: Masters
[1] 12.14094
------------------------------------------------------------------------------------------ 
dframe$education: Doctorate
[1] 14.9196
> min(MinSd)
[1] 10.94331
> names(MinSd[MinSd==min(MinSd)])
[1] "Assoc-voc"
```

C'est donc la formation `Assoc-voc` qui a le plus petit écart-type avec 10.94331.

### f) Observe-t-on un résultat similaire en utilisant l’étendue interquartiles à l’aide de la fonction IQR()?
```

> MinIqr<-by(dframe$hours_per_week, dframe$education, IQR, na.rm=T)
> MinIqr
dframe$education: Preschool
[1] 10
------------------------------------------------------------------------------------------ 
dframe$education: 1st-4th
[1] 5
------------------------------------------------------------------------------------------ 
dframe$education: 5th-6th
[1] 0
------------------------------------------------------------------------------------------ 
dframe$education: 7th-8th
[1] 5
------------------------------------------------------------------------------------------ 
dframe$education: 9th
[1] 3.25
------------------------------------------------------------------------------------------ 
dframe$education: 10th
[1] 10
------------------------------------------------------------------------------------------ 
dframe$education: 11th
[1] 20
------------------------------------------------------------------------------------------ 
dframe$education: 12th
[1] 15
------------------------------------------------------------------------------------------ 
dframe$education: HS-grad
[1] 3
------------------------------------------------------------------------------------------ 
dframe$education: Prof-school
[1] 15
------------------------------------------------------------------------------------------ 
dframe$education: Assoc-acdm
[1] 5
------------------------------------------------------------------------------------------ 
dframe$education: Assoc-voc
[1] 5
------------------------------------------------------------------------------------------ 
dframe$education: Some-college
[1] 7.75
------------------------------------------------------------------------------------------ 
dframe$education: Bachelors
[1] 10
------------------------------------------------------------------------------------------ 
dframe$education: Masters
[1] 10
------------------------------------------------------------------------------------------ 
dframe$education: Doctorate
[1] 15
> min(MinIqr)
[1] 0
> names(MinIqr[MinIqr==min(MinIqr)])
[1] "5th-6th"
```

Le résultat est maintenant différent car l'étendue interquartiles minimum est de 0 pour le type `5th-6th`.

## Exercice 5

