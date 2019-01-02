# SPACEWAR

The original Spacewar! game was written for the PDP-1 minicomputer and there were several ports to other architectures.

This port to the PDP-11 architecture were done by Bill Seiler & Larry Bryant in 1974. It was then submitted to DECUS as 11-192

![DECUS entry](https://i.imgur.com/rjaWX4X.png)

The sources were provided to me as scanned documents. With some help the original PDFs were tidied up to help the OCR process. Then I partly OCRed and manually transcribed all files into text files.
The files were then fed into the PAL11-S assembler running on SimH and all errors found were corrected.

## Assembling

It is possible to assemble the files located in the SRC folder using PAL11-S assembler under SimH. PAL11-S is part of the [PDP-11 Paper Tape Software](http://bitsavers.informatik.uni-stuttgart.de/www.computer.museum.uq.edu.au/pdf/DEC-11-XPTSA-B-D%20PDP-11%20Paper%20Tape%20Software%20Handbook.pdf). 

First toggle in the bootstrap loader
```
DEPOSIT 037744 016701
DEPOSIT 037746 000026
DEPOSIT 037750 012702
DEPOSIT 037752 000352
DEPOSIT 037754 005211
DEPOSIT 037756 105711
DEPOSIT 037760 100376
DEPOSIT 037762 116162
DEPOSIT 037764 000002
DEPOSIT 037766 037400
DEPOSIT 037770 005267
DEPOSIT 037772 177756
DEPOSIT 037774 000765
DEPOSIT 037776 177550
;  Attach the Absolute Loader paper tape
ATTACH PTR DEC-11-L2PC-PO.ptap

ECHO ... Execute Bootstrap Loader (to load Absolute Loader from paper tape)
GO 037744
ATTACH PTR dec-uplsa-a-pl.bin
GO 037500

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


## Linking

Linking involves running the LINK11-S similar to how to run the assembler. After feeding all source modules unfortunatley I get two undefined symbols.
```
sim> c


* U
$ICI  
$ICO  

* 
```

These two subroutines are from the FPMP-11 package but unfortunatley right now I don't know how to get them into the linked program. Just giving the FPMP-11 package object file as input to the linker does not seem to solev the problem.
[FPMP-11 documentation.](http://bitsavers.informatik.uni-stuttgart.de/www.computer.museum.uq.edu.au/pdf/DEC-11-NFPMA-C-D%20FPMP-11%20User's%20Manual.pdf)
