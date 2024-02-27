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
All the following keygens were run with the command:
`python3 keygen-x.py | xargs ./control_flow_x $1`

When describing the required key and key format, the character `-` means that any character can to into that slot in the key.


### Control Flow 1
The key for this control flow requires that the first four characters are `A602`, the 8th character is `%`, and the 16th character is `*`. 

Essentially, the key must take the form of `A602---%-------*`.
```python3
#
# A602---%-------*
#
key = [random.choice(string.ascii_letters) for i in range(16)]
key[0] = 'A'
key[1] = '6'
key[2] = '0'
key[3] = '2'
key[7] = '%'
key[15] = '*'
key = ''.join(key)
print(key)
```

### Control Flow 2
The key for this control flow requires that:
- 7th character to be `Y`
- 9th character to be [(character - 35) & 0x3f] == 0. It happens to be that `#` satifies this.
- 11th character to be `A`
- 14th character to be `6` 

Essentially, the key must take the form of `------Y-#-A*-6--`.
```python3
#
# ------Y-#-A*-6--
#
key = [random.choice(string.ascii_letters) for i in range(16)]
key[6] = 'Y'
key[8] = '#'
key[10] = 'A'
key[11] = '*'
key[13] = '6'
key = ''.join(key)
print(key)
```

### Control Flow 3
The key for this control flow requires that: 
- ((key[1] + key[3]) - key[5]) == key[6]. It happens to be that the following characters satisfies this requirement:
  - 2nd character `A`
  - 4th character `B`
  - 6th character `C`
  - 7th character `@` 
- (key[6] XOR key[7]) < 3. Anything XOR with itself is 0, and 0 < 3, so: 8th character = '@' (the same as 7th character) 
- key[10] == key[12]. So the 11th character and 13th character must be the same. I decided to set it them as `A`
- (key[8] XOR key[7]) >= 4. So, the 9th character XOR with 8th needs to be greater than or equal to 4. The character `a` satisfies this. 
- input[8] != input[9]
- (input[12] XOR input[8] XOR input[9]) != (input[10] < 3). So, ('A' ^ 'a' ^ n) != ( 'A' < 3 ). A value that can fill in for n could be `b`. So the 10th character is `b`. 


(input[12] ^ input[8] ^ input[9]) != (input[10] < 3)
( 'A'  ^ 'a' ^ n) != ( 'A' < 3 )

Essentially, the key must take the form of `-A-B-C@@abA-A---`.
```python3
#
# -A-B-C@@abA-A---
#
key = [random.choice(string.ascii_letters) for i in range(16)]
key[1] = 'A'
key[3] = 'B'
key[5] = 'C'
key[6] = '@'
key[7] = '@'
key[8] = 'a'
key[9] = 'b'
key[10] = 'A'
key[12] = 'A'
key = ''.join(key)
print(key)

```



