# Projet 2 "Détection et classification des sons"
###### Réalisé par : *Zaynab ROMENE*
#
Le but de ce projet est de détecter les mots suivants: " résistance" , "transistor", "FPGA"," microcontrôleur" pour faire clignoter la led graduellement à chaque fois qu’on prononce un mot particulier. Ceci est fait à l'aide d'un modèle d'intellignece artificielle qu'on va mettre en place dans ce même TP.

## 1- Matériel utilisé: 
#### 1-1- Hardware 
Nous utilisons le Kit d'apprentissage Tiny ML d'arduino pour explorer l'Embedded Machine Learning.C'est une nouvelle technologie qui active l'intelligence artificielle juste à côté du monde physique à l'aide des données récupérées par les capteurs. Ce kit comprend un Arduino Nano 33 BLE Sense. Il est basé sur le microcontrôleur nRF52840 et fonctionne sur le système d'exploitation Arm® Mbed™. Le Nano 33 BLE Sense offre non seulement la possibilité de se connecter via Bluetooth® Low Energy, mais est également équipée de capteurs pour détecter la couleur, la proximité, le mouvement, la température, l'humidité, l'audio et plus encore. Vous pouvez cliquer sur l'image ci-dessous pour plus de détails ou bien il suffit de lire [le datasheet de la carte][df4]. 
[![N|Solid](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ7IfVcPvM7SwqYIDul2PXhhBmPBYTT7S1rNZ-sMr3BMiu8tbeMrWcBBKtpYS2mg7CYHs4&usqp=CAU)](https://docs.arduino.cc/hardware/nano-33-ble-sense) 
Le processeur est caractérisé par : 
* 64 MHz Arm® Cortex-M4F (with FPU)
* 1 MB Flash + 256 KB RAM

Ce qui nous intéresse dans ce projet c'est le microphone. Le microphone de notre carte est le MP34DT05. Commen mentionné dans son [datasheet][df1] , le MP34DT05 permet de détecter et d'analyser le son en temps réel et peut être utilisé pour créer une interface vocale.
![] (MP34DT05.png "La capteur MP34DT05")
Le MP34DT05 est un microphone MEMS numérique omnidirectionnel ultra-compact à faible consommation avec une interface IC.
- Rapport signal/bruit : 64 dB
- Sensibilité : -26dBFS ±3dB (capable de détecter les ondes acoustiques ) 
- Plage de température : -40 à 85°C

#### 1-2- Software
Edge Impulse c'est un site qui permet de faire l'acquisition des données qui serviront à entrainer une IA. L'acquisition peut être faite à l'aide des capteurs du capteur ou bien par plusieurs autres alternatives.En proposant, une approche simplifiée de l'intelligence artificielle à l'aide des blocs, il permet d'entrainer le modèle avec la possibilité de personnaliser les paramètres. Ensuite, il fournit le code à implanter dans le microcontrôleur afin d'utiliser le réseau entrainé ous peuvent avoir plus d'informations en cliquant sur l'image suivante:
[![N|Solid](data:image/png;base64,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)](https://www.edgeimpulse.com/) 

Bien évidemment , on utilise l'IDE de la carte Arduino pour la programmation de la carte et pour utiliser le modèle entrainé. Vous pouvez avoir plus d'informations en cliquant sur l'image suivante:
[![N|Solid](data:image/png;base64,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)](https://www.arduino.cc/)

## 2- Les étapes : 
### 2-1- Collecter, traiter et tester la data 
On commence par créer un projet Edge Impulse. Pour acquérir les données, on peut utiliser plusieurs méthodes concrètement le téléphone ou bien notre carte nano 33 ble sense. On opte pour le microphone MP34DT05 de la carte. 
#### 2-1-1- Connecter la carte au compte Edge Impulse
Il faut créer un compte Edge impulse et connecter la carte en utilisant cette [ vidéo Youtube][df3] et ce [document][df2]. Ceci est fait en téléchargeant le fichier compressé de Edge Impulse sur le PC et puis la dernière version du firmware dans la carte. Une fois le "flashing" est terminé, on appuie une fois sur le bouton RESET pour lancer le nouveau firmware. On lance ensuite la commande dans le terminal :
```sh
edge-impulse-daemon
```
Ceci va nous permettre de connecter la carte directement au compte Edge Impulse en écrivant juste l'adresse et le mot de passe. Par la suite, la carte sera visible sur le tableau de bord d'Edge Impulse.
![] (device connected.png "Zaynab's Arduino Nano 33 BLE Sense")
#### 2-1-2- Enregistrer les mots
Une fois la carte est connecter au tableau de bord d'e 'Edge Impulse, il devient possible d'utiliser le MP34DT05 pour enregistrer les mots " résistance" , "transistor", "FPGA"," microcontrôleur". On donne à chaque catégorie le label correspondant. 
Il s'est avéré que changer la voix ou d'enregistrer les mots à l'aide de plusieurs personnes peut aboutir à une base donnée plus performante. J'ai choisi d'enregistrer 10 mots sur 10s en fixant le **sampling length** à 10s.Puis, j'ai divisé les morceaux sur 10 pour obtenir un mot à chaque 1s d'audio. La quantité de data qui peut-être capturée d'un seul coup varie selon la carte utilisée. La mémoire de notre carte Arduino Nano 33 BLE Sense nous permet d'enregistrer 16s en un chaque essai. 
Pour rendre la base de données plus variée et plus fonctionnelle on ajoute là-dessus une autre base de données de bruit conçue spécialement  pour la détection des mots-clés. Lors de l'enregistrement des nouveaux mots, les données seront ajoutées directement à la base d'entraînement. Pour équilibrer les pourcentages de données entre la base d'entraînement et la base de test, on click sur >  `dashboard` --> `rebalacing data`
Personnellement, j'ai collecté juste 5 min de data avec environ 1 min de chaque label :  noise, résistance, FPGA, microcontrôleur, transistor.
### 2-2- Paramétrer , entraîner et télècharger le modèle 
Une fois, les données collectées, on passe au paramétrage dans la section Impulse design. Pour préparer le modèle, on ajoute: 
- un bloc de traitement " a processing bloc" : J'ai choisi MFCC bloc qui est dédié à la parole humaine. 
- un bloc d'apprentissage " a learning bloc" : J'ai choisi Classification bloc qui permet de trouver des patterns dans la data et les appliquer aux nouvelles données.

![] blocs.png "Les deux blocs"

Après l'enregistrement de ces paramètres, on  vérifie le fonctionnement prévu du modèle sur la carte BLE sens autrement dit la rapidité d l'exécution du modèle sur la carte. On trouve ces informations dans la section de MFCC.
Pour notre cas, le modèle va prendre 17 kb de la mémoire et va s'exécuter dans 177ms. Ceci est négligeable puisque la carte contient 256kb de RAM. Et même l'exécution est considérée rapide. Ces résultats varient selon la quantité des données.
![] (On-device performance.png "On-device performance"
On génère ensuite les features de notre base de données et on visualise la data en 3D pour vérifier que les données sont bien séparées ou bien s'il y a des problèmes à corriger.
![] (problems detection.png "Problèmes de classification"
On peut détecter les problèmes de classification et les corriger en les supprimant ou bien en changeant le label. 

On passe ensuite au classifier, où on va entraîner le modèle à distinguer entre les différentes classes. J'ai laissé les paramètres par défaut.
![] (results.png “Les résultats”

Une fois le modèle testé et corrigé, on passe au déploiement. On télécharge le projet sous forme de bibliothèques Arduino en ajoutant ou désactivant l'optimisation.
![] (build.png “Déploiement”

### 2-3- Allumer La LED L graduellement
 Puis, on l'ajoute dans l'IDE et on ouvre le sketch *nano_ble33_sense_microphone_continuous* afin de tester notre modèle avant d'ajouter la partie de la LED. 
Une fois le programme fonctionne et donne les probabilités comme suit: 
![] (without Led.png “Détection de mots” 
En respectant l'énoncé, j'ai graduellement clignoté le LED pour un seul mot particulier " transistor" en utilisant le code suivant;
```sh
   if(result.classification[4].value > 0.7) {
      for(int i=0; i<=40; i++){
         digitalWrite(led_pin ,LOW);
         delay(i*10);
         digitalWrite(led_pin ,HIGH);
         delay(i*10);
      }
   }else {
    digitalWrite(led_pin ,LOW);  
   }
```
## 3- Résultat : 
On a clignoté la LED L  graduellement en respectant l'énoncé. On pourra, toutefois, utiliser le LED RGB qui existe sur la carte et la clignoter graduellement en utilisant une différente couleur pour chaque mot.
On a réussi alors à détecter les mots en clignotant la Led pour un mot particulier ( j'ai choisi transistor). Autrement dit, le modèle de machine Learning entrainer sur Edge Impulse fonctionne parfaitement même après l'adaptation du programme.
## Ressources :
- https://docs.arduino.cc/tutorials/nano-33-ble-sense/edge-impulse 
- https://docs.arduino.cc/tutorials/nano-33-ble-sense/microphone-sensor 
- https://www.youtube.com/watch?v=vbIg4Up1Ts0&t=1094s 
- https://docs.edgeimpulse.com/docs/tutorials/audio-classification
- https://docs.edgeimpulse.com/docs/development-platforms/officially-supported-mcu-targets/arduino-nano-33-ble-sense 
- https://www.youtube.com/watch?v=ERoQiQhJ38U 
- https://www.youtube.com/watch?v=muIe9IAI4-A
- https://www.youtube.com/watch?v=FseGCn-oBA0&list=PL7VEa1KauMQp9bQdo2jLlJCdzprWkc7zC 
- https://www.deviceplus.com/arduino/the-basics-of-arduino-adjusting-led-brightness/#:~:text=You%20can%20easily%20switch%20an,use%20the%20%E2%80%9CPWM%E2%80%9D%20output.
- https://pijaeducation.com/arduino/how-to-take-output-from-arduino-uno/led-brightness-control-using-arduino/

[df1]: https://content.arduino.cc/assets/Nano_BLE_Sense_mp34dt05-a.pdf?_gl=1*b34798*_ga*MTg0NTMwMTQ0NC4xNjcwNDMwOTEw*_ga_NEXN8H46L5*MTY3Mjk0MjIyOS45LjEuMTY3Mjk0NzQxMy4wLjAuMA..
[df2]:https://docs.edgeimpulse.com/docs/development-platforms/officially-supported-mcu-targets/arduino-nano-33-ble-sense
[df3]: https://www.youtube.com/watch?v=wOkMZUaPLUM 
[df4]: https://docs.arduino.cc/static/bdb53f29f29a67b0df0243b265617e7b/ABX00031-datasheet.pdf
