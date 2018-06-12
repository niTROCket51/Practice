## SoRandom (PicoCTF 2017)
#### Important: Please note that you must run the decoder program on Python 2.7 due to the changes in random.seed() function.

### Problem
> We found [sorandom.py](/PicoCTF2017/SoRandom/sorandom.py) running at shell2017.picoctf.com:37968. It seems to be outputting the flag but randomizing all the characters first. Is there anyway to get back the original flag?

### Solution
Upon examining the code, it can be concluded that [sorandom.py](/PicoCTF2017/SoRandom/sorandom.py) takes a file named "flag" as input and returns an encoded flag, [encflag](/PicoCTF2017/SoRandom/encflag).

On connecting to shell2017.picoctf.com:37968 with `netcat`, the following message is received:
> Unguessably Randomized Flag: BNZQ:jn0y1313td7975784y0361tp3xou1g44

It must be the encoded flag. All I needed to do was to decode this string. Since, the `seed` is known, the effect of `random.randrange(0, 26)` can be reproduced.

So, I wrote the [decode script](/PicoCTF2017/SoRandom/sorandom.py).
```
encflag += chr((ord(c)-ord('a')+random.randrange(0,26))%26 + ord('a'))
```
Since, `enflag` is known, I just looped through all possible values of `ord(c)` until the equation is satisfied.
Upon running the program, I got the flag:<br>
`FLAG:ac8c0490fb0508767f1625cb8cea8c34`
