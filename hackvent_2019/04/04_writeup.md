# HV19.04 password policy circumvention

**Task:**

Santa released a new password policy (more than 40 characters, upper, lower, digit, special).

The elves can't remember such long passwords, so they found a way to continue to use their old (bad) password:

```
merry christmas geeks
```

[HV19-PPC.zip](https://academy.hacking-lab.com/api/media/challenge/zip/6473254e-1cb3-444e-9dac-5baeaaaf6d11.zip)

**Flag:** `HV19{R3memb3r, rem3mber - the 24th 0f December}`

## Research

The downloaded `.ahk` is a file format for "AutoHotkey", a tool to script keyboard shortcuts (and more) in Windows (https://www.autohotkey.com/).

I installed it and continuously typed `merry christmas geeks`.

However, my typing speed is way above average and AutoHotkey just couldn't keep up with it. 

Type slow you must!

After even more unsuccessful attempts and finally typing exceedingly slow (seriously, those elves aren't working in a capitalist enterprise), it revealed the flag. 