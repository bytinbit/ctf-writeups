# Hidden Flag 01

**Flag:** `HV19{1stHiddenFound}`

The challenge of day 6, "Egg and Bacon", had a strange field with unformatted text. I found it very suspicious, but the information provided was accurate, the source code of the site looked ok.

When highlighted, the text displayed strange whitespace, but I just couldn't figure what to do with it. After banging my head even harder on the source code, I found a way to display the whitespace characters 
(https://www.soscisurvey.de/tools/view-chars.php):

<img src="01.png" width="90%;" />

No it's not Bacon cipher, no it's not Morse, it's... SNOW - Steganographic Nature of Whitespace! So... let it SNOW:

``` 
$ stegsnow -C 01.txt
HV19{1stHiddenFound}%   
```

Sauce:

* https://0x00sec.org/t/steganography-concealing-messages-in-text-files/500
* http://manpages.ubuntu.com/manpages/bionic/man1/stegsnow.1.html

# Hidden Flag 02

**Flag:** `HV19{Dont_confuse_0_and_O}`

We learned from the first hidden flag that the day it is published, the day's challenge is the first place to go. Hidden Flag 2 was published on day 7, whose "Santa Rider"-video has two download options: One is directly embedded in the video, the other is a separate download link. Very suspicious.

The file via the specific download link itself reveals nothing. The filename, however, is *very* suspicious. What is this string? It's not Base64, not Base32 but...? Cyberchef (http://icyberchef.com) reveals the answer: Base58!

Input string: `3DULK2N7DcpXFg8qGo9Z9qEQqvaEDpUCBB1v`
Output: `HV19{Dont_confuse_0_and_O}`



# Hidden Flag 04

**Flag**: `HV19{Squ4ring the Circle}`

The flag from challenge 14 did not look like a typical flag with 1337-code and all. Also, it turned out to be very hard if you wanted to just memorize all the characters eaten by the snake:

`s@@jSfx4gPcvtiwxPCagrtQ@,y^p-za-oPQ^a-z\x20\n^&&s[(.)(..)][\2\1]g;s%4(...)%"p$1t"%ee`

The reason: Achtung, das flag contains *another flag*! If you save the above code and execute it, the hidden flag is revealed:

```bash
$ perl hidden_snek.pl 
Squ4ring the Circle
```

