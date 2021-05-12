# SPACEWAR

Click to see video clip:

[![Watch the video](https://i.imgur.com/zEJahsal.jpg)](https://youtu.be/fTiHRAKjyho)

![Score](https://i.imgur.com/jzlCG28l.jpg)

![Score](https://i.imgur.com/74w9Fgel.jpg)

![Score](https://i.imgur.com/aq88NXRl.jpg)



Please note that all patches for the AR11 board is on the ar11-patch branch.

The original [Spacewar!](https://en.wikipedia.org/wiki/Spacewar!) game was written for the PDP-1 minicomputer and there were several ports to other architectures.

This port to the PDP-11 architecture were done by Bill Seiler & Larry Bryant in 1974. It was then submitted to DECUS as 11-192

![DECUS entry](https://i.imgur.com/rjaWX4X.png)

Ufortunatley DECUS threw away all the sources many years ago. By luck I got in contact with Bill Seiler and he sent me the source files. The sources were provided to me as scanned documents. With some help the original PDFs were tidied up to help the OCR process. Then I partly OCRed and manually transcribed all files into text files.
The files were then then fed into the PAL11-S assembler and numerous errors were detected already when trying to get the files pass the syntax check. Then the output listing files was compared side by side with the original scanned files. Byte for byte were checked and yet more problems were found. Finally during linking a number of issues with global symbols were found. In the end the program assembled and linked correctly.

## Assembling

In case you just want to run the binary and not want the fun of building it from scratch, just grab the SPACEWAR.ABS file in the ar11-patch branch (if you have a AR11 board in you machine) and run in from 025424. Make sure that the switch register is set to all zeroes. I have seen that otherwise it gets stuck in the IOX library. Probably a non-zero SR indicate some optons to the IOX library, but I haven't investigated that further. Also make sure to also load the IOX binary in the IOX-11 subfolder into memory before starting the SPACEWAR. 

It is possible to assemble the files located in the SRC folder using PAL11-S assembler under SimH. PAL11-S is part of the [PDP-11 Paper Tape Software](http://bitsavers.informatik.uni-stuttgart.de/www.computer.museum.uq.edu.au/pdf/DEC-11-XPTSA-B-D%20PDP-11%20Paper%20Tape%20Software%20Handbook.pdf). 


First toggle in the bootstrap loader
```
sim> SET CPU 11/05
Disabling XQ
sim> SET CPU 16K
sim> 
sim> 
sim> ; Disable devices that we don't need
sim> SET HK DISABLE
sim> SET DZ DISABLE
sim> SET RL DISABLE
sim> SET RX DISABLE
sim> SET RP DISABLE
sim> SET RQ DISABLE
sim> SET TM DISABLE
sim> SET TQ DISABLE
sim> SET RK DISABLE
sim> 
sim> ; Enable the high-speed paper tape reader/punch
sim> SET PTR ENABLE
sim> SET PTP ENABLE
sim> DEPOSIT 037744 016701
sim> DEPOSIT 037746 000026
sim> DEPOSIT 037750 012702
sim> DEPOSIT 037752 000352
sim> DEPOSIT 037754 005211
sim> DEPOSIT 037756 105711
sim> DEPOSIT 037760 100376
sim> DEPOSIT 037762 116162
sim> DEPOSIT 037764 000002
sim> DEPOSIT 037766 037400
sim> DEPOSIT 037770 005267
sim> DEPOSIT 037772 177756
sim> DEPOSIT 037774 000765
sim> DEPOSIT 037776 177550
sim> ATTACH PTR TOOLS/DEC-11-L2PC-PO.ptap 
sim> GO 037744

HALT instruction, PC: 037500 (MOV PC,SP)
sim> D SR 37500 
sim> ATTACH PTR TOOLS/dec-uplsa-a-pl.bin 
PTR: unit is read only
sim> GO 037500

PAL-11S  V003A

*S 
```
Now pressing Ctrl-E allows for attaching the correct input and output virtual paper tapes.
```
Simulation stopped, PC: 035110 (CMPB R1,#14)
sim> attach ptr SPCWAR.PAL
sim> attach ptp SPCWAR.OBJ
PTP: creating new file
sim> attach lpt SPCWAR.LST
LPT: creating new file
sim> c
```
Now we are back in the PAL11-S assmebler. But it has alread greated you above with an S(ource) so it won't do that again. Simply type H for input via High-Speed reader. The same for the O(utput). Send L(ist) to the P(rinter) and the T(able) to the T(eletype) output.
```
H

*B H

*L P

*T T

 ADBR   = 176772 G     ADCS   = 176770 G     AMMO     000216RG  
 ANGH1    000154RG     ANGH2    000202RG     ANGL1    000152RG  
 ANGL2    000200RG     ANGMID = 000702 G     COMPAR = ****** G  
 DAC0   = 176750 G     DAC1   = 176752 G     DAC2   = 176754 G  
 FIRE   = 001440 G     FLAG1    000132RG     FLAG2    000160RG  
 GAME1    000222RG     GAME2    000224RG     GRAVTY = ****** G  
 GRVFLG   000226RG     HIT1     000204RG     HIT2     000206RG  
 MASK   = 002100 G     PARM   = ****** G     PC     =%000007    
 POINT  = ****** G     PTNUM  = ****** G     PTREP    000230RG  
 RESET  = ****** G     RKTSIZ = 007000 G     RKT1   = ****** G  
 RKT2   = ****** G     R0     =%000000       R1     =%000001    
 R2     =%000002       R3     =%000003       R4     =%000004    
 R5     =%000005       R6     =%000006       R7     =%000007    
 SHOTS1   000210RG     SHOTS2   000212RG     SP     =%000006    
 SPCRST   000020RG     SPCWAR   000000RG     SPC01    000066R   
 SUNSZ  = 004000 G     TSPEED   000220RG     UPDAT1 = ****** G  
 UPDAT2 = ****** G     WIN      000214RG     XDISP1   000150RG  
 XDISP2   000176RG     XSIZE1   000142RG     XSIZE2   000170RG  
 XVELH1   000146RG     XVELH2   000174RG     XVELL1   000144RG  
 XVELL2   000172RG     YDISP1   000140RG     YDISP2   000166RG  
 YSIZE1   000130RG     YSIZE2   000156RG     YVELH1   000136RG  
 YVELH2   000000RG     YVELL1   000134RG     YVELL2   000162RG  
 .      = 000232R     

 END ?
```

So now the assembler want to run the second pass which means that we have to load our virtual source tape again. Press Ctrl-E now to get into SimH.
```
Simulation stopped, PC: 035464 (BR 35334)
sim> attach ptr SPCWAR.PAL
sim> c
```
And then press return when back into the PAL11-S environment.

```


000000 ERRORS


PAL-11S  V003A

*S 
```
Do this for each and every file to produce object files.

## Linking

Linking involves running the LINK11-S which is similar to how to run the assembler.
```
sim> SET CPU 11/05
Disabling XQ
sim> SET CPU 16K
sim>  
sim> ; Disable devices that we don't need
sim> SET HK DISABLE
sim> SET DZ DISABLE
sim> SET RL DISABLE
sim> SET RX DISABLE
sim> SET RP DISABLE
sim> SET RQ DISABLE
sim> SET TM DISABLE
sim> SET TQ DISABLE
sim> SET RK DISABLE
sim> 
sim> ; Enable the high-speed paper tape reader/punch
sim> SET PTR ENABLE
sim> SET PTP ENABLE
sim> DEPOSIT 037744 016701
sim> DEPOSIT 037746 000026
sim> DEPOSIT 037750 012702
sim> DEPOSIT 037752 000352
sim> DEPOSIT 037754 005211
sim> DEPOSIT 037756 105711
sim> DEPOSIT 037760 100376
sim> DEPOSIT 037762 116162
sim> DEPOSIT 037764 000002
sim> DEPOSIT 037766 037400
sim> DEPOSIT 037770 005267
sim> DEPOSIT 037772 177756
sim> DEPOSIT 037774 000765
sim> DEPOSIT 037776 177550
sim> ATTACH PTR TOOLS/DEC-11-L2PC-PO.ptap 
sim> GO 037744

HALT instruction, PC: 037500 (MOV PC,SP)
sim> ATTACH PTR TOOLS/dec-11-ulksa-a-pl.bin 
PTR: unit is read only
sim> D SR 0
sim> GO 037500

LINK-11S V002A
*I H

*O H

*M P

*T 

*B 

PASS 1

* 
Simulation stopped, PC: 035140 (MOV R5,14(SP))
sim> ATTACH PTP SPACEWAR.ABS
sim> ATTACH LPT SPACEWAR.MAP
sim> ATTACH PTR OBJ/CHAR.OBJ 
```
To the linker you specify the I(input) and O(utput) deivce. H(igh-speed) reader or L(ow-speed). (load) M(ap) is either not generated at all if pressing RETURN or by pressing H(igh speed punch) or T(teletype), or L(ine Printer). The there are two questions (T and B) how the relocation should be done. Press RETURN for top of memory. 

In this case I have chosen to have the resulting binary to be generated on the paper tape punch and the module map on the line printer.

After this each file is fed into the linker by attaching it to the reader and pressing RETURN at the * prompt. When all object files has been presented to the first pass of the linker you can press U a the * prompt to show the remaining undefined symbols. When all is done press E and the linker will print the load map and then start the seconds pass, requesting the same object files in the same order.


```
* U

* E

PASS 2

* 
Simulation stopped, PC: 035122 (ADD #34662,R2)
sim> ATTACH PTR FPMP-11/FPMP-11-SPACEWAR.OBJ
sim> c
```

When you supplied the last file you should be greated with another linker startup message.

```
LINK-11S V002A
*I 
```
Then you know that all is good and there should be a binary file and map file generated.

The first time I tried to run the linker was not succesful. After feeding all source modules unfortunatley I got two undefined symbols.
```
sim> c


* U
$ICI  
$ICO  

* 
```

These two subroutines are from the FPMP-11 package. Just adding one of the two pre-made FPMP-11 object files does not resolve the two undefined symbols.
[FPMP-11 documentation.](http://bitsavers.informatik.uni-stuttgart.de/www.computer.museum.uq.edu.au/pdf/DEC-11-NFPMA-C-D%20FPMP-11%20User's%20Manual.pdf)

Reading the manual a bit more revealed that the reason is that the two premade object files just contain a subset of the FPMP-11 package, not the $ICI and $ICO modules! To get those into the build one has to add a special file with defines for two variables used by the conditional build of the FPMP-11 packaged. By feeding this file
```
;******************************************************
;
;               CONDITIONAL SWITCHES 
;               FOR FPMP-11 PACKAGE
;               WHEN USED WITH SPACE WAR
;
       CND$24=1
       CND$25=1
       .EOT
```
before the six FPMP-11 source files I had a tailor made object paper tape with just the $ICI and $ICO symbols defined! 

Please note that the SPCWAR module has to be the first module and the POINT module the last module. See comments further below. All the other modules comes inbetween including the FPMP-11 library built previously.
 
~~This new object tape was then fed first into the linker, followed by all the 19 object paper tapes of SPACE WAR ending with the module POINT which has to be the last module.~~ And indeed the link completeted without any unresolved symbols this time.
This was the resulting MODULE MAP from LINK11-S:
```
* U

* E


LOAD MAP

TRANSFER ADDRESS: 000001
LOW LIMIT: 025424
HIGH LIMIT: 037460
**********
MODULE  SPCWAR
SECTION ENTRY   ADDRESS SIZE
<. ABS.>        000000  000000
<      >        025424  000232
        ADBR    170402
        ADCS    170400
        AMMO    025642
        ANGH1   025600
        ANGH2   025626
        ANGL1   025576
        ANGL2   025624
        ANGMID  000702
        DAC0    170412
        DAC1    170414
        DPCS    170410
        ERASE   010000
        FIRE    001440
        FLAG1   025556
        FLAG2   025604
        GAME1   025646
        GAME2   025650
        GRVFLG  025652
        HIT1    025630
        HIT2    025632
        MASK    002100
        PTREP   025654
        RKTSIZ  007000
        SHOTS1  025634
        SHOTS2  025636
        SPCRST  025444
        SPCWAR  025424
        SUNSZ   004000
        TSPEED  025644
        WIN     025640
        XDISP1  025574
        XDISP2  025622
        XSIZE1  025566
        XSIZE2  025614
        XVELH1  025572
        XVELH2  025620
        XVELL1  025570
        XVELL2  025616
        YDISP1  025564
        YDISP2  025612
        YSIZE1  025554
        YSIZE2  025602
        YVELH1  025562
        YVELH2  025610
        YVELL1  025560
        YVELL2  025606
**********
MODULE  CHAR  
SECTION ENTRY   ADDRESS SIZE
<      >        025656  000302
        CHAR    025656
**********
MODULE  CHRTAB
SECTION ENTRY   ADDRESS SIZE
<      >        026160  000606
        CHRTAB  026160
**********
MODULE  COMPAR
SECTION ENTRY   ADDRESS SIZE
<      >        026766  000444
        COMPAR  026766
**********
MODULE  EXPLOD
SECTION ENTRY   ADDRESS SIZE
<      >        027432  000330
        EXPLOD  027432
        EXPREP  027550
        EXPX    027556
        EXPY    027560
**********
MODULE  GRAVTY
SECTION ENTRY   ADDRESS SIZE
<      >        027762  000450
        GRAVTY  027762
**********
MODULE  MULPLY
SECTION ENTRY   ADDRESS SIZE
<      >        030432  000304
        MULPLY  030432
**********
MODULE  PARM  
SECTION ENTRY   ADDRESS SIZE
<      >        030736  001214
        PARM    030736
**********
MODULE  PWRUP 
SECTION ENTRY   ADDRESS SIZE
<      >        032152  000106
**********
MODULE  RESET 
SECTION ENTRY   ADDRESS SIZE
<      >        032260  000206
        CENTER  040000
        ORBIT   000400
        RESET   032260
        RES01   032446
**********
MODULE  RKT1  
SECTION ENTRY   ADDRESS SIZE
<      >        032466  000160
        RKT1    032466
**********
MODULE  RKT2  
SECTION ENTRY   ADDRESS SIZE
<      >        032646  000142
        RKT2    032646
**********
MODULE  SCORE 
SECTION ENTRY   ADDRESS SIZE
<      >        033010  000266
        SCORE   033010
**********
MODULE  SINCOS
SECTION ENTRY   ADDRESS SIZE
<      >        033276  000462
        SINCOS  033276
**********
MODULE  SLINE 
SECTION ENTRY   ADDRESS SIZE
<      >        033760  000140
        SLINE   033760
**********
MODULE  SUN   
SECTION ENTRY   ADDRESS SIZE
<      >        034120  000304
        SUN     034120
**********
MODULE  UPDAT1
SECTION ENTRY   ADDRESS SIZE
<      >        034424  000372
        UPDAT1  034424
**********
MODULE  UPDAT2
SECTION ENTRY   ADDRESS SIZE
<      >        035016  000372
        UPDAT2  035016
**********
MODULE  FPMP11
SECTION ENTRY   ADDRESS SIZE
<      >        035410  000640
        $ERR    036212
        $ERRA   036222
        $ERVEC  036242
        $ICI    035416
        $ICO    035722
        $OCI    035410
        $OCO    035714
        $POLSH  036206
        $V20A   036206
**********
MODULE  POINT 
SECTION ENTRY   ADDRESS SIZE
<      >        036250  001210
        POINT   036250
        PTNUM   036456
PASS 2

* 
```
And it also resulted a 7323 byte file in Absolute Binary format! From the load map one can deduce the starting address of the binary. 034604 in this case.

## Running SPACE WAR

The SPACE WAR program make use of the IOX-11 package for teletype IO, which means that this has to be loaded before the SPACE WAR program. Since the IOX package make use of the IOT instruction for all operations there is no linking needed. But the address of the IOX package has to not collide with the SPACE WAR program itself address space wise. It appears that the IOX-11 pacakge is fixed at 015100. There are no instruction on how to assemble the package and a try in PAL-11S results in 245 errors. The IOX-11 package is an absolute binary and is loaded by the abolute loader as most other programs.



```
sim> SET CPU 11/05
Disabling XQ
sim> SET CPU 16K
sim> 
sim> ; Disable devices that we don't need
sim> SET HK DISABLE
sim> SET DZ DISABLE
sim> SET RL DISABLE
sim> SET RX DISABLE
sim> SET RP DISABLE
sim> SET RQ DISABLE
sim> SET TM DISABLE
sim> SET TQ DISABLE
sim> SET RK DISABLE
sim> 
sim> ; Enable the high-speed paper tape reader/punch
sim> SET PTR ENABLE
sim> SET PTP ENABLE
sim> DEPOSIT 037744 016701
sim> DEPOSIT 037746 000026
sim> DEPOSIT 037750 012702
sim> DEPOSIT 037752 000352
sim> DEPOSIT 037754 005211
sim> DEPOSIT 037756 105711
sim> DEPOSIT 037760 100376
sim> DEPOSIT 037762 116162
sim> DEPOSIT 037764 000002
sim> DEPOSIT 037766 037400
sim> DEPOSIT 037770 005267
sim> DEPOSIT 037772 177756
sim> DEPOSIT 037774 000765
sim> DEPOSIT 037776 177550
sim> ATTACH PTR TOOLS/DEC-11-L2PC-PO.ptap 
sim> GO 037744

HALT instruction, PC: 037500 (MOV PC,SP)
sim> d sr 0
sim> ATTACH PTR IOX-11/dec-11-xioxa-a-pb.bin 
sim> GO 037500

HALT instruction, PC: 037712 (BR 37514)
sim> ATTACH PTR SPACEWAR.ABS 
sim> GO 037500

HALT instruction, PC: 037712 (BR 37514)
sim> GO 034604

SPACE WAR
ANY CHANGES? (YES-NO)
Y
WANT A SUN WITH GRAVITY? (YES-NO)
Y
WANT REAPPEARING PHOTON TORPEDSOES? (YES-NO)
Y
INPUT # OF TORPEDOS RKTS WILL HAVE. STD=64
64
INPUT # OF HITS TO DESTROY A RKT  STD=32
32
INPUT SPEED OF TORPEDOES. STD=16000
16000

HALT instruction, PC: 000042 (BIC R4,SP)
sim>
```
So why is it halting? There is a Non existing memory (NXM) handler installed by the IOX-11 package.
```
sim> e 0-200
0:	000000
2:	000000
4:	015604
6:	000340
10:	000000
12:	000000
14:	000000
16:	000000
20:	015256
22:	000340
24:	032662
26:	000340
30:	000000
32:	000000
34:	000000
36:	000000
40:	000000
42:	040406
44:	000000
46:	000000
50:	000000
52:	000000
54:	000000
56:	000000
60:	016040
62:	000200
64:	016672
66:	000200
70:	016132
72:	000200
74:	017056
76:	000200
```
Location 4 is the NXM trap handler and the handler is located at address 15604. Then there are handlers for the paper taper reader, the tty keybaord and printer etc. 

A breakpoint in SimH at that address show that the SP is pointing to 
```
sim> break 15604
sim> go 034604

SPACE WAR
ANY CHANGES? (YES-NO)
Y
.
.
.
Breakpoint, PC: 015604 (JSR R0,15220)
sim> e sp
SP:	034600
```
Now checking the stack at this SP value:
```
sim> e 34600
34600:	034632
```
So the Stack contain the PC of the next instruction after the one causing the NXM trap. 
```
e -m 34624-34700
34624:	MOV #401,176770
34632:	MOV 35020,35010
34640:	MOV 35020,35012
34646:	MOV 35022,35014
34654:	MOV 35022,35016
34662:	CLR 36456
34666:	JSR PC,32770
34672:	MOV #34734,R2
34676:	JSR PC,35342
```
So the offending instruction is the one at 34624 which is trying to reference the ADCS register at address 176770. And since this device is not implemented in SimH this address will not respond and thus the NXM trap!

The code at 15604 will subsequently make a JMP to address 40 where the HALT instruction is executed. Thus stopping the processor.
```
15600:	JMP 40
15604:	JSR R0,15220
15610:	CLR R0
15612:	MOV 14(SP),R1
15616:	BR 15600
```

Then when I had installed the correct hardware and patched the source to use AR11 it still failed. Then I discovered that the stackpointer is initialized with the SPCWAR symbol. Meaning that it grows downwards from that point. Not having the SPCWAR module as the first module when then be a problem since the stack will overwrite the code. I.e. The SPCWAR module has to be the first module and the POINT module has to be that last module. Otherwise the order is of no importance.


## Adaptations to the AR11

Since I don't have any [AD01](http://manx-docs.org/collections/hcps/AD01_manual.pdf) neither any [AA11](http://manx-docs.org/collections/hcps/AA11-D_manual.pdf) subsystem for my PDP-11 and the objective is to have the program running on as like hardware as possible I have to modify it to the hardware I do have, which is the AR11. One obvious benefit of the AR11 is that it is a single HEX module compared with the AD01 and AA11 which are bulky rack-mount system units as big as the computer it self (PDP-11/10).

One disadvantage is that the [AR11](http://bitsavers.trailing-edge.com/pdf/dec/unibus/AR11_UsersMan.pdf) is 10 bit only, meaning that som small changes has to be done to handle 12 vs 10 bits.

One obvious difference is that the AR11 make use of completely different IO address. The definitions of the DAC0, DAC1, DAC2, ADCS and ADBR has to change. The use of DAC2 is a little bit peculiar. The DAC2 is used for the Z-axis of the X-Y-scope and the only bit that is used is the MSB which is changed from off to on and the off to signify a dot. The AR11 doesn't have a third DAC at all. Instead it have one single bit called Intensify in the control status register. It makes sense to use this bit instead. But at least the character drawing routine in CHAR.PAL will turn on Z-axis and then change the X and Y coordinates when drawing a character. When done with each character it will switch off the Z-axis. This means that the intensify pulse cannot be used. Instead one of the logical signals will be used to control the Z-axis. 

Another issue is that the oscilloscope screens requires certain time to deflect the beam. The AR11 include a functionality that when setting the intensify bit it will delay the acutual intensify operation 20 us and then issue a 2 us pulse. The AR11 will then signal back a DONE flag to indicate that next dot can be painted on screen. The SPACEWAR code does not wait for any DONE flag but draws as fast as the CPU can. This may be ok for a PDP-11/10 but might be to fast for a 11/45? And it may be too fast for certain oscilloscope hardware but OK for others.

I ended up using the ERASE (12) bit in the display status register to control the Z-axis. It is a open collector signal so there need to be a pull up resistor in the end of the line at the Z-input of the scope. I also discovered that the AA11 subsystem is 2's complment while the AR11 is not. So besides shifting the output value two steps to the right to make it fit into a then bit number I also had to add 512 to the number before sending it off to the D/A converter.

The AD01 is 10 bit so there were very few changes appart from changing addresses, except for one thing. The AR11 has to be but into unipolar mode since the potentiometers of the joysticks are fed from the internal 5V source of the board.

All adaptations for the AR11 went into a branch called ar11-patch. Make sure to get that one.

## Joysticks


![Joysticks](https://i.imgur.com/8dp26sCl.jpg)

This is basically almost only parts ordered from Aliexpress.
 * [2-axis potentiometer Joystick switch 201 Series. Model M1 with B cap.](https://www.aliexpress.com/item/1785099158.html?spm=a2g0s.9042311.0.0.27424c4dmlXlFu)
 * [New Arcade Push Button Durable Multicade MAME Jamma Game Long Switch - Red](https://www.aliexpress.com/item/32716886319.html?spm=a2g0s.9042311.0.0.27424c4dmlXlFu)
 * [Waterproof Plastic Enclosure Box Electronic Project Instrument Case Electrical Project Box Outdoor Junction Box Housing. Product properties: 115 x 90 x 55mm + White](https://www.aliexpress.com/item/32922219606.html?spm=a2g0s.9042311.0.0.27424c4dmlXlFu) 
 * Cable bending protection.

Each cable going from the connector has +5V, GND and Angle and Acceleration. I used a cable with three pairs and individual sheilding for each pair. The pairs are:
* +5V and GND
* Accleration and GND
* Angle and GND.

The fire button is connected to short circuit the acceleration potentiometer to the 5V supply. 

## Cable connections

I used 47217-000LF/48254-000LF contact elements and 65043-015LF contact housings. The cable is a three pair shieled cable for the Joysticks and the X, Y and Z inputs. An extra cable of simpler type was used for the intensify signal in case I ever wanted to use it.

|  Pin |  Signal   |  Connect to  |
|------|-----------|--------------|
|  A   | WRITH THRU L |  NC       |
|  B   | DISP CH02 L  |  NC       |
|  C   | ERASE L      |  Pull up to 5V in the scope end + Z input of scope |
|  D   | INTESIFY     |  Optional Z input if the standard point plotting mechanism of the AR11 is to be used for other purposes. |
|  E   | NON-STORE L  |  NC     |
|  F   | X OUT        |  Scope X input |
|  H   | ERASE RET L  |  NC |
|  J   | Y OUT        |  Scope Y input |
|  K   | Analog ground | ground for X and Y twisted pair. Including shield |
|  L   |  A/D CH 17 input | NC |
|  M   | Analog ground | NC |
|  N   |  A/D CH 16 input | NC |
|  P   | Logic ground  |  Z - axis pair ground and sheild |
|  R   |  A/D CH 15 input | NC |
|  S   | Logic ground  |  Intensify Z- axis pair return and shield (optional) |
|  T   | A/D CH 14 input | NC |
|  U   | EXT AD ST L |   NC |
|  V   | A/D CH 13 input | NC |
|  W  | Analog ground | NC |
|  X   | A/D CH 12 input | NC |
|  Y  | Analog ground | NC |
|  Z   | A/D CH 11 input | NC |
|  AA  | Analog ground | NC |
|  BB   | A/D CH 10 input | NC |
|  CC  | Analog ground | NC |
|  DD   | A/D CH 7 input | NC |
|  EE  | Analog ground | Joystick 2 Acceleration pot ground and shield |
|  FF   | A/D CH 6 input | NC |
|  HH  | Analog ground | Joystick 2 Angle pot ground and shield |
|  JJ   | A/D CH 5 input | NC |
|  KK  | Analog ground | Joystick 1 Acceleration pot ground and shield |
|  LL   | A/D CH 4 input | NC |
|  MM  | Analog ground | Joystick 1 Angle pot ground and shield |
|  NN   | A/D CH 3 input | Joystick 2 Acceleration pot viper |
|  PP  | -14V HQ | NC |
|  RR   | A/D CH 2 input | Joystick 2 Angle pot viper |
|  SS  | +14V HQ| NC |
|  TT   | A/D CH 1 input | Joystick 1 Acceleration pot viper |
|  UU  | +5V output | Power to feed joysticks and pull-up resistor for Z-axis |
|  VV   | A/D CH 0 input | Joystick 1 Angle pot viper |



