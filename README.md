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
### Struture d'une commande




  
