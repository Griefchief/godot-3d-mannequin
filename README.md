- [Credits](#credits)
- [Quick Start Usage](#quick-start-usage)
- [Inner Workings](#inner-workings)
	- [Player](#player)
	- [Camera](#camera)
- [Configuration](#configuration)
- [Customization](#customization)
- [Support our work](#support-our-work)

An Open Source 3d character and character controller for the Godot game engine.

![The mannequin in-game](./assets/screenshots/prototype-1.png)

This is a third person character controller designed to work both with the keyboard and a gamepad. It features a camera that can auto-rotate or that can be controlled with a joystick.

## Credits

1. The Godot mannequin is a character made by [Luciano Muñoz](https://twitter.com/lucianomunoz_) In blender 2.80.
1. Godot code by Josh aka [Cheeseness](https://twitter.com/ValiantCheese)
1. Additional code by Francois Belair aka [Razoric480](https://twitter.com/Razoric480)

## Quick Start Usage

The 3D Third Person Character Controller is composed of two scenes:

* Camera.tscn - Holds a 3D camera rig with a state machine for aiming
* Player.tscn - Holds the player's kinematic body and state machine for movement and functions, and holds the player's animation rig.

The procedure to use the controller as-is, then, is to add both the Camera and Player scenes as instanced children to your 3D scene.

See `Game.tscn` for a simple example - the obstacles are mesh instances with static body collisions making up a cube world, and has the Camera and Player as instanced children. The camera, additionally, has been moved up to head level.

In short, the Player scene holds the 3D model and its animations, and the movement logic of the game character. What constitutes as 'forwards' is based on the Camera scene's orientation.

## Inner Workings

### Player

The scene that deals with the movement, collision, and logic of the player. The player is a KinematicBody with a capsule collision shape, and the movement logic is within a Finite State Machine.

The scene also holds an instance of the PlayerMesh for animation purposes. This scene lives in the `PlayerMesh.tscn` scene. It holds the skeletal rig for the mesh's animation, the 3D model of the body and head sepearately, and the animation tree and player to actually control the animation workflow of the model. The lot is wrapped up in a spatial node with some logic to transition to which animation based on which state the player is in.

### Camera

The scene that deals with the Camera movement. It is separate from the player, though keeps track of it and follows it. It has a SpringArm node to help with preventing collision with level geometry - moving the viewpoint forwards to prevent moving the camera inside geometry. It also has a system that holds the raycast for aiming-mode, and the 3D sprite that is a projected reticule. The logic is held in a finite state machine.

## Configuration

Most of the configuration available for player movement are located on the Move state in the Player scene - the player speed and the rotational speed.

The Camera has more options. On the main Camera state in the Camera scene are items like the default field of view, whether Y is inverted, and sensitivity.

In addition, the Aim state allows some finer-tuned changes, like whether the aiming camera is first or third person, and by how much it should be offset over-the-shoulder of the character.

## Customization

While the scenes can be modified extensively with new nodes and raw code, the state machine model allow for some simple, new functionality with relative ease.

As an example, there is the `Extensions` folder which contains additional player states for using the aiming view to fire a hookshot that pulls you towards the reticle. Once those states have been added to the Player's `Move` state, you only need to replace the `pass` in Move's `enter` with code like `owner.camera.connect("aim_fired", self, "on_Camera_aim_fired")` and Move's `exit` with code like `owner.camera.disconnect("aim_fired", self, "on_Camera_aim_fired")`

## Support our work

This free series is sponsored by our [platformer game creation course](https://gdquest.mavenseed.com).

GDQuest is a social company focused on education and bringing people together around Free Software. 

We share **the techniques professionals use to make games** and open source the code for most of our projects on [our GitHub page](https://github.com/GDquest/).

You can:

- Join the community on [Discord](https://discord.gg/dKUX7m3)
- Follow us on [Twitter](https://twitter.com/NathanGDQuest)

