# LostAdventure
Repository for the work on my C++ and Unreal Live Project.  The premise was to design a new 3D level from the bottom up.  I utilized a lot of resources for inspiration on a lot of this code, but all work is original.
## Skills Acquired
- Experience with visual scripting
- Generate AI behavior
- Creating a combat system
- Creating a health and respawn system
- Triggering Boss spawn and environement effects
- Generating UI widgets based upon specific conditions
- Creating and modifying animations within the Unreal Engine and not via third party software
- Creating interactive blueprints utilizing blueprint interfaces
- Producing non-overlapping powerup systems utilizing layered Niagara effects and self-created materials
- Utilizing lighting to create both ambient and situational atmospheres
- Experience working withing an Agile/Scrum Environment utilizing Microsoft Azure DevOps

## Level
The first Story in the creation process was foundational level design.  The landscape was designed in landscape modeusing the terrain tool in Unreal using assets that were pre-installed in the project.  The hills and grass were all built using this feature.  The buildings and interior platforms were all constructued using CubeGrids and materials downloaded from Quizel Bridge.  Here is a quick run through of the overall level.

https://github.com/Keefer184/LostAdventure/assets/136768491/c78414af-0594-4c92-aec3-d46e758f2eaa

## Traps/BP Interfaces
The game featured two main styles of traps, a swinging spike pendulum and large arrows/spears as projectiles.  Both of these traps were triggered using pressure plates.  The coding was made easier by utilizing Blueprint Interfaces.  
![Trap Trigger](./LostAdventureGifsandSS/TrapTrigger.png)

The "On Activate" node is what called the action of the pendulums.  Each pressure plate was given an array of pendulum BP or arrow BP to activate utizing a public array.  The array size was not specified so that each plate could trigger any amount of pendulums or arrows desired.  Here is a clip of the arrow traps in action.

![Arrow Trap](./LostAdventureGifsandSS/ArrowTraps.gif)

In order to trigger health loss and also cause visible collision, for each trap two identical meshes layered on top of each other were utilized. One set blocked all collisions, while the other overlapped all.

![OverlapAndBlock](OverlapANDBlock.gif)

The elevators that you can activate to go both up and down made use of timelines to control the animations, but were triggered in the same way that the traps were triggerd.  The only difference was that the trigger was a switch in which the player needs to provide an input to activate.

![SwitchBPI](SwitchBPI.png)

A gate was used so that the player must be within a certain range (outlined by a Box collider) and only enabled input when the character entered the box, but disabled input when they left.  Here is an example of both the elevator and pendulum traps being triggered.

![ElevatorTraps](./LostAdventureGifsandSS/ElevatorTrapsRespawn.png)

## Collectables and Win Condition
The next task was to create a win condition as well as collectables that the player could pick up.  To encourage full play through of the level, I created collectables that the player needed four of in order to enable to final door (win condition) to be opened.  The collectables were created using a statue asset for the mesh, but then included some hovering text and a simple niagara system to make them stand out in the game.  Each collectable that was "picked up" would dissapear on pick up and then add one to the number of keys attained (controlled by the game mode called ThirdPersonGameMode).

![CollectableBP](./LostAdventureGifsandSS/CollectableBP.png)

The win condition was a little tricky.  When the player entered the box collider trigger surrounding the door, the door needed to check for the amount of collectables.  The boolean "HasEnough" was set to true once the character picked up four collecatbles and set the visibility of the "Key Obtained" text in the HUD to visible only if it is true.  Once the door checked for the key and showed it was obtained, input was enabled and an ending sequence was set in motion.  The game then paused and the final Widget congratulating the character was displayed.

![Final Door BP](FinalCheckpoint.png)

![WinCondition](WinCondition.gif)

## Main Character
Probably my favorite part of the project was creating powers, different attacks, and a health system for the main character.  
### Powers
In a bit of personal creativity, I wanted the character to have mulitple powers.  Each power was brough upon by a different input.  To create the effects of each power, I utilized the fountain niagra system and used the mesh reproduction sprite and the base skeletal mesh of the character as the volume.  Then I created unique infinately spawning sprites that were based upon materials that looked like the powers (fire, ice, earth/grass, lightning).  In addition, the player was overlayed by a system of materials that utliezed the panner node to create the appearane that the caharacter was engulfed by their powers. Here are some examples of the niagara system and material set ups.

![Niagra](NiagraEX.png)

![Materials](MovingMaterial.png)

One of the challenges of this system was to make sure that the powers were never activated at the same time because it would eat so much memory.  I created a boolean for each power and with each input checked to ensure neither of the other powers were activated using an OR node.  A simple FlipFlop node made sure that I could deactivate the powers to reset the boolean and stop the niagra system before I could activate a different power.  

![PowerUps](PowerupEX.png)

The project timeline was two weeks, and if I had a bit longer I would have created animations to go with each powerups. 

### Attacks
A cool trick I implemented was allowing the character to use the same attack button to launch a projectile, but he could only do it once his powers were activated, and the projectile was different for each power.  For example, fire launged a fireball, ice launched an ice Spear, Earth launched a grass blade (a mesh I had to create myself), and lightning launched essentially a thunderball.  The trick was simple, utilizing the same booleans for the power ups, a series of branches check which power was activated and spawned a different BP from the same location and with the same force. The trick to these (as well as the arrow traps) in order to fly straight was to disable the gravity for each BP.

![CharAttacks](VariedCharAttacks.png)

I had to move the cast arrow out in front of the character enough so that it looked like the character threw the projectile (using a self-created animation) and becuase each one applied damage on collision, so the player wouldn't deplete his own health.

### Player Health/Hud
![Damage and Regen](./LostAdventureGifsandSS/DamageRegen.gif)

A widget that was called on start acted as the HUD for the game. Along with the number of collectables attained, it also had the counter for the player lives and the health bar for the character.  Since there were an absurd number of ways that the player took damage (fall damage, trap damage, NPC and Boss attacks) I also create a regeneration system that slowly refilled the character health overtime.

![PlayerHealth](PlayerHealth.png)

A simple clamp to ensure the player only took enough damage to lose one life was utilized as well as a cast to the game mode to update the lives if he was killed.


https://github.com/Keefer184/LostAdventure/assets/136768491/e9040159-eb88-40bf-ae86-e56a2c5c823c

