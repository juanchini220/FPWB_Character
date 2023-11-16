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



## Utilization of Enhanced Input System:

This project utilizes the Enhanced Input system provided within the Unreal Engine 5's Third Person Game template. This functionality offers an enhanced set of tools for managing player input, enabling smoother and more advanced interaction with the game controls.


![image](https://github.com/juanchini220/FPWB_Character/assets/53541328/72f1d619-9857-416b-af4f-1199e2cba2f0)


## Implementation of Walking/Running:

The logic behind this feature relies on checking various conditions when the player triggers the action. It verifies if the breathing audio is playing and if the player is in motion to ensure the system operates only when the character is actually running. Subsequently, it triggers the stamina feedback sound function and calls the UnCrouch event as a precaution in case the character is crouched.
SetTimerByFunctionName is employed to execute a function in a way that it repeats at specific intervals. This function regulates the increase or decrease of stamina. While running, the stamina increase function is paused, and the decrease function is executed, and this process is reversed when the character is still or walking.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/EventGraph_Run-StaminaLogic.PNG?raw=true)


To conclude this section, a TimeLine is utilized to provide a float named Alpha. This value gradually alters the MaxWalkSpeed and adjusts the camera's FieldOfView to add visual feedback that conveys a sense of speed while running.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/EventGraph_Run-StaminaLogic_02.PNG?raw=true)


## Implementation of Crouching:

For the crouching action, we use a simple FlipFlop that triggers the inherited functions from the Character Movement Component, Crouch, and UnCrouch. These functions alternate between walking speed and crouched speed, as well as adjust the size of the character's capsule collision.

Upon crouching, we also call the StopRun event. If we are running, this event stops the run using the previously mentioned stamina system.

The attached diagram also illustrates the jump logic. We utilize inherited functions from the Character Movement Component called Jump and StopJumping. Additionally, we call a JumpEvent, which is used to play the jump sound, a feature we will delve into later.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/EventGraph_CrouchJumpLogic.PNG?raw=true)
To conclude, we call the UnCrouchEvent, which causes the character to stand up if we try to jump while crouched.


## Implementation of Locomotion Sound:

We use Notifies provided by Animation Sequences to trigger left and right footstep events. These events are used to play audio and can also be utilized in future projects to trigger particle systems, footprint decals, or other actions.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/AnimSequencer_Notifies.PNG?raw=true)
Distinct MetaSound sources have been created for each physical material, dividing sounds between left and right footsteps for walking and running.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/MetaSound_FS_Concrete.PNG?raw=true)

In the character's Event Graph, we employ a function that changes the MetaSound source depending on the physical material. This function utilizes the FindFloor function of the Character Movement Component to detect the type of physical material in contact with the character's capsule, allowing the audio source for footsteps to be changed accordingly.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ChangeSoundPhysicalMaterial_function.PNG?raw=true)
Subsequently, the sound index parameter is set to play a random sound from the set of left or right footsteps, depending on whether the character is walking or running. Then, a brief delay is added, and the corresponding layer of cloth movement audio is played.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/EventGraph_LocomotionSounds.PNG?raw=true)
For the jumping action, we follow a similar process. The MetaSound's index parameter changes the set of jump and landing audio.



