### Chaos Engineering, principes et mise en application (B. Gakic)

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
  - ==Recommencer==

- Pourquoi le chaos engineering:
  - La disponibilité est plus préoccupante que la sécurité.

- Prévenir la panne avant qu'elle n'arrive -> ==Chaos Monkey==, un pattern plus qu'un outil.

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

### Mastering Chaos - A Netflix Guide to Microservices - Josh Evans

![type:video](https://www.youtube.com/embed/CZ3wIuvmHeM)

#### Challenges and solutions


##### Dependency

- Intra-Service requests
  - from MicroService A to MicroService B in order to fullfill a client request
  - Crossing the chasm
    - Network latency
    - Congestion
    - Failure
    - Logical or scaling failure
- Cascading failure
- Hystrix
  - Timeout retries
  - ==Fallback==
  - Isolated thread pools
  - Failfast
  - ==Circuit breaker==
- FIT: Netflix Fault injection testing
  - Synthetic transactions
  - Override by device or account
  - % of live traffic up to 100%
  - ==Flags critical micro services==
    - Instead of testing every service combination
- Client Libraries
  - Simple logic

##### Persistence

- CAP Theorem
  - In the presence of network partition, you must choose between consistency and availability
- Cassandra Local Quorum
  - In case a transaction is performed on multiple nodes, but only a subset acknowledged it.
  - Reconciliation
  - ==Eventual consistency==
- Infrastructure
  - Don't put all eggs in one basket
    - ==Multi-Region strategy==

##### Scale 

- Stateless Services
  - Not a cache or a database
  - Frequently access metadata
  - No instance affinity
  - Loss of a node is a non-event
  - ==AutoScaling Groups==
    - Compute efficiency
      - Surviving instance failure
  - Chaos under load

- Stateful Services
  - Databases and caches
  - Customs apps that holds large amount of data
    - avoid it in case of replication
    - Spof
  - ==Redundancy== is fundamental
    - EVCache wrapper around memcached
      - Every write is replicated to several AZs

- Hybrid Services
  - FailFast
    - not fallback to service and Db
  - ==Workload partitioning==
    - Different systems for batch and realtime
  - Request level caching
  - Secure token fallback
  - Chaos exercises

- ==Failure driven design==
- ==Chaos under load==

##### Variance

- Variety in your architecture
- Use cases
  - Operational drift
    - Alert thresholds
    - Timeouts, retries, fallbacks
    - Throughput (RPS)
  - Across microservices
    - Reliability best practices

- Continuous Learning and Automation cycle
  - Incident
  - Resolution
    - Call
  - Review
    - Understand what is happening
  - Remediation
    - Make technically it works well
  - Analysis
    - Is this a new pattern ?
  - Best practice
  - Automation
  - Adoption
    - Make sure that gets adopted
    - Knowledge becomes code and is deployed in microservice architecture

- Production ready checklist
  - Alerts
  - Apache & Tomcat
  - Automated canary analysis
  - AutoScaling
  - Chaos
  - Consistent naming
  - ELB config
  - Healthcheck
  - Immutable machine images
  - Squeeze testing
  - Staged, red/black deployment
  - Timeouts, retries, fallbacks

- Polyglot and containers
  - Paved road
    - Java
    - Ec2
  - Internal teams drifted to
    - Python
    - Ruby
    - nodeJs
    - Docker
  - ==Putting new technologies to critical path is challenging==
  - ==Understand cost of variance==
  
##### Change

- How to achieve velocity with confidence ?
  - Global Cloud Management and Delivery
    - [Spinnaker](https://spinnaker.io/)
    - ==Integrated automated practice==
      - Conformity checks
      - Red/black pipelines
      - Automated canaries
      - Staged deployments
        - One region at a time
      - [Squeeze tests](http://blog.mattcallanan.net/2013/12/yow-netflix-workshop-11dec2013.html)
  - ==Production ready checklist should be integrated into the delivery pipeline==


#### Organization and architecture

- Productivity & new capabilities
- Refactored organizations
- ==Solutions first, team second==
- Reconfigure teams to best support your architecture
