# ☁️ Jour 8 / 10 — Power BI : Power BI Service & Collaboration

> **Série : 10 Days of Power BI** · Jour 8/10  
> Concepts : Publication · Espaces de travail · Partage · Alertes · Abonnements · Applications Power BI

---

## 📁 Fichiers du projet

```
day-08-service/
│
├── service_powerbi_j8.xlsx
│   ├── Ventes       ← 294 transactions
│   ├── Produits     ← 5 produits
│   ├── Clients      ← 30 clients
│   ├── Vendeurs     ← 5 vendeurs avec email
│   └── Calendrier   ← 366 jours 2024
└── README.md
```

---

## 🚀 ÉTAPE 1 — Préparer le rapport dans Power BI Desktop

```
1. Obtenir des données → Excel → service_powerbi_j8.xlsx
2. Cocher les 5 feuilles → Charger
3. Vue Modèle → créer les 4 relations :
   Produits[ID Produit] → Ventes[ID Produit]
   Clients[ID Client]   → Ventes[ID Client]
   Vendeurs[ID Vendeur] → Ventes[ID Vendeur]
   Calendrier[Date]     → Ventes[Date]
4. Marquer Calendrier comme table de dates
```

### Mesures DAX

```dax
CA Total        = SUM(Ventes[Montant])
Marge Brute     = SUM(Ventes[Montant]) - SUM(Ventes[Coût Total])
Taux de Marge   = DIVIDE([Marge Brute], [CA Total], 0)
Nb Transactions = COUNTROWS(Ventes)
Atteinte Obj    = DIVIDE([CA Total], SUM(Vendeurs[Objectif Annuel]), 0)
CA YTD          = TOTALYTD([CA Total], Calendrier[Date])
```

### Construire 3 pages

```
Page 1 "Vue Globale" :
→ 4 KPI cards : CA Total · Marge Brute · Taux de Marge · Nb Transactions
→ Graphique courbe : évolution mensuelle CA
→ Histogramme : CA par Catégorie
→ Segment : Trimestre

Page 2 "Performance Vendeurs" :
→ Tableau : Vendeurs[Nom] · CA Total · Atteinte Obj · Nb Transactions
→ Jauge : Atteinte Objectif globale
→ Barres : CA par Vendeur avec couleur conditionnelle

Page 3 "Analyse Produits" :
→ Matrice : Produits[Nom] × Calendrier[Trimestre] → CA Total
→ Anneau : Répartition par Catégorie
→ KPI : Taux de Marge vs objectif 35%
```

---

## 🔑 ÉTAPE 2 — Publier sur Power BI Service

```
Prérequis : compte Microsoft (compte scolaire ou pro)
            → Si pas de compte : créer un compte d'essai sur
              https://app.powerbi.com/signupAuthorize

Dans Power BI Desktop :
1. Sauvegarder : Fichier → Enregistrer sous → rapport_j8.pbix
2. Accueil → Publier
3. Choisir : "Mon espace de travail"
4. Cliquer Publier
5. Cliquer le lien → ouvre app.powerbi.com dans le navigateur
```

---

## 🔑 ÉTAPE 3 — Explorer Power BI Service

```
Sur app.powerbi.com :

Panneau gauche :
→ Accueil          : rapports récents et recommandés
→ Mon espace de travail : tes rapports et jeux de données
→ Créer            : nouveau rapport depuis une source
→ Applications     : applications publiées par ton org

Dans "Mon espace de travail" tu trouveras :
→ rapport_j8       : le rapport (fichier .pbix)
→ service_powerbi_j8 : le jeu de données (la source de données)
```

---

## 🔑 ÉTAPE 4 — Créer un espace de travail partagé

```
(Nécessite une licence Power BI Pro ou Premium Per User)

Panneau gauche → Espaces de travail → Créer un espace de travail

Nom : "Portfolio Data — Équipe Commerciale"
Description : "Rapports de performance commerciale 2024"

Ajouter des membres :
→ Membres → Entrer un email → choisir le rôle :
   Administrateur : accès total, peut modifier
   Membre         : peut modifier les rapports
   Contributeur   : peut publier du contenu
   Spectateur     : lecture seule

Déplacer le rapport :
→ Retourner dans "Mon espace de travail"
→ "..." à côté du rapport → Déplacer vers
→ "Portfolio Data — Équipe Commerciale"
```

---

## 🔑 ÉTAPE 5 — Partager un rapport

```
Méthode 1 — Partage direct :
→ Ouvrir le rapport sur Power BI Service
→ Partager (icône en haut à droite)
→ Entrer les emails des destinataires
→ Options :
   ✓ Autoriser les destinataires à partager ce rapport
   ✓ Autoriser les destinataires à créer du contenu avec les données
→ Envoyer

Méthode 2 — Lien de partage :
→ Partager → Copier le lien
→ "Personnes de votre organisation" → Appliquer
→ Copier et envoyer le lien par email ou Teams

Méthode 3 — Publication sur le web (public) :
→ Fichier → Incorporer le rapport → Publier sur le web
→ Créer le code incorporé
→ ⚠️ Données visibles par tout le monde — ne jamais utiliser
  avec des données sensibles
→ Idéal pour GitHub README : coller l'URL dans le README
```

---

## 🔑 ÉTAPE 6 — Épingler des visuels sur un tableau de bord

```
Sur le rapport dans Power BI Service :

1. Survoler un visuel → icône punaise 📌 en haut à droite
2. Cliquer → "Épingler sur le tableau de bord"
3. Choisir : "Nouveau tableau de bord"
   Nom : "Tableau de bord Commerciale 2024"
4. Épingler

Répéter pour les visuels clés :
→ Carte CA Total
→ Carte Taux de Marge
→ Courbe évolution mensuelle

Le tableau de bord se retrouve dans le panneau gauche :
→ Vision d'ensemble de plusieurs rapports sur une seule page
→ Mise à jour automatique quand les données changent
```

---

## 🔑 ÉTAPE 7 — Créer une alerte de données

```
Sur le tableau de bord :
→ Cliquer sur une carte KPI (ex : CA Total)
→ "..." → Gérer les alertes

Configurer l'alerte :
→ Ajouter une règle d'alerte
→ Condition : Au-dessus de
→ Seuil : 50000 (€)
→ Fréquence : Au plus une fois par heure
→ Notification : Email + notification Push (app mobile)
→ Enregistrer et fermer

→ Quand le CA dépasse 50 000€, une notification est envoyée automatiquement
```

---

## 🔑 ÉTAPE 8 — S'abonner à un rapport

```
Sur le rapport dans Power BI Service :
→ S'abonner (icône enveloppe en haut)

Configurer l'abonnement :
→ Nom : "Rapport hebdo CA"
→ Fréquence : Hebdomadaire → Lundi
→ Heure : 08:00
→ Page : Vue Globale
→ Inclure ma frise temporelle : ✓
→ Ajouter les emails des destinataires
→ Enregistrer

Résultat :
→ Chaque lundi à 8h, une capture du rapport est envoyée par email
→ Les destinataires n'ont pas besoin de compte Power BI
```

---

## 🔑 ÉTAPE 9 — Créer une Application Power BI

```
(Regroupe plusieurs rapports en une seule "app" pour les utilisateurs)

Dans l'espace de travail partagé :
→ Créer une application (bouton en haut à droite)

Configuration :
→ Onglet "Configuration" :
   Nom : "Performance Commerciale 2024"
   Description : "Rapport de suivi des ventes et des KPIs"
   Couleur : bleu (#0EA5E9)

→ Onglet "Navigation" :
   Ajouter les pages/rapports dans l'ordre voulu
   Renommer si besoin

→ Onglet "Autorisations" :
   Ajouter les utilisateurs ou groupes qui auront accès

→ Publier l'application

Accès des utilisateurs :
→ Ils reçoivent un email avec le lien
→ Ils voient l'app dans "Applications" sur Power BI Service
→ Interface simplifiée sans voir les options d'édition
```

---

## 💡 Récap — Power BI Service vs Power BI Desktop

| | Desktop | Service |
|---|---|---|
| **Usage** | Créer et modifier les rapports | Consulter, partager, collaborer |
| **Accès** | Application Windows locale | Navigateur web (app.powerbi.com) |
| **Partage** | Non | Oui |
| **Alertes** | Non | Oui |
| **Abonnements** | Non | Oui |
| **Actualisation auto** | Non | Oui (avec passerelle) |

---



---

⭐ **Si ce projet t'aide, mets une étoile !**
