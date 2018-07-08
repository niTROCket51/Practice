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
Since, `encflag` is known, I just looped through all possible values of `ord(c)` until the equation is satisfied.
Upon running the program, I got the flag:<br>
```
FLAG:ac8c0490fb0508767f1625cb8cea8c34
```

**Note:** Running the program in Python 3 does not give the correct flag as the original flag was encoded with Python 2 and the `random.seed()` function works differently in Python 3 for non-integer inputs.
An excerpt from [What's New in Python 3.2](https://docs.python.org/dev/whatsnew/3.2.html#porting-to-python-3-2)
> The `random.seed()` function and method now salt string seeds with an sha512 hash function. To access the previous version of seed in order to reproduce Python 3.1 sequences, set the version argument to 1, random.seed(s, version=1).

Despite setting the version to 1, `random.randrange(0, 26)` was generating different values. It is probably due to change in `randrange()` function.

> Most of the random module’s algorithms and seeding functions are subject to change across Python versions, but two aspects are guaranteed not to change:
> * If a new seeding method is added, then a backward compatible seeder will be offered.
> * The generator’s random() method will continue to produce the same sequence when the compatible seeder is given the same seed.

It states that a backward compatible seeder will be offered, not that the existing method will be backward compatible. So, only `random.random()` gives same result. `random.shuffle()` and `random.randrange()` produce different results.

#### Peace. :blush:
