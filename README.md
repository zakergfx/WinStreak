# WinStreak
AI Gamified Calorie Tracker

# Why
J'ai toujours eu du mal à perdre du poids sur le long terme. En parallèle j'ai toujours été accroc aux jeux vidéos. Monter les rang, progresser, monter dans le classement c'est quelque chose qui m'a toujours plu. L'idée de l'application est donc de rendre la perte de poids amusante et sainement compétitive.

# How it works
## Tracking des calories
- Via une photo du repas
- Via une description du repas
- Via la dictée vocale
- Via une base de données (Données provenant d'OpenFoodFacts)

## Historique
Chaque semaine, mesurez votre évolution, surveillez votre IMC, et constatez vos progrès en temps réel.

## Gamification
Un système de streak vous pousse à ne jamais abandonner. Chaque fois que vous vous tenez à votre max calorique, vous gagner 1 point dans le streak.

Le système de classement vous fait monter en rang chaque fois que vous respectez votre total calorique quotidien. Attention à ne pas craquer si vous ne voulez pas perdre de points.

# Code
Code is not available pulbicly since it is an app that is currently in development in aim to grow and generate revenue. But here are the technologies used.

## Frontend
### React Native
J'utilise React Native pour gérer le frontend. Ca me permet de développer à la fois sur IOS et Android avec une majorité de code en commun.
### Expo
J'utilise Expo pour packager facilement l'application en .ipa et .aab et pouvoir les publier sur l'App Store et Play Store.

## Backend
### Django
J'utilise Django (Framework Python) pour gérer le backend. L'authentification Jwt intégrée permet de gérer l'authentification simplement sans réinventer la roue, et Le système de serializers et d'ORM permet de facilement communiquer avec le Frontend et la base de données.
###PostgreSQL
La DB utilisée est PostgreSQL. Le système d'indexation trigramme m'a permis d'avoir une base de données d'aliments qui répond en 10-50 ms malgré les 200 000 entrées.
