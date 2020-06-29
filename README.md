## Consommation d'électricité par secteur d'activite.

### Donnees [Open Data AgenceORE]([https://opendata.agenceore.fr/explore/dataset/conso-elec-gaz-annuelle-par-secteur-dactivite-agregee-commune/export/](https://opendata.agenceore.fr/explore/dataset/conso-elec-gaz-annuelle-par-secteur-dactivite-agregee-commune/export/)

Traitement et representation graphique.

- Creation du DataFrame principal
  
  ```python
  import pandas as pd
  
  elec = pd.read_csv('conso-elec.csv', encoding='utf_8', sep=';')
  ```

- Creation d'un DataFrame base sur _elec_ ne comportant que la consommation par secteur
  
  ```python
  conso_secteur = elec.filter(like='Consommation', axis=1).astype(int)
  ```
  
  - On calcule les totaux de consommation pour chaque secteur et on extrait les etiquettes ou _labels_
  
  ```python
  secteurs = [nom_colonne for nom_colonne in conso_secteur.columns]
  
  totaux = []
  for colonne in conso_secteur:
      total_secteur = conso_secteur[colonne].sum()
      totaux.append(total_secteur)
  ```

- Les ettiquettes pretes et les totaux calcules, on peut rapidement visualiser la part d'electricite dediee a chaque secteur d'activite
  
  ```python
  import matplotlib.pyplot as plt
  
  exp = [0, 0, 0, 0.1, 0]
  plt.figure(dpi=110)
  plt.pie(totaux, labels=secteurs, explode=exp, autopct='%1.1f%%', startangle=0, labeldistance=1.4, pctdistance=0.8)
  plt.axis('equal')
  plt.show()
  ```
  
  - On obtient la figure suivante. Elle represente la consommation en MWh de chaque secteur d'activite, de 2011 a 2018

![image](/home/alexis/Desktop/IPython/Electricite/Open Data - ORE/camembert.png)

---

### Nous souhaitons a present voir l'evolution de la consommation par an

- On groupe le _DataFrame_ et on additionne les lignes
  
  ```python
  par_an = elec.groupby('Année').sum()
  par_an.reset_index(inplace=True)
  ```

- Liste des annees pour l'axe _x_ du graphique
  
  ```python
  annees = [ligne for ligne in par_an['Année']]
  ```

- Rendre plus lisibles les totaux de consommation energetique
  
  ```python
  totaux_annee = [total for total in par_an['consototale'].astype(int) / 10**6]
  ```

- On trace notre graphique
  
  ```python
  plt.figure(dpi=150)
  plt.bar(annees, totaux_annee)
  plt.xticks(annees, rotation='90', fontsize=8)
  plt.ylabel('Consommation totale tous secteurs en TWh')
  plt.title('Consommation totale d\'electricite par an')
  plt.show()
  ```
  
  On obtient une figure de la facon suivante

![image](/home/alexis/Desktop/IPython/Electricite/Open Data - ORE/barres.png)

---

### Faisons cela un peu plus interessant en representant l'evolution par an _et_ par secteur

- Filtrer les colonnes qui nous interessent pas (pour le moment)
  
  ```python
  par_an_et_secteur = par_an.filter(like='Consommation', axis=1)
  ```

- Et on trace le graphique
  
  ```python
  par_an_et_secteur.plot(kind='bar', stacked=True, figsize=(15,10), rot=0, colormap='inferno')
  ```
  
  - On obtient

![image](/home/alexis/Desktop/IPython/Electricite/Open Data - ORE/barres2.png)


