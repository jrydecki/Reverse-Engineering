# Assignment 6

## Crackme 1 Solution ():
To solve this crackme, you need to disassemble the `crackme05` linux executable file in order to pull out the serial number constraints. With this contraints you can pass a valid serial number as an argument to the program which solves it.

A solution is `0VW0-C..B-B..C-0WX0`. Here is my program to generate any valid serial number:
```python
valid_chars = string.ascii_letters + "0123456789"
key = [random.choice(valid_chars) for i in range(19)]

# Paper
key[8] = key[10]
key[5] = key[13]
key[0] = '0'
key[3] = '0'
key[15] = '0'
key[18] = '0'

# Scissors
key[1] = random.choice("VWXYZ")
key[2] = random.choice("VWXYZ")
key[16] = random.choice("VWXYZ")

tmp = random.choice("VWXYZ")
while ord(key[1]) + ord(key[2]) == (ord(key[16]) + ord(tmp)):
    tmp = random.choice("VWXYZ")
key[17] = tmp

# Cracker
key[4] = "-"
key[9] = "-"
key[14] = "-"

print(''.join(key), end='')
```

### How I did it using Ghidra:
1. I opened the crackme in Ghidra
2. I opened the `main` function and noticed 5 important function calls: `rock`, `paper`, `scissors`, `cracker`, and `decraycray`. Within all these functions there is another function called `bomb`, which seems to be an error-out function -- so in all the functions I avoid the conditions where `bomb` is called. Also, in all the following explanations, the constraints are made clear by if-else conditions found in the code of the crackme.
3. The function `rock` adds the following constraints to the inputted serial:
  - All characters are >= `'-'`
  - No characters are between `'-'` and `'0'`
  - All characters are either `0-9`, `A-Z`, or `a-z`
  - The input is 19 characters.
4. The function `paper` adds the following constraints to the input:
  - `0 <= (input[8] ^ input[10]) < 10`
  - `0 <= (input[5] ^ input[13]) < 10`
  - `input[3]` and `input[15]` must be equal to `(input[10] ^ input[8]) + 48`
  - `input[0]` and `input[18]` must be equal to `(input[13] ^ input[5]) + 48`
  - **NOTE**: For simplicity, we can require that `input[8]` and `input[10]` are the same, likewise `input[5]` and `input[13]` be the same, which requires `input[3]`, `input[15]`, `input[0]`, and `input[18]` to all be `'0'`.

5. The function `scissors` adds the following constraints to the input:
  - `input[1] + input[2] > 171`
  - `input[16] + input[17] > 171`
  - `(input[1] + input[2]) != (input[16] + input[17])`
  - **NOTE**: This is essentially saying that `input[1]` and `input[2]` must be `V-Z` and likewise `input[16]` and `input[17]` must be `V-Z`, but between the two pairs, they can't both add up to the same number.

6. The function `cracker` adds the following constraints to the input:
  - `(input[4] + input[9] + input[14]) = 135`
  - **NOTE**: The only way this is satisfied (when considering the previous rules) is for all three characters to be `-`

7. A working serial with the constraints (where `.` is any character): `0VW0-C..B-B..C-0WX0`

8. Finally, the `decraycray` function prints out the success message.


## Crackme 2 Solution ():
To solve this crackme, you need to ____.

My solution is ____.
```python
```

### How I did it using Ghidra:
1. I opened the crackme in Ghidra
2. I found the `main` function and first noticed a while loop that asks for a username input, and will continue to ask until the input is between 8 and 12 characters.
3. Then, the program asks for a "serial number" (which is essentially a password) and the input is stored in an `sn` variable.
4. Next, there is a for loop that goes through each character of the username and performs the following operations:
  - If current character of the username is even: change that character to lowercase.
  - If the current char of the username is odd: change that character to uppercase.
  Then, this new string gets stored in `serialstring`.
5. We then set `usernamelength = (serialstring - 8) * 2`
6. Then, set `serialstring` gets set to substring `serialstring.substr(usernamelength)`.
6. Then, `serialstring2` is populated with the first 8 characters of `serialstring`


abcdefghij
serialstring = "aBcDeFgHiJ"
usernamelength = 4
serialstring = serialstring.substr(4) = cDeFgHiJ

The correct password is the first 8 characters of serialstring


abcdefgh
aBcDeFgH
HgFeDcBa


## Crackme 3 Solutions():



### How I did it using Ghidra:
1. I opened the crackme in Ghidra
2. I found the `entry` function (because I couldn't find a main), and then I found the function being called by `__libc_start_main`, which I then renamed `main`.
3. I went into main and


puVar - 8 = argv[1]
puvar -4 = argv[2]
puVar - 16 = argv[2]

argv[2] must be length of 6.
---------

puVar4 - 16 = argv[1]
sVar2 = len(argv[1])

iVar3 = return (input ^ 0x3b) & 0x3f;

STRING[iVar3] must equal argv[2]
---------

-16 = sVar2 = len(argv[1])
-20 = argv[1]
bVar2 = FUNC_#3( argv[1], len )

STRING[bVar2] must equal argv[2] + 1
----------

-16 = -12 = len(argv[1])




## Crackme 5 Solutions():
To solve this crackme, you need to patch the given `crackme04_32bit` Linux executable in order to first disable the self-destruction properties of the program that are activated if the program is not run at 13:37 or if the PID is not 1337. After patching the program, you need to disassemble it in order to pull out the correct serial-number generation algorithm (based on the PID). After you have this algorithm you must use it to generate the correct serial number, and input it into the program (that no longer self-destructs on run) to solve the crackme.

My solution is the serial number: `217552398261569-545326772-1082328767-1082328760-1082328765-1082328753`. The program that I used to generate this can be found in the section below with additional context.


### How I did it using Ghidra:
1. I opened the crackme in Ghidra.
2. I found the `main` function where I noticed that the program checks first that the time must be `"13:37"` and also that the PID of the program is `1337`. If either of these conditions are not met, the program immediately dies in the function `theOtherEnd` (which also deletes the program itself).

    In order to prevent the program from deleting itself, I patched the assembly such that the conditions are different. I did the patching in Ghidra, where I right-clicked the assembly line I wanted to change, selected "Patch Assembly", and made the changes. Then, I selected `File->Export Program->Original File` and got a new and patched file to run. 

    Here are the changes I actually made: For the time compare, I changed the instruction `TEST EAX, EAX` to `CMP EAX, EAX`. This means that as long as the time IS NOT 13:37, the program should run without deleting itself. 
    
    For the PID comparison, I first changed the assembly line `CALL <EXTERNAL>::getpid` and it's following `MOV dword ptr [ESP + pid_var], EAX` line, to `MOV dword ptr [ESP + pid_var], 1337`. After this change, there was a 1-byte gap in the assembly which would crash the program if not fixed. In order to fix this program, I patched that 1-byte gap with the `NOP` instruction. This stands for "No Operation", and as such, does nothing in the program, but its presence fixed the crash of the program. My changes mean that the PID variable always holds the correct PID value (`1337`), and the `getpid` function never gets called in the first place.

3. I then noticed a for loop (seen below) which performed some XORing and concatenation 7 times. This loop includes a function called `oil`, which similarly does some more XORing and ORing. That `result_var` variable is what will be looked at towards the end to determine if we have the correct serial.


![Crackme5 For-Loop](./src/6-5-forloop.png)

4. Instead of solving this by hand, I created the following C program to solve this for-loop for me. A surprising challenge was attempting to intrepret what I have labeled `result_var`, as Ghidra defines it as an `undefined4`, but at the same time seems to cast it as a `char *`, but also set it to `0` at some point. I knew that the pointer itself could not be `0`, so after a while I settled on defining `result_var` as a buffer-type variable, whose first character is a NULL byte -- which ended up working.
```C
#include <stdio.h>
#include <string.h>

void oil(int* num1, int* num2){
	*num2 = *num2 ^ 4;
	*num1 = *num1 | 0x2e39f3;
	return;
} 

int main(){
  int num_1;
  int sVar1;
	int int_21 = 218232;
	char *rand_str = "7030726e";
	int pid = 1337;
	char buff1[356];

	char result_var[100];
	result_var[0] = 0;

  for (int j = 0; j < 7; j = j + 1) {
      int_21 = pid ^ int_21;
      num_1 = int_21 + rand_str[j] + 92;
      oil(&int_21,&num_1);
      sprintf(buff1,"%d",num_1);
      sVar1 = strlen(buff1);
      strncat(result_var,buff1,sVar1);
      int_21 = int_21 << 7;
  }
	printf("%s\n", result_var);
}
```
This program ended by printing out the string `217552398261569-545326772-1082328767-1082328760-1082328765-1082328753`. So for the situation where the PID is `1337` (either naturally, or artifically as I have it), this is the correct serial number that solves the crackme.

5. Finally, I noticed in the main function that after the program prompts the user to enter the serial number, the program checks to see if the input matches the output of the for-loop operations, which is the string above. After entering the above string at the prompt, I received a message saying `YOU WIN!`, signifying that this was a correct answer.