1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [Decision log](Decision-log_5144598.html)

# Sous-Marin ASUQTR : FlexBE Implementation For asuqtr\_mission\_control

Created by Gabriel Tremblay, last modified on Oct 25, 2020

![](attachments/37355618/37355619.png)

## Background

[![](https://jira.asuqtr.com/images/icons/issuetypes/story.svg)ASUQTR-338](https://jira.asuqtr.com/browse/ASUQTR-338?src=confmacro) - Implémenter le mode autonome de gestion de missions Done

Une implémentation possible de flexbe pour le code actuel d'ASUQTR, svp donnez vos opinions, inquiétudes, analyses etc.

Il en existe d'autres avec d'autres problèmes, configurations, modularités, etc. mais celle là me semble la plus intuitive.

## What Does It Implies ?

### **Pros:**

Puisque FlexBE est le "top module", les 2 nodes d'asuqtr\_mission\_control seront **flexbe\_onboard** et **flexbe\_widget**:

1. La première est le core de flexbe qui execute les behaviors
2. la 2iem est la node qui fait la première requête pour le **global\_behavior**au lancement de ROS par un launch file. Par la suite, cette node pourra interprêter les rêquetes de l'application GUI FlexBE App pendant les phases de tests en piscine.

Cette configuration permettrais donc d'être totalement operationelle pour le mode 100% autonome de la compétition et le mode autonome testing où une console d'opérateur (un PC sur le réseau) est connectée au FlexBE runtime sur le Xavier pour avoir un feedback.

### **Cons:**

Un seul **global\_behavior** sera en opération en tout temps. Celui-ci devra être capable de tomber en mode "Idle" si un interrupteur de sécurité est tiré. Il devra également gérer la priorité des missions disponibles en fonction informations des capteurs (vision/position/hydrophones).

Ces points sont considérés négatifs dans le sens où le "framework" de FlexBE nous impose un certain paradigme de programmation et il sera peut être difficile (en termes de développement) d'exécuter une évaluation de priorité de mission en parallèle avec l'exécution des missions. Une implémentation alternative, où le **global\_behavior** est un module ASUQTR qui incorpore les objets FlexbeOnboard() et BehaviorLauncher() permetterait peut-être plus de liberté pour la création d'un module de gestion de priorité des missions en fonction de l'environnement.

## Action Items

* [William Flynn](https://confluence.asuqtr.com/display/~CrimsonCrow)
* [Maxime Verreault](https://confluence.asuqtr.com/display/~niggamax24)
* [Bastien Cote](https://confluence.asuqtr.com/display/~mysterfreeze)
* [Marc-Andre Morrissette-Soucy](https://confluence.asuqtr.com/display/~Soucym)
* [Gabriel Tremblay](https://confluence.asuqtr.com/display/~Tremblayg)

||
<colgroup><col /><col /></colgroup>|---|
|Status|DONE|
|Stakeholders|[Marc-Andre Morrissette-Soucy](https://confluence.asuqtr.com/display/~Soucym) [Maxime Verreault](https://confluence.asuqtr.com/display/~niggamax24) [William Flynn](https://confluence.asuqtr.com/display/~CrimsonCrow) [Bastien Cote](https://confluence.asuqtr.com/display/~mysterfreeze) [Gabriel Tremblay](https://confluence.asuqtr.com/display/~Tremblayg)|
|Outcome|On adopte cette implémentation avec FlexBE en Top Module|
|Due date|<time>16 Sep 2020</time>|
|Owner|[Gabriel Tremblay](https://confluence.asuqtr.com/display/~Tremblayg) [Bastien Cote](https://confluence.asuqtr.com/display/~mysterfreeze)|

## Attachments:

![](images/icons/bullet_blue.gif) [FlexBE\_requirements.png](attachments/37355618/37355619.png) (image/png)

## Comments:

||
|---|
|<font>1. Aurais-tu des liens pertinents concernant les choses mentionnées dans la page? (sur quoi tu t'es basé pour créer le flowchart).
2. Le flexbe code, c'est un package Python? Est-ce que t'aurais un lien vers ce package?</font>![](images/icons/contenttypes/comment_16.png) Posted by niggamax24 at Sep 14, 2020 10:44|
|<font>1. Les liens desquels je tire mes conclusions sont le github de flexbe, je te mets en lien les objets spécifique mentionnés dans le flowchart\[1\]\[2\]. Regarde les dossier bin/ pour voir comment les objets sont appelés dans node. L'idée de garder flexbe en top module au lieu de wraper les objets dans un module de gestion de mission ASUQTR vient de cette mention là d'un tutoriel : [http://wiki.ros.org/flexbe/Tutorials/Running%20Behaviors%20Without%20Operator](http://wiki.ros.org/flexbe/Tutorials/Running%20Behaviors%20Without%20Operator)
2. Flexbe est exclusivement un package ROS pcq le FlexBE core utilise evidemment rospy, smach et autre packages ROS.

Jte laisse le soin de regarder tout ça, y'a beaucoup de stock. Je suis ouvert à considerer autre choses et implémenter notre wrapper ASUQTR par dessus par exemple. Tiens moi au courant!

[1](/pages/createpage.action?spaceKey=SUBUQTR&title=1) [https://github.com/team-vigir/flexbe\_behavior\_engine/tree/master/flexbe\_onboard](https://github.com/team-vigir/flexbe_behavior_engine/tree/master/flexbe_onboard)

[2](/pages/createpage.action?spaceKey=SUBUQTR&title=2) [https://github.com/team-vigir/flexbe\_behavior\_engine/blob/master/flexbe\_widget/src/flexbe\_widget/behavior\_launcher.py](https://github.com/team-vigir/flexbe_behavior_engine/blob/master/flexbe_widget/src/flexbe_widget/behavior_launcher.py)</font>![](images/icons/contenttypes/comment_16.png) Posted by Tremblayg at Sep 14, 2020 14:30|
|<font>Nice job d'avoir résumé ça dans un flowchart de même</font>![](images/icons/contenttypes/comment_16.png) Posted by niggamax24 at Sep 14, 2020 14:47|

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)