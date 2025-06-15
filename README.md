## 1. Compréhension des affaires  

**Contexte** :  
L'entreprise prévoit d'acquérir 50 avions d'occasion pour démarrer son activité.  
**Problématique** : 23% des avions de plus de 20 ans sont impliqués dans des accidents graves (source : NTSB).  

**Livrables** :  
-  Classement des modèles par taux de sécurité  
-  Liste des pièces à inspecter prioritairement  
-  3 recommandations stratégiques

  **Objectif** :  
Transformer la colonne des dates en format datetime pour permettre des analyses temporelles précises.

**Gestion des erreurs** :  
Les dates invalides seront converties en `NaT` (Not a Time) grâce à `errors='coerce'`
#  Questions d'Analyse Clés

## 1. Compagnies et Modèles à Risque  
**Objectif** :  
Identifier les acteurs aériens présentant les taux d'accidents graves les plus élevés.  

**Méthodologie** :  
- Filtrage des accidents avec victimes (`Total.Fatal.Injuries > 0`)  
- Agrégation par compagnie (`Air.Carrier`) et modèle (`Model`)  
- Visualisation par diagrammes barres horizontaux  

**Attendu** :  
```python
top_compagnies = df_graves['Air.Carrier'].value_counts().head(10)
top_modeles = df_graves['Model'].value_counts().head(10)
```

---

## 2. Saisonnalité des Accidents  
**Objectif** :  
Détecter les périodes de l'année à risque accru.  

**Approche** :  
- Extraction du mois (`Event.Date.dt.month`)  
- Analyse de fréquence par mois  
- Visualisation par diagramme circulaire  

**Exemple** :  
```python
accidents_par_mois = df_clean['Event.Date'].dt.month.value_counts()
```

---

## 3. Taux de Mortalité  
**Objectif** :  
Calculer la proportion d'accidents entraînant au moins un décès.  

**Formule** :  
$$
\text{Taux mortalité} = \frac{\text{Nb accidents avec décès}}{\text{Nb total accidents}} \times 100
$$

**Implémentation** :  
```python
taux_mortel = (df_clean['Total.Fatal.Injuries'] > 0).mean() * 100
```

---

## 4. Zones Géographiques Sensibles  
**Objectif** :  
Localiser les pays où la fréquence d'accidents est anormalement élevée.  

**Indicateurs** :  
- Nombre d'accidents par pays (`Country`)  
- Taux d'accidents par million de vols (si données disponibles)  

**Visualisation** :  
```python
plt.figure(figsize=(12,6))
df_clean['Country'].value_counts().head(15).plot(kind='barh')
```

## Identifier les périodes de l'année présentant la plus forte occurrence d'accidents aériens.
