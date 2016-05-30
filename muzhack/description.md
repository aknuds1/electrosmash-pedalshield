# pedalSHIELD DUE Arduino Guitar Pedal.
**Please note that this is an unofficial project page, the official one is [here](http://www.electrosmash.com/pedalshield).**

pedalSHIELD DUE is a programmable Arduino Open Source & Open Hardware guitar pedal made for guitarists, hackers and programmers. Users can program their own effects in C/C++ or download ready effects from the [online library](http://www.electrosmash.com/forum/software-pedalshield).

It is designed to be a platform to learn about digital signal processing, effects, synthesizers and experiment without deep knowledge in electronics or programming.

[!youtube](COPaqJBekBQ)
![Parts](https://aknuds1.github.io/electrosmash-pedalshield/images/pedalSHIELD-parts-small.jpg)

## How Does it Work?

The shield has three parts:

* The Input Stage or Preamp: Amplifies the guitar input signal and sends it to the Arduino microcontroller to be processed.
* Arduino Board: It does all the Digital Signal Processing (DSP) modifying the signal and adding the effect (delay, echo, distortion, volume...).
* The Output Stage: Once the waveform is processed, the signal is taken from the Arduino DACs and prepared to be sent to the Guitar Amplifier.
This part also includes a Summing Amplifier which is very useful for delay effects like echo or chorus.

![Parts](https://aknuds1.github.io/electrosmash-pedalshield/images/pedalshield-arduino-guitar-pedal-diagram.jpg)

pedalSHIELD uses 2 ADCs and 2 DACS in parallel in order to achieve higher bit resolution (2 x 12bits). However using only 1 DAC and 1 ADC is possible without modifications. You can check all the details in the [hardware design section](http://www.electrosmash.com/pedalshield#hw) and in the [pedalSHIELD Hardware forum](http://www.electrosmash.com/forum/hardware-pedalshield).

![View](https://aknuds1.github.io/electrosmash-pedalshield/images/pedalSHIELD-PCB-360-view-small.jpg)

## How to Program it?

The shield is programmed in C/C++ using the standard free Arduino platform (Linux/Windows/Mac). All tools and programs are open source and compatible with Arduino libraries.

Basic knowledge of C is needed. The best way to show how to program it is showing an example pedal:

1. Clean Volume/Booster Pedal:

  This code creates a Volume/Booster pedal, the architecture of the program looks like

![Example code](https://aknuds1.github.io/electrosmash-pedalshield/images/example-code.jpg)

    // Licensed under a Creative Commons Attribution 3.0 Unported License.
    // Based on rcarduino.blogspot.com previous work.
    // www.electrosmash.com/pedalshield

    int in_ADC0, in_ADC1;  //variables for 2 ADCs values (ADC0, ADC1)
    int POT0, POT1, POT2, out_DAC0, out_DAC1; //variables for 3 pots (ADC8, ADC9, ADC10)
    int LED = 3;
    int FOOTSWITCH = 7;
    int TOGGLE = 2;

    void setup()
    {
      //ADC Configuration
      ADC->ADC_MR |= 0x80;   // DAC in free running mode.
      ADC->ADC_CR=2;         // Starts ADC conversion.
      ADC->ADC_CHER=0x1CC0;  // Enable ADC channels 0 and 1.  

      //DAC Configuration
      analogWrite(DAC0,0);  // Enables DAC0
      analogWrite(DAC1,0);  // Enables DAC0
    }

    void loop()
    {
      //Read the ADCs
      while((ADC->ADC_ISR & 0x1CC0)!=0x1CC0);// wait for ADC 0, 1, 8, 9, 10 conversion complete.
      in_ADC0=ADC->ADC_CDR[7];               // read data from ADC0
      in_ADC1=ADC->ADC_CDR[6];               // read data from ADC1  
      POT0=ADC->ADC_CDR[10];                 // read data from ADC8        
      POT1=ADC->ADC_CDR[11];                 // read data from ADC9   
      POT2=ADC->ADC_CDR[12];                 // read data from ADC10

      //Add volume feature with POT2
      out_DAC0=map(in_ADC0,0,4095,1,POT2);
      out_DAC1=map(in_ADC1,0,4095,1,POT2);

      //Write the DACs
      dacc_set_channel_selection(DACC_INTERFACE, 0);       //select DAC channel 0
      dacc_write_conversion_data(DACC_INTERFACE, out_DAC0);//write on DAC
      dacc_set_channel_selection(DACC_INTERFACE, 1);       //select DAC channel 1
      dacc_write_conversion_data(DACC_INTERFACE, out_DAC1);//write on DAC
    }

You can check all the codes and more examples at the [pedalSHIELD Software Forum](http://www.electrosmash.com/forum/software-pedalshield).

You can [listen in SoundCloud](https://soundcloud.com/electro-smash) to all the already programmed effects (delay, reverb, echo, metronome, tremolo, distortion, etc...).

[!soundcloud](122503208)
[!soundcloud](122504367)
[!soundcloud](122498414)

## pedalSHIELD Hardware Design

### Specifications:

* Based on Arduino Due.
* Configurable sampling time from 8kHz to 192kHz
* More than 2200 instructions per sample at 48kHz
* Micro controller:
  * 84MHz 32bit Atmel SAM3X8E ARM Cortex-M3.
  * 96KB RAM, 512KB Flash Memory.
  * Integrated DMA.
  * 12 bit ADC/DAC integrated sampling up to 1Mbps.
* Interface:
  * 3 Configurable potentiometers.
  * 2 Configurable switches.
  * Blue power-on led PWM controlled.
  * True Bypass Footswitch.
* Connectors:
  * Input Jack, 1/4 inch unbalanced, Zin=10MΩ.
  * Output Jack, 1/4 inch unbalanced, Zout=1KΩ.
  * Power supply: power taken from Arduino Due board.

The design was created using [KiCad](http://www.kicad-pcb.org/), an open-source GNU free of charge electronic design CAD tool. All [schematic native files](http://www.electrosmash.com/forum/hardware-pedalshield/18-kicad-schematics-pedalshield) and [bill of materials](http://www.electrosmash.com/forum/hardware-pedalshield/17-pedalshield-bill-of-materials-and-alternatives) are public. The circuit can be broken down into 5 simpler blocks: Power Supply, Input Stage, Output Stage, User Interface and Arduino Connectors:

![Schematic](https://aknuds1.github.io/electrosmash-pedalshield/images/pedalshield-schematic-small.png)

The functionality is simple; 2 opamps will prepare the signal to be digitized and also 2 opamps will recover the signal from
the micro controller. Two ADCs in parallel can be used to read the guitar signal, improving the bit depth (2x12bits). Furthermore, this arrange is also compatible with the ["Double Span and Digitize Signals using Two ADCs"](http://www.electrosmash.com/forum/hardware-pedalshield/22-double-span-and-digitize-signals-using-two-adcs) by placing the Jumper1.

* **Input Stage / Preamp:** The guitar signal is amplified for better acquisition by the first op-amp which follows the MicroAmp guitar pedal design. The trimmer VR1 adjusts the gain of this amplifier. There is also program which helps to automatically adjust this trimmer. The second inverting op-amp inverts the amplified signal to be applied to the ADC1. The Diodes D1, D2, D3, D4 are clamping diodes that protect the Arduino's ADC from signals above 3,3V and below 0V.
* **The Output State:** Using a Differential amplifier (Gain=1) two DACs can be read in parallel improving the bit resolution (2x12bits). However, if a signal is generated at DAC0 and DAC1 is not used, the Differential amplifier behaves as a normal Buffer. The last op-amp is in a Summing configuration, adding the processed signal and the original one if the Mix Switch is ON. This stage is very convenient to implement some pedals like delay, flanger, chorus, metronome, etc..
* **The Power Supply:** Generates ±5V to feed the operational amplifiers and achieve maximum signal swing without distortion.
* **User Interface:** Is composed by the configurable potentiometers, switches, footswitch and LED.
* **Arduino Connectors:** 5 connectors link the shield with Arduino transferring the signals.

![360 view](https://aknuds1.github.io/electrosmash-pedalshield/images/pedalSHIELD-360-view-small.jpg)

## Order a pedalSHIELD online

There are 3 options in the [store](http://store.electrosmash.com/index.php?route=product/category&path=33):

1. [Order only the PCB](http://store.electrosmash.com/index.php?route=product/product&product_id=50). It uses easy-to-find standard components and you can build the kit yourself. The bill of materials is public with all the references and the Mouser part numbers.
2. [Order the PCB + Transparent Plexiglas Cover](http://store.electrosmash.com/index.php?route=product/product&product_id=51).
3. [Order the Full Kit](http://store.electrosmash.com/index.php?route=product/product&product_id=31): This kit includes the PCB, Cover and all the components to build pedalSHIELD at home.

All the transactions are done though PayPal for maximum security.

If you have any questions, [contact us](http://www.electrosmash.com/contact).

![Intro](https://aknuds1.github.io/electrosmash-pedalshield/images/pedalSHIELD-intro.jpg)

## Frequently Asked Questions (FAQ):

What is Arduino?

* Arduino is a single-board micro controller to make using electronics in multidisciplinary projects more accessible. The hardware and software are open-source.

Where can I buy Arduino Due board?

* All major suppliers like [Mouser](http://es.mouser.com/ProductDetail/Arduino/A000062/?qs=sGAEpiMZZMs5EsmM6MQhfSBmF%252bOwLqsr), [element-14](http://www.element14.com/community/search.jspa?q=arduino+due), [Digikey](http://www.digikey.com/product-search/en?vendor=0&keywords=arduino+due), [RS-Online](http://uk.rs-online.com/web/p/processor-microcontroller-development-kits/7697412/?searchTerm=arduino+due&relevancy-data=636F3D3126696E3D4931384E44656661756C74266C753D656E266D6D3D6D61746368616C6C7061727469616C26706D3D5E5C442B5C735C442B2426706F3D3926736E3D592673743D4B4559574F52445F4D554C54495F414C504841267573743D61726475696E6F206475652673633D592677633D4E4F4E4526) have it in their catalog and also you can find it in eBay and physical electronic shops.

Does pedalSHIELD work with all Arduino boards?

* No, only with Arduino DUE.

Is pedalSHIELD suitable for bass players?

* Yes, but you need to swap 6 capacitors (C1 C4 C6 C7 C10 C11) from 0.1uF to 0.5uF, that is all.

Can I plug headphones directly into pedalSHIELD?

* No, pedalSHIELD is not an amplifier. It needs to be plugged into a guitar amplifier, or to a multieffects unit (Line 6 pods, etc..).
