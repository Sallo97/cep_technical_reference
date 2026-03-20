# I/O Pseudoistruzioni [[7](../0_Additional_resources/0_reference.md)]
The handling of I/O operations is delegated to a set of pseudoistruzioni. This set distinguish psedoistruzioni in:

- **program pseudoistruzioni**, which can be directly used in a symbolic program or in auxiliary pseudoistruzioni.

- **auxiliary pseudoistruzioni**, contain common code used by a set of program pseudoistruzioni.

# Sistema programmativo di entrata e di uscita dei dati per la CEP [[7](../0_Additional_resources/0_reference.md)]

The subprogram delegated to I/O handling is called Sistema Programmativo di Entrata e di Uscita dei dati per la CEP (I/O program in short). It is created by matching the necessary program pseudoistruzioni with their auxiliary pseudoistruzioni, saving lots of memory in the process.

The I/O program mainly implements the receivining and sending of numbers in single or double precision. Still, it can be easily extended to support higher precision by defining the necessary pseudoistruzioni.

It can be used directly in a symbolic program or it can be produced during the translation of a Fortran program. The service programs use it.

An important feature of the I/O program is that its implementation is mostly independent from the auxiliary peripherals connected to the CEP.

The pseudoistruzione are dependent to the peripherals in use are:

- a pseudoistruzione for selecting the input peripheral.

- a pseudoistruzione for selecting the output peripheral.

- a pseudoistruzione for sending characters in output, convering them in the format requested by the current output peripheral.

# Input pseudoistruzioni behaviour [[7](../0_Additional_resources/0_reference.md)]

1. Read the data coming from the punched tape in one of the two photoelectic sensors.

2. Convert the data in:
    - integer decimal number to integer binary numbers.
    
    - integer octal number to integer binary number.
    
    - fixed point decimal number to fixed point binary number.
    
    - decimal number with an exponential part to floating point binary number.

3. Stores the converted number in the requested memory location(s). 

Incoming data can be sperated between each other by all characters that introduce no ambiguity during the scanning of them.

The general form of a floating point decimal number D is:

$D = +/- dd \ldots d \ . \ dd \ldots dE  +/- dd \ldots ... d$ where:

- $d$ is a digit.

- '.' (not to confuse with the ellipsis: '$\ldots$') is used to distinguish between the integer and (optional) floating point portion of the mantissa.

- 'E' declares the (optional) start of the exponent. 

- The '+' sign is optional, while the '-' is necessary for declaring negative numbers.

- Insignificat trailing zeroes can be omitted.

A number can be immediately preceded by any character which is not a digit, '.', and '-'; it can be followed by any character that isn't a digit or 'E'.

# Number ranges

- a decimal integer number can be converted in binary if in the range: 

    -34359738368 $\rightarrow$ 34359738368

- a octal integer number can be converted in binary if in the range:

    0 $\rightarrow$ 777777777777

- a decimal number can be converted to a single or double precision binary if in the range:

    -1 $\rightarrow$ 1

- a decimal number can be converted to a floating point single precision binary if its absolute value is in the range:
    
    $10^{-38} \rightarrow 0 \rightarrow 10^{38}$

- a decimal number can be converted to a floating point double precision binary if its absolute value is in the range:
    
    $10^{-4932} \rightarrow 0 \rightarrow 10^{4932}$

The maximum number of digits after the '.' in a fixed point number is $10$ for single precision or $20$ for double precision.

The maximun number of digits in the mandissa for a floating point number is $8$ for single precision or $16$ for double precision.

If the number of digits exceeds said length, the conversion step will truncate them.

# Output pseudoistruzioni behaviour

1. Convert data to send in:
    - integer binary numbers in integer decimal numbers.

    - fixed point binary numbers in decimal numbers withouth the exponential part.

    - floating point binary numbers in decimal numbers with or without an exponential part.

2. punch or print the converted data, each in a specified field and (optionally) with a specified number of decimal digits after the '.', multiplied or divided by a given scale factor, and preceded by an alphabetic sequence.

The general form of an outputted floating point decimal number $D$ is:

$\sigma \sigma \ldots \sigma \ dd \ldots d \ . \ dd \ldots d \ E \ \sigma dd$ where:

- $\sigma$ are arbitrary spaces.

- $d$ is a decimal digit.

- $E$ the (optional) part of the exponent part, which has always two digits. If the digits are missing, they are interpreted as zeroes.

All trailing zeroes of the binary number are translated in spaces. Rounding up is applied if the original binary number exceeded the general form of the output.