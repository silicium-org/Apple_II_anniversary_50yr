TODO

notes d'hier soir à mettre au propre

- nommer la carte / le projet
- Trouver d’autres easter eggs pour le PCB ("Apple II 50e anniversaire", "Silicium", A2CP, A2FF, "in magret we trust", signature des participants, etc.)
- transférer les dessins sur github
- prendre les mesures de l'emplacement dans le coin de la carte-mère
- trouver un SVG de la pomme Apple, détourer le PCB
- tester sur mon //e qu'un simple fil en parallèle d'une paire Xi/Yj est bien détecté comme une touche et ce en parallèle d'appuis sur le vrai clavier, puis confirmer avec une Arduino que ça marche aussi avec une GPIO en collecteur ouvert
- compléter la spec en consultant quelques points dans la datasheet stm32f413/423
- regarder comment FlashFloppy (github) fait la mise à jour USB-OTG avec un câble USB-A vers A
- Compter les GPIO du F423 et s'il y en a assez, prévoir le connecteur SWD et les 2 IDC 2x5 non soudés, avec pinout adapté joystick et souris sérigraphiés « ext.1 (joystick?) » et « ext.2 (souris ?) » (les IDC ne seront pas soudés, c'est juste une provision pour écrire le sw plus tard)
- prévoir 4 pins pour ces écrans OLED I2C à 3€ (très optionnel)
- easter egg musical, choisir parmi les idées suivantes :
    - livrer le pcb avec une carte musicale d'anniversaire
    - mettre un chip de carte musicale sur notre PCB
    - mettre un buzzer piezzo et faire jouer une musique d'anniversaire PWM aléatoirement ou sur raccourci clavier (ou jouer aléatoirement, jusqu'à ce que le client trouve le raccourci clavier qui écrit dans l'EEPROM pour désactiver la musique aléatoire...)
    - faire que notre PCB aille de l'IDC-26 du clavier au connecteur je-sais-plus-quoi du haut-parleur (mais pas sûr qu'il soit prudent de brancher le STM32 au HP en parallèle de la carte-mère : TODO vérifier l'ampli sur le schéma de la carte-mère)
