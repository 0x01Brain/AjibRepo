# AjibRepo
Ajib Arabic Speech Recognizer

This is an arabic speech recognizer, it uses mbrola binary as a synthesizer for speaking and interacting with the user,
the program also uses CMU Sphinx library to recognize users speech.


To Get Started:

Windows Installation
The SAPI5 version of eSpeak uses the mbrola.dll.

    Install eSpeak. Include the voice mb-en1 in the list of voices during the eSpeak installation.

    Install the PC/Windows version of Mbrola (MbrolaTools35.exe) from: http://www.tcts.fpms.ac.be/synthesis/mbrola/bin/pcwin/MbrolaTools35.exe.

    Get the en1 voice from: http://www.tcts.fpms.ac.be/synthesis/mbrola/mbrcopybin.html unpack the archive, and copy the "en1" data file (not the whole "en1" directory) into C:/Program Files/eSpeak/espeak-data/mbrola.

    Use the voice espeak-MB-EN1 from the list of SAPI5 voices. 



Linux Installation
From eSpeak version 1.44 onwards, eSpeak calls the mbrola program directly, rather than passing phoneme data to it using a pipe.

    To install the Linux Mbrola binary, download: http://www.tcts.fpms.ac.be/synthesis/mbrola/bin/pclinux/mbr301h.zip. Unpack the archive, and copy and rename the file from: mbrola-linux-i386 to mbrola somewhere in your executable path (eg. /usr/bin/mbrola ).

    Get the en1 voice from: http://www.tcts.fpms.ac.be/synthesis/mbrola/mbrcopybin.html. Unpack the archive, and copy the "en1" data file (not the whole "en1" directory) to /usr/share/mbrola/en1.

    eSpeak will look for mbrola voices firstly in espeak-data/mbrola and then in /usr/share/mbrola

    If you use the eSpeak voice such as "mb-en1" then eSpeak will use the mbrola "en1" voice, eg:
    espeak -v mb-en1 "Hello world"

    To generate mbrola phoneme data (.pho file) you can use:
    espeak -v mb-en1 -q --pho "Hello world"
    or
    espeak -v mb-en1 -q --pho --phonout=out.pho "Hello world" 

Mbrola Voice Files
eSpeak's voice files for Mbrola voices are in directory espeak-data/voices/mbrola. They contain a line:
  mbrola <voice> <translation>
eg.
  mbrola en1 en1_phtrans

    <voice> is the name of the Mbrola voice.

    <translation> is a translation file to convert between eSpeak phonemes and the equivalent Mbrola phonemes. These are kept in: espeak-data/mbrola_ph 

They are binary files which are compiled, using espeakedit, from source files in phsource/mbrola, see below.
Mbrola Phoneme Translation Data
Mbrola phoneme translation files specify translations from eSpeak phoneme names to mbrola phoneme names. They are referenced from voice files.

The source files are in phsource/mbrola. These are compiled using the espeakedit program (Compile->Compile mbrola phonemes list) to produce data files in espeak-data/mbrola_ph which are used by eSpeak.

Each line in the mbrola phoneme translation file contains:

<control> <espeak ph1> <espeak ph2> <percent> <mbrola ph1> [<mbrola ph2>]

    <control>
        bit 0   skip the next phoneme
        bit 1   match this and Previous phoneme
        bit 2   only at the start of a word
        bit 3   don't match two phonemes across a word boundary 

    <espeak ph1>
    The eSpeak phoneme which is to be translated to an mbrola phoneme.

    <espeak ph2>
    If this field is not NULL, then the match only occurs if this field matches the next phoneme. If control bit 1 is set, then the previous rather than the next phoneme is matched. This field may also have the following values:
    VWL   matches any Vowel phoneme.

    <percent>
    If this field is zero then only one mbrola phoneme is used. If this field is non-zero, then two mbrola phonemes are used, and this value gives the percentage length of the first mbrola phoneme.

    <mbrola ph1>
    The mbrola phoneme to which the eSpeak phoneme is translated. This field may be NULL.

    <mbrola ph2>
    The second mbrola phoneme. This field is only used if the <percent> field is not zero.

The list is searched from start to finish, until a match is found. Therefore, a line with more specific match condition should appear before a line which matches the same eSpeak phoneme but with a more general condition.

The file dictsource/dict_phonemes lists the eSpeak phonemes which are used for each language. Translations for all these should be given in the mbrola phoneme translation file. In addition, some phonemes which are referenced from phoneme files (eg. phsource/ph_language, phsource/phonemes) in lines such as:

   beforenotvowel   l/
   reduceto  a#  0

should also be included, even though they don't appear in dictsource/dict_phonemes.

If the language's *_list or *_rules files includes rules to speak words "as English" the mbrola phoneme translation file should include rules which translate English phonemes into near equivalents, so that they can spoken by the mbrola voice. 


Reference: http://espeak.sourceforge.net/mbrola.html
