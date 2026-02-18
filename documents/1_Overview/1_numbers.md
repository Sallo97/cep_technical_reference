# Supported numbers [[9](../0_Additional_resources/0_reference.md)]
The CEP supports a variety of number types.

## Integer numbers
An integer number $x$ occupies a memory word, i.e. $x = x_0x_1x_2\ldots x_{35}$, ranging in-between $-34359738367 \leq x \leq +34359738367$. 

The first bit $x_0$ determines the sign of the number:

- if $x_0 = 0 \rightarrow x \geq 0$

- if $x_0 = 1 \rightarrow x < 0$

The remaining 35 bits specify the value of the number according to the following formula:

$x_1 \times 2^{34} + x_2 \times 2^{33} + \ldots + x_{35} * 2^{0}$

## Fixed Point Numbers
Fixed point numbers can be both in **simple precision** or **double precision**.

### Simple Precision Fixed Point Numbers
Simple precision fixed point numbers occupy one memory word, i.e. $x = x_0 x_1 x_2 \ldots x_{35}$ with the value ranging in-between $-1 \leq x < 1$. 

The sequence of bits is interpreted as:

$- x_0 \times 2^0 + x_1 \times 2^{-1} + x_2 \times 2^{-2} + \ldots + x_{35} \times 2^{-35}$

### Double Precision Fixed Point Numbers
Doubles precision fixed point numbers **occupy two memory words**, i.e. $x = x_0 x_1 x_2 \ldots x_{35} \ 0 \ x_{36} x_{37} \ldots x_{70}$ with values ranging in-between $-1 \leq x < 1$.

The sequence of bits is interpreted as:

$- x_0 \times 2^0 + x_1 \times 2^{-1} + x_2 \times 2^{-2} + \ldots + x_{35} \times 2^{-35} + x_{36} \times 2^{-36} + \ldots + x_{70} \times 2^{-70} $

## Floating Point Numbers
Floating point numbers can be both **simple precision** or **double precision**.

### Simple Precision Floating Point Numbers
Simple precision floating point numbers occupy one memory word, i.e. $x = x_0 x_1 x_2 \ldots x_{35}$. 

Their value is defined as $x = x_f \times 2^{x_e}$ where $x_f$  is the **fractional part** and $x_e$ is the **exponent**.

#### The fractional part $x_f$ 

$x_f$ is the fractional part, composed by the first 27-bits of the sequences ($x_f = x_0 x_1 x_2 \ldots x_{27}$). The value is computed as:

$- x_0 \times 2^0 + x_1 \times 2^{-1} + x_2 \times 2^{-2} + \ldots + x_{27} \times 2^{-27}$

The value can range in-between:

- if $x < 0 \rightarrow -1 \leq x_f < - \frac{1}{2}$

- if $x > 0 \rightarrow \frac{1}{2} \leq x_f < 1 $

- if $x = 0 \rightarrow x_f = 0$

#### The exponent part $x_e$

$x_e$ is the exponent part, composed by the last 8-bits of the original sequence ($x_e = x_28 x_29 x_30 \ldots x_{35}$ ).

Its value is computed as:

$x_{28} \times 2^{7} + x_{29} \times 2^{6} + \ldots x_{35} \times 2^{0} - 2^7$

The value can range in between $-128 \leq x_e \leq 127$ with $x_e = -128$ when $x = 0$


### Double Precision Floating Point Numbers
Double precision floating point numbers occupy **two memory words**, i.e. $y = y_0 y_1 y_2 \ldots y_{35} \ 0 \ y_{36} y_{37} \ldots y_{70}$. 

Their value is defined as $y = y_{f} \times 2^{y_e}$  where $y_f$ is the **fractional part** and $y_e$ is the **exponent**.

#### The fractional part $y_f$ 
$y_f$ is the **fraction part**, composed by the first 55-bits of the sequence ($y_f = y_0 y_1 y_2 \ldots y_{55}$). The value is computed as:

$-y_0 \times 2^{0} + y_1 \times 2^{-1} + y_2 \times 2^{-2} + \ldots + y_{55} \times y^{-55}$

The value can range in-between:

- if $y < 0 \rightarrow -1 \leq y_f < - \frac{1}{2}$

- if $y > 0 \rightarrow \frac {1}{2} \leq y_f < 1$

- if $y = 0 \rightarrow y_f = 0$

#### The exponent part $y_e$
$y_e$ is the exponent part, composed by the last 15-bits of the original sequence ($y_e = y_{56} y_{57} \ldots y_{70}$). The value is computed as:

$y_{56} \times 2^{14} + y_{57} \times 2^{13} + \ldots + y_{70} \times 2^{0} - 2^{14}$

The value ranges in-between $-2^{14} \leq y_e \leq 2^{14} - 1$ with $y_e = - 2^{14}$ if $y = 0$.

## Conversion rate from binary numbers to decimal numbers

Fixed point numbers in simple or double precision are equivalent to decimal numbers having up to $10$ or $20$ digits respectively.

Floating point numbers in simple or double precision are equivalent to decimal numbers having:

- the fractional part up to $8$ or $16$ digits respectively.

- the exponent part in base $10$ ranging in $[-38, +38]$ for single precision and $[-4932, +4932]$ respectively.