# Dataviz2

```python
#modification du dossier par défaut
import os
os.chdir("C:/home/etienne/dataviz2")
```

```python
#importation des données
import pandas
fromage = pandas.read_table("fromage2.txt",sep="\t",header=0,index_col=0)
```
```python
#dimension des données
print(fromage.shape)
```
(29,9)
> 29 fromages et 9 propriétés. Ceci correspond a l'envergure des données
```python
#6 premières lignes des données
print(fromage.iloc[0:6,:])
```
|             | calories | sodium | calcium | lipides | retinol | folates | proteines |     |     |
|-------------|----------|--------|---------|---------|---------|---------|-----------|-----|-----|
| Fromages    |          |        |         |         |         |         |           |     |     |
| CarredelEST | 314      | 353.5  | 72.6    | 26.3    | 51.6    | 30.3    | 21.0      | 70  | 20  |
| Babybel     | 314      | 238.0  | 209.8   | 25.1    | 63.7    | 6.4     | 22.6      | 70  | 27  |
| Beaufort    | 401      | 112.0  | 259.4   | 33.3    | 54.9    | 1.2     | 26.6      | 120 | 41  |
| Bleu        | 342      | 336.0  | 211.1   | 28.9    | 37.1    | 27.5    | 20.2      | 60  | 20  |
| ...         | ...      | ...    | ...     | ...     | ...     | ...     | ...       | ... | ... |
>Cette fois ci nous avons les noms des fromages
```python
#statistiques descriptives
print(fromage.describe())
```
![](https://github.com/EPradillon/Dataviz2/blob/main/describe.png)
```python
import pandas
```
```python
#centrage réduction des données
from sklearn import preprocessing
fromage_cr = preprocessing.scale(fromage)
```
```python
#librairies pour la CAH
import matplotlib.pyplot as plt
from scipy.cluster.hierarchy import dendrogram, linkage, fcluster
```
```python
#générer la matrice des liens
Z = linkage(fromage_cr,method='ward',metric='euclidean')
```
```python
plt.title("CAH")
dendrogram(Z,labels=fromage.index,orientation='left',color_threshold=0)
plt.show()
```
![](https://github.com/EPradillon/Dataviz2/blob/main/nocolor.png)
> On peut imaginer un découpage en 4 groupe a partir du dendrogramme
> On s'apperçoit que rapidement un groupe de fromage est dissocié
```python
#matérialisation des 4 classes (hauteur t = 7)
plt.title('CAH avec matérialisation des 4 classes')
dendrogram(Z,labels=fromage.index,orientation='left',color_threshold=7)
plt.show()
```
![](https://github.com/EPradillon/Dataviz2/blob/main/color.png)
> Finalement le regroupement nécéssaire pour inclure les fromages frais avec le reste des fromages laisse penser qu'ils ne sont pas associabes
```python
#découpage à la hauteur t = 7 ==> 4 identifiants de groupes obtenus
groupes_cah = fcluster(Z,t=7,criterion='distance')
print(groupes_cah)
```
`[4 4 3 4 2 4 2 2 4 3 4 4 3 2 4 1 1 4 4 3 1 4 4 4 4 4 4 4 1]`
Si on associe les groupes identifiés avec chacun des fromages; on peut se rendre compte que les fromages sont associés par types. Les fromages frais / les fromages à pates moles et les fromages a pate dure
