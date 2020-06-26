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
![game2](/img/gida2.png)\
\
Wow! Nice! This is clearly virtual function registry function for the internal vm. This is the point in which i realize this was a game maker game.\
\
I will say that in all my time reversing games, this has never come up, so there was a learning curve at first,\
however I have some experience with the CoD vm so things felt very familiar here\
\
A quick F5 shows us a group of *juicy dev functions* waiting to be called...\
![game3](/img/gida3.png)\
\
Now I could work on calling these functions, but upon inspecting the subs I found what I originally had feared with a script VM: Virtual params\
![game4](/img/gida4.png)\
\
In a scripting VM, normally there is a virtual stack and virtual params. This means in order to call script functions, we would have to populate the script arguments. Now after completing the challenge, I learned of a way to do this, however it was unecessary as by doing a bit more searching I discovered a more useful function.

# room_goto
In gamemaker, levels are called rooms. Each room has an ID and an index. Rooms can be linked with triggers or a 'next room' property.\
\
Why is this useful? Well, any kind of level transition will call this function. With our handy debugger, this can be whatever room we want.\
\
After inspecting the internals of this function we notice it takes one parameter\
![game5](/img/gida5.png)\
\
In this screenshot I have it labeled, but during the challenge I just looked up the handy official documentation\
![site1](/img/site1.png)\
\
Now you may have noticed that the `room_goto` function searches for the first script argument and sets a variable to it. This is excellent for us because we can set a breakpoint at the stack push of EAX (param1 in `__CDECL`) and just set the room id to whatever we want\
![game7](/img/gida7.png)\
\
![game8](/img/gida8.png)\
\
Upon reloading, dieing, warping, we hit our breakpoint, and from there, its just an enumeration of room indexes until we find `0xC`, aka, flag room\
![game9](/img/gida9.png)\
![gameA](/img/gidaA.png)\
\
That's it, right? We clearly see flag! However, there is just one problem\
![meme](/img/meme.jpg)\
