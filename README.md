# Somfy Protexiom 600 : comment remplacer la transmission GSM 2G après l'arrêt du réseau

Tutoriel pour contourner l'arrêt de la 2G sur Somfy Protexiom grace à un module relais 4G (GP4-WLTE):

Face à l'arrêt progressif des réseaux 2G qui rend les transmetteurs d'origine obsolètes et génère l'alerte « Pas de réseau » sur l'application, voici comment j'ai mis en place un système de secours autonome par SMS à moindre coût, en récupérant l'information d'alarme.
Je précise que j'utilise un carte SIM Free et non un service payant comme 123-SMS.Net.

Ce tuto s'adresse aux possesseurs d'une alarme protection 600 avec module 2G, car dans l'onglet "Réglages téléphonie filaire" le menu 123-SMS est absent.

Il faut être un peu bricoleur.

# Matériel utilisé:
- Une centrale d'alarme Somfy Protexiom 600
- Un module relais temporisé XY-J02 (disponible sur Amazon: https://www.amazon.fr/temporis%C3%A9-d%C3%A9clencheur-interrupteur-temporisation-minutes/dp/B0FS6C89GJ?th=1)
- Un transmetteur 4G autonome à déclenchement sec GP4-WLTE avec carte SIM (Choisir la version GP4-WLTE-EC sur Aliexpress: https://fr.aliexpress.com/item/1005006284883131.html?gatewayAdapt=glo2fra#nav-specification)

# Principe de montage:  
La centrale n'ayant pas de sortie "rapport transmetteur" accessible directement sur sa carte principale, il faut récupérer l'information d'alerte en amont via l'un de ces deux moyens:
- Soit exploiter la LED d'une sirène extérieure en récupérant le signal directement sur son circuit de commande. Il faudra installer la sirène au sec car le transmetteur devra être à coté et alimenté par du courant continu vie le transformateur fournit.
- Soit utiliser un module d'éclairage RTS (référence principale : micro-module ON/OFF Somfy 2401161, également listé sous la référence fabricant ⁠SO2401161⁠).
Précision importante: Si vous optez pour l'utilisation d'un module d'éclairage, cela ne transmettra sur votre module 4G que les alarmes liées à l'intrusion (ce qui reste l'usage principal recherché).
- Dans mon cas j'ai acheté une sirène extérieure dont j'ai déconnecté la sirène et la led.

# Détails des branchements pour réaliser le câblage de manière propre et sécurisée:  
# Relais XY-J02 et transmetteur 4G:
- Reliez une source d'alimentation continue aux bornes "Input +" et "Input -" du module relais. Par exemple utilisez une alimentation 12V externe adaptée, ou bien soudez des fils pour récupérer l'alimentation des piles depuis la sirène extérieure.
- Reliez le fil de commande (signal LED vers le relais): Connectez le fil rouge de la LED sur la borne "Trigger" et le fil noir de la LED sur la borne "GND_Trigger" du relais (il faudra couper le fil au plus près de la led qui est reliée à la carte de la sirène).
- Reliez la sortie sans potentiel (contact sec) du relais vers le transmetteur 4G: Utilisez 2 fils pour connecter la borne "COM" du relais sur le "GND" du transmetteur (au niveau des entrées digitales) et la borne "NO" (Normalement Ouvert) du relais sur l'entrée DI1 du transmetteur.
- Lorsque le relais s'active, il ferme le contact sec entre COM et NO, ce qui déclenche instantanément l'envoi du SMS.  
  Paramètres à appliquer pour configurer le relais XY-J02:
- Mode de fonctionnement: Choisir le mode P1.1 (le relais s'active sur impulsion pendant le temps défini et ignore les déclenchements répétés tant qu'il est actif, évitant les coupures intempestives).
- Temporisation (Paramètre OP): Régler sur 030. (soit 30 secondes). Pourquoi 30 secondes? Cela permet de compenser parfaitement le décalage habituel (notamment le délai d'environ 15 secondes entre le déclenchement de la centrale et l'activation de la sirène extérieure), tout en garantissant un contact sec assez long pour que le transmetteur 4G ait le temps d'envoyer le SMS d'alerte.

- Manuel et spécifications du relais temporisé XY-J02: https://ja-bots.com/wp-content/uploads/2022/06/XY-J02.pdf

# Schéma de principe:
1. ALIMENTATION DU RELAIS (XY-J02):
- [ Alimentation 12V Externe ]
- ├── (+) ------------------------> [ Borne "Input +" (XY-J02) ]
- └── (-) -------------------------> [ Borne "Input -" (XY-J02) ]

2. SIGNAL DE DÉCLENCHEMENT (Source d'alerte vers Relais):
- [Sirène Extérieure (LED) OU Module d'éclairage RTS (2401161)]
- ├── Fil Rouge (Signal) -----------> [ Borne "Trigger signal" (XY-J02) ]
- └── Fil Noir (Masse / GND) -----> [ Borne "GND-Trigger" (XY-J02) ]

3. SORTIE CONTACT SEC (Relais vers Transmetteur 4G GP4-WLTE):
- [ Module Relais XY-J02 ] --------------> [ Transmetteur 4G GP4-WLTE ]
- ├── Borne "COM" (Commun) -------> [ Borne "GND" ]
- └── Borne "NO" (Normal. Ouvert) --> [ Borne "DI1"]

# Transmetteur GP4-WLTE:
- Paramétrez la SIM pour ne pas avoir de code PIM !! j'ai utilisé un ancien iPad afin de supprimer le code PIN.
- Insérer la SIM au format Micro dans le transmetteur (attention la plupart des SIM actuelles sont au format Nano!). Le signal GSM est OK lorsque lorsque la led rouge clignote lentement.
- Envoyez un SMS depuis votre portable vers le numéro de la carte SIM du transmetteur et tapez: SN0000RPO. Vous recevrez quelques minutes plus tard une confirmation par SMS que votre numéro de mobile est enregistré dans le transmetteur pour les entrées digitales. Vous pouvez ajouter jusqu'à 4 numéros en procédant de la même manieère avec d'autre ligne mobiles qui recevrons les alertes d'alarmes.

# Paramétrage de l'application WLTE Control:
- Téléchargez l'application dans le store Apple ou Android.
- Allez dans "Setting" puis "Device Control": Faites un appui long sur "Channel1" puis changez le nom du Channel (par example: Alarme_Somfy), changez le nom de l'input (par example: Alarme_Maison).
- Allez dans "Setting" puis "Input Control": Cliquez sur "Alarme_Maison1", sélectionez "Connected" et cochez le bouton en haut à droite afin qu'il soit vert.

Dans le menu "Input Control" vous devez voir "Alarme_Maison1 (Connected) Activate push".  
- Manuel et spécifications du transmetteur: https://manuals.plus/ae/1005002484411405

# Résultat:
- Dès que l'alarme se déclenche, le signal de la led s'active, fait coller le relais XY-J02 en mode P1.1 pendant 30 secondes, ce qui ferme le
contact sec du transmetteur 4G et déclenche l'envoi immédiat du SMS d'alerte sur votre téléphone, le tout de manière parfaitement stable.  
- Contactez moi si besoin: protexiom600@free.fr

Somfy Protexiom 600 / arrêt du réseau 2G / remplacement GSM 4G / transmetteur GSM / migration 2G vers 4G / solution alternative Somfy
