# PoopyHealth

Notre entreprise vise à permettre aux utilisateurs de mieux comprendre son corps via ses selles et de détecter les maladies avant qu'il ne soit trop tard.

## Le projet : 

Savez vous qu'une analyse d’urine peut révéler nos habitudes alimentaires ou de sommeil, notre propension à l'activité physique ou à la consommation de médicaments ? Elle peut aussi aider à déceler plus de 600 maladies comme le cancer, le diabète ou des maladies du rein. C'est pourquoi les chercheurs se sont demandé si une surveillance étroite des urines d'un individu pourrait fournir des informations utiles  sur sa santé en temps réel.

Ce projet vise donc à commercialiser les premières toilettes connectées pour analyser les selles et l'urine ce qui permet de détecter, par exemple, un début de cancer colorectal, une infection urinaire ou des problèmes rénaux. 
Or en matière de santé, plus une maladie est détectée tôt, plus on a de chance de la guérir. En faisant une analyse systématique, à chaque fois que l’on va aux petits coins, cela permet de suivre en permanence notre état de santé, sans avoir à faire d’effort en particulier. 

L’utilisateur est détécté via empreinte digitale et doit au préalable créer son compte et indiquer son sexe. 
Les capteurs envoient info, communiquent avec une base de données, puis renvoient l’info sur le compte utilisateur via une application mobile.
Les WC n’ont pas besoin de branchement, tout marche via une batterie rechargeable. 
Les capteurs d’uroflowmetrie et les biomarqueurs fécaux permettent de savoir si l’utilisateur a une maladie, une carence quelconque, une grossesse ou autre.


## Etat de l'art : 

Plusieurs projets de toilettes connectées existent. Cependant, la plupart concernent les toilettes japonaises. On a pu retrouver plusieurs projets de toilettes japonaises (chasse d’eau auto, jet de nettoyage, cuvette chauffante, séchage, spray désodorisant etc. ) de différentes entreprises (Leroy Merlin, Xiaomi, Reuter, Homary etc.) 
Par exemple, le projet de Xiaomi concerne des toilettes japonaises connectées avec: 
un siège chauffant 
un petit jet pour le derrière (mixte) et pour l’avant (dames) ;
une lunette qui se relève et/ou se baisse automatiquement selon si on lui fait face ou tourne le dos.
On retrouve également d’autres technologies sur d’autres toilettes : éclairage led, séchage, rinçage etc. 

Il existe néanmoins 2 projets pour analyser les selles et détecter les maladies mais qui n’ont jamais été commercialisés (ou pas encore), seulement des prototypes avec quelques tests sur des volontaires. Mais le sujet n’a pas été approfondi.

## Matériels utilisés : 

Composants :

- Toilettes (fonctionnement habituel)
- Boitier d’alimentation : Batterie et Capteur de niveau de charge de la batterie
- Led (3 couleurs)
- Bouton ON/OFF
- Capteurs : 
1. Empreinte digitale 
2. Capteur d’urine (technologie d’uroflowmetrie)
3. Capteur de selles (biomarqueurs fécaux)
4. Capteur wifi

## Conception 3D : 



<img width="441" alt="image" src="https://user-images.githubusercontent.com/89772039/219391909-d8b3341d-c05a-4f1e-9d86-c47c2c2e7589.png">


<img width="428" alt="image" src="https://user-images.githubusercontent.com/89772039/219391621-8584aef1-7a2d-4ae6-befe-21e4f9ae457a.png">

## Conception Arduino 


<img width="962" alt="image" src="https://user-images.githubusercontent.com/89772039/219408656-27a89e26-fe45-4e99-9947-16188652a5f4.png">


## Code : 

// C++ code
#include <Adafruit_NeoPixel.h>
#define battery A3
#define button A0
#define capteurEmpreinte A1
#define capteurSelles A2
#define redLight  2
#define blueLight  7

void setup()
{
  //Initilaisation des composants
  pinMode(battery, INPUT);
  pinMode(button,INPUT_PULLUP); 
  pinMode(capteurEmpreinte, INPUT);
  pinMode(capteurSelles, INPUT);
  pinMode(redLight, OUTPUT);
  pinMode(blueLight, OUTPUT);
  Serial.begin(9600);
}
void loop()
{
  int batteryValue = analogRead(battery);
  int btnValue = analogRead(button); 
  int capteurEmpreinteValue = analogRead(capteurEmpreinte);
  int capteurSellesValue = analogRead(capteurSelles);  
  //Batterie suffisante
  if(batteryValue >= 50){    
    //Analyse de l'empreinte
  	if(capteurEmpreinteValue > 1013){
    	digitalWrite(redLight,LOW);
    	digitalWrite(blueLight,HIGH);
      
      //Pression du bouton
      if(btnValue < 200){
    	if(capteurSellesValue >=0 && capteurSellesValue <= 70){
          	Serial.println("Taux d'emoglobine : " << capteurSellesValue);
      		Serial.println("Vous êtes en bonne santé");
    	}
    	else if(capteurSellesValue >=71 && capteurSellesValue <= 150){
          	Serial.println("Taux d'emoglobine : " << capteurSellesValue);
      		Serial.println("D'après l'analyse de votre caca, vous manquez de fer et de vitamine C, surveillez-votre alimentation.");
    	}
    	else if(capteurSellesValue >=151 && capteurSellesValue <= 250){
      		Serial.println("Votre Etat de santé est inquiétant, vous devriez consulté votre médecin");
    	}
    	else if(capteurSellesValue >=251){
          	Serial.println("Taux d'emoglobine : " + capteurSellesValue);
      		Serial.println("Vous avez une gastro ou avez trop bu la veille (Attention l'alcool est dangereux pour la santé, à consommé avec modération");
    	}
      }
      else{
        Serial.println("Appuyez sur le bouton pour analyser vos selles");
      }
  	}
  	else{
    	digitalWrite(redLight,HIGH);
    	digitalWrite(blueLight,LOW);
  	}
  } 
  else{
    Serial.println("Batterie insuffisante pour démarer le service. Recharger svp.");
  }
  delay(3000);//Actualisation de la mesure toutes les secondes 
}



# Exemple de Front proposé : 

<img width="495" alt="image" src="https://user-images.githubusercontent.com/89772039/219393631-1e0c40f9-803a-4fbc-a151-6133f9a105c4.png">

<img width="435" alt="image" src="https://user-images.githubusercontent.com/89772039/219393770-52ea079f-501c-4293-9de3-73329d239576.png">


<img width="431" alt="image" src="https://user-images.githubusercontent.com/89772039/219393870-cfa04ad9-6de3-4ced-acb2-80f09c3d3c8f.png">




