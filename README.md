# Somfy Protexiom 600: Comment remplacer la transmission GSM 2G après l'arrêt du réseau

## Tutoriel pour contourner l'arrêt de la 2G sur Somfy Protexiom:

Face à l'arrêt progressif du réseau 2G, qui rend le transmetteur GSM Somfy obsolète et génère l'alerte système « Perte réseau GSM », voici comment mettre en place un système de secours autonome par SMS à moindre coût, en récupérant les informations d'alarmes. Cette solution nécessite une carte SIM 4G. 

Ce tuto s'adresse aux possesseurs d'une alarme Somfy Protexiom 600 équipée du module GSM 2G.

Pour les possesseurs d'une alarme Protexiom 600 équipée du module RTC, vous pouvez soit:
- Aller dans le menu "Réglages téléphonie filaire" (si présent) et utiliser l'envoi de SMS via le service payant 123-SMS (https://www.123-sms.net)
- Suivre ce tuto.

#### Pro's/Con's:
- Avanatages: La solution d'envoi de SMS via un site internet intermédiaire permet de garder le détail des alarmes (pile faibles, éléments d'intrusion, éléments d'incenide, etc...). Cependant, en fonction de la versoin software de la carte de la centrale, le menu 123-SMS est parfois absent (ce qui est mon cas).
- Désavantages: Il faut approvisionner un compteur SMS sur le site 123-SMS. Le transmetteur 4G ne fournira que 2 indications: Alarmes et perte d'alimentation.

### Attention: il faut être un peu bricoleur si vous optez pour le transmetteur 4G!

# Matériel utilisé
- Une sirène extérieure RTS SY2400935: https://www.domo-confort.com/sir-ne-ext-rieure-avec-flash-alarme-protexial-protexiom-rts-somfy-remplac-par-la-ref-sy2400935-sy1875068.html ou module d'éclairage RTS 2401161: https://boutique.somfy.fr/micro-module-pour-eclairage.html?)utm_source=bing&utm_medium=cpc_shopping&utm_campaign_id=18844935816&utm_campaign=fr_b2c_conv_allproducts&utm_source=bing&utm_medium=cpc_shopping&utm_campaign_id=494187834&utm_campaign=fr_b2c_conv_allproducts&utm_content=
- Un module relais temporisé "XY-J02". Disponible sur Amazon: https://www.amazon.fr/temporis%C3%A9-d%C3%A9clencheur-interrupteur-temporisation-minutes/dp/B0FS6C89GJ?th=1)
- Un transmetteur 4G autonome à déclenchement sec "GP4-WLTE avec carte SIM et batterie des secours". Choisir la version "GP4-WLTE-EC" sur Aliexpress: https://fr.aliexpress.com/item/1005006284883131.html?gatewayAdapt=glo2fra#nav-specification). Cette version est compatible pour l'Europe et possède une batterie de secours.

## Principe de montage:  
La centrale n'ayant pas de sortie "rapport transmetteur" accessible directement sur sa carte principale, il faut récupérer l'information d'alerte en amont via l'un de ces deux moyens:
- Exploiter la LED de la sirène extérieure en récupérant le signal directement sur son circuit de commande. Il faudra installer la sirène au sec car le transmetteur sera placé à coté et alimenté par du courant continu via le transformateur fournit. J'ai opté pour cette solution car elle transmet les alarmes incendie et intrusion.
- Utiliser un module d'éclairage RTS. Si vous optez pour l'utilisation du module d'éclairage, vous transmettrez les alarmes intrusion uniquement vers votre module 4G.


# Détails des branchements
## Relais XY-J02 et transmetteur 4G:
- Reliez une source d'alimentation continue aux bornes "Input +" et "Input -" du module relais. Par exemple utilisez une alimentation Micro-USB 5V externe adaptée, ou bien soudez des fils pour récupérer l'alimentation 6V des piles depuis la sirène extérieure.
- Reliez le fil de commande (signal LED vers le relais): Connectez le fil rouge de la LED sur la borne "Trigger" et le fil noir de la LED sur la borne "GND_Trigger" du relais (il faudra couper le fil au plus près de la led qui est reliée à la carte de la sirène).
- Reliez la sortie sans potentiel (contact sec) du relais vers le transmetteur 4G: Utilisez 2 fils pour connecter la borne "COM" du relais sur le "GND" du transmetteur (au niveau des entrées digitales) et la borne "NO" (Normalement Ouvert) du relais sur l'entrée DI1 du transmetteur.
- Lorsque le relais s'active, il ferme le contact sec entre COM et NO, ce qui déclenche instantanément l'envoi du SMS.  
  Paramètres à appliquer pour configurer le relais XY-J02:
- Mode de fonctionnement: Choisir le mode P1.1 (le relais s'active sur impulsion pendant le temps défini et ignore les déclenchements répétés tant qu'il est actif, évitant les coupures intempestives).
- Temporisation (Paramètre OP): Réglez sur 030. (soit 30 secondes*). Pourquoi 30 secondes? Cela permet de compenser parfaitement le décalage habituel (notamment le délai d'environ 15 secondes entre le déclenchement de la centrale et l'activation de la sirène extérieure), tout en garantissant un contact sec assez long pour que le transmetteur 4G ait le temps d'envoyer le SMS d'alerte.
#### Attention: l'emplacement du point après le chiffre 0 est très important => Voir le manuel pour déplacer le point.

### Manuel et spécifications du relais temporisé: https://ja-bots.com/wp-content/uploads/2022/06/XY-J02.pdf

# Schéma de principe
##### 1. ALIMENTATION DU RELAIS (XY-J02):
 [ Alimentation 5V externe USB ou interne 6V ]

├── (+) ------------------------> [ Borne "Input +" (XY-J02) ]

└── (-) -------------------------> [ Borne "Input -" (XY-J02) ]

##### 2. SIGNAL DE DÉCLENCHEMENT (Source d'alerte vers Relais):
[Sirène Extérieure (LED) ou Module d'éclairage RTS (2401161)]

├── Fil Rouge (Signal) -----------> [ Borne "Trigger signal" (XY-J02) ]

└── Fil Noir (Masse / GND) -----> [ Borne "GND-Trigger" (XY-J02) ]

##### 3. SORTIE CONTACT SEC (Relais vers Transmetteur 4G GP4-WLTE):
[ Module Relais XY-J02  --------------->  Transmetteur 4G GP4-WLTE ]

├── Borne "COM" (Commun) -------> [ Borne "GND" ]

└── Borne "NO" (Normal. Ouvert) --> [ Borne "DI1"]

## Paramétarge du transmetteur GP4-WLTE:
- Paramétrez la SIM pour ne pas avoir de code PIM !! j'ai utilisé un ancien iPad afin de supprimer le code PIN.
- Insérer la SIM au format Micro dans le transmetteur (attention la plupart des SIM actuelles sont au format Nano!). Le signal GSM est OK lorsque la led rouge clignote lentement.
- Envoyez un SMS depuis votre portable vers le numéro de la carte SIM du transmetteur et tapez: SN0000RPO. Vous recevrez quelques minutes plus tard une confirmation par SMS que votre numéro de mobile est enregistré dans le transmetteur pour les entrées digitales. Vous pouvez ajouter jusqu'à 4 numéros en procédant de la même manière avec d'autre ligne mobiles qui recevrons les alertes d'alarmes.

## Paramétrage de l'application WLTE Control:
- Téléchargez l'application dans le store Apple ou Android.
- Allez dans "Setting" puis "Device Control": Faites un appui long sur "Channel1" puis changez le nom du Channel (par example: Alarme_Somfy), changez le nom de l'input (par example: Alarme_Maison).
- Allez dans "Setting" puis "Input Control": Cliquez sur "Alarme_Maison1", sélectionnez "Connected" et cochez le bouton en haut à droite afin qu'il soit vert.

Dans le menu "Input Control" vous devez voir "Alarme_Maison1 (Connected) Activate push".  
### Manuel et spécifications du transmetteur 4G: https://manuals.plus/ae/1005002484411405

# Résultat
- Dès que l'alarme se déclenche, le signal de la led s'active, fait coller le relais XY-J02 en mode P1.1 pendant 30 secondes, ce qui ferme le
contact sec du transmetteur 4G et déclenche l'envoi immédiat du SMS d'alerte sur votre/vos téléphone(s), le tout de manière parfaitement stable.
- Afin de ne plus avoir d'alerte système "perte réseau GSM", il faut enlever le module GSM de la centrale. Je conseille de débrancher l'alimentation secteur de la centrale ainsi que d'enlever 1 pile avant de retirer le module GSM. Le module est fixé avec 2 vis qu'il faut dévisser. Ensuite il faut tirer délicatement le module, sachant qu'il est maintenu sur la gauche de la carte de la centrale via un petit port.   

### Contactez moi si besoin: protexiom600@free.fr

Somfy Protexiom 600 / arrêt du réseau 2G / remplacement GSM 4G / transmetteur GSM / migration 2G vers 4G / solution alternative Somfy
