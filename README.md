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
# Extraire le mois
df_clean['Month'] = df_clean['Event.Date'].dt.month

# Visualisation
plt.figure(figsize=(5,3))
df_clean['Month'].value_counts().sort_index().plot(kind='bar', color='skyblue')
plt.title("Nombre d'accidents par mois")
plt.xticks(range(12), ['Jan', 'Fév', 'Mar', 'Avr', 'Mai', 'Juin', 'Juil', 'Août', 'Sep', 'Oct', 'Nov', 'Déc'])
plt.show()
<div style="text-align: center;">
    <img src="data/Screenshot 2025-06-14 194724.png" width="400">
    <p style="font-style: italic; color: #7f8c8d;">
    </p>
</div>
 ## Taux d'accidents par million de vols
 taux_mortalite = (df_clean['Total.Fatal.Injuries'] > 0).mean() * 100
print(f" {taux_mortalite:.1f}% des accidents ont causé au moins un décès")

# Visualisation
plt.figure(figsize=(6,6))
plt.pie([taux_mortalite, 100-taux_mortalite], 
        labels=['Accidents mortels', 'Autres'], 
        colors=['#e74c3c', '#3498db'], 
        autopct='%1.1f%%')
plt.title("Proportion d'accidents mortels")
plt.show()
<div style="text-align: center;">
    <img src="data/Screenshot 2025-06-14 194900.png" width="400">
    <p style="font-style: italic; color: #7f8c8d;">
    </p>
</div>
plt.figure(figsize=(7,4))
sns.barplot(
    x=top_pays.values,
    y=top_pays.index,
    hue=top_pays.index,  # Ajout du paramètre hue
    palette='Reds_r',
    legend=False,        # Désactivation de la légende superflue
    dodge=False          # Empêche le décalage des barres
)
plt.title("Pays avec le plus d'accidents aériens (1962-2023)", pad=20)
plt.xlabel("Nombre d'accidents", labelpad=10)
plt.ylabel("")  # Supprime le label Y inutile
sns.despine(left=True)  # Supprime le cadre gauche
plt.tight_layout()
plt.show()
<div style="text-align: center;">
    <img src="data/Screenshot 2025-06-14 195216.png" width="400">
    <p style="font-style: italic; color: #7f8c8d;">
    </p>
