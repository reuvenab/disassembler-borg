# Borg 2.28

Borg is an old school disassembler by Cronos (cronos@ntlworld.com), the original version can be found [here](http://www.caesum.com/files/borg228.zip), but this is just a copy to preserve this amazing project.

There are some notes at the bottom of this file (just tend to be working notes from which I add and delete stuff and generally take absolutely no notice of it ;)) which summarise the list of things to be done yet, and any current bugs. Basically if it's on this list then it isn't implemented yet!  Suggestions for change are also welcome, in fact I would appreciate just receiving an email with your top ten list of things you would like to see changed in Borg. If you are reporting problems back it would be useful to know as much about any problems as possible :-

1. Which file was loaded up ?
2. What was the problem ?
3. Is it reproducible, if so how ? If not then what was happening at the time ?

I haven't done much testing in some areas (eg Z80 processors) and so there could well be a few errors in some parts. 

A lot of the current work on Borg is on making the code a bit better in terms of structure and readability. I am trying to move variables from global->module->local definitions, renaming of variables and routines, splitting up difficult and hard to follow code into simpler and easier to follow routines, and reconsidering many of the techniques that have been used. Routines have been moved in and out of classes and there is still quite a bit of restructuring to do.

Current Bugs:
-------------
1. Some data can be lost at the end of a seg when two segs overlap (happens in PE files when resources are loaded and segs created for each resource).

To Do:
------
1. Complete NE format. NE needs sorting out - imported functions, exported functions and the whole rest of the NE format. Very basic load at the moment.
2. LE/LX file format. Don't think I have time to look at this for quite a while - anyone want to take it on ? May need a rethink on segment types, and identifying 16/32 bit code. This won't be looked at by me for some time to come (probably).
3. Dos exes. Better dseg detection - practically no detection of dseg at the moment. Load additional code at end of DOS exe, as an option. Possibly extend seg in a DOS exe when code runs over seg bounds. More reloc/offset identifications.
4. Split seg option.
5. Work on xrefs. More xrefs/Check what is being xreffed.
6. Work on save as text and save as asm options. Full xrefs in save as text option. Xrefs -> loc_n in "save as asm" options. Need to look at save as asm and improve it more. 
7. Work on interface. Icons at top of screen. Tooltip help.
8. Data identification. Identification of strings in data. Identification of bytes/words/dwords in data.
9. Work on general navigation and use options. Jump to ADDR. Offsets by current seg/any seg. Change processor type during running. View summary info option. Database integrity check and recovery option. Help files (someone to take this on ?). Search engine additions (I have some plans).
10. Internal changes to program. Definite data and possible data priorities. Arg type changed from offset type - need to delete xref. Rep table, for rep KNI insts.
11. Better name demangler is needed.
12. Comment and Help Support - eg for Windows functions and for DOS calls.
13. Library routine identification.
14. Code flow analysis. Pseudo-code output and partial decompilation.
15. Renaming stack variables.
16. Macro support (perl suggested, or ficl a possibility - or 4th).
17. VB 'disassembly', which after all is only turning tokens into asm type instructions. So this could be added in more or less the same way as a different processor, although further code would be needed I don't think it would be too hard - an interactive VB disassembler ? Java bytecode ?
18. Support for unpackers, either a front end run of an unpacker program or inclusion of code for a built in unpacker. [decompression/decryption through emulation and control on loading file].
19. A resource parser - data analysis of version_info structure, menus, accelerators, message tables (? anything else i missed ?).
20. Built in debugger.
21. Hex editting, and updating the original file. Patching.
22. Identify args of windows routines.
23. OBJ file support. Allow analysis of obj files from within Borg, to generate a library of function names for loading in order to add compilers or to add obj files for a specific project.
24. Support debug sections.
25. search for next code/undefined.
26. jump to address.
27. rework some of the older code. Rewrite main engine for readability, more routines, better naming and clearer function use. separate disio and disasm better.



Latest Additions:
-----------------

Version 2.28
------------
I have actually moved over to MSVC at the moment, since Borland C++ Builder does not seem to want to compile Borg easily without adding its own WinMain, etc. Perhaps I just don't know enough about Builder ? Anyway, MSVC v6.0. This has necessitated moving some declarations around due to a lot of template warnings re deleting objects....
1. schedule comment memory is now managed by the scheduler. Should resolve some memory leakage.
2. most structs for savefiles should now be surrounded by #pragma pack....
3. fixed a bug in the display code that caused occasional crashes.
4. fixed a bug when loading a database file for a file where write permission cannot be obtained that resulted in loading the data from the database instead of the exe.
5. jump to address added to menu, allows user to enter an offset to jump to (within the current segment). I wont be adding seg/offs values in the future as Borg will be focussing on PE/Win32 files from now (and so single seg).
6. added oep to global options data structure - which means that file database has an altered format.
7. added an option to change the oep of the program (PE files only), with an optional patching of the executable file. This will be more useful later when I add the single step debug functions for unpacking programs.
8. Changed bool FAR PASCAL into BOOL CALLBACK for MSVC....... grr.
9. fixed small bug which stopped the secondary thread process when trying to decrypt before setting top/bottom points.


Version 2.27
------------
1. File save as text/asm now use new ofn functions in common.cpp.
2. Extra debug function added to debug.cpp.
3. Changes to file loading, now checks for zero size file.
4. Bugfix in display code re looping at start of segs in display output.
5. Bugfix - export code outside known data area was causing a crash.


Version 2.26
------------
1. Bugfix re string printing on unicode and dos style strings.
2. Bugfix re some sib formats.
3. Bigfix re string resource id number generation.
4. Changed delfunc and compare to virtual functions which seems to have cured compiler problems.


Version 2.25
------------
1. revisited header includes and cleaned up a few unnecessary includes.
2. bugfix in finding the current line.
3. bugfix in asm output for uninitdata. db ??
4. db now has h after hex numbers.
5. rep/repne and lock are now treated as proper prefixes rather than separate instructions.
6. sib indexes now appear in separate []'s. eg [eax][ebx*4].
7. ds has now been changed to db, with hex values for non-printing chars.
8. a lot of changes on output to enable easier reassembly of saved listings. 'Dword ptr' and 'Word ptr' are now used more, for tasm compatibility. all hex values starting with alphas are now preceded with zero. all pointer types should now be recognised by assemblers (byte, word, dword, fword, qword, tbyte are used in place of double-real, etc).
9. fld freg now shows only one argument, and fxch.
10. instructions are now added to the database outside the decodeinst instruction as this was leaving corrupted databases in cases where prefix bytes made the instruction straddle comments.


Version 2.24
------------
1. Just added a makefile...... should remember to include and update it with each version now.
2. More common routines to help with interfacing to common dialogs.
3. Some structs are now internal to modules, for example taskitem, as nowhere else needs to know about the data structure.
4. decrypter now has read_item and write_item functions and data structure has been internalised. Similar changes to relocs. Some changes to data.cpp along the same lines.
5. default location naming, for example loc_00401000 for xreffed locations.
6. comments now appear before names.
7. locs are now printed on all lines.
8. bugfix re 2.23 problems with database files not working.
9. bugfix re 2.23 not working on NT.


Version 2.23
------------
1. deleting xrefs now calls window update to update the number of xrefs.
2. updated the segviewer to use findnext.
3. added between(lwb,upb) to lptr class.
4. changed findseg to always leave the iterator pointing to the next segment.
5. cleaned up search code. bugfix - will now find a string if it exactly contains the last byte of a segment.
6. introduced more macros for decoding convoluted structures.
7. moved dialogs from decrypt.cpp to user_fn.cpp
8. corrected a few minor problems, and removed stop/start of thread from setting block extents.
9. commented out the compare function from the list class as it was causing MSVC problems with compilation. MSVC is still problematic though, and I recommend Borland C++ 5.00.
10. renamed dlg_ldopt.* to dlg_ldop.*, 8 chars is better for my batch files! 
11. added a short tutorial on using Borg, an introduction for newbies to Borg :)
12. dialogs now return true on processing messages except wm_initdlg which returns false.


Version 2.22
------------
1.  Reworking of SYS file support.
2.  All functions now have brief comments in the sourcecode, also did some reformatting of some statements.
3.  Removed a lot of colour message processing from dialogs and windows, all the WM_CTLCOLORSTATIC, etc. I think this was in originally due to an older compiler and the development version of comctl32 which was with it.
4.  Now using macros in some areas where complex typecasts were common (lists and disasm.cpp for example).
5.  More basic functions added to simplify coding in some areas, eg disasm.cpp.
6.  Reworking of nextseg and lastseg functions and calling code.
7.  Formed help.cpp and registry.cpp to take some functions which need less updates.
8.  Added CenterWindow(HWND) to support functions and recoded various Dialogs.
9.  Removed some redundant code from Dialogs.
10.  BOOL,FALSE and TRUE are now bool, false and true.
11.  list.cpp is now a template, and is contained in list.h. Removed a lot of unnecessary casting and most of the macros.
12.  deletion and compare functions for the lists are now part of the list class and overridden as required.
13.  user_dlg.cpp file has been created for user dialogs. This will contain dialogs and code for viewing things, naming things, etc, which helps separate primary thread and secondary/engine thread code out. The viewers are now more disocciated from their classes. Consequences of this are moving exports, imports and names to the gname class and deleting the exports.cpp, exports.h, imports.cpp, imports.h, names.cpp and names.h files.
14.  demangle has been moved from the gname class to general functions.
15.  naming locations is now directly interfaced to the engine and the secondary thread is stopped rather than going through the scheduler.
16.  set range top and bottom now stop the thread for processing.
17.  user_dlg routines now enforce stricter stopping and starting of the secondary thread.
18.  range viewer moved to user_dlg.cpp
19.  xrefs viewer moved to user_dlg.cpp and repeater made a global variable.
20.  added separate parsing routines for different searches and cleaned up the search code.
21.  added database.cpp for routines for database control of loading and saving.
22.  changed fileload to exeload with the intention that exe loading routines will be split up more. added dlg_ldopt which deals with dialogs for file-open for new files and setting of options. the routines here call the appropriate loader.
23.  all load and save routines for databases are now in database.cpp and extracted from classes. This has meant making more class variables public (to be looked at later). database.cpp only exposes loaddb and savedb procedures to the rest of the program.
24.  naming locations, adding and changing comments now go through the scheduler instead of the primary thread.
25.  repeater variable for dialog boxes has been dropped. dialogs must send a request through the scheduler instead. the request when processed by the scheduler simply posts a windows message back to the main window to reshow the dialog.
26.  xref deletion now goes through the scheduler.


Version 2.21
------------
1. Comments added to decrypt.cpp, schedule.cpp, proctab.cpp, range.cpp, disio.cpp, common.cpp, search.cpp, relocs.cpp, mainwind.cpp, disasm.cpp, dasm.cpp.
2. More changes to critical sections and code in the scheduler to increase thread safety and reduce the potential for thread clashes.


Version 2.20
------------
1. If a file cant be opened with write permission it is opened readonly and patching is disabled, with a warning message issued.
2. Imports by name added to NE file parsing - still need to add import by ordinal, which is the majority of imports. To add the imports a segment at 0xffff:0x0000 is created with offset 1 being the first import, and each import being given an address. This segment can be viewed at the end of the disassembly, with its xrefs etc. This is a bit contrived but suffices for the file analysis.
3. PE Files without an entry point are now started at the first section. Checks are now made for valid addresses on naming, and setting the ouput line.
4. Bugfix - was crashing on some resources due to long descriptions in segheaders, now fixed.
5. Segviewer size increased slightly to allow long addresses in some dlls to be seen fully.
6. Full comments added to list.cpp, data.cpp, gname.cpp, debug.cpp, exports.cpp, imports.cpp, stack.cpp, savefile.cpp and names.cpp (well at least function header comments with some dialogs and explanations to why things are the way they are :)).


Version 2.19
------------
1. Been commenting some bits of code for a change. (Mostly brief function overviews, I feel guilty after starting to read Code Complete ;)
2. Bugfix - wasnt always finding the correct line for the currently selected line.
3. Bugfix - was still crashing due to long strings printing beyond the screen buffer.
4. Bugfix - making the top line into a string now keeps the new name on the top line.
5. Compiler options changes to make Borg use less memory, although it still eats memory ferociously, also made savefiles incompatible with previous versions in the process :)
6. Small changes to some instruction sequences on exit after having some problems with exitting from Borg. This appears to happen when disassembling large programs and when memory is paged out. Not sure if this fully resolved yet. It seems that the program is trying to delete objects which are in paged out memory, so a fault is generated but the memory isnt being paged back in, and it just keeps page-faulting within the Borland C++ exitcode and trying to page the memory back which keeps failing. Not sure why this is happening, but I have the debug version of WinME now, and so this could be a problem just on my system :(
7. Fixed a bug regarding PE headers which meant that for some files the header size wasnt calculated properly. Appears to be a rare thing though, as I have only just found one file like it!
8. Added simple decryptor to Borg. It allows a block to be xorred, rotted, mulled, added or subbed with a byte/word or dword. The result can be written to the original file (patch the file), so it could also be used for simple encryption as well. Further - added xadd decryptor to it (xchg and add through loop). Further - added array decryption. This allows two arrays in the file to be xorred/added/etc. The decrypt list is saved in database files to enable reconstruction of the file when it is reloaded. 
9. More changes to the scheduler and exit routines with critical sections and threadpause/threadstopped variables.
10. Removed #pragma warnings for non-Borland compilers. Had a go with msvc and changed some signed variables to unsigned to reduce warnings, as well as adding some further typecasting.


Version 2.18
------------
1. Bugfix - loaded databases vertical scrollbar should now work ok.
2. Dialogs are now disassembled into data and commented.


Version 2.17
------------
1. Bugfix re searching: was finding the first match for a segment twice in some cases.
2. Search again option added to menu. 
3. F3 now brings up search box (first time), or searches again if one search has been done.


Version 2.16
------------
1. Stringtables are now decoded and named by id_no of the string.
2. Strings are now limited in the size which is shown in the output, as long strings were being problematic and causing crashes.
3. Added data analysis types of single, double and long double (10 byte) floating point reals.
4. Added single real override for immediate values, to display immediate (single real) floats.


Version 2.15
------------
1. Blocks can now be selected. First select top, and then bottom (from one of the menus). Shortcut keys are t and b to define the blocks extents.
2. Added undefine block to main menu.
3. Added save block as asm/text to main menu.
4. Also added a dialog to view the bounds which are currently set (Quick hack really).


Version 2.14
------------
1. Added 'Enter Comment' to right click menu, and changed some enabled buttons to grayed at the start (no file loaded).
2. More instructions are now xreffed - mostly 32bit modr/m type references with a pure disp32, as well as straight 32-bit memory stores/fetches.
3. More locations can now be reached by pressing enter - mainly added various 32bit modr/m types to the list.
4. Added more flags to the disassembly, for display purposes. [So savefiles are once again invalid from older versions].
5. Added negation of immediate values. Shortcut is '-' key. Note that signed immediates can't be negated by this (ie where the instruction is already explicitly using a signed immediate).


Version 2.13
------------
1. Added font selection, 5 fonts on the menu, with 3 Courier New fonts and their sizes, the ANSI_FIXED and SYSTEM_FIXED.
2. Fixed display bug when scrolling beyond the end of the last piece of data.
3. Reorganised the main menu.
4. Added a longer undefine option - to next gap/comment/section or  xreffed location.


Version 2.12
------------
1. Had a most interesting email off Eugen Polukhin who has managed to rewrite the display routines in a most interesting and enlightening way. The result of this is that I reconsidered the way in which I was painting the client area and appear to have solved the problems of the flickering display. Looks much nicer now, although I haven't implemented all of Eugens solution, which is most interesting. I will undoubtedly return to Eugens methods at a later point in time, but if you want to see what he did then check out www.sinor.ru/~eugen.
2. Added buttons to the help about box for mailto and http, inspired by Eugen again ;)
3. Changed to ANSI_FIXED_FONT from SYSTEM_FIXED_FONT.
4. Added colour options for background, text and highlight bar which are saved as reg entries. So you can now use whatever colours you want to use, and if you wonder why i have colour in some places and color in other places, then don't worry about it. colour is the english spelling and color the american and programmatical, so yeah i get mixed up at times ;)


Version 2.11
------------
1. Internal changes re new stack class, for return stack and in partial preparation for a later emulator for unpacking code. However in order to do this I had to change the file format of saved files slightly, and this will be incompatible with previous versions.
2. Disasm class has now been split into two classes as it was getting unmanageable. The two classes comprise disio = disasm io functions, and disasm the main disassembly database class. There are some friend functions in the disio class due to the complexity, and it should be noted that these access the iterators of the disasm class. Whilst cleaning up these classes I moved some functions around and made some changes (for example setting a byte override by pressing a key now only goes through the scheduler once rather than twice), and some common code has been put into other private functions.
3. Shortcuts disabled until file loaded.
4. Comments added properly. Can now enter comments during disassembly and they appear in the listings, etc.
5. Added a savefile object and file functions, to allow file compression to be added to the files more easily. The new class has read and write functions where file compression can be added.
6. Added some RLE compression to the savefiles, using nibbles. Results in around 30% reduction to savefile sizes. May as well make all the save file changes at the same time ;)
7. Made a proper vertical scroll bar, which really wasnt that much of a problem after all. It works on addresses rather than number of lines in the disassembly, which seems fine. Made the horizontal scrolling a little better by allowing dragging of the slider. Both only update the position on release and not throughout the motion, which would be more difficult to implement (and avoid sending millions of messages for updates).
8. Added version info and checks to savefiles. Hopefully the savefiles should be ok now, and won't change so much in future versions!
9. Disassembly from the location of an export only takes place if it is in a code segment now since any address can be exported, including a data address (eg for debug hooks, etc).
