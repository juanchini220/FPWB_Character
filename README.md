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

---

## Utilization of Enhanced Input System:

This project utilizes the Enhanced Input system provided within the Unreal Engine 5's Third Person Game template. This functionality offers an enhanced set of tools for managing player input, enabling smoother and more advanced interaction with the game controls.


![image](https://github.com/juanchini220/FPWB_Character/assets/53541328/72f1d619-9857-416b-af4f-1199e2cba2f0)

---

## Implementation of Walking/Running:

The logic behind this feature relies on checking various conditions when the player triggers the action. It verifies if the breathing audio is playing and if the player is in motion to ensure the system operates only when the character is actually running. Subsequently, it triggers the stamina feedback sound function and calls the UnCrouch event as a precaution in case the character is crouched.
SetTimerByFunctionName is employed to execute a function in a way that it repeats at specific intervals. This function regulates the increase or decrease of stamina. While running, the stamina increase function is paused, and the decrease function is executed, and this process is reversed when the character is still or walking.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/EventGraph_Run-StaminaLogic.PNG?raw=true)


To conclude this section, a TimeLine is utilized to provide a float named Alpha. This value gradually alters the MaxWalkSpeed and adjusts the camera's FieldOfView to add visual feedback that conveys a sense of speed while running.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/EventGraph_Run-StaminaLogic_02.PNG?raw=true)

---

## Implementation of Crouching:

For the crouching action, we use a simple FlipFlop that triggers the inherited functions from the Character Movement Component, Crouch, and UnCrouch. These functions alternate between walking speed and crouched speed, as well as adjust the size of the character's capsule collision.

Upon crouching, we also call the StopRun event. If we are running, this event stops the run using the previously mentioned stamina system.

The attached diagram also illustrates the jump logic. We utilize inherited functions from the Character Movement Component called Jump and StopJumping. Additionally, we call a JumpEvent, which is used to play the jump sound, a feature we will delve into later.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/EventGraph_CrouchJumpLogic.PNG?raw=true)
To conclude, we call the UnCrouchEvent, which causes the character to stand up if we try to jump while crouched.

---

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

---

**Usage of BlueprintThreadSafeUpdateAnimation:**

In the character's Animation Blueprint, we employ the BlueprintThreadSafeUpdateAnimation function. This function is used to compute all necessary variables for animation behaviors. This practice helps prevent potential bottlenecks when fetching data for animations, ensuring optimal performance.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ABP_ThreadSafe.PNG?raw=true)

---

Now, let's proceed with the AimOffset feature:

---

## Implementation of AimOffset:
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ABP_MainAnimGraph.PNG?raw=true)
We've utilized a Linked Anim Graph in the main Animation Blueprint to maintain organization. Within ABP_AimOffset, we blend the locomotion pose with an AimOffset that receives values for Yaw and Pitch.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ABP_AimOffserABP.PNG?raw=true)
The Pitch values are obtained using BaseAimRotation, calculated within the Calculate Yaw Offset function.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ABP_CalculateYawOffset.PNG?raw=true)
The Yaw values are obtained through RootYawOffset, calculated within the TurnInPlace function. We'll delve deeper into this function in the next section when exploring the following feature.

---

## Implementation of Turn in Place:

Similar to the Aim Offset, we use the same Linked Anim Graph for organizational purposes. In this case, we blend a RotateRootBone and utilize the Root Yaw Offset parameter, which is calculated in the TurnInPlace function.

Within the TurnInPlace function, the initial check ensures that the character is not in motion because this animation only plays when the character is stationary.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ABP_TurnInPlace_01.PNG?raw=true)

In the visuals, we observe how rotations are calculated and cross-checked with the curves of Animation Instances named "Turning" to determine if the character is turning. Then, deltas are computed using the "RemainingTurnYaw" curve provided by the rotation Animation Instances.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ABP_TurnInPlace_02.PNG?raw=true)
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ABP_TurnInPlace_03.PNG?raw=true)
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ABP_TurnInPlace_04.PNG?raw=true)

Curve from animation instance

![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/TurnCurve.PNG?raw=true)

---

## Implementation of Foot IK:

Similar to the other features, we've created another Linked Anim Graph that combines all animations with a specific Control Rig for Foot IK. This Control Rig is activated only when the character is in contact with the ground.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ABP_MainAnimGraph.PNG?raw=true)
Below is the structure of the implemented Control Rig that procedurally positions the character's feet.
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ControlRig_01.PNG?raw=true)
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ControlRig_02.PNG?raw=true)

---

Foot Trace Function
![image](https://github.com/juanchini220/FPWB_Character/blob/main/Images%20and%20GIFs/ControlRig_FootTraceFunction.PNG?raw=true)

---

## Postmortem:

While the project has been successful in its implementation, I acknowledge that there are aspects that could be optimized in a more elegant manner. For instance, I consider the utilization of components that I will likely implement in future updates to the template.

This experience has been an invaluable opportunity to discover the various tools offered by Unreal Engine for animation logic and sound implementation with MetaSound, a tool that I personally love and plan to work with more in the future.

I'd like to express my gratitude to the YouTube channel of [Druid Mechanics](https://www.youtube.com/@DruidMechanicsGameDevelopment) for their invaluable assistance in understanding and developing the Turn In Place topic, a resource that was fundamental to the project.

In the future, I intend to further refine certain aspects of the animations. Now, my next step will be to initiate a project based on this template to explore how far I can go using this solid foundation.

---

## Thank You for Reading:

I genuinely appreciate your time spent reviewing this project. I hope the information provided has been interesting and useful. I hope you enjoy the project as much as I enjoyed creating it!


