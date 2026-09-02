EMA — Actionneur Électromécanique pour Gouverne d'Aileron

Projet portfolio personnel : conception complète d'un actionneur électromécanique (EMA), du cahier des charges jusqu'à l'électronique embarquée — référence d'application : gouverne d'aileron de Pilatus PC-12.

<!-- Ajoutez ici une image de couverture : rendu CAO, ou photo du montage si prototype physique -->
🎯 Contexte

Ce projet couvre le cycle de conception complet d'un actionneur électromécanique linéaire destiné à piloter une gouverne d'aileron : cahier des charges, conception mécanique, modélisation dynamique, commande en boucle fermée, validation par simulation, et conception de l'électronique embarquée.

📐 Spécifications clés
Paramètre	Valeur
Moteur	DC 24V / 300W (ATO-D5BLD300)
Réducteur	Rapport 5:1
Vis à billes	SFU1605
Course	±34,2 mm
Butées mécaniques (linéaire)	±37 mm
Débattement angulaire (gouverne)	+25° / −15° (40° total)
Modèle de charge aérodynamique	T_aero = k·θ (k nominal = 11,73 N·m/deg)
🗂️ Avancement du projet
 Phase 0 — Cahier des charges
 Phase 1 — Conception CAO (SolidWorks)
 Phase 2 — Modélisation électromécanique (Simscape)
 Phase 3 — Commande PID en cascade (courant → vitesse → angle)
 Phase 4 — Validation en simulation
 Phase 5 — Électronique + microcontrôleur (schéma + PCB KiCad)
 Phase 6 — Prototype physique (optionnelle, non prévue dans le cadre de ce projet)
 Phase 7 — Documentation finale (en cours)
🔧 Détail des phases
Phases 1-2 — CAO & Modélisation
<img width="858" height="694" alt="Capture d&#39;écran 2026-09-02 200703" src="https://github.com/user-attachments/assets/e4ed4dd6-00cb-41c3-b080-93502ae880c1" />


Conception mécanique complète sous SolidWorks, puis modélisation électromécanique du système sous Simscape (MATLAB/Simulink) pour disposer d'un modèle dynamique fidèle avant de synthétiser la commande.

Phase 1 (révision) — Butée mécanique angulaire
<!-- Ajoutez ici une capture de la liaison d'angle avec limites dans SolidWorks -->

Après validation de la commande en simulation (Phase 4), retour sur la CAO pour traduire la plage validée en contrainte mécanique cohérente : une liaison d'angle avec limites (+25° / −15°) sur l'axe de charnière — seul élément de la chaîne cinématique dont la rotation reste fixe dans l'espace, donc le seul candidat pertinent pour une butée directe.

La chaîne cinématique complète a été reconstruite et validée dans SolidWorks avec les liaisons mécaniques appropriées : liaison Pignon (couplage rigide réducteur → vis à billes via l'accouplement) et liaison Vis (conversion rotation → translation, pas de 5mm) pour la vis à billes. Une seconde liaison d'angle, en marge de sécurité, protège le bras de levier contre les configurations géométriques non réalistes (le bras combinant rotation et translation, sans butée physique propre dans la réalité).

Phase 3 — Commande
<img width="1207" height="740" alt="Capture d&#39;écran 2026-09-02 201233" src="https://github.com/user-attachments/assets/a8671538-ebc1-453d-b92b-7942ffd4f40c" />

Architecture de commande en cascade à trois boucles imbriquées : courant → vitesse → angle, chacune réglée successivement de l'intérieur vers l'extérieur.

Phase 4 — Validation en simulation

Tests par échelons (+25° / −15°) face à un modèle de charge aérodynamique. Un point notable du processus : la limitation de courant initiale (±5A) empêchait l'actionneur d'atteindre l'angle cible en butée de saturation — corrigée à ±12A après vérification du courant nominal réel du moteur. Un test de robustesse complémentaire (doublement de la raideur de charge aérodynamique k) a confirmé la tenue du système face à une charge plus sévère que le cas nominal.

Phase 5 — Électronique (schéma + PCB)
<img width="1120" height="749" alt="Capture d&#39;écran 2026-09-02 200900" src="https://github.com/user-attachments/assets/26520fb2-c623-492c-9305-95068d1a67d0" />
<img width="1399" height="748" alt="Capture d&#39;écran 2026-09-02 200851" src="https://github.com/user-attachments/assets/72b1cf11-64fb-4838-9adf-747708ac0cdf" />


Conception complète sous KiCad :

Alimentation : 28V → 5V (LM2596S) → 3,3V (AMS1117), régulation à deux étages
Driver moteur : BTS7960 (pont en H)
Mesure de courant : INA240 + shunt 0,01Ω, dimensionné pour ~14A avec marge de sécurité
Retour de position : encodeur magnétique absolu AS5048A (interface SPI)
Microcontrôleur : STM32F401RETx

Schéma validé (ERC : 0 erreur). PCB 120×80mm entièrement routé et validé (DRC : 0 violation, 0 connexion manquante), avec 5 zones de cuivre dédiées à la puissance (+28V, GND double face, retour moteur).

Limitation connue : les pistes de puissance sont restées à la largeur standard (0,25mm) plutôt que la largeur calculée pour ~14A (8mm), documentée directement sur le PCB. Choix assumé : ce PCB est un exercice de conception, non destiné à la fabrication physique dans le cadre de ce projet.

🛠️ Stack technique
CAO : SolidWorks
Simulation & Commande : MATLAB/Simulink, Simscape
Électronique : KiCad
Microcontrôleur cible : STM32F401RETx
🚀 Suite du projet
Développement du firmware embarqué (implémentation temps réel de la boucle de commande sur STM32)
Documentation finale consolidée
👤 À propos

Projet réalisé par Amir Ben Dhiab, étudiant ingénieur en Génie Électromécanique (spécialisation Robotique & IA) à l'École Polytechnique de Sousse (EPS), Tunisie — dans le cadre de la préparation aux candidatures de stage de fin d'études (PFE), février 2027.

🔗 github.com/amir-bendhiab
