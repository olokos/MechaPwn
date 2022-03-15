# MechaPwn

> For questions @[balika on twitter.com](https://twitter.com/balika011).

Any issue opened that is not an actual issue, but a question will be closed.

> :warning: Disclaimer: DO NOT USE A ON A REAL DTL/DEX; This sets the QA flag that forces the use of the retail keystore. This would break memory card compatibility. :warning:

Real DEX (non-QA) flags have not been added to the public version of MechaPwn for your safety.

The authors hold no responsibility should you break/damage your PlayStation 2 console using this software

This tool can be used to change the region and configuration flags from Dragon based mechacon consoles

This means PlayStation 2 consoles from the SCPH-5000X systems to the SCPH-90000X are supported
(except the DESR (PSX) consoles that are not supported at this time, a future update is planned to address this)

Older PlayStation 2 units do NOT use a Dragon-based mechacon and therefore are not supported. No support is planned for those in the future.

## How to use?

0) Make sure if you are using FMCB **you have v1.966 "multi install" installed**.

1) Run MechaPwn once to install the exploit patch/payload. An initial backup of your mechacon EEPROM will be made to the USB mass storage device (keep it safe!), then power off the console by disconnecting it from the mains when asked (mechacon is on even when the ps2 CPUs is off, so you MUST disconnect the power cord!)
2) Run MechaPwn again to choose which region you want.
3) Choose whether you want to use force unlock or restore the original patch set (and uninstall the exploit patch) from your mechacon backup.
_You will need to reinstall the exploit patch to change your region again_

### Explanations of the menu options

CEX (Retail) will just set the region flag and machine ID of your choosing (it is not advised to mix and match different machine ID types, for example, setting an SCPH-75001 ID to an SCPH-50004 console.

Retail-DEX (Debug) will let you set a QA Flagged DEX configuration/region with a DEX machine ID of your choosing. This allows mechacon to read discs from all regions as well as masterdiscs (the retail option does not).

How does it work?

The Dragon-based MechaCon (SCPH-500xx and newer) store configuration flags and patches encrypted in their EEPROM, the patch DES key was eventually bruteforced that allows code execution on those units and for the full keystore to be dumped.

Normally the patch area is write-protected and cannot be written to at runtime except while using PMAP in TEST mode (this requires soldering). Furthermore, the configuration area can only be written to when it is empty.

This is done to prevent an attacker or anyone outside of Sony's factory from overwriting the mechacon configuration.

However, an exploitable bug was found in the WriteConfig function that allows writing arbitrary data to the patch area. This allows writing a mechacon patch that disables the write protection on mechacon configuration bits and thus sets specific regions and flags to mechacon.

This allows the following:

On SCPH-500xx and SCPH-700xx:

- Disable disc region checks (ps1 and ps2 discs from all-region as well as masterdiscs mount with data accessible)

- Change the region the console reports as, as well as change the disc/KELF region that mechacon allows

- BOOT original PS2 discs from NTSC-J and NTSC-U regions directly from the OSD (NTSC-J units only)

- BOOT original PS2 discs all regions directly from the OSD (NTSC-U and ASIA (non NTSC-J) units only)

- BOOT original PS2 discs all regions as well as PS2 Masterdiscs from all-region by skipping the logo check (for example, by loading a disc using [wlaunchELF](https://github.com/ps2homebrew/wLaunchELF))

- NTSC-J and PAL consoles: BOOT PS1 disc originals from the console's original region

- NTSC-U and ASIA (non NTSC-J) consoles: BOOT PS1 disc originals from all regions

On SCPH-7500X and later models (also known as Deckard consoles)

- Disable disc region checks (ps1 and ps2 discs from all-region as well as masterdiscs mount with data accessible)

- Change the IOP ROM region (the ROM sets a specific bank according to the mechacon region flags)

- Change the region the console reports as, as well as change the disc/KELF region that mechacon allows

- BOOT original PS2 discs all regions directly from the OSD

- BOOT original PS2 discs all regions as well as PS2 Masterdiscs from all-region by skipping the logo check (for example, by loading a disc using [wlaunchELF](https://github.com/ps2homebrew/wLaunchELF))

- BOOT PS1 disc originals from all regions

FAQ:

### Why do PAL/NTSC-J consoles do not play NTSC/PAL discs (on SCPH-70000 and earlier)?

The IOP ROM on those consoles has enforced strict logo decryption checks both in the PS1 and PS2 BIOS which the NTSC-U (also used in ASIA consoles) BIOS does not have.

### Why do consoles not run masterdiscs directly from the OSD?

Additional protection exists on the DSP, which is different in retail and debug consoles, the debug one allows mechacon to store the masterdisc xor key in its registers, the retail one does not.
Bypassing the logo check bypasses this protection (this can be done using [wlaunchELF](https://github.com/ps2homebrew/wLaunchELF) or a future cdvdman patch using a ps2 homebrew)

### My PS2 has a modchip. Will it work for me?

We don't know. This depends on what modchip you are using. It has been reported that some revisions of the modbo 4.0 chip do not truly disable when "disabled" causing conflicts/compatibility issues with MechaPwn region changes, whereas we have seen it work with a modbo 5.0 with disabling that does work as intended (with all modchip features still working when enabled on the converted console).

### Why is my PS2 doesn't play DVDs anymore / FreeDVDBoot stopped working?

The DVD Player KELF inside the BootROM (BIOS) is region locked on non-Deckard (pre SCPH-750XX). Since you changed the region of the console it can't run that file.
To fix that you need to install a DVD Player update to your memory card.

### Why did FreeMCBoot stop working?

Version 1.8 is piggybacking a DVD Player KELF and faces the same issue as DVD Player players.

Version 1.9 is supported, but you have to make sure you have the multi-region ("multi install") version installed.

### What is force unlock?

Force-Unlock is a very much needed feature for homebrew applications, like Neo Geo emulation, PS2 Linux for reading CD and DVD-ROM discs.
What it's meant to do is unlock the disk drive for access.

> :warning: Important note: If you enable Force Unlock every CD and DVD will be detected as a PS2 disc, meaning it breaks PS1 games and DVD-Video**
