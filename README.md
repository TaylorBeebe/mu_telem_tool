# CPER Parsing

## Supplying Data to the Parser
Events parsed here require either just the raw data blobs separated by newlines, or events exported from the Windows Event Logger in a .evtx file. Use the
```-s``` flag to specify the source file. If a non-absolute path is provided, the root directory of this repo (the location of this README file)
will be used as the root of the address.

## Extending known GUIDs
If there are guids you (the user) recognize which are either not currently being replaced by the -f (--Friendly) flag or are being replaced by a friendly 
name you do not prefer, simply add/edit the ```predefined_guids``` dictionary in [cper_parsers.py](edk2-pytool-library\edk2toollib\windows\telem\cper_parsers.py)

## Extending Section Parsing
Without any updates, the sections within each CPER record will simply be hex dumped. If there is a specific section type (GUID) that you (the user) 
recognize, you can funnel that section data to your own parser by extending the ```SectionDataParser``` class, and placing the extension within 
the directory [edk2-pytool-library\edk2toollib\windows\telem\decoders](edk2-pytool-library\edk2toollib\windows\telem\decoders). An 
[example](edk2-pytool-library\edk2toollib\windows\telem\decoders\example_plugin.py) is located within that directory. If the function CanParse() returns true, 
the section data will be passed to the plugin's Parse() function expecting a string in return. 

## Usage
1. Use the command ```parse_cper -h``` to see the relevant commands
2. Try parsing a CPER. Export Microsoft-Windows-Kernel-WHEA and/or Microsoft-Windows-WHEA-Logger events and specify
them using the ```-s``` flag: Ex. ```parse_cper -f -s example.evtx -o output.txt```