# VMC_Helios_to_mqtt
Passerelle MODBUS TCP / MQTT pour VMC Helios sous node-red

![alt text](https://raw.githubusercontent.com/NextDom/VMC_Helios_to_mqtt/master/Helios_to_mqtt.png)

## Description

Passerelle permettant la communication entre une VMC Hélios et un système domotique (NextDom, Jeedom, ...) au travers du protocole MQTT. Dans le principe, il doit être possible de communiquer avec d'autres produits Hélios disposant de l'interface easyControls.
Il est possible de récupérer des données dans la VMC, comme de lui envoyer des commandes (vitesse, mode manu ou auto, mode absence, etc...).
La liste des données accessibles en lecture / écriture, via l'interface easyControls des VMC hélios, est disponible sur le site [easyControls.net](https://www.easycontrols.net/de/service/downloads/send/4-software/16-modbus-dokumentation-f%C3%BCr-kwl-easycontrols-ger%C3%A4te).

## Prérequis

Afin de faire fonctionner la passerelle, les prérequis sont les suivants:
- Avoir installer un brocker MQTT ([mosquitto](https://mosquitto.org/), [activemq](https://activemq.apache.org/), etc).
- Avoir installer node-red ([node-red](https://nodered.org/)).
- Avoir installer les palettes supplémentaires suivantes dans node-red:
  - [node-red-contrib-modbus](https://www.npmjs.com/package/node-red-contrib-modbus).
  - [node-red-contrib-actionflows](https://www.npmjs.com/package/node-red-contrib-actionflows).
  - [node-red-contrib-simple-message-queue](https://www.npmjs.com/package/node-red-contrib-simple-message-queue).
  - [node-red-node-rbe](https://www.npmjs.com/package/node-red-node-rbe).
  

## Installation

- Importer le fichier [Helios_to_mqtt](https://github.com/NextDom/VMC_Helios_to_mqtt/blob/master/Helios_to_mqtt.json) dans node-red.
- Editer le bloc `write data to VMC`, puis éditer les paramètres du serveur `VMC`. Renseigner l'adresse IP de votre VMC.
- Editer le bloc `send mqtt messages`, puis éditer les paramètres du serveur `mqtt`. Renseigner l'adresse IP de votre brocker mqtt.
- Déployer le flow.

## Envoi de commandes à la VMC

### Topic d'émission des commandes

Les commandes de lecture ou d'écriture à destination de la VMC sont à publier sous le topic `vmc/commandes`. Il s'agit d'une payload json, qui peut regrouper une ou plusieurs commandes, en lecture comme en écriture. La passerelle se chargera de décomposer les commandes reçues et de séquencer leur envoi à la VMC.

### Struture d'une commande

Une commande est contituée de la façon suivante:
```
{
    "a": {
        "variable": "v00101",
        "registers": 5,
        "value": "0",
        "type": "number",
        "topic": ""
    }
}
```
La description des clés est la suivante:
- `variable` : clé obligatoire renseignant la variable à lire ou à écrire dans la VMC (cf doc easyControls pour la liste complète des variables).
- `registers` : clé obligatoire renseignant le nombre de registres Modbus à lire ou écrire. Ce paramètre correspond à la colonne `count` du tableau de variables (cf doc easyControls).
- `value`: cette clé est optionnelle et n'est nécessaire que pour mettre à jour une variable de la VMC. La valeur doit être renseignée de façon cohérente par rapport au tableau de variables.
- `type`: clé optionnelle, qui permet de retourner la valeur lue dans la vmc soit au format `string` (chaîne de caratère, valeur par défaut) soit au format `number` (nombre). 
- `topic`: clé optionnelle, qui permet de personnaliser le topic de retour de la variable lue. Par défaut, le topic de retour sera `vmc\commandes\nom_de_la_variable`. Si une valeur est renseignée pour la clé `topic`, le topic de retour sera `vmc\commandes\valeur_de_topic`.

Il est possible de passer plusieurs commandes en même temps, dans la même payload, par exemple:
```
{
    "a": {
        "variable": "v00101",
        "registers": 5,
        "value": "0",
    },
    "b": {
        "variable": "v00102",
        "registers": 5,
        "type": "number",
        "topic": "vitesse"
    }
}
```

## Avertissement

`La passerelle n'effectue aucun contrôle sur les valeurs que vous pourriez écrire dans la VMC. Il est de votre responsabilté de vous assurez que les valeurs à écrire sont conformes au tableau des variables easyControls (cf doc easyControls).`

## Remerciements

Je remercie xavax et lionel68, pour avoir servis de cobayes pour les tests et le débogage.

  
