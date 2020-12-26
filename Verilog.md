# Verilog 编程

---



### 程序

----

This is an simple add.v

- In verilog you can use `+`, `-`, `*`, `/`, `^`…and so on

```verilog
module add (in1, in2, out);
    input [1:0] in1;
    input [1:0] in2;
    output [2:0] out;
    
    assign out = in1 + in2;
    
endmodule
```



The simulation source:

```verilog
module add_sim();
    reg [1:0] in1_sim = 2'b00; // initialize by clearify
    reg [1:0] in2_sim = 2'b00;
    wire [2:0] out_sim;
    
    add_sim uut(.sw(in1_sim), .sw(in2_sim), .led(out_sim));
    
    always #10 in1_sim = in1_sim + 1; // until out of bound
    
endmodule
```



> you can use `signed [1:0]` to clearify a signed input, or it will be treated as unsigned by defult.

---





This is a case of __data flow__ and __structure design__.

- Ignore the stupid output plz.

```verilog
module lab2(
    input in1, input in2,
    output out1, output out2,
    output out3, output out4,
    output out5, output out6,
    output out7, output out8,
    output out9, output out10
    );
    assign out1 = in1;
    assign out2 = in2;   
    
    //data flow;
    assign out3 = ~(in1 | in2);
    assign out4 = ~in1 & ~in2;
    assign out5 = ~(in1 & in2);
    assign out6 = ~in1 | ~in2;
    
    //structure disign;
    wire xORy, NOTx, NOTy, xANDy;
    
    or or1(xORy, in1, in2);
    not notx(NOTx, in1);
    not noty(NOTy, in2);
    and and1(xANDy, in1, in2);
    
    not not1(out7, xORy);
    and and2(out8, NOTx, NOTy);
    not not2(out9, xANDy);
    or or2(out10, NOTx, NOTy); // you need to put output in the first place!
    
endmodule
```



simulation source: 

```verilog
module lab2_sim();
    reg x, y; // 1 bit by defult
    wire xout, yout, out1a, out1b, out1c, out1d, out2a, out2b, out2c, out2d;

    lab2 usd(.in1(x), .in2(y), 
    		.out1(xout), .out2(yout),
    		.out3(out1a), .out4(out1b), .out5(out1c), .out6(out1d), 
    		.out7(out2a), .out8(out2b), .out9(out2c), .out10(out2d));
    
    //data flow;
//    always begin
//        #10 {x, y} = {x, y} + 1;
//        end
initial
begin
    x = 0; y = 0;
    #10
    x = 1; y = 0;
    #10
    x = 0; y = 1;
    #10
    x = 1; y = 1;
end
endmodule
```

* `repeat()`

```verilog
module ...
	initial
    begin
        {sS, sI3, sI2, sI1, sI0} = 6'b0000000;
        repeat(63) 
            #10 {sS, sI3, sI2, sI1, sI0} = {sS, sI3, sI2, sI1, sI0} + 1;
        #10 $finish;
    end
endmodule
```







### 软件操作

----




- When using `initial`, all the `initial` blocks start together.

- `#10 x=2b'00` sequential block(begin... end)
- `#10(or #40, some time) y=2'b00` Parallel block(fork... join)

- difference between `#10 x=2b'00` and `x= #10 2b'00` is the sequence of 'getting RHS' and 'assignment'

1 Bit Full ADDER:

1. data flow
```verilog
module full_add_1b(a,b,cin,sum,cout);
// cin 上一个运算的进位, cout 下一个进位
input a,b,cin;
output sum,cout;
assign {cout,sum}=a+b+cin;
endmodule
```

2. block design
```verilog
module full_add_2b(a,b,cin,sum,cout);
input [1:0]a,b;
input cin;
output [1:0]sum;
output cout;

wire cout1;
full_add_1b u0(a[0],b[0],cin,sum[0],cout1);
full_add_1b u1(a[1],b[1],cout1,sum[1],cout);
endmodule
```



* `input` without type statment: `wire` type
* `output` without type statment: `reg` type
	* `reg` can equal to other type of data, like `wire`





### 上课

----



__Boolean function__

- $x + x\cdot y = x$

    $= x\cdot x + x\cdot y = x\cdot (x + y) = x\cdot 1 = x$



- $x + y\cdot z = (x+y)\cdot (x+z)$

    $= x\cdot 1 + y\cdot z = x\cdot(x+y+z) + y\cdot z = x\cdot x + x\cdot y + x\cdot z + y\cdot z = (x+y)\cdot (x+z)$



- something strange: $(x\cdot y) + (y\cdot z) = (x+y)\cdot (x+z)\cdot (y+x)\cdot (y+z)$



- $x\cdot y+x’\cdot z+y\cdot z$

    $= x\cdot y\cdot (z+z’)+x’\cdot (y+y’)\cdot z+(x+x’)\cdot y \cdot z \\ =x\cdot y\cdot z+x\cdot y\cdot z’+x’\cdot y\cdot z+x’\cdot y’\cdot z+x\cdot y\cdot z+x’\cdot y\cdot z \\ =x\cdot y\cdot z+x\cdot y\cdot z’+x’\cdot y\cdot z+x’\cdot y’\cdot z$

    normalize



- x\^y\^z

    $x\bigoplus y\bigoplus z$





#### Implementation of Boolean Functions

- Minterm: Each of the 2^n^ **AND** combinations (terms) of n logic variables (prime and non-prime form)
    - `0` -> `FALSE`; `1` -> `TRUE`
- Maxterm: Each of the 2^n^ **OR** combinations (terms) of n logic variables (prime and non-prime form)
    - `1` -> `FALSE`; `0` -> `TRUE`

|  x   |  y   |  z   | MinTerm | Designation | MaxTerm  | Designation |
| :--: | :--: | :--: | :-----: | :---------: | :------: | :---------: |
|  0   |  0   |  0   | x'y'z'  |    m~0~     | x +y +z  |    M~0~     |
|  0   |  0   |  1   |  x'y'z  |    m~1~     | x +y +z' |    M~1~     |
|  0   |  1   |  0   | x'y z'  |    m~2~     | x +y'+z  |    M~2~     |
|  0   |  1   |  1   |  x'y z  |    m~3~     | x +y'+z' |    M~3~     |
|  1   |  0   |  0   | x y'z'  |    m~4~     | x'+y +z  |    M~4~     |
|  1   |  0   |  1   |  x y'z  |    m~5~     | x'+y +z' |    M~5~     |
|  1   |  1   |  0   | x y z'  |    m~6~     | x'+y'+z  |    M~6~     |
|  1   |  1   |  1   |  x y z  |    m~7~     | x'+y'+z' |    M~7~     |

> You don't need to simplify the equation to find the minterm, instead, you can use the TRUTH Table to figure out which Minterm appers. 
> Then use ((F)')', use the following Minterms then change it into Maxterms to Find the Answer



#### Karnaugh Map (2,3,4 variables)

The index in the Map:

| AB\CD  |  00  |  01  |  11  |  10  |
| :----: | :--: | :--: | :--: | :--: |
| **00** |  0   |  1   |  3   |  2   |
| **01** |  4   |  5   |  7   |  6   |
| **11** |  12  |  13  |  15  |  14  |
| **10** |  8   |  9   |  11  |  10  |

Minterms
- Sum of minterms (maxterm is also possible)
- Combining all the possible situation of a variable (x and x')
- **The Map Can Fold**
- Cover the 1's in a bigger box as possible
- 

A typical four Karnaugh Map:

| AB\CD  |  00  |  01  |  11  |  10  |
| :----: | :--: | :--: | :--: | :--: |
| **00** |  1   |  1   |      |  1   |
| **01** |      |      |      |  1   |
| **11** |      |      |      |      |
| **10** |  1   |  1   |      |  1   |

- Simplify: $\rm{A'B'C'D' + A'BC'D + A'B'CD' + A'BCD' + AB'C'D' + AB'C'D + AB'CD'}$

Maxterms
- Combind all the 0's
- We get a simplified pattern in minterm pattern, which is **F'**. Then we need to use De'Morgan, **( F' )'**, to reduce it as a product of sum
- Don't write 0's in the Map directly, converted it into **F'** first and use minterm to fill the blank, then convert it back.

| AB\CD  |  00  |  01  |  11  |  10  |
| :----: | :--: | :--: | :--: | :--: |
| **00** |  0   |  0   |      |  0   |
| **01** |      |      |      |  0   |
| **11** |      |      |      |      |
| **10** |  0   |  0   |      |  0   |


- $F(x,y,z) = \sum (1,3,4,6) = \prod (0,2,5,7)$



#### Karnaugh Map (5 variables)

- 32 squares
- two 4-variable maps
- **One Map is ABOVE another**: it has z-axis
- 

A = 0
| BC\DE  |  00  |  01  |  11  |  10  |
| :----: | :--: | :--: | :--: | :--: |
| **00** |  0   |  1   |  3   |  2   |
| **01** |  4   |  5   |  7   |  6   |
| **11** |  12  |  13  |  15  |  14  |
| **10** |  8   |  9   |  11  |  10  |

A = 1
| BC\DE  |  00  |  01  |  11  |  10  |
| :----: | :--: | :--: | :--: | :--: |
| **00** |  16  |  17  |  19  |  18  |
| **01** |  20  |  21  |  23  |  22  |
| **11** |  28  |  29  |  31  |  30  |
| **10** |  24  |  25  |  27  |  26  |

- Don't Care Conditions

In practice, there are some applications where the function is not specified for certain combinations of variables, that is called a "don't care" condition. 

>  In Excess-3 code the 0000, 0001, 0010, 1101, 1110 and 1111 are invalid or unspecified. These are called don’t cares. Also, in design of 4-bit BCD-to-XS-3 code converter, the input combinations 1010, 1011, 1100, 1101, 1110, and 1111 are don’t cares.

- *Prime Implicant*: product term obtained by combining the maximum possible number of squares in the map.
- *Essential* Prime Implicant if it includes a unique( not included by other PI ) minterm.






### Combinational Logic

- the output of the circuit at any time is determined directly from the present combination of inputs
    - operation fully specified logially by a set of Boolean functions
- Design Procedure:
    - The problem is stated
    - The number of available input variables and required output variables is determined.

* How to Convert BCD to EXCESS-3 code?

1. List all the Conversion situations.
2. Write down the minterms of each OUTPUT bits represented by input bits.
3. Simplify it (Don’t care conditions)
4. Draw the final circuit



* Half Adder
    - 2 inputs, 2 outputs (进位 & 原位)
* Full Adder
    - 3 inputs （进位 & x & y）, 2 outputs





#### BCD Addeer (Important)

* Map





#### Four-bit magnitude comparator

Take 2 n-bit numbers and produce 1 if they are equal and 0 otherwise





#### Three to eight line decoder





#### Multiiplexer (复用器)

multiple inputs & selection inputs –> select one input –> multiple parallel process –> select one output and copy it to the output.

| selection — | – input  | output |
| ----------- | -------- | ------ |
| **s~0~**    | **s~1~** | **Y**  |
| 0           | 0        | D~0~   |
| 0           | 1        | D~1~   |
| 1           | 0        | D~2~   |
| 1           | 1        | D~3~   |

$Y=m_0\cdot D_0 +m_1\cdot D_1+m_2\cdot D_2+m_3\cdot D_3$

$Y=m_0\cdot (s_0'+s_1')+m_1\cdot (s_0'+s_1)+m_2\cdot (s_0+s_1')+m_3\cdot (s_0+s_1)$

* Verilog Code:

```verilog
module multiplexer(
	input w,
	input x,
    input y,
    input z,
    input [1:0] s,//select
    output reg o
);
    always @*
        begin
            case (s)
                2'b00: o = w;
                2'b01: o = x;
                2'b10: o = y;
                2'b11: o = z;
            endcase
        end
endmodule
```



