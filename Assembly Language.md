# MIPS



### Overview

- Assembly language 
    - The main body of assembly language is assembly instruction. Assembly instructions and machine instructions differ in instruction expression. Assembly instruction is a *human-friendly* writing format of machine instruction. Assembly language is the mnemonic(记忆的) of machine language. 

        - For example 

            machine instruction “1000100111011000” means that send the contents of register BX to AX. 

            The assembly instructions are written as “mov ax, bx”. 

            Such writing is similar to human language and easy to read and remember. 

    - Feature：programs written in assembly language are inherently machine-specific and must be totally rewritten to run on another computer architecture 

    - CISC (intel x86) vs RISC (ARM , MIPS)



### Assembly Program Structure

- Program Structure 
    - data declarations
        - `.data`
    - program code
        - `.text` 
    - data declaration section followed by program code section 

- **Data** Declarations 
    - placed in section of program identified with assembler directive `.data` 
    - declare variable names used in program; storage allocated in main memory (RAM) 

- **Code** 
    - placed in section of text identified with assembler directive `.text`
    - contains program code (instructions) 
    - starting point for code e.g .ecution given label main: 
    - ending point of main code should use exit system call (see below in “System Calls” part) 

- Comments 
    - anything following # on a line 

        \# This stuff would be consideredd as comment





### **System Calls** 

| Service      | System call code | Arguments                                              | Result                      |
| ------------ | :--------------: | ------------------------------------------------------ | --------------------------- |
| Print_int    |        1         | $a0 = integer                                          |                             |
| Print_float  |        2         | $f12 = float                                           |                             |
| Print_double |        3         | $f12 = double                                          |                             |
| Print_string |        4         | $a0 = address of null-terminated string to print       |                             |
| Read_int     |        5         |                                                        | integer (in \$v0)           |
| Read_float   |        6         |                                                        | float (in \$f0)             |
| Read_double  |        7         |                                                        | double (in \$f0)            |
| Read_string  |        8         | $a0 = buffer, \$a1 = length                            |                             |
| sbrk         |        9         | $a0 = amount                                           | address (in \$v0)           |
| exit         |        10        |                                                        |                             |
| Print_char   |        11        | $a0 = char                                             |                             |
| Read_char    |        12        |                                                        | char (in \$v0)              |
| open         |        13        | $a0 = filename (string),  \$a1 = flags,  \$a2 = mode   | file descriptor (in \$v0)   |
| read         |        14        | $a0 = file descriptor,  \$a1 = buffer,  \$a2 = length  | num chars read (in \$v0)    |
| write        |        15        | \$a0 = file descriptor,  \$a1 = buffer,  \$a2 = length | num chars written (in \$v0) |
| close        |        16        | \$a0 = file descriptor                                 |                             |
| exit2        |        17        | \$a0 = result                                          |                             |



### Machine Language & Assembly Language

- **Machine instruction** ：a binary representation used for communication with a computer system. 

- **Assembly instruction**： a symbolic representation of machine language 

- **Assembler**: translate assembly codes into binary instructions



### Data Types and Literals

In **MIPS32** 

- Unit Conversion 
    - **1 word = 32bit = 2\*half word(2\*16bit) = 4\* byte(4\*8bit)** 

- Data Storage: 
    - Instructions are **all 32 bits**(1 word) 
    - A character requires **1 byte** of storage 

- Literals： 
    - Characters enclosed in single quotes. e.g. ‘C' 
    - Strings enclosed in double quotes. E.g. “a String” 
    - Numbers in code. e.g. 10



### Registers

- **Registers** are small storage areas used to store data in the CPU, which are used to temporarily store the data and results involved in the operation. 
    - \$zero, \$at, \$v0~1, \$a0~3, \$t0~1
- All MIPS arithmetic instructions must operate on registers 
- The **size of registers** in MIPS32 is **32 bits**

| NAME          | NUMBER | USE                                                        | PRESERVED <br/> ACROSS<br/> A CALL? |
| ------------- | :----: | ---------------------------------------------------------- | :---------------------------------: |
| **\$zero**    |   0    | The Constant Value 0                                       |                N.A.                 |
| **\$at**      |   1    | Assembler Temporary                                        |                 No                  |
| **\$v0~\$v1** |  2~3   | Values for Function Results<br/> and Expression Evaluation |                 No                  |
| **\$a0~\$a3** |  4~7   | Arguments                                                  |                 No                  |
| **\$t0~\$t7** |  8~15  | Temporaries                                                |                 No                  |
| **\$s0~\$s7** | 16~23  | Saved Temporaries                                          |                *Yes*                |
| **\$t8~\$t9** | 24~25  | Temporaries                                                |                 No                  |
| **\$k0~\$k1** | 26~27  | Reserved for OS Kernel                                     |                 No                  |
| **\$gp**      |   28   | Global Pointer                                             |                *Yes*                |
| **\$sp**      |   29   | Stack Pointer                                              |                *Yes*                |
| **\$fp**      |   30   | Frame Pointer                                              |                *Yes*                |
| **\$ra**      |   31   | Return Address                                             |                 No                  |





### Data Declaration

​		`name:		storeage_type	value`

```assembly
var1:		.word	3			# create a single integer:
								# variable with initial value 3

array1:		.byte	'a','b'		# create a 2-element character
								# array with elements initialized
								# to a and b

array2: 	.space	40			# allocate 40 consecutive bytes,
								# with storage unintialized(\0)
								# could be used as a 40-element
								# character array, or a 
								# 10-element integer array;
								# a comment should indicate it.

string1:	.asciiz "Print this\n"		# declare a string ends with \0

string2:	.ascii	"Print that\n"		# declare a string ends without \0
```



### Load ( Load to Register)

```assembly
lw		register_destination,	RAM_source		# lw == load word
							#<--
												# copy word (4 bytes) at
												# RAM_source location
												# to register_destination

lb		register_destination,	RAM_source		# lb == load byte
							#<--
												# copy byte at source RAM
												# location to low-order byte of
												# destination register,
												# and fill up rest of the bytes
                                                # with the sign byte

li		register_destination,	value			# li == load immediate
							#<--
												# load immediate value into
												# destination register
```

Differences between `lw` and `lb` : `RAM_source` of `lw` must be multiple of 4, other than `lb`, which loads byte.



### Store (Store to Memory)

```assembly
sw		register_source,	RAM_destination		# sw == save word
						#-->
												# store the word in source register
												# into RAM_destination

sb		register_source,	RAM_destination		# sb == save byte
						#-->
												# store the byte (low-order) in
												# source register into RAM_distination
```





### Addressing on the Memory

- Direct addressing :
    - `la` : load the address into the register
- Indirect addressing :
    - `lw` & `sw` : using the content in register as address
- Baseline / index addressing : 
    - using the sum of baseline address and offset as address



#### Direct Addressing

`la		$t0, var1`      <—

- **Load the address which is labeled by `var1`** into the Register `t0` . 
    - NOTIC : `var1` could be either *a label of data* or *a MIPS instruction*.



#### Indirect Addressing

`lw		$t2, ($t0)`       <—

- **Load the word from the memory unit whose address is in the register `t0`** to the register `t2`.

`sw		$t2, ($t0)`       —>

- **Store the word in register `t2`** to the memory unit whose address is in the register `t0`.



#### Baseline / Index Addressing

`lw		$t2, 4($t0)`     <—

- **Load the word** from the memory unit whose address is the value in register `t0` + 4  to the register `t2`.

`sw		$t2, -12($t0)`    —> 

- **Store the word** in register `t2` to the memory unit whose address is the value in the register `t0` - 12. 





### Arithmetic Instructions

```assembly
add		$t0,$t1,$t2		# $t0 = $t1 + $t2;
						# add as signed integers (2's complement)

sub		$t2,$t3,$t4		# $t2 = $t3 - $t4

addi	$t2,$t3,5		# $t2 = $t3 + 5; "add immediate"
# no subi

addu	$t1,$t6,$t7		# $t1 = $t6 + $t7;
addu	$t1,$t6,5		# $t1 = $t6 + 5;
						# add as unsigned integers

subu	$t1,$t6,$t7		# $t1 = $t6 - $t7;
subu	$t1,$t6,5		# $t1 = $t6 - 5;
						# subtract as unsigned integers
```
```assembly
mult	$t3,$t4			# multiply 32-bit quantities in $t3
						# and $t4, and store 64-bit
						# result in special registers
						# Hi and Lo

div		$t5,$t6			# Lo = $t5 / $t6	(integer quotient)
						# Hi = $t5 mod $t6	(remainder)

mfhi	$t0				# move from Hi
						# $t0 = Hi

mflo	$t0				# move from Lo
						# $t0 = Lo
```









## Assembly Codes :



##### Hello World!

```assembly
# data segment
	.data
str:	.asciiz "hello Internet\n"

# text segment
	.text
	.globl main
main:					# execution starts here
	 la $a0,str			# put string adress into $a0
	 li $v0,4			# system call to print
	syscall
	
	 li $v0,10			# exit
	syscall
```



##### Print Aa

```assembly
.data 
	str: .asciiz "A"
.text
main:
	la $a0, str
	li $v0, 4
	syscall
	lb $t0,($a0)
	addi $t0,$t0,32			# the ascii of 'A' add 32
	sb $t0,str				# get the ascii of 'a'
	syscall
	li $v0,10
	syscall 
```



##### Calculate dividend 

```assembly
.data 
	str1: .asciiz "13/4 quotient is: "
	str2: .asciiz ", reminder is: "
.text
main:
	la $a0,str1
	li $v0,4
	syscall
	
	li $t0,13
	li $t1,4
	divu $t0,$t1
	mflo $a0				# load Lo, which is the quotient
	li $v0,1				# print an int
	syscall
	
	la $a0,str2
	li $v0,4
	syscall
	
	mfhi $a0				# load Hi, which is the remainder
	li $v0,1
	syscall
	
	li $v0,10
	syscall
```





##### Print Names

```assembly
.data
	name:	.space 16
	mick:	.ascii "mick\n"
	alice:	.asciiz "alice\n"	# can change the .asciiz to .ascii
	tony:	.asciiz "tony\n"
	chen:	.asciiz "chen\n"
	
.text
main:
	la $t0,name
	
	la $t1,mick				# load address of mick to t1
	sw $t1,($t0)			# save address in t1 to address in t0, which is name
	la $t1,alice
	sw $t1,4($t0)
	la $t1,tony
	sw $t1,8($t0)
	la $t1,chen
	sw $t1,12($t0)
	
	li $v0,4
	lw $a0,0($t0)
	syscall
	
	li $v0,10
	syscall
```

This will print from the lable `mick` till the next `\0`

