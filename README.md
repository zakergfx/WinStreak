# WinStreak  
**Gamified AI Calorie Tracker**

## Why
I’ve always struggled to lose weight over the long term. At the same time, I’ve always been addicted to video games. Climbing ranks, progressing, and moving up leaderboards is something I’ve always enjoyed.  
The idea behind this app is to make weight loss fun and healthily competitive.

---

## How it works

### Calorie tracking

Via a photo of your meal  

<img width="292" height="633" alt="IMG_0644" src="https://github.com/user-attachments/assets/d52436f2-854f-44f0-ae2e-f0848da1597e" />

Via a text or voice description of the meal  

<img width="292" height="633" alt="IMG_0645" src="https://github.com/user-attachments/assets/4353e25f-6545-44f7-9833-75fd169e6a02" />

Via a food database (data provided by OpenFoodFacts)  

<img width="292" height="633" alt="IMG_0646" src="https://github.com/user-attachments/assets/ea590e78-46e8-4334-9da0-ffc6f44dcd1c" />

---

### History
Each week, track your progress, monitor your BMI, and see your improvements in real time.

<img width="292" height="633" alt="IMG_0647" src="https://github.com/user-attachments/assets/0c7491df-a573-4165-94e6-8bd50720ee01" />

---

### Gamification

A streak system pushes you to never give up.  
Every time you stay within your daily calorie limit, you earn 1 streak point.

<img width="292" height="633" alt="IMG_0649" src="https://github.com/user-attachments/assets/d34f9116-3eef-4039-a66d-885c56b20689" />

A ranking system lets you climb ranks every time you respect your daily calorie target.  
Be careful not to slip up if you don’t want to lose points.

<img width="292" height="633" alt="IMG_0648" src="https://github.com/user-attachments/assets/8f2c70f8-d550-4cc9-b801-4baabb8474a0" />

<img width="292" height="633" alt="IMG_0650" src="https://github.com/user-attachments/assets/775bb718-bb7f-45ea-9f52-d6425ab958f2" />

A trophy system motivates completionists to keep using the app.

<img width="292" height="633" alt="IMG_0651" src="https://github.com/user-attachments/assets/17d634af-db81-4097-9c37-64eb3a392cd7" />

---

## Code
The code is not publicly available since the app is currently in development with the goal of growing and generating revenue.  
However, here are the technologies used.

---

## Frontend

### React Native
I use React Native to handle the frontend.  
It allows me to develop for both iOS and Android with most of the code shared.

### Expo
I use Expo to easily package the app into `.ipa` and `.aab` files and publish it on the App Store and Play Store.

---

## Backend

### Django
I use Django (a Python framework) for the backend.  
Built-in JWT authentication makes it easy to handle auth without reinventing the wheel, and the serializer and ORM systems make communication with the frontend and the database straightforward.

### PostgreSQL
The database used is PostgreSQL.  
The trigram indexing system allows the food database to respond in **10–50 ms** despite having more than **200,000 entries**.

### RabbitMQ
RabbitMQ is used to handle scheduled and ordered tasks via **django-celery**.

---

## Download

**iOS**  
https://apps.apple.com/be/app/streakgg/id6754155267?l=fr-FR  

**Android**  
https://play.google.com/store/apps/details?id=com.anonymous.calcounter&hl=fr  
