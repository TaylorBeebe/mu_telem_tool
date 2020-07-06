## About
This is a temporary repo for staging a work-in-progress which will eventually make it into the 
[Edk2-Pytool-Library](https://github.com/tianocore/edk2-pytool-extensions) and 
[Edk2-Pytool-Extensions](https://github.com/tianocore/edk2-pytool-library) repositories. This is NOT a 
full release for parsing CPER/WHEA records, but provides enough functionality for most situations.

**WARNING: Remaining links in README only work if this repo has been cloned locally and ```git submodule update --init``` run**

Because this is meant to be an addition to the aformentioned repositories, much of what is inside here isn't needed for parsing CPER records. 
For users intending to parse CPER records, the relevant functionality is within the 
[edk2-pytool-library\edk2toollib\windows\telem](https://github.com/TaylorBeebe/edk2-pytool-library/tree/1d74ec420ec3d428dda8ea12355efcaf474467c0/edk2toollib/windows/telem) 
and [edk2-pytool-extensions\edk2toolext\telem](edk2-pytool-extensions\edk2toolext\telem) folders. The edk2-pytool-extensions portion 
provides the command line interface while the edk2-pytool-library portion provides the actual parsing.

## Supplying Data to the Parser
Events parsed here require either just the raw data blobs separated by newlines, or events exported from the Windows Event Logger in a .evtx file. Use the
```-s``` flag to specify the source file. If a non-absolute path is provided, the root directory of this repo (the location of this README file)
will be used as the root of the address.

## Extending known GUIDs
If there are guids you (the user) recognize which are either not currently being replaced by the -f (--Friendly) flag or are being replaced by a friendly 
name you do not prefer, simply add/edit the ```predefined_guids``` dictionary in [cper_parser_tool.py](edk2-pytool-library\edk2toollib\windows\telem\cper_parser_tool.py)

## Extending Section Parsing
Without any updates, the sections within each CPER record will simply be hex dumped. If there is a specific section type (GUID) that you (the user) 
recognize, you can funnel that section data to your own parser by extending the ```SECTION_DATA_PARSER``` class, and placing the extension within 
the directory [edk2-pytool-library\edk2toollib\windows\telem\decoders](edk2-pytool-library\edk2toollib\windows\telem\decoders). An 
[example](edk2-pytool-library\edk2toollib\windows\telem\decoders\example_plugin.py) is located within that directory. If the function CanParse() returns true, 
the section data will be passed to the plugin's Parse() function expecting a string in return. 

## Usage
Please follow these steps for parsing records:
1. Open a command line within the repo root directory 
2. Run the command ```git submodule update --init``` 
3. Type the command ```.\setup.bat``` which will simply install locally the two subrepos in this directory and pip install the evtx requirement. 
4. Use the command ```parse_cper -h``` to see the relevant commands
5. Try parsing a CPER. Export Microsoft-Windows-Kernel-WHEA and/or Microsoft-Windows-WHEA-Logger events and specify
them using the ```-s``` flag: Ex. ```parse_cper -f -s example.evtx -o output.txt```