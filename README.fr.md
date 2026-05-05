# 🏠 Dashboard Tablette Home Assistant

https://github.com/user-attachments/assets/1954b7b9-e99b-45d3-94ab-533569a17c86

Un dashboard Home Assistant riche en fonctionnalités, conçu pour les **tablettes murales** (ou toute tablette) en **orientation paysage uniquement**. Construit avec une collection de cartes Lovelace personnalisées, il offre une expérience soignée, prête pour une installation en mode kiosque.

> 🚧 **Travaux en cours** — Ce dashboard est encore en développement actif. Des changements, de la documentation manquante et des fonctionnalités incomplètes sont à prévoir.

> ⚠️ **Ce dashboard n'est pas destiné aux débutants.** Il faudra adapter les IDs d'entités, les positions et les dimensions en fonction de votre propre installation et du ratio/résolution de votre écran.

---

## ✨ Fonctionnalités

- Mise en page paysage uniquement, optimisée pour les tablettes murales
- Apparence épurée grâce à un thème Home Assistant personnalisé (masque l'interface native, fournit le style)
- **Deux versions disponibles :**
  - **Version Fully Kiosk Browser** — utilise deux onglets pour basculer entre les vues sans rechargement des dashboards
  - **Version navigation Lovelace classique** — utilise la navigation standard de Lovelace
- Historique de détection de mouvement avec filtrage par caméra
- Affichage des prix carburant des stations proches
- Panneau d'alarme, lecteur média, météo, calendrier, graphiques, et plus encore

---

## 📋 Prérequis

- [Home Assistant](https://www.home-assistant.io/) (version récente recommandée)
- [HACS](https://hacs.xyz/) installé
- Connaissance de base de la configuration YAML Lovelace
- *(Optionnel)* [Fully Kiosk Browser](https://www.fully-kiosk.com/) pour la version à deux onglets

---

## 📦 Plugins requis

Tous les plugins ci-dessous sont disponibles via HACS sauf indication contraire.

### Dépôts HACS standards

| Plugin | Description |
|---|---|
| [MACS](https://github.com/glyndavidson/MACS) | Mood-Aware Character SVG — compagnon animé |
| [button-card](https://github.com/custom-cards/button-card) | Carte bouton hautement personnalisable |
| [alarmo-card](https://github.com/nielsfaber/alarmo-card) | Carte panneau d'alarme pour l'intégration Alarmo |
| [lovelace-state-switch](https://github.com/thomasloven/lovelace-state-switch) | Afficher/masquer des cartes selon l'état d'une entité |
| [embedded-view-card](https://github.com/redkanoon/embedded-view-card) | Intégrer une vue Lovelace dans une autre |
| [Bubble Card](https://github.com/Clooos/Bubble-Card) | Carte minimaliste avec support pop-up |
| [clock-weather-card](https://github.com/pkissling/clock-weather-card) | Carte combinée horloge et météo |
| [simple-swipe-card](https://github.com/nutteloost/simple-swipe-card) | Navigation tactile par glissement entre cartes |
| [mini-graph-card](https://github.com/kalkih/mini-graph-card) | Carte graphique compacte pour l'historique de capteurs |
| [restriction-card](https://github.com/iantrich/restriction-card) | Ajouter des restrictions/protection à n'importe quelle carte |
| [horizontal-waterfall-history-card](https://github.com/sxdjt/horizontal-waterfall-history-card) | Visualisation historique en cascade horizontale |
| [calendar-card-pro](https://github.com/alexpfau/calendar-card-pro) | Carte calendrier avancée |
| [lovelace-plotly-graph-card](https://github.com/dbuezas/lovelace-plotly-graph-card) | Carte graphique puissante basée sur Plotly |
| [hash-timer-card](https://github.com/Arubinu/hash-timer-card) | Carte minuterie (dépôt personnalisé — voir ci-dessous) |
| [hass-prixcarburant](https://github.com/Aohzan/hass-prixcarburant) | Intégration des prix carburant français |
| [Spook](https://github.com/frenck/spook) | Appels de services étendus pour HA, utilisé dans les automatisations |

### Dépôts personnalisés (à ajouter manuellement dans HACS)

Ces dépôts ne sont pas dans le catalogue HACS par défaut et doivent être ajoutés en tant que **dépôts personnalisés** :

| Plugin | Description |
|---|---|
| [AlertTicker-Card](https://github.com/djdevil/AlertTicker-Card) | Carte bandeau d'alerte défilant |
| [FileTrack](https://github.com/TheScubaDiver/FileTrack) | Capteur de suivi de fichiers dans un dossier (utilisé pour l'historique de détection) |
| [mediocre-hass-media-player-cards](https://github.com/antontanderup/mediocre-hass-media-player-cards) | Cartes lecteur média améliorées |
| [hash-timer-card](https://github.com/Arubinu/hash-timer-card) | Carte minuterie personnalisée |
| `camera-detection-slider` | *(Dépôt à venir)* Carte curseur de détection caméra |

### Style : card-mod ou uix (au choix)

Pour le style des cartes, vous avez besoin de **l'un ou l'autre**. Les deux fonctionnent — choisissez selon vos préférences :

| Plugin | Notes |
|---|---|
| [lovelace-card-mod](https://github.com/thomasloven/lovelace-card-mod) | Le plugin de style classique, très répandu |
| [uix](https://github.com/Lint-Free-Technology/uix) | S'appuie sur card-mod en interne ; maintenu comme abstraction de plus haut niveau |

---

## ⚙️ Configuration

### Prix carburant (`configuration.yaml`)

Ajoutez ce bloc dans votre `configuration.yaml`. Les IDs de stations se trouvent sur le [site officiel des prix des carburants](https://www.prix-carburants.gouv.fr) — ouvrez la fiche détail d'une station et relevez les derniers chiffres de l'URL.

```yaml
sensor:
  - platform: prix_carburant
    stations:
      - 00000000  # Remplacez par l'ID de votre station
      - 00000000  # Remplacez par l'ID de votre station
```

### Historique de détection avec FileTrack (`configuration.yaml`)

Dupliquez le bloc capteur pour chaque caméra à surveiller. Le champ `filter` supporte les caractères génériques pour filtrer par nom de fichier.

```yaml
filetrack:
  sensors:
    - name: 'Argus 1 FileTrack'
      folder: /config/www/media/Enregistrements
      filter: 'Argus 1*'
      sort: date
```

### Accès externe aux enregistrements (`configuration.yaml`)

Nécessaire pour que FileTrack puisse accéder aux fichiers et pour rendre les enregistrements accessibles depuis l'extérieur du réseau local :

```yaml
homeassistant:
  allowlist_external_dirs:
    - /config/www/media/Enregistrements

  media_dirs:
    enregistrements: /media/Enregistrements
```

> 📁 **Note :** Le chemin `/media/Enregistrements` est exposé via un **stockage réseau** ajouté directement depuis l'interface Home Assistant (**Paramètres → Système → Stockage → Ajouter un stockage réseau**). Sans cette étape, le dossier ne sera pas visible par Home Assistant ni par FileTrack.

---

## 🤖 Automatisations incluses

Les fichiers d'automatisation suivants sont inclus dans le dépôt et nécessitent une configuration spécifique.

### `interaction.yaml` — Émotion M.A.C.S. sur détection de mouvement

Déclenche une animation d'émotion aléatoire sur le personnage [M.A.C.S.](https://github.com/glyndavidson/MACS) lorsqu'un mouvement est détecté devant la tablette. Nécessite :

- [Spook](https://github.com/frenck/spook) pour les appels de services étendus utilisés dans l'automatisation
- Un appareil Fully Kiosk Browser déclaré dans les intégrations Home Assistant
- Le capteur binaire `binary_sensor.kiosk` lié à cet appareil Fully Kiosk (à adapter selon votre configuration)

### `letters.yaml` — Alerte boîte aux lettres

Affiche une alerte lors de la détection de courrier dans la boîte aux lettres. Nécessite la création manuelle de l'assistant suivant dans Home Assistant :

**Paramètres → Appareils et services → Entrées → Créer une entrée → Texte**

```
input_text.boite_aux_lettre_alerte
```

### `home.yaml` — Retour automatique à la vue principale

Revient automatiquement à la vue principale du dashboard après **1 minute sans interaction**. Aucune configuration supplémentaire n'est nécessaire, hormis l'adaptation de la cible (dashboard/vue) à votre propre configuration.

---

## 📐 Adaptation à votre écran

Ce dashboard a été conçu pour une taille et un ratio d'écran spécifiques. Vous **devrez** adapter :

- **Les IDs d'entités** — remplacez toutes les entités par les vôtres
- **Les positions et dimensions** — les positions des cartes (notamment en mise en page absolue) dépendent de la résolution et du ratio de votre écran
- **Les paramètres du thème** — configurez le thème selon vos préférences
- **Le slug de l'URL du dashboard** — le dashboard doit être créé avec le slug `m-a-c-s` (défini lors de la création du dashboard dans Home Assistant) pour éviter d'avoir à mettre à jour les références d'URL dans tout le YAML

---

## 🖥️ Version Fully Kiosk Browser

Si vous utilisez [Fully Kiosk Browser](https://www.fully-kiosk.com/), la version à deux onglets permet de basculer entre les vues du dashboard **sans rechargement**. Cela offre des transitions plus fluides et sans temps de chargement lors du changement de vue.

### Intégration Home Assistant

Installez l'intégration **Fully Kiosk Browser** dans Home Assistant (**Paramètres → Appareils et services → Ajouter une intégration → Fully Kiosk Browser**). Cela expose la tablette en tant qu'appareil avec des capteurs de détection de mouvement, des services de contrôle de l'écran, et plus encore — requis pour l'automatisation `interaction.yaml`.

### Configuration des deux onglets

Dans les paramètres de Fully Kiosk Browser, renseignez les deux URLs de vos dashboards dans le champ **URL de démarrage** en saisissant chaque URL sur une ligne séparée. Fully Kiosk les chargera comme deux onglets indépendants, maintenant les deux dashboards en mémoire simultanément.

Configurez les permissions appropriées (écran toujours allumé, démarrage automatique au démarrage, etc.) pour que la tablette reste active en permanence.

### Injection JavaScript — Outils de développement mobile

Les navigateurs sur tablette n'exposent pas de panneau développeur. Il est possible d'injecter [Eruda](https://github.com/liriliri/eruda), une console développeur adaptée au mobile, directement via l'**interface d'administration de Fully Kiosk Browser** (Paramètres Web avancés → Injecter du JavaScript). Cela simule les outils développeur du navigateur et s'avère très utile pour déboguer le dashboard :

```javascript
var script = document.createElement('script');
script.src = 'https://cdn.jsdelivr.net/npm/eruda';
script.onload = function() { eruda.init(); };
document.head.appendChild(script);
```

---

## 🧩 Personnalisation par utilisateur avec Browser Mod

[Browser Mod](https://github.com/thomasloven/hass-browser_mod) est une intégration complémentaire fortement recommandée qui permet une personnalisation par appareil et par utilisateur directement depuis Home Assistant. Elle s'installe en tant que **dépôt personnalisé HACS**.

Elle permet notamment de :

- **Définir un dashboard par défaut** par utilisateur ou par navigateur/appareil
- **Masquer le menu latéral** (navigation gauche) pour une expérience kiosque plus épurée
- **Masquer la barre de titre/en-tête Lovelace** pour maximiser l'espace écran
- **Gérer les permissions utilisateur** au niveau du navigateur

C'est particulièrement utile pour les tablettes murales où l'on souhaite un affichage plein écran sans distractions, sans avoir à dépendre uniquement de Fully Kiosk Browser pour le contrôle de l'interface.

---

## 🤝 Contribuer

Les pull requests sont les bienvenues. Pour les changements importants, merci d'ouvrir une issue au préalable.

---

## 📄 Licence

[MIT](LICENSE)
