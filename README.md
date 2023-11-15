# First Person with Body Character (FPWB_Character) for Unreal Engine 5

This project is a foundational template designed to create a first-person gaming experience that integrates the visualization of the character's body, offering players a heightened sense of realism. The primary goal is to provide a robust, modular structure allowing for the implementation and expansion of features related to character locomotion and interaction within the game world.

## Key Features

- **Walking/Running:** Implementation of a movement system enabling the character to seamlessly switch between walking and running with gradual changes in speed. A subtle visual feedback is incorporated into the camera to enhance player immersion. Additionally, an integrated stamina system is represented through sound effects.

- **Crouching:** The character has the ability to crouch, slowing down their movement speed. When running, the character automatically stands up to allow for faster movement.

- **Locomotion Sound:** Dynamic sound integration that varies based on the character's speed (walking/running) and the type of surface being traversed. This includes specific sounds for jumps, landings, and various audio layers (footsteps, clothing sounds) utilizing Unreal Engine's Metasounds for an immersive auditory experience.

- **Aim Offset:** The character's head aligns with the direction pointed by the camera, providing additional realism to the player's interaction with the environment.

- **Turn in place:** Hip rotation animation of the character to reflect the direction the camera rotates, depending on the degrees of rotation. This feature ensures a smooth and realistic transition when turning without moving from the spot.

- **Foot IK:** Procedural repositioning system for the character's feet to prevent floating at different heights, maintaining a more realistic interaction with the ground and environment.

Throughout this repository, you'll find a detailed description of each of these features, along with examples, explanations, and technical details to help you understand their implementation and functioning within the project.


## Project Foundation

This project is built upon Unreal Engine 5's Third Person template, which incorporates the use of the Enhanced Input system. This template provides a solid initial structure for user input handling and basic character interaction within the game environment. The FPWB_Character utilizes this foundation as a starting point, expanding and customizing functionalities to create a first-person experience with the inclusion of the character's body and other specific features outlined earlier.


![](![image](https://github.com/juanchini220/FPWB_Character/assets/53541328/72f1d619-9857-416b-af4f-1199e2cba2f0)

