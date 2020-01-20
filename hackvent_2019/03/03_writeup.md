# HV19.03 Hodor, Hodor, Hodor

**Task:**

```
$HODOR: hhodor. Hodor. Hodor!?  = `hodor?!? HODOR!? hodor? Hodor oHodor. hodor? , HODOR!?! ohodor!?  dhodor? hodor odhodor? d HodorHodor  Hodor!? HODOR HODOR? hodor! hodor!? HODOR hodor! hodor? ! 

hodor?!? Hodor  Hodor Hodor? Hodor  HODOR  rhodor? HODOR Hodor!?  h4Hodor?!? Hodor?!? 0r hhodor?  Hodor!? oHodor?! hodor? Hodor  Hodor! HODOR Hodor hodor? 64 HODOR Hodor  HODOR!? hodor? Hodor!? Hodor!? .

HODOR?!? hodor- hodorHoOodoOor Hodor?!? OHoOodoOorHooodorrHODOR hodor. oHODOR... Dhodor- hodor?! HooodorrHODOR HoOodoOorHooodorrHODOR RoHODOR... HODOR!?! 1hodor?! HODOR... DHODOR- HODOR!?! HooodorrHODOR Hodor- HODORHoOodoOor HODOR!?! HODOR... DHODORHoOodoOor hodor. Hodor! HoOodoOorHodor HODORHoOodoOor 0Hooodorrhodor HoOodoOorHooodorrHODOR 0=`;
hodor.hod(hhodor. Hodor. Hodor!? );
```

**Flag:** `HV19{h01d-th3-d00r-4204-ld4Y}`

## Research

If there's Brainfuck, there must be Hodor: http://www.hodor-lang.org

I first started writing a parser based on https://github.com/hummingbirdtech/hodor/blob/master/legend.js.

It didn't parse nicely, and after I've given some thought it was much easier to just install the language and run the script:

```bash
$ npm install -g hodor-lang                                         
$ hodor hodor.hd
HODOR: \-> hodor.hd
Awesome, you decoded Hodors language! 

As sis a real h4xx0r he loves base64 as well.

SFYxOXtoMDFkLXRoMy1kMDByLTQyMDQtbGQ0WX0=
$ echo "SFYxOXtoMDFkLXRoMy1kMDByLTQyMDQtbGQ0WX0=" | base64 -d         
HV19{h01d-th3-d00r-4204-ld4Y}
```

