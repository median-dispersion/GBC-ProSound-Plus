# GBC ProSound+

## 📝 Introduction

This is a hardware modification for the Game Boy Color to get a cleaner audio signal from the internal headphone jack. It is based on [
Capcomposer's Game Boy Color Internal Pro-Sound mod](https://capcomposer.blogspot.com/2010/01/gameboy-color-internal-pro-sound-mod.html) with some small additional tweaks, hence the "plus" in the name. It consists of two parts. The first part is a bypass of the internal noisy audio amplifier to get rid of the low-frequency hum or buzz, and the second part is a boost in capacitance for the voltage regulator to get rid of the high-frequency hiss or screeching.

## ⚠️ Disclaimer

> [!WARNING]
> This modification is not completely risk-free!
> Only do this if you are comfortable with the risk of potentially damaging your device.

### Explanation

When bypassing the internal audio amplifier, you will be essentially connecting the headphone jack directly to one of the CPU pins (with a couple of passive components in between). This will give you the best possible audio signal, but it is not ideal for driving headphones. This is because normally CPUs do not have the capability of outputting a lot of power or current through their pins, so powering headphones from one of those pins might damage the CPU. The volume wheel, which is a variable resistor, does protect against excessive power draw somewhat because it resists the flow of current, but when you put it at maximum volume, the resistance will be essentially zero. This is why this modification puts two 4.7 kΩ resistors in series with the 510 Ω resistor that is already installed on the board. So even in the worst-case scenario when you accidentally short the audio output to ground, it limits the current at 2.5 V RMS to around 0.5 mA. This might still be enough current to damage the CPU, but without an official datasheet rating, this is still better than nothing and hopefully protects the CPU somewhat if something goes wrong.

### Headphone selection

Limiting the current like this will also have the effect that the volume on most headphones will be fairly quiet or even zero. This is why this modification is limited to in-ear headphones only. I've tested it with some inexpensive but very sensitive KZ ZSN Pro X in-ear headphones, and even at the middle position of the volume wheel, it was still plenty loud. For larger over-ear or power-hungry headphones, an external audio amplifier must be used. Using an external amplifier will also protect you against the risk of drawing too much current from the CPU pins because at that point the headphone jack is essentially just a line-out.

### Board revisions and component labels

These instructions were written and performed on a Game Boy Color with the board revision `CGB-CPU-02` which can be located on the back side of the PCB. Your board might be a different revision and therefore has different component labels. For example, a `CGB-CPU-05` also has a group of inductors containing EM2 and EM3 but no EM4, like my board has. This shouldn't be a problem because, as far as I can tell, all the components that need to be modified are located in the same places and identified by the same labels for all board revisions.

## 🔨 Tools and components

For this modification, you will need a soldering iron, some solder, and these components:

| Component                                    | Quantity | Label         |
| -------------------------------------------- | -------- | ------------- |
| Thin gauge or enameled copper wire           | ~ 50 cm  |               |
| 4.7 kΩ resistor                              | 2        | Rmod1 & Rmod2 |
| 500 - 1000 uF low ESR electrolytic capacitor | 1        | Cmod1         |

## 🎛️ Bypassing the amplifier

The first step is to bypass the noisy internal audio amplifier to get rid of the low-frequency buzzing. This is done by connecting some resistors to the volume wheel, which gets a clean audio signal directly from the CPU, and then using some wire to skip all the noisy components. To do this, you will need to modify the original circuitry according to this schematic:

<a href="./Assets/Audio amplifier bypass schematic.svg">
    <img alt="Audio amplifier bypass schematic" src="./Assets/Audio amplifier bypass schematic.svg" width="100%">
</a>

Solder a 4.7 kΩ resistor onto the VR1-LIN and VR1-RIN pins of the volume wheel. VR1-LIN is the second and VR1-RIN is the third pin from the top of the volume wheel. Then solder a thin wire onto each resistor and route them next to the cartridge slot along the right side of the motherboard until you reach a small SMD capacitor labeled C31. Bend the wires 90 degrees and route them along the bottom half of the motherboard towards the speaker. Fold the wires around the PCB by the speaker cutout and solder the two ends to pins 2 and 3 of the headphone jack. Make sure pin 2 is connected to VR1-LIN and pin 3 is connected to VR1-RIN; otherwise, your left and right audio channels will be swapped! Finally, find and remove the two small inductors labeled EM2 and EM3 located in the center of the board above the headphone jack. Removing EM2 and EM3 is very important because this disconnects the original amplifier from the headphone jack, bypassing it completely.

## 🔋 Adding bulk capacitance

The second step is to add bulk capacitance to the original voltage regulator to remove the high-frequency hissing. This is very simple because you will just need to add a capacitor in parallel with C32. The capacitance of the new capacitor is not that important; it should be at least 100 uF and higher is usually better. I would look for a capacitance in the range of 500 - 1000 uF. What's more important is that the capacitor has a low ESR (equivalent series resistance) rating to more effectively filter out the switching noise coming from the voltage regulator. To do this, you will need to modify the original circuitry according to this schematic:

<a href="./Assets/Bulk capacitance schematic.svg">
    <img alt="Bulk capacitance schematic" src="./Assets/Bulk capacitance schematic.svg" width="100%">
</a>

Solder some thin-gauge wire onto the legs of the new capacitor long enough to span from the speaker over to the original capacitor C32. Then connect the wires to the pins of C32, making sure the polarity of the new capacitor matches the original polarity. Stick down the new capacitor with some hot glue onto the speaker in a way where you don't block or melt the speaker. That's it.

## 🩺 Troubleshooting

### No power

The most likely cause of a Game Boy not powering on after completing this modification is a shorted power supply. Make sure the polarity of the newly added capacitor matches the polarity of the original capacitor. Also check that you didn't accidentally bridge any solder pads that shouldn't be bridged. Try to remove the added capacitor and see if it powers up again. If it doesn't, something else has gone wrong.

### No Audio

If you don't get any audio through the headphone, make sure that you have removed the inductors EM2 and EM3. If you have, check that all the wires and resistors make good contact and that nothing is shorting out. If you still don't get any audio, try different headphones, preferably low-power in-ear ones.

### Low volume

If you get low volume through the headphone jack, make sure you have removed the inductors EM2 and EM3. If you have and still get low volume, it just means your headphones are too power-hungry, and you either need to use an external audio amplifier or some lower-power in-ear headphones.