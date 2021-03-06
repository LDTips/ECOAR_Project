# BARCODE CODE 128 TYPE C DECODER - SŁAWOMIR BATRUCH

main_new.asm - Main program\
codes.asm - list of the code 128 codes, in hexadecimal\
<Name>.bmp - bmp file from which barcode is read. Name differes between tests\

## List of tests
test1 - should output 2018. Checks 1.bmp\
test2 - should output 2018. Checks 2.bmp. The difference between 1.bmp is the width of bars\
test3 - should output 031428. Checks 3.bmp. This tests checks if 0 at the start is properly outputted\

test4 and test5 - should output barcode type a or type b error\
test6 - should put invalid descriptor\
test7 - should put invalid resolution\
test8 - should put invalid resolution\
test9 - should put invalid bits/pixel (depth)\
test10 - should put "not a barcode" (no black pixel)\
test11 - should put "not a barcode" (max black bar width exceeded)\
test12 - should put "invalid stopsign" (the check for the validity is done at the end)\

## General information
Documentation and explanations regarding functions and instructions provided in comments.
Almost every instruction is commented, and the general principle of the program explained.

Binary numbers were just converted to hexadecimal for every 128 code code. Mentioned in codes.asm

Slight issue (already removed - look edit): for 3.bmp, first 0 is not printed out.
Edit: This has been fixed by implementing a new function (check_zero_start) that adds the 0 if the first element
to print is < 10, i.e. 01, 02, 03 etc...

## Brief description of the program structure
1. Load the file
2. Load header
3. Check neccessary metadata (bits/depth, width, height, filetype etc.)
4. Allocate memory for pixel array and load pixel array into this memory
5. Locate first black pixel
6. Determine width of the narrowest bar
7. Read a single bar
8. Store it in a register as a bit: 0 for white, 1 for black. Then shift left to read next bars
9. Repeat from 7 until the length of a single code length has been read. 
10. Seek for a matching code
11. When code found, Perform neccessary calculations for the checksum
12. Store the code number in an array
13. Repeat from step 7 until stop sign is found
14. Read additional 2 bits for the stop sign
15. Determine correctness of the stop sign. if uncorrect, exit
16. Determine correctness of the checksum. if uncorrect, exit
17. Prepare the array for printing out the codes (add NUL at end)
18. Print integers one by one by iterating through the array and syscalling 1
19. Exit
