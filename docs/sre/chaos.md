### Chaos Engineering

#### Chaos Engineering, principes et mise en application (B. Gakic)

![type:video](https://www.youtube.com/embed/WTT2GJquAWY)

- 4 situations de complexité :
  - Simple
      - Des causes et des effets qui sont évidents
  - Compliqué
      - Les causes et les effets sont compliquées
      - 1 expert suffit
  - Complexe
      - Les systèmes compliqués interragissent entre eux
      - Besoin de plusieurs experts
  - Chaotiques
      - Impossible de rapprocher les causes et les effets
      - Situation d'urgence
      - Besoin d'agir en premier lieu

Les pratiques (~Chaos Engineering) visent à sortir du chaotique pour aller vers de la complexité ou du compliqué.

- Une notion importante Resilience:
  - Capacité à absorber une perturbation et continuer de fonctionner, même en mode dégradé.

- Le chaos engineering vise à
  - rendre le système plus résilient
  - absorber les pannes.
  - décrire plus finement les effets papillons dans le système d'information

- mais pas 
  - à tester en PROD

Pendant la mise en oeuvre du Chaos engineering (en PROD), nécessité d'observer les choses par un oeil humain, via les dashboards, après avoir injecté la pertubation. 

- Le chaos engineering est plus large que les tests logiciels:
  - simuler la perte d'un Data Center complet

Définition du Chaos engineering:
> Discipline de l'expérimentation sur un système distribué afin de renforcer la confiance dans la capacité du système à résister à des conditions turbulentes en production 

- Initiée par NetFlix en 2011, cf 
  - https://principleofchaos.org
  - https://chaos.community/broadcast/
  - https://www.amazon.com/Chaos-Engineering-System-Resiliency-Practice/dp/1492043869

- Demarche scientifique:
  - Que cherche t'on à prouver ?
  - Restreindre le périmètre
  - Communiquer (avec les exploitants)
  - Injecter le chaos
  - Analyser consciencieusement les impacts
  - **Recommencer**

- Pourquoi le chaos engineering:
  - La disponibilité est plus préoccupante que la sécurité.

- Prévenir la panne avant qu'elle n'arrive -> **Chaos Monkey**, un pattern plus qu'un outil.

- Un chaos monkey plus simple:
  - ```Kill -9```

Netflix Simian Army

25:20
> Le Chaos engineering est une démarche, comme toutes les démarches une action unique ne suffit pas

Game day, journée basée sur le volontariat, scenario basée sur un univers jeu video.
Les exploitants font des pannes, que les devs doivent détecter et réparer.
Question bonus. Prise de conscience de la part des devs, des besoins: Supervision et alerting, Tests techniques, Partages de connaissances, Arbres d'analyse.

Insuffler les pratiques par la Gamification.


https://github.com/Netflix/vizceral

#### Mastering Chaos - A Netflix Guide to Microservices

![type:video](https://www.youtube.com/embed/CZ3wIuvmHeM)