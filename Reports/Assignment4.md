# Assignment 4

## Easy Crackmes

### `easy_crackme_1`

Here are the steps I followed to get the password for `easy_crackme_1`:

1. I opened the given binary file in Ghidra and analyzed it. 
2. I navigated to the function labeled `main` and started looking through it.
3. I noticed a function originalled called `getinput` that took in a string and a character pointer (essentially a buffer). I navigated to this function.
4. I quickly saw that the first parameter was junk, and the function took in input from the user with the `getline` function -- a C library function -- and stored this input into the passed buffer. Interestingly, this function also trimmed off the last character of whatever the user inputted.
5. I went back to the `main` function and saw that the program compares this inputted value to a constant labeled `FIRST_PASSWORD`. If the two strings were the same, the program would complete with a success.
6. I found this constant (by clicking on it in Ghidra) and found it was defined as `picklecucumberl337`

Through this process, I found that the password for this crackme was in fact `picklecucumberl337`, and I verified this by running the crackme on a Linux VM and typing this password, and receiving the text `You cracked me! Now make a keygen!`.

After some investigating, I found that the trimming of the last character, as described in step 4, was for trimming the newline that is gathered when using the `getline` function in C. Therefore, if the user were to feel the password into the program with a pipe or through a file, the password might not work unless there is a newline (or any other character) at the end.

### `easy_crackme_2`

The steps I followed for getting the password for this crackme were the same as for the last crackme. In summary:

1. I opened the binary in Ghidra and analyzed it.
2. I went to the `main` function, gathered again that the same `getinput` function stored the user input into a buffer.
3. This input was again compared to a constant again called `FIRST_PASSWORD`, but this time the constant was defined as `artificaltree`. If the input was the same as this constant, the program completed successfully.

Through this process, I found that the password for this crackme as `artificaltree`, and I verified this by running the crackme on a Linux VM, typing this password, and receiving the text `You cracked me! Now make a keygen!`.

### `easy_crackme_3`

1. I opened the binary in Ghidra and analyzed it.
2. I found the same `getinput` function which takes user input and stores it in a buffer.
3. Another buffer was found, and after some concatenations of `FIRST_PASSWORD1` and `FIRST_PASSWORD2` -- constants defined as `strawberry` and `kiwi` respectively -- this concatenation was stored into the buffer. So now this buffer contains `strawberrykiwi`.
4. There are then a series of comparisons:
    - Compare the input with `"kiwi"` -- this value is stored
    - Compare the input with `"strawberrykiwi"` -- this value is stored, and its result it saved to a boolean-type variable.
    - Compare the input with `"strawberry"` -- this value is overrides the value of the previous compare.
5. There is then a conditional that essentially makes a series of checks where in the `if` expression nothing should be true, such that the `else` would run for a correct outcome and to move on. 
6. The most telling and useful conditional to read this code was the following conditional, which essentially reads `if (input == concatentation)` --> your password is correct (The conditional actually used the boolean-type variabled mentioned in step 4). This conditional is what most confidently told me that the password was the concatenation.

Therefore, I entered the password `strawberrykiwi` into the program on a Linux VM, and received the output `You cracked me! Now make a keygen!`, confirming my password to be correct.




## Control Flow Crackmes
