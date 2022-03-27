# My reverse-engineering journey
I decided I wanted to learn reverse engineering, and there's no clear way to learn it, so I'll document my learning experience, how 'good' can I get, and what resources you should and shouldn't get.
I have a decent-ish background in programming, at the time of writing, I'm comfortable with C#, go, Python, C, and getting int C++. For reverse engineering programming isn't "required", but definitely helps a lot, especially C, I recommend Cherno's C++ course (free on youtube), and to use tools like CLion's memory view (vs has one too), to see what "actually goes on" when you are programming.

I wanted to start with Paul Chin's first course (reverse engineering with x64dbg), since I have all of his courses (at the moment), most of them (if not all) were free with coupons (for promoting the courses), but since I couldn't get x64dbg to run properly on linux (it wouldn't open files), I decided to watch the ghidra course first

CrackMe1.exe - for this you could just use Detect it Easy or a hex editor and searching for the string since it's just a string serial hardcoded, instead of it using a proper serial algorithm.

Since I moved to the ghidra course, here the teacher uses hackaday-u's github repo for the crackme's, the first crackme (session-one/exercises/c1)'s password was discoverable using DIE, but using ghidra let us see that the password doesn't have to be the one stored as the string, but anything starting with it, since the program only compares first n characters of the password you provide, n being the length of their password.

The second crackme already does other checks, in this case it checks the first character and the 5th one (index 4), so any password that starts with hXXXu should work, this is a bit more interesting because you can't just use any hex tool to search the password

The C3 is even more interesting, like with the rest, we have to rename our function definition, and optionally the variables, this time we see that the character with the index 2 (position 3) of the is `0x20` higher than the character from the 2nd index, which is 32 in decimal, looking at the ascii table, we can see that lowercase letters are stored on the position of the UPPERCASE characters but with an offset of 32, so any password that has XXyYX as the start, XX being any character, y being any lowercase character, Y being any uppercase character will work, also since it doesn't check for ascii characters, something like `XX{[TEST` will work too

Looking at C4, the first thing that pops is the for loop, because the loop is so simple, I didn't evne try to get the 'password' manually, i wrote a simple c script to get the password
```
char* str = "hackaday-u";  
for (int i=0; i<strlen(str); i++)  
    printf("%c",str[i]+2);
```
This gives us the password ``jcemcfc{/w`` which is the correct answer, this was easily doable by hand, either by looking at the ascii table, or by manually checking all the characters with
```
printf("%c",'y'+2);
```
where y is the character we want.

I couldn't find C5, so I had to watch the video to see the code. It just checked if the length was less than 16 and if the ascii code were consecutive

For the gui crackme, again, a simple string search was enough, not very interesting except the part that explains entry points, a very tiny bit on how gui's work and string searches from ghidra

The next videos were pretty boring and basic, but this much in the course, I finally got very annoyed with the quality, the teacher speaks so slow that it becomes a burden to watch the videos, you cannot speed them up without making the quality way worse