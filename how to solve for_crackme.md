first we comple program 'gcc -g if_crackme.c -o if_crackme'
then we start it using gdb 'gdb ./if_crackme'
declare brakepoint 'b main'
then we run it 'r'
disassemble it 'disas main'

                            ASM review.

movl   $0x14,-0x4(%rbp) // 0x14 is 20 in hexadesimal. -0x4(%rsp) is x. int x = 20; in C.
              printf
0x0000000000400485 <+15>:	mov    -0x4(%rbp),%eax        // we put -0x4(x) in %eax // finale argument for printf.
0x0000000000400488 <+18>:	mov    %eax,%esi              // put %eax in to %esi // 2nd argument for printf.
0x000000000040048a <+20>:	mov    $0x4011b8,%edi         // link address: '0x4011b8' to %edi // 1st argument for printf.
0x000000000040048f <+25>:	mov    $0x0,%eax              // clear %eax using 0x0
0x0000000000400494 <+30>:	call   0x400380 <printf@plt>  // print

              compare
0x0000000000400499 <+35>:	cmpl   $0x5,-0x4(%rbp)    // compare 0x5(5) to -0x4(x). if(x > 5){}; in C.
0x000000000040049d <+39>:	jle    0x4004ab <main+53> // jump to <main+53>.

            if statemant:
0x000000000040049d <+39>:	jle    0x4004ab <main+53> // jump to <main+53>.
0x000000000040049f <+41>:	mov    $0x4011ce,%edi     // puts address: '$0x4011ce' in to %edi.
0x00000000004004a4 <+46>:	call   0x400370 <puts@plt>// printf.

                else:
0x00000000004004a9 <+51>:	jmp    0x4004b5 <main+63> // jump to <main+63> skips all and finish program.
0x00000000004004ab <+53>:	mov    $0x4011e3,%edi     // puts address: '0x4011e3' to %edi.
0x00000000004004b0 <+58>:	call   0x400370 <puts@plt>// printf().

          clean up and close.
0x00000000004004b5 <+63>:	mov    $0x0,%eax // clean up %eax using 0x0.
0x00000000004004ba <+68>:	leave            // leave.
0x00000000004004bb <+69>:	ret              //close and return.

                                  HOW TO CRACK?
if you want to change the x > ?.

you have to quit your programm 'q'.
then declare a breakpoint 'b *0x400499' or what do have in cmpl line -> 0x0000000000400499 <+35>:	cmpl   $0x5,-0x4(%rbp) we need 0x0000000000400499.
and run it 'r'.
use 'x/5b 0x400499'        //  you will see something like: '0x400499 <main+35>:	-125	125	-4	5	126' we need 5 here.
use set {char}0x40049c = 2 // or what you want we use 0x40049c because 0x400499 + 3 bytes = 0x40049c.
to check use 'x/5b 0x400499'.

if u want to go thrue statement or go derectly at if statement.

you have to quit your programm 'q'.
then declare main breakpoint using 'b main'.
after that use 'set $rip = 0x4004a9' // this will take u to else statement.
or use 'set $rip = 0x40049d' to go to if statement.

if you want to change x's value.
you have to quit your programm 'q'.
then declare main breakpoint using 'b *0x40047e' // this is address of -0x4(x).
use 'x/b 0x40047e' // we dont need to add anything.
use 'set {int}0x40047e = 30 // or any number you want.
