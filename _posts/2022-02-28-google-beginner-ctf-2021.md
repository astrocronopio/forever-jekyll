---
layout: post
title: Task 1 - Google CTF Beginners Quest 2021 
categories: [coding, CTF, google]
---


CTF (Catch the Flag) in the programming world is a competition where you hack/solve puzzles in order to get a code to get the next puzzle. You might to use several tricks and a plenty of imagination to get something like this: `CTF{This_is_how_flags_usually_look_like}`.

A really nice introduction to what CTF competition is  [LiveOverflow YouTube Channel](https://www.youtube.com/watch?v=BFMmSJ3PRZM) with their pros and cons. I took the Google CTF Beginners Quest 2021,  I have learned some interesting stuff so I wanted to make a post detailing some of them. 


You can go and check this CFT at [https://capturetheflag.withgoogle.com/beginners-quest](https://capturetheflag.withgoogle.com/beginners-quest).


## Task 1:

Get the password for this website [https://cctv-web.2021.ctfcompetition.com/](https://cctv-web.2021.ctfcompetition.com/), this is what it looks like.

![This is website](/media/task1nopass.png "This is website")

Check the inspect the html code using your favorite web browser. You get the following code:

```javascript
const checkPassword = () => {
  const v = document.getElementById("password").value;
  const p = Array.from(v).map(a => 0xCafe + a.charCodeAt(0));

  if(p[0] === 52037 &&
     p[6] === 52081 &&
     p[5] === 52063 &&
     p[1] === 52077 &&
     p[9] === 52077 &&
     p[10] === 52080 &&
     p[4] === 52046 &&
     p[3] === 52066 &&
     p[8] === 52085 &&
     p[7] === 52081 &&
     p[2] === 52077 &&
     p[11] === 52066) {
    window.location.replace(v + ".html");
  } else {
    alert("Wrong password!");
  }
}

```
We enter the code and the program stores it inside the `const v`, then the code transforms the characters into integer values after adding `0xCafe`. To obtain the password, we need to check what values are stored in the `p[i]`: Python is our friend.

```
import numpy as np

# Checking the code, we need at least 12 values

p = np.zeros(12)

p[0] = 52037
p[6] = 52081
p[5] = 52063
p[1] = 52077
p[9] = 52077
p[10]= 52080
p[4] = 52046
p[3] = 52066
p[8] = 52085
p[7] = 52081
p[2] = 52077 
p[11]= 52066 #12 values

#Convert 0xCafe to integer value
#16 is the ASCII base, represented by '0x'
offset = int('0xCafe',16) 

p -=  offset # Deletes the offset 

for i in p: 
     # Print the password
     print(chr(int(i)), end="")

```

And the password is `GoodPassword`, it takes you to a new page with the flag at the bottom.

![This is website](/media/task1pass.png "This is website")