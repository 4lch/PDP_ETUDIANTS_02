# INTERNNEEEEET

## <ins>Prérequis</ins>
- Avoir un groupe
- Avoir un compte [Github](https://github.com)
- [Git / Git Bash](https://gitforwindows.org/)
- [VS Code](https://code.visualstudio.com/)
- [Platformio](https://platformio.org/platformio-ide)

---

## <ins>Objectif</ins>
Maintenant que votre prototype de capteur sur breadboard fonctionne, il est temps de remonter les données vers un cloud pour les exploiter. Ici, on se contentera d'utiliser un réseau WiFi mis à disposition par un agriculteur pour nos tests de capteurs connectés. Cependant, dans le cadre d'un projet industriel, on se serait probablement tournés vers des réseaux spécialisés **IOT LPWAN comme LoRa ou Sigfox**.

Par exemple, le réseau LoRa offre une grande flexibilité, permettant de déployer nos propres passerelles privées, en charge de recevoir les données des capteurs (portée de ~1-5km) et les relayer vers un cloud comme **AWS IOT Core** (privé) ou **The Things Network** (TTN, semi-public) avec le protocole **LoRaWAN**. Ces réseaux sont parfaits pour remonter de très petites quantités de données de facon périodique et en consommant très peu d'énergie pour préserver la batterie des  objets connectés.

Comme on fait au plus simple et que l'on utilise du WiFi, on peut se tourner vers la plateforme Paas (Platform as a service) **Blynk** qui permet de prototyper cette application très rapidement, en intégrant l'envoi et la réception de données et leur affichage avec un dashboard.

Le but de ce TP est de faire remonter les données de température et humidité lues lors du TP précédent vers Blynk pour les afficher, et d'explorer la communication bidirectionnelle proposée par cette plateforme.

## <ins>Préparation</ins>
1. `Forkez` le projet du TP sur votre compte github avec le nom **"PDP_ETUDIANTS_02_TD<`NUMERO_DE_TD`>\_GP<`NUMERO_DE_GROUPE`>"**. Par exemple, le groupe 7 du TD 2 devra créer le repo **"PDP_ETUDIANTS_02_TD02_GP07"**
2. <ins>`ATTENTION : un projet mal nommé sera sanctionné par un 0 sur le suivi du TP si l'outil de correction ne peut pas le trouver.`</ins>
3. Clonez **votre fork** sur votre machine avec `git clone <url>`
4. Ouvrir le projet avec PlatformIO (sinon les options de compilation et téléversement peuvent ne pas apparaître)
5. Compléter le code pour compléter chaque étape et `commit` **REGULIEREMENT**

Afin de vérifier votre compréhension du fonctionnement de l'environnement de développement, vous allez travailler avec Git, vous devrez donc `commit` vos modifications régulièrement. Chaque étape devra faire l'objet d'une `branch` distincte sur votre `repository`. Pour indiquer qu'un exercice est terminé, vous devrez tagguer le commit corrrespondant avec un formalisme indiqué dans les instructions. Comme vous devez travailler sur des branches par étape, le `master` devrait être inchangé à la fin.

## <ins>Evaluation</ins>
L'évaluation de votre travail sera réalisée sur les critères suivants :
- Etapes terminées
- Respect des consignes de nommage des `branch` et `tag` git
- Contenu et description des `commit`
- Qualité du code (structure, lisibilité, commentaires utiles si et seulement si nécessaire...)
- Temps nécessaire pour réaliser les étapes

## <ins>Quelques ressources</ins>
- [Documentation AZ-Delivery](https://cdn.shopify.com/s/files/1/1509/1638/files/ESP_-_32_NodeMCU_Developmentboard_Datenblatt_AZ-Delivery_Vertriebs_GmbH_10f68f6c-a9bb-49c6-a825-07979441739f.pdf?v=1598356497)
- [Reference framework Arduino](https://www.arduino.cc/reference/en/)
- [Datasheet DHT11](https://www.mouser.com/datasheet/2/758/DHT11-Technical-Data-Sheet-Translated-Version-1143054.pdf)
- [Le site web de Blynk](https://blynk.io/)
---

## <ins>Etape 1 - INTERNNEEEEET</ins>
Une seule étape dans ce TP 😎

## Mise en place
1. Créer une `branch` etape_1 à partir de master avec `git checkout -b etape_1 master`. Cette commande crée la nouvelle branche et vous met dessus directement. Si vous la créez avec `git branch`, pensez à switcher dessus avec `git checkout etape_1`. La récupération des repos et des branches est automatisée. **Si la branche n'existe pas sur votre repo github ou qu'elle est mal nommée, elle ne sera pas corrigée.**
2. Vérifier que le code fourni compile et fonctionne. Il reprend là où vous avez laissé la fin du TP1.

## Blynk
1. Créer un compte avec le free tier sur [Blynk](https://blynk.io/) (Start free en haut à droite)

2. Assurez-vous que votre compte est bien en mode développeur. Vous pouvez aussi changer la langue facilement.
<p align="center">
  <img src="https://i.imgur.com/5kTyLmP.png" />
</p>

3. Naviguer vers "Zone des développeurs", puis "Mes modèles". On va à présent créer un modèle. Pour Blynk, cela correspond à un type de dispositif (ESP32...), à la façon dont il est connecté à Internet, et aux données associées. Ces modèles sont utilisés pour créer des dispositifs rapidement, surtout dans un contexte où on doit créer une flotte de capteurs.
<p align="center">
  <img src="https://i.imgur.com/YqJOoOX.png" />
</p>

4. Cliquer sur "+ Nouveau Modèle", lui donner un nom et configurer le matériel avec "ESP32" et le type de connexion avec "WiFi".
<p align="center">
  <img src="https://i.imgur.com/YE9NkCB.png" />
</p>

5. On va à présent créer les flux de données que l'on veut associer avec notre modèle. Dans cet exemple, on crée un pin virtuel (épingle virtuelle sur Blynk, traduction bof...) qui représente une donnée abstraite qui n'est pas liée ni à un pin numérique, ni analogique directement. Utiliser ces réglages pour configurer le pin virtuel pour l'humidité et faire de même pour la température (utiliser V1 pour la température au lieu de V0).
<p align="center">
  <img src="https://i.imgur.com/5sytWE8.png" />
</p>
<p align="center">
  <img src="https://imgur.com/ufM13m8.png" />
</p>

6. Créer un pin virtuel pour contrôler la LED. Le pin 26 n'est pas disponible dans les options de pin numérique, alors on passe par un nouveau pin virtuel
<p align="center">
  <img src="https://imgur.com/7JVlP7A.png" />
</p>

7. Sauvegarder le modèle que vous venez de créer.
<p align="center">
  <img src="https://imgur.com/kCh0DNQ.png" />
</p>

8. Maintenant que le modèle est créé, on peut ajouter notre dispositif de mesure qui va l'utiliser. Pour cela, aller dans l'onglet "Dispositifs" et cliquer sur "+ Nouvel appareil". Créer le nouveau dispositif en sélectionnant le modèle que l'on vient de créer et en lui donnant un nom.
<p align="center">
  <img src="https://imgur.com/HoWniQi.png" />
</p>

9. Naviguer vers l'onglet "Info appareil" et copier-coller les `#define` de `BLYNK_TEMPLATE_ID `, `BLYNK_TEMPLATE_NAME ` et `BLYNK_AUTH_TOKEN ` en haut de votre code, après les blocs `#include`.

10. Ajouter les lignes suivantes à votre code pour inclure la fonctionnalité WiFi à votre projet. Les bibliothèques WiFi sont déjà incluses avec PlatformIO, mais vous devez ajouter la bibliothèque "Blynk" par Volodymyr Shymanskyy avec l'outil de bibliothèque de PlatformIO. Vous pouvez aussi simplement importer cette bibliothèque en ajoutant `blynkkk/Blynk@^1.3.2` à `lib_deps` dans le fichier de configuration `platformio.ini`
``` C
#include <WiFi.h>
#include <WiFiClient.h>

//...

// EN DESSOUS DES DEFINES POUR BLYNK_TEMPLATE ID, ETC
#include <BlynkSimpleEsp32.h>

#define BLYNK_PRINT Serial
```
⚠️ `Vérifier que l'include de la bibliothèque Blynk est en-dessous des defines spécifiques à Blynk` ⚠️

11. Avant le setup, définir les identifiants à utiliser pour le WiFi, et appeler la fonction pour démarrer la session Blynk au début du setup, après la connexion au port série.
``` C
// Avant le setup
char ssid[] = "<YourNetworkName>";
char pass[] = "<YourPassword>";

// ...

// Au début du setup, après la connexion série
Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
Blynk.run();
```

12. Avant de rentrer en deep sleep, utiliser la fonction `virtualWrite` de Blynk pour remonter les données d'humidité et de température vers leurs flux respectifs.

13. Compiler et téléverser le code sur votre carte, vérifier que tout semble fonctionner avec le débug série.

14. Utilisez le "Tableau de bord Web" de votre modèle ou l'application mobile Blynk pour créer un template de dashboard simple mais efficace (avec des noms, les bonnes unités, des couleurs cohérentes, etc.). Utiliser une jauge pour l'humidité et un graphe pour la température.

15. A présent, vous devriez voir les valeurs reçues s'afficher !

16. Ajouter un bouton et une "LED" sur votre dashboard, liés au pin virtuel de la LED. Le but est de simuler un signal qui permettrait d'identifier le sac dans lequel le dispositif est installé. On peut imaginer que dans une vraie application, un agriculteur pourrait vouloir remplacer un dispositif avec une batterie faible par exemple. Il pourrait donc déclencher cette identification, et à son prochain réveil, le dispositif pourrait utiliser un buzzer pour faire du bruit et s'identifier.

17. Ajouter cette fonction au-dessus du setup. Il s'agit d'une fonction standard de Blynk qui permet d'exécuter du code sur mesure lorsqu'un signal est reçu d'un pin virtuel. Remplacer V2 par votre signal si besoin.
``` C
BLYNK_WRITE(V2)
{
  int pinValue = param.asInt(); // assigning incoming value from pin V0 to a variable
  Serial.print("Received value from Blynk: ");
  Serial.println(pinValue);
  digitalWrite(LED,pinValue);
  // Delay is only there so that we get a chance to see the LED value properly.
  delay(1000);
}
```

18. Dans notre application, vu le peu de temps où le dispositif est actif, il est très improbable que l'utilisateur appuie sur le bouton pendant cette période. On force donc la mise à jour des pins virtuels avec la dernière valeur stockée sur le serveur avec cette fonction (remplacer V2 par votre signal si besoin), à appeler tout de suite après `Blynk.run();` :
``` C
Blynk.syncVirtual(V2);
```

18. Vérifier que tout fonctionne et m'appeler pour valider le TP 😁

## Git
1. Pensez à nettoyer le code si besoin 😇
2.  `commit` le code si ce n'est pas déjà fait.
3.  Tagguer le dernier commit à corriger avec "e1" avec la commande `git tag e1 HEAD`. Cette commande utilise "HEAD" comme référence au commit le plus récent.
4.  Publier vos `commit` avec vos tags avec `git push origin --tags` (ou `git push --set-upstream origin etape_1 --tags` pour associer la branche sur le repo distant si c'est votre premier push sur cette branche)
---
