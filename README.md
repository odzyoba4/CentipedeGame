# CentipedeGame
Video Demo: <br>
[![CentipedeDemo](https://img.youtube.com/vi/yPkErqfitr4/0.jpg)](https://www.youtube.com/watch?v=yPkErqfitr4) <br>
## Overview
This project is an attempted replica of the 2D Atari Centipede game. During this project I learned how to handle systems that totaled up to 100 classes, and how to use design patterns like factory pattern, command pattern, strategy pattern, finite state machine, and others to create more efficient systems. <br/>

GameController Class Diagram:
![CentipedeGameFlow](/images/CentipedeGameFlow.png)

## Finite State Machine (Centipede)
One of the systems I implemented was the main centipede's movement. To do this, I used a finite state machine to create static state classes to decide between actions that the centipede took when going in a zig-zag motion.
<br/> 

Class Diagram: <br/>
![CentipedeGameFlow](/images/CentipedeFSM.png)

<br/>
The state machine helped me create a more computationally efficient update method for the centipede, which only checked the necessary data for the state that it was in, and also made graphical updates to the sprites easier.

## Command Pattern (Sound Manager)
Another pattern I learned to implement was the command pattern, which I used to create the centralized sound manager for the game. With the command pattern, I could avoid too many sounds combining at once by having a sound manager that would accept commands to play and stop a sound. 
This way, a sound was played only once per frame, even if multiple objects called to play the sound. <br>
![CentipedeSoundManager](/images/CentipedeSound.png)


