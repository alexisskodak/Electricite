### Etude de la consommation d'electricite par domaine en 2017

Probleme rencontre:

lors de la creation d'un _sub dataframe_ qui comprend seulement les colonnes correspondant a la consommation par secteur (Agriculture, Industrie, Tertiaire, Residentiel, Autres. Respectivement: CONSOA, CONSOI, CONSOT, CONSOR, CONSONA)
on s'appercoit que nos resultats semblent incoherents par rapport a d'autres analyses (par exemple celle de EDF). En effet:

- En creeant le jeu de donnes:

```python
conso = pd.DataFrame()
conso = elec2017[['CONSOA', 'CONSOI', 'CONSOT', 'CONSOR', 'CONSONA']]
conso.head(5)
```

- Et en calculant ses totaux respectifs (en MWh)

```python
total_a = conso['CONSOA'].sum()
total_i = conso['CONSOI'].sum()
total_t = conso['CONSOT'].sum()
total_r = conso['CONSOR'].sum()
total_n = conso['CONSONA'].sum()

totaux = [total_a, total_i, total_t, total_r, total_n]
total_ensemble = 0
for total in totaux:
    total_ensemble += total
    
def get_relatifs(liste, T):
    relatifs = []
    for element in liste:
        element = element * 100 / T
        relatifs.append(element)
    return relatifs
```

On obtient le graphique suivant

![image](graph2017.png)

Le premier reflexe a ete de verifier la methodologie pour obtenir ces chiffres, mais en verifiant les montants obtenus, on a:

```python
totaux_dict = dict(zip(domaines, totaux))
totaux_dict
```

Output: 

```
{'agriculture': 3572605,
 'industrie': 1192,
 'tertiaire': 1290562,
 'residentiel': 23718681830,
 'autre': 32716187830}
```

Ce qui correspond bien aux totaux par colonnes.

### Hypotheses:

- Nous avons mal manipule le jeu de donnees
  
  - TODO: Se renseigner sur la signifcation des variables aupres de la source [Statistiques developement durable]([https://www.statistiques.developpement-durable.gouv.fr/](https://www.statistiques.developpement-durable.gouv.fr/)

- Le jeu de donnes 2017 possede une anomalie
  
  - TODO: Verifier aupres d'autres reutilisations dans Data Gouv

- Le jeu de donnes 2017 n'a pas d'anomalies, et nous l'avons bien manipule. Il represente tout simplement une annee en dehours du commun en termes de la quantite d'electricite consomme par secteur d'activite.
  
  - TODO: Dans ce cas la, nous allons creer un DataFrame pour chaque anne, en tirer les moyennes, les mediannes, les deviations et les totaux individuels et historiques
  
  
