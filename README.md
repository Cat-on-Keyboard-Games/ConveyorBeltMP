# Multiplayer Conveyor Belt in Blueprint
This video first shows the glitching and stuttering issue you have on the client when using the `AddActorWorldOffset` solution that is mostly found in conveyor belt tutorials online.
Then the video shows the solution that uses fake bases that gives a much smoother result even at high frame rates.
[![Watch the video](https://img.youtube.com/vi/hswa4SmwDHc/maxresdefault.jpg)](https://youtu.be/hswa4SmwDHc)

It's not perfect yet, but I thought I'd share it so others might find it helpful. If anyone has any better solution, please share! I didn't find any ...

When you set the `t.MaxFPS` to 60 the first solution also works, but over a real network you will probably still have issues.

Made in Unreal 5.5.4. (May work in older and newer versions of the engine as well.)

## How it works
When a `Character` is standing on something that moves (like a platform), this is decently replicated in a multiplayer game by default. It's built-in into the `CharacterMovementComponent`.

I use this feature to make the Character move more smoothly.

But instead of using a visible platform, I use something like an invisible wall (or platform in this case). It has collision like a normal mesh, but you don't see it, so the character can stand on it. I call it a Fake Base.

If the character is on the conveyor belt, I check if its Fake Base is underneath it and if not, move it over there. I have to make sure not to move the Fake Base while the character is still partially on it or you will see glitches.

The Fake Base of a character that is on the conveyor belt is moved at the speed of the belt so it appears it is moving along with the belt. The Fake Base is actually 1 Unreal unit (cm) above the mesh, but you hardly notice that.

Each character needs its own Fake Base and each Fake Base needs to be set up to ignore any other characters. Otherwise you will have glitches when multiple characters are on the conveyor belt at the same time.

### Some known issues
If the conveyor belt is not on the floor, the character will not fall off. It will remain floating in the air (because it's still on the invisible Fake Base).

The Blueprint in this repo does `UpdateCharacters` every frame which isn't very efficient. There are also some issues with replication I think because there are still glitches when multiple characters are on the conveyor belt at the same time. If you put your Characters already in the level in the editor and then possess them when a new player connects you can call `UpdateCharacters` just once in `BeginPlay`. That's what I did in my game...

## Setup
In Project Settings -> Collision, add a new Preset called `InvisibleWallBlockOnlyPawn`:

![image](https://github.com/user-attachments/assets/164dec69-5921-4c5b-9ce5-7a7d83ddf2a8)

Copy the ConveyorBelt folder from this repo to your Content folder or just have a look at the Blueprint links below

Here are some screenshots of the properties you need to modify of your actor and its components:

![Screenshot 2025-05-16 142415](https://github.com/user-attachments/assets/a70e20c2-2ff1-485b-be45-95f062022191)
![Screenshot 2025-05-16 142427](https://github.com/user-attachments/assets/11634766-010b-4b69-879a-c68907f1dc6b)
![Screenshot 2025-05-16 142450](https://github.com/user-attachments/assets/0b38f6f3-7098-420b-bdec-f65f072e1c9b)
![Screenshot 2025-05-16 142513](https://github.com/user-attachments/assets/dffb729b-a333-4c67-a729-2b5ef879a795)

The Blueprint variables:

![image](https://github.com/user-attachments/assets/178456a1-4889-493f-9959-a161bccd2ce8)

Only the Speed variable needs to be set to be replicated using `RepNotify`:

![image](https://github.com/user-attachments/assets/ba1f21e2-83d0-48b0-8d63-25737c8b26ee)


Blueprints:
- [Construction Script](https://blueprintue.com/blueprint/lwxo5c7o/)
- [Event Graph](https://blueprintue.com/blueprint/lo0tnkzq/)
- [UpdateCharacters](https://blueprintue.com/blueprint/s9m3wop3/)
- [MoveOverlappingActors](https://blueprintue.com/blueprint/mwdqrtym/)
- [MoveFakeBases](https://blueprintue.com/blueprint/hvrdyu_x/)
- [GetFakeBaseForCharacter](https://blueprintue.com/blueprint/5fg73v4s/)
- [OnRep_Speed](https://blueprintue.com/blueprint/lvvzv04x/)
