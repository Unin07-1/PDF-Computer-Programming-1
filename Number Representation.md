Binary representation of numbers takes place via sequences of bits (1 and 0)
Different representation:
+ positive integers
+ signed integers
+ real numbers
###### Positive integers
A sequence of bits   $b_n-1 ... b_1, b_0$   represent the number:
Where 1+1=10

given n bits, it is possible to represents the numbers in the range $(0, 2^n -1)$
+ (with 8 bits) the range 0, 255   --->    $2^8 - 1 = 255$
+ (with 32 bits) the range 0, 4294967295

##### Decimal to binary conversion
A sequence of divisions per 2 and analysis of the reminder
Convert $728_10$               $728:2=364$ 
Result $1011011000_2$

+ Convert $3249_10$
Result: $110010110001_2$
Test
$2^11+2^10+2^7+2^5+2^4+2^0=2048+1024+128+32+16+1$

###### Subtraction
$149-95 = 54$
$10010101 - 01011111 = 00110110$                           $(128+16+4+1) - (64+16+8+4+2+1) = 32+16+4+2$


If range is exceeded, the reminder bit is lost (overflow) --> arithmetic module $2^N$
$11110101 + 00010011 = 1|00001000$

###### Integers with sign (signed)
Signed integers can be represented by various encoding
The most used coding are:
+ encoding sign-value
+ encoding 2-complement

###### Encoding sign-value
The first bit represents the sign, the remaining bits represent the value
+ 1 represents a negative number
+ 0 represent a positive number
+  es (8 bits): 10001001 represents -9

It implies a double representations of the zero:
+ 00000000 and 10000000
The numbers represented are in the range $(-2^n¯^1 +1,    2^n¯^1-1)$ 

###### Encoding 2-complement
The most significative bit represent the sign information. A negative number is obtained by calculating the complement of the positive number (zeros and ones are inverted) and then adding 1

It implies a unique representation of the zero: 00000000
The numbers represented in the range $(-2^n¯^1 +1,    2^n¯^1-1)$
+ (with 32 bits) (-2147483648; 2147483647)
+ (with 8 bits) (-128; 127)

It is cyclic representation : -X in a 2-complement is written as $2^n-X$ in the representation without the sign (unsigned)

Applying 2-complement twice gets you the number you started from: $X = f (f(x))$
