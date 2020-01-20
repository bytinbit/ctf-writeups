# HV19.08 SmileNcryptor 4.0

**Task:** You hacked into the system of very-secure-shopping.com and you found a SQL-Dump with $$-creditcards numbers. As a good hacker you inform the company from which you got the dump. The managers tell you that they don't worry, because the data is encrypted.

Dump-File: [dump.zip](https://academy.hacking-lab.com/api/media/challenge/zip/c635204a-6347-45d7-91f8-bd7b94b111f1.zip)

Analyze the "Encryption"-method and try to decrypt the flag.

**Flag:** `HV19{5M113-420H4-KK3A1-19801}`

# Research

The interesting part in this dump are the flags and the credit-card entries. Assumptions: 

* The credit card numbers use the same encryption as the flag
* One clear-text character maps to one encrypted character, since the length of the encrypted credit card strings are equal to the typical length of credit card numbers
* The smilies are just decoration and not necessary.

``` sql
(1,'Sirius Black',':)QVXSZUVY\ZYYZ[a','12/2020'), 
(2,'Hermione Granger',':)QOUW[VT^VY]bZ_','04/2021'),
(3,'Draco Malfoy',':)SPPVSSYVV\YY_\\]','05/2020'),
(4,'Severus Snape',':)RPQRSTUVWXYZ[\]^','10/2020'),
(5,'Ron Weasley',':)QTVWRSVUXW[_Z`\b','11/2020');
-- ... --
REATE TABLE `flags` (
  `flag_id` int(11) NOT NULL AUTO_INCREMENT,
  `flag_prefix` varchar(5) NOT NULL,
  `flag_content` varchar(29) NOT NULL,
  `flag_suffix` varchar(1) NOT NULL,
  PRIMARY KEY (`flag_id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

LOCK TABLES `flags` WRITE;
/*!40000 ALTER TABLE `flags` DISABLE KEYS */;
INSERT INTO `flags` VALUES (1,'HV19{',':)SlQRUPXWVo\Vuv_n_\ajjce','}');
/*!40000 ALTER TABLE `flags` ENABLE KEYS */;
UNLOCK TABLES;
```

Structure of credit cards:

* The first (leading) digit of the IIN identifies the major industry of the card issuer.
* The first 6 digits are the issuer identification number
* Variable length up to 12 digits as account identifier
* A single check digit calculated using the luhn algorithm

The first digit is different for each card network:

* Diners Club and Carte Blanche cards – Begin with a 3, followed by a 0, 6, or 8 and have 14 digits
* American Express cards – Begin with a 3, followed by a 4 or a 7 has 15 digits
* Visa cards – Begin with a 4 and have 13 or 16 digits
* Mastercard cards – Begin with a 5 and has 16 digits
* Discover cards – Begin with a 6 and have 16 digits

Length of the password strings without smileys:

```
1. length 15 => American Express: QVXSZUVY\ZYYZ[a ==> 3(4|7)
2. length 14 => Diner's Club: QOUW[VT^VY]bZ_  ==> 3(0|6|8)
3. length 16 => SPPVSSYVV\YY_\\] 
4. length 16 => RPQRSTUVWXYZ[\]^
5. length 16 => QTVWRSVUXW[_Z`\b 
```

We know now for sure that `Q` maps to `3`. And if a ciphertext reveals patterns, we win!

Initial thought: the charset of the encryption is in a very close range within the ASCII-table and maps to the the numbers `1, 2, 3, 4..0`. This results in a map of ``` [('O', 1), ('P', 2), ('Q', 3), ('R', 4), ('S', 5), ('T', 6), ('U', 7), ('V', 8), ('W', 9), ('X', 0), ('Y', 1), ('Z', 2), ('[', 3), ('\\', 4), (']', 5), ('^', 6), ('_', 7), ('', 8), ('a', 9), ('b', 0)]```. 
However, this was wrong.

Rather, it is a polyalphabetic substitution cipher following Johannes Trithemius: After each encryption, the whole key is shifted by one. 

``` python
def shift_cipher(word):
    n = ord('Q') - ord('3')  # = 30
    result = ""
    for c in word:
        result += chr(ord(c) - n)
        n += 1  # following Trithemius and shift by 1
    return result

entries = {"one": r"QVXSZUVY\ZYYZ[a", "two": r"QOUW[VT^VY]bZ_", "three": r"SPPVSSYVV\YY_\\]", "four": r"RPQRSTUVWXYZ[\]^", "five": r"QTVWRSVUXW[_Z`\b", "flag": r"SlQRUPXWVo\Vuv_n_\ajjce"}

print(shift_cipher(entries["flag"]))
```

When we do this with the first and second credit card entry, they look legit: `378282246310005`, `30569309025904`. Entering the flag in the above script reveals: `5M113-420H4-KK3A1-19801`, which is the flag-part within the curly brackets.

Note: it is not necessary to know the whole key (e.g. if the whole ASCII table is used or just parts of it), it is enough to start with the first letter in the ciphertext and then always shift by one for the next encryption process.

Sauce: https://crypto.interactive-maths.com/polyalphabetic-substitution-ciphers.html







