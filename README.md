# 🗺 Carte des Élus du Bas-Rhin (67)

Une webapp cartographique interactive recensant l'ensemble des élus du département du Bas-Rhin, construite à partir des données officielles du portail de l'État français.

![HTML](https://img.shields.io/badge/HTML-single%20file-blue)
![Leaflet](https://img.shields.io/badge/Leaflet-1.9.4-green)
![Données](https://img.shields.io/badge/Données-data.gouv.fr-red)
![Licence](https://img.shields.io/badge/Licence-Ouverte%20v2.0-lightgrey)

---

## 📸 Fonctionnalités

- **Carte interactive** centrée sur le Bas-Rhin, fond sombre CartoDB
- **511 communes** géolocalisées via l'API officielle `geo.api.gouv.fr`
- **Code couleur par type de mandat** (voir légende ci-dessous)
- **Panneau latéral** au clic sur une commune : maire, adjoints, conseillers municipaux, EPCI
- **Badge nuance politique** du maire (communes > 1 000 hab.), source Ministère de l'Intérieur 2026
- **Panneau supra-communal** : conseillers départementaux, sénateurs, députés, conseillers régionaux
- **Barre de recherche** par nom de commune ou de maire
- **100% autonome** : un seul fichier HTML, aucun serveur requis

---

## 🎨 Code couleur

| Couleur | Mandat |
|---------|--------|
| 🔵 Bleu | Maires & Conseillers municipaux |
| 🟠 Orange | Conseillers communautaires (EPCI) |
| 🟣 Violet | Députés |
| 🟢 Vert | Sénateurs |
| 🟤 Brun | Conseillers départementaux (CEA) |
| 🟡 Jaune | Conseillers régionaux (Grand Est / section Alsace) |

---

## 📂 Sources de données

Toutes les données sont issues du portail open data de l'État français ([data.gouv.fr](https://www.data.gouv.fr)), sous **Licence Ouverte / Open Licence v2.0**.

| Fichier | Source |
|---------|--------|
| `elus-maires-mai.csv` | [Répertoire national des élus — Maires](https://www.data.gouv.fr/datasets/repertoire-national-des-elus-1/) |
| `elus-conseillers-municipaux-cm.csv` | [RNE — Conseillers municipaux](https://www.data.gouv.fr/datasets/repertoire-national-des-elus-1/) |
| `elus-conseillers-communautaires-epci.csv` | [RNE — Conseillers communautaires](https://www.data.gouv.fr/datasets/repertoire-national-des-elus-1/) |
| `elus-conseillers-departementaux-cd.csv` | [RNE — Conseillers départementaux](https://www.data.gouv.fr/datasets/repertoire-national-des-elus-1/) |
| `elus-conseillers-regionaux-cr.csv` | [RNE — Conseillers régionaux](https://www.data.gouv.fr/datasets/repertoire-national-des-elus-1/) |
| `elus-deputes-dep.csv` | [RNE — Députés](https://www.data.gouv.fr/datasets/repertoire-national-des-elus-1/) |
| `elus-senateurs-sen.csv` | [RNE — Sénateurs](https://www.data.gouv.fr/datasets/repertoire-national-des-elus-1/) |
| `municipales-2026-candidatures-tour-1.csv` | [Candidatures municipales 2026 — Ministère de l'Intérieur](https://www.data.gouv.fr/datasets/elections-municipales-2026-listes-candidates-au-premier-tour/) |

**Coordonnées géographiques** : API [geo.api.gouv.fr](https://geo.api.gouv.fr) (chargement à la volée côté client).

---

## 🚀 Utilisation

### Option 1 — Ouvrir directement dans le navigateur

```bash
# Cloner le dépôt
git clone https://github.com/votre-user/elus-bas-rhin.git
cd elus-bas-rhin

# Ouvrir dans le navigateur (connexion internet requise)
open elus_bas_rhin_67.html   # macOS
xdg-open elus_bas_rhin_67.html   # Linux
```

> ⚠️ Une **connexion internet** est nécessaire pour charger les tuiles de fond de carte (CartoDB) et les coordonnées GPS (geo.api.gouv.fr). Les données électorales sont entièrement embarquées dans le fichier HTML.

### Option 2 — Serveur local

```bash
python3 -m http.server 8000
# Puis ouvrir http://localhost:8000/elus_bas_rhin_67.html
```

---

## 🏗 Architecture technique

Le projet est intentionnellement **mono-fichier** pour la portabilité maximale.

```
elus_bas_rhin_67.html
├── <style>        CSS inline (thème sombre, responsive)
├── <script src>   Leaflet 1.9.4 (CDN)
├── const ELUS     Données électorales JSON embarquées (~490 KB)
├── const NUANCES  Nuances politiques 2026 (54 communes)
└── <script>       Logique carte, panneau, recherche
```

**Stack** : HTML5 · CSS3 · JavaScript vanilla · [Leaflet.js](https://leafletjs.com/)

**Pas de framework, pas de build, pas de dépendances npm.**

---

## ⚠️ Limites connues

- **Nuances politiques** : disponibles uniquement pour les communes de plus de 1 000 habitants (~54 communes sur 511). Le Ministère de l'Intérieur ne renseigne pas les nuances pour les petites communes — c'est une limite structurelle de la source officielle, non un manque de traitement.
- **Conseillers régionaux** : la section Alsace (code `6AE`) couvre les deux départements 67 et 68 indistinctement depuis la création de la Collectivité Européenne d'Alsace.
- **Données RNE** : le Répertoire National des Élus est mis à jour en continu par le Ministère de l'Intérieur. Les données embarquées correspondent à la version téléchargée en mai 2026.
- **Eurodéputés** : non inclus, car ils n'ont pas d'ancrage territorial départemental.

---

## 🔄 Mise à jour des données

Pour régénérer le fichier HTML avec des données fraîches :

1. Télécharger les nouveaux fichiers CSV depuis [data.gouv.fr/datasets/repertoire-national-des-elus-1](https://www.data.gouv.fr/datasets/repertoire-national-des-elus-1/)
2. Lancer le script de génération :

```bash
pip install pandas
python3 generate.py   # à venir
```

> 💡 Un script `generate.py` standalone est prévu pour automatiser la génération à partir des CSV bruts.

---

## 📄 Licence

Les **données** sont sous [Licence Ouverte / Open Licence v2.0](https://www.etalab.gouv.fr/licence-ouverte-open-licence) — Etalab / data.gouv.fr.

Le **code** de la webapp est libre d'utilisation et de modification.
