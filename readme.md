# Node-RED pour créer un dashboard en no-code

## <ins>Prérequis</ins>
- Avoir un groupe
- Avoir un compte [Github](https://github.com)
- [Git / Git Bash](https://gitforwindows.org/)
- [VS Code](https://code.visualstudio.com/)
- [Platformio](https://platformio.org/platformio-ide)
- [NodeJS](https://nodejs.org/en/download/)
---

## <ins>Objectif</ins>
Maintenant que les données de notre capteur connecté remontent avec MQTT, nous cherchons un moyen rapide de les visualiser. 

Node-RED est un outil low-code qui sert à programmer et prototyper des applications réactives à des évènements. Cet outil permet de relier ensemble des objets connectés, des APIs, des services web, etc, à l'aide d'une interface graphique composée de noeuds qui ensemble forment des "flows". L'environnement qui accueille les flows est appelé la "palette".
Node-RED est construit sur l'environnement NodeJS que vous avez sûrement déjà croisé.

Avec Node-RED, on va pouvoir recevoir des données par MQTT, et créer un dashboard, vous voyez où on va en venir 🤓

## <ins>Préparation</ins>
1. `Forkez` le projet du TP sur votre compte github avec le nom **"PDP_ETUDIANTS_03_TD<`NUMERO_DE_TD`>\_GP<`NUMERO_DE_GROUPE`>"**. Par exemple, le groupe 7 du TD 4 devra créer le repo **"PDP_ETUDIANTS_03_TD04_GP07"**
2. <ins>`ATTENTION : un projet mal nommé sera sanctionné de -5pts sur le suivi du TP. C'est littéralement la partie la plus facile pour vous et ça facilite énormément la correction.`</ins>
3. Clonez **votre fork** sur votre machine avec `git clone <url>`
4. Ouvrir le projet avec VS Code
5. Pour ce TP, pas de code ou de projet à compléter, mais il faudra ajouter les sauvegardes de votre palette Node-RED.

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
- [Le site web de MQTT](https://mqtt.org/)
- [Télécharger NodeJS](https://nodejs.org/en/download/)
---

## <ins>Etape 1 - Dashboard</ins>
Une seule étape dans ce TP :)

## Réalisation
1. Créer une `branch` etape_1 à partir de master avec `git checkout -b etape_1 master`. Cette commande crée la nouvelle branche et vous met dessus directement. Si vous la créez avec `git branch`, pensez à switcher dessus avec `git checkout etape_1`. La récupération des repos et des branches est automatisée. **Si la branche n'existe pas sur votre repo github ou qu'elle est mal nommée, elle ne sera pas corrigée.**
2. Une fois n'est pas coutume, il n'y a rien du tout dans le repo à part le readme, vous allez créer le projet vous-même avec Node-RED.
3. Installer Node-RED pour le dashboard...
```bash
# Installer Node-RED avec npm, le package manager de NodeJS
npm install -g --unsafe-perm node-red
# Vérifier que Node-RED est bien installé
node-red -h
```

4. ...Puis démarrer Node-RED
```
node-red
```
Vous pouvez alors ouvrir la page Node-RED locale sur [http://127.0.0.1:1880](http://127.0.0.1:1880) avec votre navigateur.
<p align="center">
  <img src="https://i.imgur.com/kES9LM4.png" />
</p>

5. Nous allons utiliser la palette "node-red-dashboard" qui n'est pas installée par défaut, il faut donc l'installer avec "Manage palette".
<p align="center">
  <img src="https://hackster.imgix.net/uploads/attachments/1232251/image_sxIySUEVa0.png?auto=compress%2Cformat&w=740&h=555&fit=max" />
</p>

6. Chercher "node-red-dashboard" dans l'onglet "Install" et l'installer.
<p align="center">
  <img src="https://i.imgur.com/goKmJbg.png" />
</p>

7. A présent, vous pouvez commencer à agencer la palette pour créer le dashboard.
8. Glisser deux nodes "`mqtt in`" sur la palette, l'un sera utilisé pour recevoir la température, et l'autre pour l'humidité relative.
9. Il faut maintenant configurer les nodes mqtt in pour qu'ils puissent recevoir les données. Double-cliquer sur le premier pour ouvrir sa configuration. Dans  `Server`, sélectionnez "`Add new mqtt-broker`", puis l'icone pour éditer. Cette action va créer un noeud de type `mqtt-broker` réutilisable qui sert de configuration pour notre broker.
10. Configurer le `mqtt-broker` ainsi :
    - `Name`: Broker
    - Onglet `Connection`:
      - `Server`: 27cc61dbaffc4da08cd0081cabd8cf01.s2.eu.hivemq.cloud
      - `Port`: 8883
      - `Connect automatically`: Oui
      - `Use TLS`: Oui, le broker hébergé par HiveMQ n'est accessible qu'avec une connexion sécurisée. Pas besoin de créer un noeud de configuration `tls-config` ici.
      - `Protocol`: MQTT V5
      - Le reste par défaut.
    - Onglet `Security`:
      - `Username`: ocres4ever
      - `Password`: ocresse123

11. Maintenant que vous avez créé le noeud de configuration de type `mqtt-broker`, vous pouvez l'utiliser dans l'onglet `Server` du noeud `mqtt-in` que vous étiez en train d'éditer.

12. Finir de configurer le noeud ainsi:
    - `Action`: Subscribe to single topic
    - `Topic`: Le topic sur lequel votre groupe publie la température ou l'humidité de votre capteur.
    - `QoS`: 2
    - Le reste par défaut.

13. Configurer le deuxième noeud `mqtt in` de la même façon pour l'autre sujet. Vous pouvez réutiliser le `mqtt_broker` créé précédemment.

14. Essayez de `Deploy` votre palette. Si tout a bien été configuré, les deux noeuds `mqtt in` devraient rapidement afficher le statut "connected".

15. Une fois que tout marche au niveau de MQTT, vous pouvez glisser des noeuds spécifiques au dashboard `gauge` ou `chart` et les relier aux noeuds `mqtt in` pour intégrer les données du capteur au dashboard.

16. Le dashboard est accessible à l'adresse [http://127.0.0.1:1880/ui](http://127.0.0.1:1880/ui). Pensez bien à `Deploy` vos changements pour voir la version la plus à jour.

17. Créez un dashboard simple (mais fonctionnel !) en utilisant les propriétés des noeuds, comme les groupes, la taille, le type de graphique, les unités, les couleurs...
    
18. Appelez-moi pour vérifier que tout fonctionne bien.

19. Téléchargez la source de votre palette. Faites le raccourci `ctrl+e`, sélectionnez "all flows", puis "Download".
<p align="center">
  <img src="https://i.imgur.com/JXhiUk4.png" />
</p>

20. Copiez le fichier `flows.json` dans le dossier que vous avez cloné de votre fork.
21. Ajoutez le nouveau fichier avec `git add`.
22. `commit` pour ajouter vos changements.
23. Publier votre `commit` avec `git push origin` (ou `git push --set_upstream origin etape_1` pour associer la branche sur le repo distant si c'est votre premier push sur cette branche)
---