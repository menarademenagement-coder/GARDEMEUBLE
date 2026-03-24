# 📦 AMANBOX – Système de Gestion Garde-Meuble

## 🏢 À propos
**Amanbox** est une application web complète de gestion de garde-meuble (self-stockage). Elle couvre l'ensemble du cycle de vie client : création des boxes, contrats avec signature électronique double partie, paiements, factures, relances automatisées et tableaux de bord.

---

## ✅ Fonctionnalités implémentées

### 🔐 Authentification (`index.html`)
- **Accès rapide** (sans email) via boutons Admin / Client
- Connexion classique par email + mot de passe
- Redirection automatique selon le rôle
- Session persistante via `localStorage`
- Comptes de démonstration intégrés

---

### 🖥️ Interface Administrateur (`admin.html`)

#### 📊 Dashboard
- Statistiques en temps réel (boxes, clients, CA mensuel, retards)
- Graphique d'occupation des boxes (doughnut Chart.js)
- Graphique des revenus mensuels (6 derniers mois)
- Liste des paiements en attente et contrats récents

#### 📦 Gestion des Boxes
- Vue grille et vue liste commutables
- Filtres par statut, type, recherche libre
- CRUD complet avec photo (drag & drop base64)
- Indicateurs visuels climatisation / sécurité
- Statuts : disponible, occupée, maintenance, réservée

#### 👥 Gestion des Clients
- Répertoire avec colonnes : nom, box, hub, dates, prix, caution, frais reliv., prochain paiement, statut RP
- Filtres : statut, en règle / retard de paiement, recherche libre
- CRUD complet avec photo pièce d'identité
- Fiche client détaillée (popup) : résumé financier, contrats, photo CNI
- Export CSV

#### 📃 Gestion des Contrats
- Création de contrats : client + box + hub + avec/sans caution + frais relivraison
- Signature électronique admin (pad canvas souris/tactile)
- Visualisation détaillée avec signatures
- Résilier un contrat → libère automatiquement la box
- Filtres : statut, état de signature, recherche libre

#### ⚠️ Recouvrement critique (ex-"État de recouvrement")
- **Vue liste** : tableau groupé par client — uniquement les factures `en_retard`
- Colonnes : client, nb mois retard (badge rouge), total TTC, échéance, jours retard, statut, **dernière relance** (cliquable)
- Clic sur une ligne → **page détail recouvrement** (4 paragraphes pleine page, sans popup)

#### 📋 Page détail recouvrement (au clic client)
- **§1 Informations générales** : avatar rouge, nom complet, email, badge actif/inactif, téléphone, ville, adresse ; KPIs retard (mois en retard, total dû, jours max)
- **§2 Informations de stockage** : box & hub, volume, N° contrat, dates, durée min., statut contrat ; tarification (HT/TVA/TTC/caution/frais reliv.) ; signatures client & admin ; jour prélèvement
- **§3 Recouvrement critique** : tableau avec case à cocher par ligne, mois + N° facture, montant TTC (rouge) avec détail HT+TVA, date d'échéance, badge jours retard (code couleur 🟡/🔴/🟥) ; boutons "Voir facture" et "Encaisser" ; "Facture groupée" (2+ cochés) ; "Encaisser tout" ; retour liste ; colonne "Dernière relance" cliquable vers §4
- **§4 Relances & Feedbacks** : formulaire de prise de contact (date/heure auto, moyen : appel/email/WhatsApp/SMS/visite, retour client, statut suivi, promesse de paiement) ; timeline historique avec icônes, badges et suppression ; sauvegarde en base `relances_contacts`
- Correspondance robuste client : par `client_id` **OU** `client_nom` (fallback pour données sans UUID)

#### 📅 Recouvrement du mois (page dédiée)
- **Page autonome** accessible via la sidebar (section "Recouvrement")
- **Liste fictive de démonstration** (24 entrées) si moins de 5 données réelles ce mois
- **5 KPIs** : nombre de paiements, encaissé, en attente, en retard, total à encaisser
- **Barre de progression tricolore** : vert/jaune/rouge avec % encaissé
- **Filtres** : Tous / En attente / Payé / En retard + recherche + tri
- **Colonnes** : Client (avatar, réf), Box, Période, HT, TVA, TTC, Échéance, **⏱ Paiement dû dans** (badge coloré), Statut, Actions
- **Colonne délai** : `dans Xj` (gris), `≤5j` (jaune), `Demain` (orange), `Aujourd'hui!` (clignotant), `Xj de retard` (rouge), `Xj critique` (bordeaux)
- Cases à cocher + **encaissement groupé**
- **Export CSV** avec BOM UTF-8
- Lignes colorées : vert clair = payé, rouge clair = retard (bordure gauche)

#### 🔔 Relances & Recouvrement
- Onglet Impayés : liste des factures en retard avec calcul jours, niveau auto (1/2/3)
- Onglet Historique : toutes les relances avec messages complets
- Génération automatique des relances pour tous les impayés
- Clôturer une relance
- Messages pré-remplis par niveau

#### 📈 Statistiques & Rapports
- Taux d'occupation, CA annuel estimé, total impayés, clients actifs
- Graphiques : répartition par type de box, paiements par statut
- Évolution des revenus sur 12 mois (courbe)
- Top 5 clients par loyer mensuel
- Export CSV des indicateurs clés

#### 📜 Journal d'activité
- Timeline chronologique de toutes les actions
- Icônes et codes couleur par type d'action

---

### 👤 Interface Client (`client.html`)

#### 🏠 Accueil
- Tableau de bord personnalisé (boxes actives, prochain paiement, factures impayées)
- Bannière d'alerte si paiements en retard
- Liste boxes actives + dernières factures

#### 📦 Mes Boxes
- Cartes des boxes louées (photo, type, étage, prix, clim, sécurité)
- Lien direct vers le contrat, badge "À signer"

#### 📃 Mes Contrats
- Liste complète des contrats avec toutes les conditions
- **Signature électronique client** (pad canvas)
- Détail : loyer HT/TTC, caution, jour prélèvement, frais relivraison

#### 💳 Mes Paiements
- Tableau de toutes les factures avec filtres (statut, mois)
- Affichage HT / TVA / TTC

#### 🔔 Mes Relances
- Liste des relances reçues (marquées automatiquement comme vues)
- Message complet avec montant dû
- Lien direct vers les paiements

#### 🔍 Boxes Disponibles
- Catalogue filtrable (type, climatisation, recherche)
- Bouton "Je suis intéressé(e)" → enregistre une demande dans l'historique

#### 👤 Mon Profil
- Modification des informations personnelles
- Changement de mot de passe sécurisé
- Affichage pièce d'identité enregistrée

---

## 🗂️ Structure des fichiers

```
index.html          → Page de connexion (accès rapide + formulaire)
admin.html          → Interface administrateur (8 sections)
client.html         → Interface client (7 sections)
css/
  style.css         → Styles globaux complets + responsive + print
js/
  utils.js          → Auth, API, Toast, Modal, Confirm, SignaturePad, Fmt, CSV...
  admin.js          → Logique métier admin complète
  client.js         → Logique métier client complète
```

---

## 🔗 Pages & Navigation

| URL | Description | Accès |
|-----|-------------|-------|
| `index.html` | Page de connexion | Public |
| `admin.html` | Espace administrateur | Admin uniquement |
| `client.html` | Espace client | Client uniquement |

### Sidebar Admin
| Section | ID page | Description |
|---------|---------|-------------|
| Dashboard | `dashboard` | Statistiques temps réel |
| Statistiques | `stats` | Rapports & graphiques |
| Boxes | `boxes` | Gestion des espaces de stockage |
| Contrats | `contrats` | Contrats & signatures |
| Clients | `clients` | Répertoire clients |
| Recouvrement | `paiements` | Factures en retard (Recouvrement critique) |
| Recouvrement du mois | `recouvrement-mois` | Tous les paiements du mois courant |
| Relances | `relances` | Impayés & historique des relances |
| Activité | `historique` | Journal des actions |

### Sidebar Client
| Section | ID page |
|---------|---------|
| Accueil | `accueil` |
| Mes Boxes | `mes-boxes` |
| Mes Contrats | `mes-contrats` |
| Mes Paiements | `mes-paiements` |
| Mes Relances | `mes-relances` |
| Boxes disponibles | `boxes-dispo` |
| Mon Profil | `profil` |

---

## 🗄️ Modèles de données (Tables API)

### `clients`
| Champ | Type |
|-------|------|
| nom, prenom, email, telephone | text |
| adresse, ville, code_postal | text |
| piece_identite, numero_piece | text |
| photo_identite | rich_text |
| mot_de_passe, role, hub | text |
| actif | bool |
| box_numero, volume, prix_mensuel | text/number |
| date_entree, prochain_paiement | datetime |

### `boxes`
| Champ | Type |
|-------|------|
| numero, type, type_label, etage | text |
| taille, prix_mensuel | number |
| statut | text (disponible/occupée/maintenance/réservée) |
| climatisee, securisee | bool |
| description | text |
| photo | rich_text |

### `contrats`
| Champ | Type |
|-------|------|
| numero_contrat, client_id/nom, box_id/numero | text |
| date_debut, date_fin | datetime |
| prix_mensuel, caution, jour_prelevement | number |
| avec_caution, frais_reliv, signe_client, signe_admin | bool |
| signature_client, signature_admin | rich_text |
| hub, notes | text |
| statut | text (actif/en_attente/terminé/résilié) |

### `paiements`
| Champ | Type |
|-------|------|
| numero_facture, contrat_id, client_id/nom, box_numero | text |
| periode, mode_paiement, reference_transaction | text |
| montant_ht, tva, montant_ttc | number |
| date_echeance, date_paiement | datetime |
| statut | text (en_attente/payé/en_retard/annulé) |

### `relances`
| Champ | Type |
|-------|------|
| paiement_id, client_id/nom/email | text |
| niveau, montant_du | number |
| message | rich_text |
| statut | text (envoyée/vue/répondue/clôturée) |
| date_envoi | datetime |

### `relances_contacts`
| Champ | Type |
|-------|------|
| paiement_id, client_id, client_nom | text |
| date_contact | datetime |
| moyen_contact | text (appel/email/whatsapp/sms/visite) |
| retour_client | text (promesse/rappel/aucune_reponse/injoignable/litige/autre) |
| statut_suivi | text (en_attente/a_relancer/escalade/regle) |
| date_promesse, montant_promesse | text/number |
| notes, agent_nom | text |
| factures_concernees | array |


| Champ | Type |
|-------|------|
| type_action | text (entree/sortie/paiement/contrat/relance/modification) |
| description, utilisateur_id/nom | text |
| date_action | datetime |

---

## 🚀 Comptes de démonstration

| Rôle | Email | Mot de passe |
|------|-------|-------------|
| Administrateur | admin@gardenbox.fr | admin123 |
| Cliente | sophie.martin@email.com | client123 |
| Client | thomas.bernard@email.com | client123 |
| Cliente | fatima.alaoui@email.com | client123 |

> 💡 **Accès rapide** : Cliquez simplement sur les boutons "Administrateur" ou "Client" sur la page de connexion — aucun identifiant requis.

---

## 🔧 Prochaines étapes recommandées

- [ ] Envoi réel des emails de relance (API SMTP / Mailgun / SendGrid)
- [ ] Génération de PDF pour les factures (jsPDF)
- [ ] Système de réservation en ligne avec calendrier de disponibilité
- [ ] Import/export complet des données (CSV, Excel)
- [ ] Notifications push / SMS (Twilio, Firebase)
- [ ] Gestion des codes d'accès physiques aux boxes
- [ ] Signature électronique certifiée (YouSign, DocuSign)
- [ ] Module de devis en ligne
- [ ] Application mobile PWA

---

© 2025 Amanbox – Tous droits réservés
