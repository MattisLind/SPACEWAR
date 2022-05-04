# AR11 Breakout Board for SPACEWAR
![AR11 brakout board](https://i.imgur.com/Rt9StaU.jpg)
The cable that I made for the joysticks and screen got some wear and tear from being pulled out from one computer and installed in another and it was really tricky to get the cable in place when the systems was rackmounted.
To ease installation I designed a small breakout panel that can be rackmounted in the rear of the rack. It provided a 40 pin connector for connecting to the AR11 board in the PDP-11 computer. 

Then there are two DE9 connectors for the joysticks and one DA15 connector for the screen. The DA15 has the, X, Y and Z signals. Also included are the NON-STORE and ERASE signals which is needed when connecting a storage tube. I put three BNC connectors for the X, Y, Z signals in the case I want to use coaxial cables. To make it easy to adapt to different screens there are four jumpers on the board. These control the Z-output. There are two pairs. Each pair has one inverted and non-inverted sense. One pair is for the INTENSIFY signal which typically is used for point plotting. The other pair is taken from the CH02 DISP signal. This signal is just a logic level and can thus be used for drawing vectors.

The board has three mounting holes that will allow direct mounting to the rack cabinet.
