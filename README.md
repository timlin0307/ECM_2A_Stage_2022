# **2A Stage 2022**
###### tags: `Jetson NANO Projet`
Créé par : Yen-Ting Lin
Tuteur (AMU - Laboratoire d’Informatique et Systèmes, LIS) : Seifeddine BEN ELGHALI
Tuteur (Ecole Centrale Marseille, ECM) : Laetitia ABEL-TIBERINI

----------------
## **Table Des Matières**
[TOC]

----------------
## **Introduction**
- **Intitulé du stage**
:::success
Conception et développement d'une carte Jetson Nano basée sur une skycamera à faible coût
:::

- **Durée du stage**
:::warning
De 27/06/2022 à 21/08/2022
:::

- **Descriptif de la mission**
:::info
L’objectif principale de ce stage est de développer une « skycamera » qui permet de prendre en photo le ciel chaque minute en synchronisation avec le capteur de l’énergie solaire capté par une petite cellule photovoltaïque. Cet équipement permettra de surveiller la production dans les champs photovoltaïque et détecter des défauts et/ou des pertes d’efficacité.
:::

----------------
## **Travails**
1. [Configuration de Jetson NANO](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/BJHzEBu59)
2. [Jetson NANO : Connexion à distance (SSH)](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/r1d2L5O55)
3. [Jetson NANO : Utiliser CSI RPi Camera V2](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/SkYjr6K5q)
4. [Jetson NANO : Utiliser USB Camera](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/Hy_8eJs5c)
5. [Installation Ubuntu avec VirtualBox](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/HJh7Zq39q)
6. [Jetson NANO : Python et GPIO](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/HksHoJscq)
7. [Jetson NANO : Utiliser Capteur BME680](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/Byshh0c9c)
8. [Jetson NANO : Utiliser CSI RPi Camera HQ (Méthode 1)](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/Hk5h0kqc9)
9. [Jetson NANO : Utiliser CSI RPi Camera HQ (Méthode 2)](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/rkFWNefi9)
10. [Jetson NANO : Camera HQ Py-Contrôleur (Méthode 1)](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/HkHkKXni5)
11. [Jetson NANO : Camera HQ Py-Contrôleur (Méthode 2)](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/Byph1P3jq)
13. [Jetson NANO : Py-Contrôleur with BME680](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/H1C-ZlJnc)
14. [Jetson NANO : Réseau portable connexion](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/rykj9Gu2q)
15. [Jetson NANO : Sauver les données XLSX](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/BygIVKST5)
16. [Authentification de Drive API](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/rJdsF58p9)
17. [Jetson NANO : Télécharger dans le cloud](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/Sk7NCdS65)
18. [Combinaison de Camera, BME680 et XLSX](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/BkMkh7wp5)
19. [Télécharger dans le cloud - avancé](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/ryZx4vvTq)
20. [Modification du ventilateur](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/ryuJp4rnq)
21. [Jetson NANO : Contrôler le ventilateur](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/rJXeGwt3c)
22. [Fabrication de la résistance chauffante](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/SyPai8r3c)
23. [Jetson NANO : Contrôler la résistance chauffante](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/H17mcuhp9)
24. [Jetson NANO : Les voyant lumineux](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/Hk5UjJIaq)
25. [Cam, BME680, XLSX, Cloud, Rés_chf, Voy, Venti](https://hackmd.io/@8KbRc796SnuYA2Dvsvk_BA/HJtEYHxCq)

----------------
## **Résultats**
- Schéma de connexion du circuit

![](https://i.imgur.com/jmdfilS.png)

- Image de connexion du circuit

![](https://i.imgur.com/frOFnaQ.jpg)

- Organigramme du programme init.py
```flow
st=>start: Start
para=>parallel: parallel tasks
op1=>operation: check current time
cond1=>condition: Is current
time 00:00?
op2=>operation: get sunrise time
get sunset time
flag = 0
op3=>operation: check current time
cond2=>condition: Current time =
sunrise time?
cond3=>condition: flag = 0?
op4=>operation: execute camera.py
execute sensor.py
flag = 1
cond4=>condition: Current time =
sunset time?
cond5=>condition: flag = 1?
op5=>operation: stop camera.py
stop sensor.py
execute gpio.py
execute upload.py
flag = 2

st->para
para(path1, left)->op1->cond1
para(path2, right)->op3->cond2
cond1(yes)->op2(right)->op1
cond1(no)->op1
cond2(yes)->cond3
cond2(no)->cond4
cond3(yes)->op4(right)->op3
cond3(no)->op3
cond4(yes)->cond5
cond4(no, top)->op3
cond5(yes)->op5(right)->op3
cond5(no)->op3
```

- Organigramme du programme camera.py
```flow
st=>start: Start
op1=>operation: initialize BME680
initialize camera
initialize xlsx
cond1=>condition: Is camera
still opened?
cond2=>condition: Does camera
work well?
op2=>operation: read image from
camera (in stream)
op3=>operation: count time
cond3=>condition: Is time
count = 60?
op4=>operation: save current image and
write averaged sensor
data in xlsx file
op5=>operation: time counter = 0
cond4=>condition: Is time count =
multiples of 10?
op6=>operation: save sensor data
op7=>operation: release camera
e=>end: End

st->op1->cond1
cond1(yes, right)->cond2
cond1(no, bottom)->e
cond2(yes, right)->op2->op3->cond3
cond2(no, bottom)->op7(left)->e
cond3(yes)->op4->op5(left)->op3
cond3(no)->cond4
cond4(yes)->op6(right)->op3
cond4(no)->op3
```

- Organigramme du programme sensor.py
```flow
st=>start: Start
op1=>operation: initialize BME680
initialize GPIO
define GPIO pins
cond1=>condition: Does time elapse
0.25 second?
op2=>operation: read data from sensor
cond2=>condition: Is temp >=
35 degrees?
op3=>operation: set fan PWM 25%
set heater off
only set LED1 on
cond3=>condition: Is temp >=
30 degrees?
op4=>operation: set fan PWM 50%
set heater off
only set LED2 on
cond4=>condition: Is temp >=
25 degrees?
op5=>operation: set fan PWM 75%
set heater off
only set LED3 on
cond5=>condition: Is temp <
20 degrees?
op6=>operation: set fan off
set heater on
only set 

st->op1(right)->cond1
cond1(yes, bottom)->op2->cond2
cond1(no, right)->cond1
cond2(yes, bottom)->op3(right)->cond1
cond2(no, right)->cond3
cond3(yes, bottom)->op4(right)->cond1
cond3(no, right)->cond4
cond4(yes, bottom)->op5(right)->cond1
cond4(no, right)->cond5
cond5(yes, bottom)->op6(right)->cond1
cond5(no, right)->cond1
```

- Organigramme du programme gpio.py
```flow
st=>start: Start
op1=>operation: initialize GPIO
define GPIO pins
op2=>operation: cleap up
all GPIO pins
e=>end: End

st(right)->op1(bottom)->op2(right)->e
```

- Organigramme du programme upload.py
```flow
st=>start: Start
op1=>operation: initialize pydrive
op2=>operation: list the folder
list all files from folder
op3=>operation: create folder in drive
cond1=>condition: Has all files
been uploaded yet?
op4=>operation: upload files in
the folder just
created in drive
op5=>operation: delete whole folder
e=>end: End

st(right)->op1(right)->op2(bottom)->op3->cond1
cond1(yes)->op5->e
cond1(no)->op4(top)->cond1
```

----------------
## **Rapports**
