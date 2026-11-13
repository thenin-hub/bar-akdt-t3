# Bar-AKDT — Comptoir Jetons

Application de caisse web pour le bar AKDT. L’application fonctionne dans un navigateur et peut être installée comme application web (PWA). Les données de caisse sont gérées par l’application dans le navigateur.

## Fichiers
- `index.html` : interface et logique de caisse.
- `default-products.js` : catalogue de démarrage et taux jeton/euro.
- `manifest.webmanifest` et les icônes PNG : métadonnées et icônes PWA.
- `sw.js` : cache hors ligne; version actuelle `comptoir-v18`.

## Lancement
Servir le contenu de ce dossier via un serveur web (HTTPS recommandé; `localhost` convient pour les essais), puis ouvrir `index.html`. Pour l’installation PWA et le service worker, un serveur est nécessaire; l’ouverture directe en `file://` ne fournit pas ces fonctions.

La navigation de caisse est organisée en trois niveaux : groupes (Boissons et Snacks par défaut), catégories du groupe actif, puis produits. Les catégories possèdent un groupe et les anciennes données sont migrées automatiquement sans supprimer produits, couleurs ou ordre. Le mode « Personnaliser » permet de réordonner les groupes, catégories et produits; les groupes et catégories ont une couleur configurable dans Produits. Les catégories peuvent être affectées à un groupe depuis leur sélecteur dans Produits. Les groupes non vides ne peuvent pas être supprimés. Le groupe et la catégorie actifs sont mémorisés.

La grille de caisse s’adapte à l’écran : 3 colonnes sur mobile (< 600 px), 4 sur tablette portrait (600–999 px) et 6 sur grand écran ou tablette paysage (≥ 1000 px). Les noms de produits restent entièrement visibles; les cartes reprennent la couleur personnalisée du produit ou, à défaut, de sa catégorie, avec une couleur de texte contrastée. Le mode « Personnaliser » permet de déplacer les produits et catégories à la poignée. La barre des catégories défile horizontalement; l’ajout au ticket conserve son fonctionnement.


## Catalogue et cache (v17)

Le catalogue par défaut est automatiquement ajouté si la caisse démarre sans aucun produit, y compris sur un profil déjà existant. Dans Produits, « Restaurer le catalogue par défaut » ajoute les articles par défaut manquants après confirmation sans remplacer les produits personnalisés ni leurs modifications. Le cache hors ligne est versionné `comptoir-v18`.

## Palette et typographie (v14)

La personnalisation des produits, catégories et groupes propose 20 couleurs prédéfinies — rouge, orange, ambre, jaune, citron, vert, émeraude, turquoise, cyan, bleu, indigo, violet, magenta, rose, marron, gris, ardoise, crème, bleu marine AKDT et bleu clair AKDT — ainsi qu’un sélecteur libre et une remise à la couleur par défaut. Les couleurs enregistrées par les utilisateurs restent prioritaires; le texte des cartes s’adapte automatiquement à la luminance du fond. Le thème global reprend le marine AKDT (#052F5B), le jaune (#FDD21F) et le bleu clair (#9BD4DB); les couleurs personnalisées déjà enregistrées ne sont pas remplacées. La palette de 20 teintes prédéfinies était déjà présente et a été conservée, avec sélecteur libre et remise à la couleur par défaut.

La typographie utilise partout la pile `"Avenir Next", "Avenir Next LT Pro", Avenir, "Nunito Sans", "Segoe UI", system-ui, -apple-system, sans-serif`. Avenir Next est utilisé s’il est installé sur l’appareil; Nunito Sans reste un fallback sans chargement externe, car le projet n’utilise pas de police web Google Fonts.


## Identité visuelle v14

L’interface adopte le thème AKDT : fonds et en-tête marine `#052F5B`, jaune `#FDD21F` pour les actions et montants mis en avant, et bleu clair `#9BD4DB` comme accent secondaire. Un logotype SVG « akdt » est affiché dans l’en-tête. Les cartes produits sont centrées, avec une hauteur minimale de 130 px et une grille de 3/4/6 colonnes selon la largeur. Le cache hors ligne est versionné `comptoir-v18`.

## Gestion des catégories (v17)

Dans Produits, les catégories peuvent être ajoutées avec leur nom, groupe et couleur, puis renommées, déplacées vers un autre groupe ou supprimées après confirmation. Les noms vides et doublons sont refusés. Un renommage met à jour les produits associés, qu’ils référencent leur catégorie par nom ou par identifiant; une catégorie contenant des produits ne peut être supprimée qu’en choisissant une autre catégorie vers laquelle déplacer ses produits. S’il n’existe aucune destination, la suppression est bloquée jusqu’à la création d’une autre catégorie. L’ordre et les couleurs sont conservés et enregistrés dans la configuration existante. En caisse, le badge de quantité est ancré en bas à droite de la carte, dans sa marge réservée pour éviter de recouvrir le nom, le prix ou la poignée de déplacement.

Le cache hors ligne est versionné `comptoir-v18`.


## Impression des tickets (v17)

Après validation d’une commande, le bouton « Imprimer le ticket · dernière commande » permet d’imprimer le reçu. La réimpression est aussi disponible sur chaque vente dans l’onglet Journée, y compris pour les tickets annulés (ils sont signalés sur le reçu). Dans Réglages, l’impression automatique à chaque validation peut être activée ou désactivée; elle est désactivée par défaut.

Le ticket est composé dans une zone dédiée, cachée à l’écran, puis imprimé avec `window.print()` et une feuille `@media print` en format étroit 80 mm. Il affiche le nom du bar AKDT, la date et l’heure, l’identifiant de commande, les lignes avec quantité, nom, jetons et équivalent en euros, les totaux, le taux de 1 jeton = 1,10 € et un remerciement. Le dialogue d’impression du navigateur ou de l’appareil permet d’imprimer ou d’enregistrer en PDF.

Le cache hors ligne a été renouvelé en `comptoir-v18`.


## Cagnotte de jetons (v18)

L’onglet « Cagnotte » présente le solde calculé et son équivalent en euros au taux par défaut actuel (1 jeton = 1,10 €), ainsi que les totaux cumulés des entrées et sorties. Les mouvements sont enregistrés dans la collection locale `cj1:pot` (ou la collection `pot` du stockage connecté), à côté des autres données existantes. Les anciennes installations sans cette collection démarrent avec un solde nul, sans modification des données précédentes.

Les entrées/sorties et la suppression des mouvements sont réservées aux rôles dotés du permis « Clôturer la journée » (patron et responsable par défaut); les autres employés peuvent consulter le solde et l’historique. Les entrées/sorties utilisent une quantité entière strictement positive, un motif facultatif et une confirmation. Les retraits au-delà du solde sont refusés. Le journal affiche les mouvements récents en premier et permet d’annuler/supprimer un mouvement avec confirmation; les soldes successifs sont recalculés et la suppression est bloquée si elle rendrait ultérieurement le solde négatif. Les exports texte, CSV et Excel de clôture incluent l’état courant et le journal de la cagnotte au moment de l’export. Cache PWA mis à jour : `comptoir-v18`.

## Imprimantes de tickets (v20)

Dans **Réglages → Imprimantes**, l’imprimante système (navigateur/AirPrint) est conservée comme périphérique actif par défaut afin de préserver le comportement des profils existants. L’imprimante `CaisseBar` (ESC/POS, `172.29.29.150:9100`, Shopcaisse 80mm) est également préconfigurée; elle doit être sélectionnée explicitement. Les réglages d’imprimantes et l’identifiant actif sont enregistrés dans `config/printers` (ou `cj1:config` en stockage local), y compris pour les profils liés au stockage synchronisé.

Le ticket ESC/POS en mode texte utilise l’initialisation, alignement, gras, taille, séparateurs selon la largeur, le jeu CP858 (`ESC t 19`), des sauts de ligne et une commande de coupe. L’option graphique est affichée mais n’embarque pas de rendu bitmap/logo à ce stade: elle émet le ticket texte pour compatibilité. « Tester impression » envoie un ticket de test avec accents. La recherche d’imprimantes n’effectue pas de balayage IP: les restrictions réseau du navigateur empêchent un scan fiable.

**Limite réseau importante :** une PWA web ne peut pas ouvrir de socket TCP brut vers le port 9100. Une tentative HTTP directe vers `http://IP:9100` en mode `no-cors` n’est prise en charge que si l’imprimante/le serveur l’accepte; le navigateur masque alors le statut HTTP, donc une réponse non vérifiable ne signifie pas que le papier est sorti. Les échecs réseau détectables provoquent le repli vers l’impression système (`window.print`). Le pont est la méthode recommandée:

1. Sur un PC ou Raspberry Pi du même réseau Wi-Fi que l’imprimante, installer Node.js et lancer `node print-bridge.js` (options `--host`, `--port`, `--printer-ip`, `--printer-port`; équivalents variables `BRIDGE_HOST`, `BRIDGE_PORT`, `PRINTER_IP`, `PRINTER_PORT`). Par défaut, le pont écoute sur `0.0.0.0:8080` et relaie vers `172.29.29.150:9100`.
2. Dans la configuration de l’imprimante, inscrire `http://IP-DU-PC:8080/print` dans « URL du pont d’impression » et enregistrer.
3. Autoriser le port du pont sur le réseau local et tester l’impression. Le pont répond aux requêtes CORS et transmet les octets POST tels quels.

Le pont est un relais sans authentification: ne l’exposez pas sur Internet et limitez son accès au réseau de confiance. Si l’application est servie en HTTPS, les navigateurs peuvent bloquer l’appel au pont HTTP pour contenu mixte; servir l’application en HTTP sur le réseau local ou configurer un pont HTTPS de confiance est nécessaire. Le port TCP vers l’imprimante doit être joignable depuis le PC/Raspberry.

Cache PWA renouvelé : `comptoir-v18`. `print-bridge.js` est un outil Node serveur et n’est volontairement pas précaché dans le service worker.


## Correctif navigation Réglages (v20)

La fonction de lecture des réglages de tickets (`receiptSettings`) est définie avec une valeur par défaut sûre (impression automatique désactivée) : Réglages s’ouvre aussi sur un ancien profil dépourvu de configuration d’impression. La définition en double de `printSale` a été supprimée afin que le choix d’imprimante Epson ESC/POS continue d’utiliser le relais réseau; l’impression système reste utilisée si l’imprimante choisie est le navigateur/AirPrint. Cache hors ligne : `comptoir-v20`.
