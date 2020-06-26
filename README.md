# redpwn-i-wanna-find-the-flag
This challenge involves a gamemaker studio game that is.... challenging to say the least\
\
Took me like 20 minutes to figure out how to play, because i've never seen a game that wants you to press shift to play\
\
At the save select screen, you can select a difficulty and save slot\
![game1](/img/game1.png)\
\
Upon spawning, you realize you have only one option: Warp to the boss and die. Forever.
![game1](/img/gamegif.gif)\

# Lets cheat
So its a ctf and that means cheating at the game is OK :D\
\
I utilized cheat engine a lot in this challenge, as having a debugger and all sorts of nice features was essential.
After wasting about an hour messing with player position and other interesting values, I decided it was time to just pop IDA open and explore\
\
Naturally, I opened up strings and searched debug to look for any dev options left in the game\
![game1](/img/gida1.png)\
\
Nice! This looks intersting... Lets xref\
![game1](/img/gida2.png)\
\
Wow! Nice! This is clearly virtual function registry function for the internal vm. This is the point in which i realize this was a game maker game.\
\
I will say that in all my time reversing games, this has never come up, so there was a learning curve at first,\
however I have some experience with the CoD vm so things felt very familiar here\
\
A quick F5 shows us a group of *juicy dev functions* waiting to be called...\
![game1](/img/gida3.png)\
