# pobjtohex
Restored program OBJTOHEX.COM from the Hi-Tech C compiler v3.09

objtohex contains a number of bugs, some examples
 
1. The code to write crc checksums will only work for a single byte checksum. There are several errors

a. There is a code generation bug triggered that will only write to the first location.

b. The crc calculation is only done to 16 bit precision so 32bit crcs are incorrect.

c. The crc calculation also doesn’t allow an offset > 16 bits.

2. There are several places where values that are encoded into a buffer using one of i16tob or u32tob,  but are read back via *(int *) or *(long *) casts.

For intel this is benign but is likely to fail for other architectures.

3. Function prototypes don’t appear to have been used, causing ints and longs to be passed without appropriate casts. Some of these work ok on the Intel architecture but would likely fail on other architectures. In one case it is fortunate that an arbitrarily written 16 bits are overwritten later in the code.

4. Parsing a checksum list hangs if there is an unterminated comment, as it goes into an infinite look looking for a ‘*’ when all it is seeing is EOF.
 
I would also note there is quite a bit of code to support building DOS & various flavours of Unix loadable file. Whilst the unix code may be of use as a basis for UZI180, most of this code is of little use.

To compile the objtohex run the command

      gcc -O2 -oobjtohex objtohex.c

Mark Ogden 20-08-2022
