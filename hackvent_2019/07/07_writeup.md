# HV19.07 Santa Rider

**Task:** Santa is prototyping a new gadget for his sledge. Unfortunately it still has some glitches, but look for yourself.

**Flag:** `HV19{1m_als0_w0rk1ng_0n_a_r3m0t3_c0ntr0l}`

# Research

I see 8 lights.

My brain computer is triggered by the number 8: 8 lights = 8 bits = ASCII-Values = BLINKENLIGHTS!

ACHTUNG!
ALLES TURISTEN UND NONTEKNISCHEN LOOKENSPEEPERS!
DAS KOMPUTERMASCHINE IST NICHT FÜR DER GEFINGERPOKEN UND MITTENGRABEN! ODERWISE IST EASY TO SCHNAPPEN DER SPRINGENWERK, BLOWENFUSEN UND POPPENCORKEN MIT SPITZENSPARKEN.
IST NICHT FÜR GEWERKEN BEI DUMMKOPFEN. DER RUBBERNECKEN SIGHTSEEREN KEEPEN DAS COTTONPICKEN HÄNDER IN DAS POCKETS MUSS.
ZO RELAXEN UND WATSCHEN DER BLINKENLICHTEN.

We extract all the images with the following command and then write down the binary values (light on = 1, light off = 0):

`ffmpeg -i santarider.mp4 -vf fps=10 output/blinkenlights%05d.jpg -hide_banner`

I had to tweak the amount of extracted images per second and settled on 10 per second. A short Python script printed the flag: 

```python
with open("binvalues.bin", "rb") as f:
    for line in f:
        print(chr(int(line, 2)), end="")
```

This reveals almost the flag - I interpreted three lights wrong, but my inner error correction worked successfully.