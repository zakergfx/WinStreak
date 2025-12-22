# WinStreak  
**AI Gamified Calorie Tracker**

## Why
J'ai toujours eu du mal à perdre du poids sur le long terme. En parallèle j'ai toujours été accroc aux jeux vidéos. Monter les rang, progresser, monter dans le classement c'est quelque chose qui m'a toujours plu.  
L'idée de l'application est donc de rendre la perte de poids amusante et sainement compétitive.

---

## How it works

### Tracking des calories

Via une photo du repas  

<img width="292" height="633" alt="IMG_0644" src="https://github.com/user-attachments/assets/d52436f2-854f-44f0-ae2e-f0848da1597e" />

Via une description textuelle ou vocale du repas  

<img width="292" height="633" alt="IMG_0645" src="https://github.com/user-attachments/assets/4353e25f-6545-44f7-9833-75fd169e6a02" />

Via une base de données (Données provenant d'OpenFoodFacts)  

<img width="292" height="633" alt="IMG_0646" src="https://github.com/user-attachments/assets/ea590e78-46e8-4334-9da0-ffc6f44dcd1c" />

---

### Historique
Chaque semaine, mesurez votre évolution, surveillez votre IMC, et constatez vos progrès en temps réel.

<img width="292" height="633" alt="IMG_0647" src="https://github.com/user-attachments/assets/0c7491df-a573-4165-94e6-8bd50720ee01" />

---

### Gamification

Un système de streak vous pousse à ne jamais abandonner.  
Chaque fois que vous vous tenez à votre max calorique, vous gagner 1 point dans le streak.

<img width="292" height="633" alt="IMG_0649" src="https://github.com/user-attachments/assets/d34f9116-3eef-4039-a66d-885c56b20689" />

Un système de classement vous fait monter en rang chaque fois que vous respectez votre total calorique quotidien.  
Attention à ne pas craquer si vous ne voulez pas perdre de points.

<img width="292" height="633" alt="IMG_0648" src="https://github.com/user-attachments/assets/8f2c70f8-d550-4cc9-b801-4baabb8474a0" />

<img width="292" height="633" alt="IMG_0650" src="https://github.com/user-attachments/assets/775bb718-bb7f-45ea-9f52-d6425ab958f2" />

Un système de trophées permet de motiver les complétionistes à continuer d'utiliser l'application.

<img width="292" height="633" alt="IMG_0651" src="https://github.com/user-attachments/assets/17d634af-db81-4097-9c37-64eb3a392cd7" />

---

## Code
Le code n’est pas disponible publiquement car l’application est actuellement en développement avec un objectif de croissance et de monétisation.  
Voici cependant les technologies utilisées.

---

## Frontend

### React Native
J'utilise React Native pour gérer le frontend.  
Cela me permet de développer à la fois sur iOS et Android avec une majorité de code en commun.

### Expo
J'utilise Expo pour packager facilement l'application en `.ipa` et `.aab` et pouvoir les publier sur l’App Store et le Play Store.

---

## Backend

### Django
J'utilise Django (framework Python) pour gérer le backend.  
L’authentification JWT intégrée permet de gérer l’authentification simplement sans réinventer la roue, et le système de serializers et d’ORM permet de facilement communiquer avec le frontend et la base de données.

### PostgreSQL
La base de données utilisée est PostgreSQL.  
Le système d’indexation trigramme m’a permis d’avoir une base d’aliments qui répond en **10–50 ms** malgré plus de **200 000 entrées**.

### RabbitMQ
RabbitMQ permet de gérer les tâches planifiées et ordonnées via **django-celery**.

---

## Télécharger

**iOS**  
https://apps.apple.com/be/app/streakgg/id6754155267?l=fr-FR  

**Android**  
https://play.google.com/store/apps/details?id=com.anonymous.calcounter&hl=fr  
