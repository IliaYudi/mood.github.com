---
layout: post
title:  "What is unicode?"
date:   2019-09-02 00:20:14 +0600
categories: jekyll update

---

## What are Unicode and encodings?
---
Many programmers, as well as people who have one or another attitude to working with a computer, don't understand what Unicode, encodings means, which, doesn't prevent them from using (and sometimes not so) successfully.
We will try to understand these concepts as simply as possible and find out how Unicode works.

In fact, every person in the modern world interacting with the Internet constantly uses the product of the intellectual work of the three brilliant founders of the Unicode standard. Already in 1987, work began on the creation of Unicode architecture using three people: Lee Collins, Joe Becker and Mark Davis.

![sample post]({{site.baseurl}}/images/davis-collins.jpg)


<center> Pictured (left to right): Mark Davis and Lee Collins celebrate the twenty-fifth anniversary of the Unicode Consortium in 2016. </center> 

So what did these three people give us, and why did Unicode become so popular?

Unicode is a generally accepted worldwide coding standard, from alphabet letters to mathematical symbols.
The question arises, why should symbols, letters, emoticons, signs be encoded, can they really not be used as is?
The computer only accepts bits and bytes, for it there are no letters or punctuation marks in the form in which we are accustomed to perceive them. Therefore, we see the letter or symbol in the e-mail or program in decrypted form, they are transmitted and stored in encoded form.
Encoding is a way of storing the information you want to convey.
For example, the English letter ```A```, is transmitted as ```0100 0001```. This letter weighs exactly one byte and consists, as you see, of 8 bits.
But most often we see encoded letters in the hexadecimal notation (HEX), where alphabetic values are used, in addition to numbers:

```
0 1 2 3 4 5 6 7 8 9 A B C D E F
```

Let's try to encode the word ```Documentat.io``` in ASCII encoding using the table:

![Image](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/ASCII_Code_Chart.svg/1200px-ASCII_Code_Chart.svg.png)



Our word will look like:
```
44 6F 63 75 6D 65 6E 74 61 74 2E 69 6F
```
When the computer reads the information, that you want to transfer, it must understand what encoding you used, otherwise you will encounter the fact that you will see characters that aren't translated into a normal language, the so-called "mojibake".
```
ЊСЃСЏ. R’РѕС ‚СЃРєРѕР» СЊРєРѕ РѕРЅР
```
Surely you have met such symbols more than once.

## Several problems and one solution.
---
In those days when there was no Internet, different countries came up with their own encodings. For many systems and computers in Western countries, the ASCII "American Standard Code for Information Interchange" encoding was widely used. In the days before Unicode it was the standard. Japan, the USSR and other countries developed their own unique encodings. That is why there is a great variety of them now.

All this “worked”, but in order to read a letter from Japan, for example, you had to install the encoding on the computer in which the letter was written, and there is no guarantee that the system will be able to support it. The solution was and people used faxes
![sample post]({{site.baseurl}}/images/fax.jpg)

But the era of the world wide web was approaching, and this carried a number of problems that required a concise and universal solution.
Let's formulate these problems:
1. *Versatility.*
The standard must have included many alphabets and symbols adopted in different countries of the world. And due to the many encoded characters there is a problem number 2.
2. *Storage.*
To encode a large number of letters and characters, the code space must be large.
3. *Compatibility.*
The new standard should support the old ASCII coding, while not only supporting, but coding identically, for legacy systems to work.
4. *Zeros.*
The encoded character should not have 8 zeros in a row, this is due to the fact that numerous outdated systems perceived this as the end of the transmission and stopped the further process of reading data. For them, the line ended.

The solution to the problems was the standard proposed by the non-profit organization Unicode Consortium.
Their UTF-8 encoding solved all these problems.

1. The range `0 - 007F` has been allocated for legacy ASCII
2. The range above `007F` is a specific sequence of bytes
3. At the beginning there is always a counter indicating the number of bytes in the encoded character, the counter starts with units:
`110x xxxx` - two units indicate 2 bytes in the sequence (including itself), the second byte begins with` 10xx xxxx`, where 10 indicates the continuation of the encoded character
4. If the value is encoded with 3 bytes, then it will look like this:
 `1110 xxxx`` 10xx xxxx`` 10xx xxxx`
5. Such a unique and, at the same time, simple idea solved the problem of zeros in a string, there simply cannot be 8 in a row.
6. More than one million possible encoded characters (!).

Unicode can be compared with the legend of the construction of the Tower of Babel, only if God made people speak different languages, unicode, on the contrary, combines different languages into one universal.
In UTF-8, each code position from 0 to 127 is stored in one byte, all subsequent in 2, 3, etc. up to 6 bytes.
 Let's try to encode the word ```Documentat.io```, but now in Unicode:

```
U + 0044 U + 006F U + 0063 U + 0075 U + 006D U + 0065 U + 006E U + 0074 U + 0061 U + 0074 U + 002E U + 0069 U + 006F
```
The same word can be represented in this form:
```
U + 4400 U + 6F00 U + 6300 U + 7500 U + 6D00 U + 6500 U + 6E00 U + 7400 U + 6100 U + 7400 U + 2E00 U + 6900 U + 6F00
```
The word from this will not change, but *the order of storage of bytes* in Unicode is changing. In order for the system to understand in which order to read them, special *tags* were invented: `FE FF` and` FF EE`, these tags at the beginning of each line tell the computer in what order to read this line, the tags are called BOM (Byte Order Mark)

## What to choose UTF-8 or UTF-16?
---
Now that it has become clearer to us what the UTF-8 encoding is, it will not be difficult to understand the UTF-16 encoding. UTF-8 is named because it uses at least eight bits (one byte) to store the encoded character.
Accordingly, UTF-16 uses a minimum of sixteen bits (or two bytes) to store the encoded character. This increases the memory for storing and transmitting data, but UTF-16 has its own number of advantages:
* Use a language (alphabet) that cannot fit in 1 byte - select UTF-16.
* Java, JavaScript are based on UTF-16
* The internal format of Windows libraries is also based on UTF-16, which makes compatibility with Windows systems better.

Along with the advantages, the UTF-16 encoding has its drawbacks:
* Larger size for data storage
* Many null bytes in the range `0 - 007F`
* Worst Unix compatibility compared to UTF-8
* Ability to change the byte order of UTF-16LE (little endian) and UTF-16BE (big endian)

Advantages of UTF-8:
* No null bytes
* It performs better for text storage and in working with network protocols.
* Excellent compatibility with ASCII encoding and, as a result, less data when using English.
* No byte order dependency
* Good Unix compatibility.

UTF-8 disadvantages:
* Takes up more space than UTF-16 in languages that use 2 bytes for storage
* Slower in-memory indexing compared to UTF-16.

However, you should always consider the environment in which you work and use the encoding recommended for it.
