# FT_Sprint
 This focus topic is based on a simple sprint mechanic.

## Overview

This project involves implementing a sprint mechanic, but it currently lacks input for initiating sprints. The goal is to create and assign an input action to enable sprinting:
- Create the input action.
- Update the input mapping context.

From this you will learn about input context mapping, function binding in C++ and some basic C++ logic. You can also see i have added a common style gate way mini game for you to play about with the mechanic you are making and could expand upon it.

## Where to find the relevant scripts

We have two key components you need to place for this, input management components and the C++ scripts for the character. 

### Inputs 

We should always follow a specific pattern for organising our files. As we only have one character, we can just store all the inputs in one folder: **Content->ThirdPerson->Input**
You can see it already has the **Input Mapping Context**, and the **Input Actions** are all in the **Actions** folder. 

![The location of the input folder in the Unreal Engine Content Browser.](Hints/InputMappingLocation.png)

### Scripts

From now on all our script will be stored in the **C++ Classes** folder with the name of the project so for us **C++ Classes->FT_Sprint**. We have the main character scripts here, and a **Public** folder which contains the scripts for the gates. 

![The location of the C++ scripts in the Unreal Engine Content Browser.](Hints/ScriptLocations.png)

## Task
The task is to implement the sprint input and rebind the inputs to allow the player to have a sprint functionality.

## Hints 

### Generate Visual Studio Project
Check the scripts open properly, if in visual studio you open the script and the solution explorer is blank, and the tab in the top left says miscellaneous files, you need to go to the .uproject file **right click->Show more options->Generate Visual Studio Solution**.

### Coding
Remember, the header file declares variables and methods.
The C++ file implements the methods. implementing the Methods.

Your main focus should be adding the bindings in **FT_SprintCharacter.cpp lines 95-97**

The input follows the same logic as start and stop jumping, follow that input mapping and context, you can debug the code with:
```
UE_LOG(LogTemp, Warning, TEXT("Sprinting"));
```
For the medium and hard challenges look in the FT_SprintCharacter Constructor (`AFT_SprintCharacter::AFT_SprintCharacter()`), notice the template shows us how to change key values.

## Changes from base project:

### FT_SprintCharacter.h

Lines 47-49 added the input action pointer. This will allow you to assign an **input action in the blueprint editor** for the character.
```
/** Connah Addition 1: Sprint Input Action */
UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Input, meta = (AllowPrivateAccess = "true"))
UInputAction* SprintAction;
```
you can see this by opening the **BP_ThirdPersonCharacter**, **Content->ThirdPerson->Blueprints**

![The empty input action in the BP_ThirdPersonCharacter.](Hints/AddededInputAction.png)

Lines 63-65 added a start and end sprint function declarations
```
/** Connah Addition 2 methods to ensure we handle sprint enable and disable */
void SprintStart(const FInputActionValue& Value); 
void SprintStop(const FInputActionValue& Value);
```

### FT_SprintCharacter.cpp
Lines 95-97 Added the binding for the input action, this mean when you link the character can begin to sprint.
```
//** Connah binding implementation **/ 
EnhancedInputComponent->BindAction(SprintAction, ETriggerEvent::Started, this, &AFT_SprintCharacter::SprintStart);
EnhancedInputComponent->BindAction(SprintAction, ETriggerEvent::Completed, this, &AFT_SprintCharacter::SprintStop);
```
Lines 141-155 Added some code for the start and end sprinting, which will adjust the sprint speed for the player as they start and end sprint.
```
/** Connah methods implementation */
void AFT_SprintCharacter::SprintStart(const FInputActionValue& Value)
{
	// we could set speeds, but i like scalers 
	GetCharacterMovement()->MaxWalkSpeed = GetCharacterMovement()->MaxWalkSpeed * 2;
	// make it so they have a speed up route
	GetCharacterMovement()->MinAnalogWalkSpeed = GetCharacterMovement()->MinAnalogWalkSpeed * 3;
}

void AFT_SprintCharacter::SprintStop(const FInputActionValue& Value)
{
	// Reset the speeds
	GetCharacterMovement()->MaxWalkSpeed = GetCharacterMovement()->MaxWalkSpeed / 2;
	GetCharacterMovement()->MinAnalogWalkSpeed = GetCharacterMovement()->MinAnalogWalkSpeed / 3;
}
```

## Challenges
Test your might

## Easy

- Experiment with different input types, such as toggle and hold.
- Connect Xbox controllers for testing; PS controllers can work but are not natively supported.
- Add a stamina limit considering actions like sprinting and jumping, not too advanced can charge/ deplete in the Tick method.

## Medium
- Introduce accelleration and de-acceleration, so the character takes time to get up to speed and slow down, when sprint ends.
- Add delays to stamina recharging, see the gate code.
- Try to improve the feedback of the gates as currently they are only debuging.

## Hard 
- Introduce finer elements of gameplay iteration, such as effecting the players turning radius during sprint, see Spyro the Dragon charging.
- Add in an exhaustion mechanic, that adds a longer delay to stamina recharge, or slows the recharge.

# Reference
Content is made by Connah Kendrick (Connah.Kendrick@mmu.ac.uk) using the Unreal Engine 3rd Person Template for the MMU 1st year Game Mechanics Module taught to both Game Development and Game Design Students. 