# Laboratoire 1 PST

## Exercice 2

### a)

1. Quels sont les effets de la commande `boxplot(cpus, xlab="performance relative", col="darkslategray4", horizontal=T)` ?
	Cette commande à pour effet de générer un boxplot horizontal des données avec comme légende de l'axe x `performance relative` et avec la couleur du graphique en gris.
2. Quel est l'effet de la fonction `rug()` ?
	Cette commande ajoute des graduations à l'axe horizontal pour représenter la densité des valeurs.
	
### b) Commenter la distribution des valeurs observées en se basant sur les graphiques de la Figure 5 : valeur(s) atypique(s), asymétrie

### c) Calculer la performance relative médiane et la performance relative moyenne des valeurs observées en utilisant les fonctions de R adéquates.
### Est-il plus approprié d’utiliser la médiane ou la moyenne ?

### d) Déterminer le(s) mode(s) des valeurs observées à l’aide des commandes suivantes :
```
n.cpus<-table(cpus)
as.numeric(names(n.cpus)[n.cpus==max(n.cpus)])
```

### e) Que fait la commande `summary(cpus)` ?

### f) Décrire l’effet sur la moyenne et sur la médiane des trois interventions suivantes :

1. ajouter un processeur de performance relative 43;

2. soustraire 9 à chaque valeur observée;

3. diviser chaque observation par 3.

### g) Calculer l’écart-type des performances relatives une fois avec les valeurs atypiques et une fois sans en utilisant la fonction sd(). Les valeurs atypiques peuvent être déterminées à l’aide de la fonction boxplot() avec plot=FALSE comme argument.
### Que constate-t-on ? L’écart-type est-il un indicateur robuste ?