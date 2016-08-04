# How to Build pedalSHIELD
**Please note that this is an unofficial project page, the official one is [here](http://www.electrosmash.com/pedalshield).**

This is a 5 step guide to build pedalSHIELD. With all the materials at hand it takes around
2 hours to build it successfully.

## Step 1 – Prepare the Materials.

You need a solder iron, lead and cutting pliers. Additionally cutter, scissors and pliers are
convenient. The PCB has solder mask and plated holes, so it is very easy to solder with any
15-30W cheap solder iron.

Keep at hand the PCB plan and the Bill of Materials:

### pedalSHIELD Bill of Materials

|Value |Qty |References|
|------|----|----------|
|Capacitors|||
|270p |5 |C2 C5 C8 C9 C12|
|0.1u |6 |C1 C4 C6 C7 C10 C11|
|1u |1 |C13|
|4.7u |1 |C3|
|10u |2 |C14 C15|
|47uF |1 |C18|
|Resistors|||
|1K |2 |R3 R21|
|4.7K |3 |R5 R9 R10|
|50K |1 |R7|
|100K |12 |R4 R6 R8 R11 R12 R13 R14 R15 R16 R17 R19 R20|
|10M |2 |R1 R2|
|500K |1 |RV1|
|10K |3 |RV2 RV3 RV4|
|Plastic Knobs |3 |RV2, RV3, RV4|
|Others|||
|1N5817 |4 |D1 D2 D3 D4|
|LED |1 |D5|
|SWITCH_3PDT |1 |SW1|
|SWITCH_INV |2 |SW2 SW3|
|Connectors|||
|CONN_8pins |5 |CONN2 CONN3 CONN4 CONN5 CONN6|
|JACKS |2 |JI, J2|
|IC's|||
|TL072 |2 |U1 U2|
|TC1044 |1 |U3|
|8PIN |SOCKETS |3 U1, U2, U3|

## Step 2 – Soldering Resistors and Diodes.

There are 19 resistors and 4 diodes to be placed.

![Soldering resistors and diodes](https://aknuds1.github.io/electrosmash-pedalshield/images/pedalshield-soldering-resistors.png)

* 2.a Solder the 100KΩ resistors (12 units)
* 2.b Solder the 4.7KΩ res. (3 units) and the 10MΩ res. (2 units)
* 2.c Solder the 15817 diodes (4 units), the 1KΩ res. (2 units) and the 50KΩ res. (1 unit)

### Tips

* In step 3.a pay attention to the diodes polarity, there is a line indicating the correct position.
They get hot very fast when soldering, so try to do it carefully and wait between each solder point.
* In order to solder the components, bend the leads, introduce them in the footprint and once
soldered cut the excess of lead as short as possible to avoid short circuits.

## Step 3 – Soldering the Capacitors.

There are 11 film/ceramic and 5 electrolytic caps.

![Soldering capacitors](https://aknuds1.github.io/electrosmash-pedalshield/images/pedalshield-soldering-capacitors.png)

* 3.a Solder the 100nF capacitors (6 units)
* 3.b Solder the 270pF capacitors (5 units)
* 3.c Solder the electrolytic capacitors 4.7uF, 1uF, 10uF (2 units) and 47uF.

### Tips

* Be careful with the electrolytic caps polarity, the negative lead (the short one) has to be
placed in the round hole. The positive hole is always square-shaped and it is marked with a + symbol.
* The Capacitors C16, C17 and the jumper JP1 are optionals and do not need to be mounted.

## Step 4 – Soldering the Big Components.

The last components to be placed are the dip sockets (3), connectors (5), trimmer, potentiometers
(3), switches (2), jacks (2) and the footswitch:

![Soldering the big components](https://aknuds1.github.io/electrosmash-pedalshield/images/pedalshield-soldering-big.png)

* 4.a Solder the 8pin sockets (3 units), the trimmer res. and the 8pins connectors (5 units)
* 4.b Solder the potentiometers (3 units), switches (2 units), jacks (2 units) and footswitch.

When soldering the potentiometers and switches it would be good to check their positioning
against the plastic cover. The cover will fit better if the cover its aligned.

The LED D5 is the last component to be soldered. Place the plastic cover to size the length of
the leads. The short lead (cathode) goes to the flat side of the diode mark.

### Tips
* Be careful soldering the big components perpendicularly because they tend to be slightly tilted.
* The 40 pin stripe has to be cut in 5 segments of 8 pins each. You can use a cutter to carve
a groove every 8 pins and then just bend it carefully to break it. One of the segments needs
to be cut like the side photo to avoid collision with the output jack.

## Step 5 – Checking Out the Job Done.
After these 5 stages you will have a mounted board exactly like the one shown below:

![Finished product](https://aknuds1.github.io/electrosmash-pedalshield/images/pedalshield-finished.png)

Double check your PCB with the model component by component.

Before powering it up, check these 3 ticks:

1. Visual inspection of the PCB bottom, there are no short circuits or long uncut leads.
2. The polarized components are placed correctly: diodes and electrolytic caps.
3. The ICs are not swapped or wrongly placed.

Finally the plastic cover can be installed together with 4 screws + nuts. Optionally a
plastic spacer can be cut to separate the boards and reduce the mechanic stress.

The plastic knobs and the shaft+nut for the footswitch are the last parts to be mounted.
