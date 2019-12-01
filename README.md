# Python scripts

Assortment of scripts that run on the command line, i.e. *Terminal* on Mac OS X, and *Command Prompt* on Windows.

## Description

### `buildAll.py`
For a given directory, this script goes through every subfolder and runs `makeOTF` for each font source file (`.ufo`, `.pfa`) found. 

`makeOTF` is run with the following options:  
`-r`: release mode  
    * subroutinization turned on  
    * strict error-reporting  
    * GlyphOrderAndAliasDB is applied   

`-gs`: glyphs not listed in the GlyphOrderAndAliasDB are removed from the resulting font file

---

### `checkAll.py`

Run `checkoutlinesufo -e` to remove overlaps in all source files (`.ufo`, `.pfa`) contained in a given directory tree.  

`-e`: This option results in changes to outlines. For a PFA file means that overlapping glyphs will be replaced by new ones (therefore may be lost). In a UFO file, new glyphs are written to a new `glyphs.com.adobe.type.processedGlyphs` layer, which does not interfere with the foreground, and is used by `makeotf` to generate font binaries.

---

### `hintAll.py`

Run `psautohint` to add hints to all glyphs in in all source files (`.ufo`, `.pfa`) contained in a given directory tree. 

In a UFO file, hinted glyphs are written to a new `glyphs.com.adobe.type.processedGlyphs` layer, which does not interfere with the foreground, and is used by `makeotf` to generate font binaries.

---

### `generateAllKernFiles.py`

For each source file in a given directory tree, generate a `kern.fea` file, which can be referenced in a `features` file (as expected by `makeOTF`).  
The `kern.fea` file contains only raw kern data made of groups and lookups, and must be wrapped in feature tags by way of a `include` statement.  
It may look like this:
    
    feature kern {
        include (kern.fea);
    } kern;

It is possible to change settings for generating kern files (such as minimum kerning value, writing of subtables, etc.) directly in the script file.

---

### `generateOneKernFile.py`

Generate a single `kern.fea` file for a given UFO. The resulting file can be used in the same way as explained above. This script’s options can also be changed as explained above.

---

### `generateAllMarkFiles.py`

For each font source file in a given directory tree, generates `mark.fea`, `markclasses.fea`, and `mkmk.fea` files, which can be used in a `features` file (as expected by `makeOTF`).  
The `m*.fea` files contain only raw mark data, and must be wrapped in feature tags by way of an `include` statement. It may look like this:

    include (markclasses.fea);

    feature mark {
        include (mark.fea);
    } mark;

    feature mkmk {
        include (mkmk.fea);
    } mkmk;
    

This script can also create `abvm.fea` and `blwm.fea` files for Indic scripts. More information in the script file itself.

---

### `generateOneMarkFiles.py`

Generate mark files (as explained above) for a single UFO.

---

### `ufo2pfa.py`
Convert all UFO files in a given directory tree to PFA (PostScript Type 1) files, using `tx`.

---

### `ufo2txt.py`
Convert all UFO files in a given directory tree to TXT files, using `tx`.  
TXT files are decrypted PostScript Type 1 (PFA) files.

---

### `pfa2txt.py`
Converts all PFA files in a given directory tree to TXT files, using `tx`.  
TXT files are decrypted PostScript Type 1 (PFA) files.

---

### `UFOInstanceGenerator.py`
(Deprecated workflow, but a great starter for interpolation).  
Generate UFO instances from two or more masters, in a one- or two axis MultipleMaster setup. Requires a settings file called `instances`, the contents of which are better described within the script itself.

---

### `BuildMMFont.py`
Build MM font from bez files.

---

### `checkUFOProcessedLayer.py`
For a UFO font, check that the glyph hashes stored when checkOutlinesUFO or autohint
were last ran still match the source glyph hashes. If not, all outdated glyphs are
deleted from the Adobe processed layer.

---

### `copyCFFCharstrings.py`
Copies the CFF charstrings and subroutines from source to destination fonts.

---

### `kernCheck.py`
Performs several (lengthy) checks on the GPOS table `kern` feature in an OpenType font.

This tool will report collisions between kern pairs, or, with the option `-a`, will report any collisions between any pair of glyphs. It will also report when rules in one GPOS table kern feature mask rules in a different subtable of the same lookup.

NOTE: The time needed is related to the square of the number of glyphs. This script will run in a few minutes for a font with 300 glyphs, but can take more than an hour for a font with 3000 glyphs.

---

### `makeinstances.py`
Generates Type 1 font instances (`font.pfa`) from a multiple master Type 1 font file (`mmfont.pfa`) and an `instances` text file.

---



## Installation

**Mac OS X**: A version of Python is already installed.  
**Windows**: You will need to install one of the 2.x versions available at [python.org](http://www.python.org/getit/).

## Dependencies

Some of the scripts may require additional Python packages or depend on certain tools being installed and appropriately configured to run on the command line.
Here is a list of the locations from which you may need to get the extra tools and packages:

* [Adobe Font Development Kit for OpenType (AFDKO)](https://github.com/adobe-type-tools/afdko)
    * makeotf
    * psautohint
    * tx
    * ttx
    * detype1
    * spot

* [FontTools](https://github.com/fonttools/fonttools)

* ~[RoboFab](https://github.com/robofab-developers/robofab)~
* ~[FontParts](https://github.com/robofab-developers/robofab)~

* [Type Supply Tools](https://github.com/typesupply)
    * defcon
    * ufo2fdk
    * woffTools

## General usage information

1. Download the [ZIP package](https://github.com/adobe-type-tools/python-scripts/archive/master.zip) and unzip it.
2. 
 * Most of the scripts will run by simply typing `python ` followed by the file name of the script, e.g. `python theScript.py`.
 * If the script is in a different directory from which you are trying to run it, you will need to provide the full path to the script’s file, e.g. `python /Users/myself/foldername/theScript.py`.
 * Some scripts may allow you to use options, or require that you provide input files. To learn how to use those scripts, open them in a text editor app (e.g. TextEdit, Notepad) and read the documentation near the top of the file.
