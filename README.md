# 2024-12-26-Earthlink-CTF

## 1- horseCode

### Challenge Details

- **Challenge Name**: horseCode
- **Category**: Misc
- **Points**: 10
- **Description**: [Provide the official description of the challenge]

---

### Step 1: Initial Analysis

#### Observations
- the peep peep sound looks like morse Morse code
- need to translate the sound wav to Letters

#### Tools Used
- [Morse Code Adaptive Audio Decoder](https://morsecode.world/international/decoderaudio-decoder-adaptive.html)


---

### Step 2: Approach and Execution

1. **Step-by-Step Process**
    - uplode the secret.wav file and start to play the sound and decode it

2. **Analysis of Results**
    - get the Letters as [P E E P 0 P 3 3 P O P]

---
### Step 3: Solution

#### Final Steps
- remove the spaces btw the Letters and add it inside 'ELCTF{}'

#### Flag
```
ELCTF{peep0p33pop}
```
---
## 2- Mother Of All Math



### Challenge Details

- **Challenge Name**: Mother Of All Math
- **Category**: Misc
- **Points**: 100
- **Description**: [Provide the official description of the challenge]

---

### Step 1: Initial Analysis

#### Observations
- it to fast to do it Manually :)
- make python code the take the output form the terminal (execute elf file) and do the math quiz then show the the output after finish the quiz

#### Tools Used
- Python


---

### Step 2: Approach and Execution

1. **Step-by-Step Process**
    - write python code
        ```
            import re
            import pexpect
            import math
            # Function to solve math problems
            def solve_problem(problem):
                try:
                    # Replace ^ with ** for Python to understand power operations
                    problem = problem.replace("^", "**")
                    # Use eval with support for math functions
                    # Add 'math.' prefix to known math functions
                    problem = re.sub(r"\bsqrt\b", "math.sqrt", problem)
                    problem = re.sub(r"\b(log|sin|cos|tan|exp)\b", r"math.\1", problem)

                    # Evaluate the expression
                    result = eval(problem)

                    # Format the result based on the instructions
                    if isinstance(result, float):
                        if "**" in problem:  # Power operation
                            return f"{result:.5f}"  # First 5 digits after the decimal
                        else:
                            return f"{result:.3f}"  # Round to 3 decimal places
                    return str(result)  # For integer results
                except Exception as e:
                    print(f"Error solving problem: {problem}, Error: {e}")
                    return None
            # Start the ELF file using pexpect
            elf_program = "./motherOfAllMath.elf"
            process = pexpect.spawn(elf_program)
            # Read the welcome message
            process.expect("Level 1:.*")
            while True:
                try:
                    # Extract the math problem
                    line = process.after.decode('utf-8')
                    match = re.search(r"Level \d+: (.+)", line)
                    if not match:
                        break
                    problem = match.group(1).strip()
                    print(f"Problem: {problem}")
                    # Solve the problem
                    answer = solve_problem(problem)
                    if answer is None:
                        print("Skipping problem due to error.")
                        continue
                    print(f"Answer: {answer}")
                    # Submit the answer
                    process.sendline(answer)
                    # Wait for the next prompt or flag
                    index = process.expect([r"Level \d+:.*", r"FLAG: (.+)"], timeout=5)
                    if index == 1:  # Found the flag
                        flag = process.match.group(1).decode('utf-8')
                        print(f"Flag: {flag}")
                        break
                except pexpect.exceptions.TIMEOUT:
                    print("Timeout reached. No more problems or flag found.")
                    break
            # Close the process
            process.close()
        ```

2. **Analysis of Results**
    - run the python file python Mother Of All Math.py 
    - it will show some error but then get the right flag
    - If it works, don't touch it :)

---

#### Flag
```
ELCTF{MaTh_!s_NOT_hARd_AFt3R_A1L}
```
---
## 3- weWill

### Challenge Details

- **Challenge Name**: weWill
- **Category**: Misc
- **Points**: 10
- **Description**: [Provide the official description of the challenge]

---

### Step 1: Initial Analysis

#### Observations
- it looks like zip file need password to extract it
- so its time to my old Unc 'John' to crack the file and get the password

#### Tools Used
- zip2john , john, unzip


---

### Step 2: Approach and Execution

1. **Step-by-Step Process**
    - get the hash of the zip file
    ```
    zip2john weWill.zip > zipfile.hash
    ```
    - use john tool to crack the hash with rockyou.txt file 
    ```
    sudo john zipfile.hash --wordlist=/usr/share/wordlist/rockyou.txt
    ```
2. **Analysis of Results**
    - we will get the password of the zip file 'cingular' 
    - use the passowrd to extract the txt file in zip folder

---
### Step 3: Solution

#### Flag
```
ELCTF{w3_wIL1_r0Ck_You}
```
---
## 4- exitTool

### Challenge Details

- **Challenge Name**: exitTool
- **Category**: Misc
- **Points**: 10
- **Description**: [Provide the official description of the challenge]

---

### Step 1: Initial Analysis

#### Observations
- normal image.png file with carton art 
- check the meta data of the file 

#### Tools Used
- exiftool , [CyberChef](https://gchq.github.io/CyberChef/)


---

### Step 2: Approach and Execution

1. **Step-by-Step Process**
    - use the exiftool to to get the meta data of the image.png
    ```
    exiftool image.png
    ```
    - we get Suspect information written in the form of Base64 so will use CyberChef to decode it
---
## 5- Arm And Leg

### Challenge Details

- **Challenge Name**: Arm And Leg
- **Category**: Misc
- **Points**: 60
- **Description**: [Provide the official description of the challenge]

---

### Step 1: Initial Analysis

#### Observations
- determine the file type that need to execute 
- its ARM aarch64 so need run binary file for other architectures but with the same operating

#### Tools Used
- file , qemu-aarch64


---

### Step 2: Approach and Execution

1. **Step-by-Step Process**
    - determine the file type by file tool 
    ```
    file armAndLeg
    ```
    - it is ARM aarch64 so need to use the qemu-aarch64 but befor that make the file execute
    ```
    chmod +x armAndLeg
    qemu-aarch64 armAndLeg
    ```
---
### Step 3: Solution

#### Flag
```
ELCTF{arM$_are_h@nDy}
```
---
## 6- pyCrypter
### Challenge Details
- **Challenge Name**: pyCrypter
- **Category**: ReverseEngineering
- **Points**: 70
- **Description**: [Provide the official description of the challenge]
### Step 1: Initial Analysis

#### Observations
- two files flag.txt.nc,python 
- edit on the python file to make the code work will wih the contint of flag.txt.en 

#### Tools Used
- Python
### Step 2: Approach and Execution

1. **Step-by-Step Process**
    - the python code after edit

        ```
            import base64
            def mydecode(encryptedFlag, key):
                decryptedFlag = []
                for i in range(len(encryptedFlag)):
                    keyChar = key[i % len(key)]
                    decChar = (encryptedFlag[i] - ord(keyChar)) % 256
                    decryptedFlag.append(chr(decChar))
                return ''.join(decryptedFlag)
            # Base64 decode the encrypted flag
            encrypted_data = base64.urlsafe_b64decode("ksXO1MXY2piuxrB-6MHElOXE4nvZ2HGstrrVt-I=")
            # Decrypt the flag using the same key
            decrypted_flag = mydecode(encrypted_data, "MySecretKey")
            print(decrypted_flag)
        ```
    2. **Analysis of Results**
        - run the python file ``` python pyCrypter.py  ```
    ---
#### Flag
```
EL{obfu$ca71on_1s_n0t_$3cUrE}
```
## 7- My New Safe
### Challenge Details
- **Challenge Name**: My New Safe
- **Category**: ReverseEngineering
- **Points**: 85
- **Description**: [Provide the official description of the challenge]
### Step 1: Initial Analysis

#### Observations
- execute the file and show that need passowrd 
- need to show the runtime behavior of the file maybe get the correct password that the program accpted 

#### Tools Used
- ltrace
### Step 2: Approach and Execution
 1. **Step-by-Step Process**
    - make the file executeble 
    ```
    chmod + myNewSafe.elf
    ```
    - try to use the ltrace with our file and analysis the output
    ```
    ltrace ./myNewSafe.elf
    ```
    - the output 
        ```bash
        ltrace ./myNewSafe.elf
        printf("Enter The Password: ") = 20
        fgets(Enter The Password: d
        "d\n", 50, 0x7f5acba8c8e0) = 0x7ffe07018d20
        strcspn("d\n", "\n") = 1
        strcmp("d", "Talk_Back_to_Me") = 16
        puts("Incorrect! Please try again."Incorrect! Please try again.) = 29
        +++ exited (status 0) +++
    ```
 2. **Analysis of Results**
    1. **`printf("Enter The Password: ")`**  
        - The program prompts the user to enter a password.
        - The output `= 20` means 20 characters were printed.

    2. **`fgets(Enter The Password: d\n, 50, ...)`**  
        - The program reads the user input (`d\n`) using `fgets`, allowing up to 50 characters.
        - The input, including the newline (`\n`), is stored at a specific memory address.

    3. **`strcspn("d\n", "\n") = 1`**  
        - This function removes the newline character (`\n`) from the input string.
        - The result `= 1` indicates the index of the newline.

    4. **`strcmp("d", "Talk_Back_to_Me") = 16`**  
        - The program compares the user’s input (`d`) with the correct password (`Talk_Back_to_Me`).
        - The result `= 16` means the strings do not match.

    5. **`puts("Incorrect! Please try again.")`**  
        - Since the input was incorrect, it prints the failure message.

    6. **`+++ exited (status 0) +++`**  
        - The program terminates normally with a status code of `0`.
---
### How to Solve
    - The correct password is revealed in the `strcmp` function: `"Talk_Back_to_Me"`.
    - To solve the challenge, rerun the program and input the correct passwo
---
#### Flag
```
ELCTF{rE_!$_niCE}
```
---
## 8- Little Endian route
### Challenge Details
- **Challenge Name**: Little Endian route
- **Category**: Web
- **Points**: 15
- **Description**: [Provide the official description of the challenge]
### Step 1: Initial Analysis

#### Observations
- get message with sentence and some numbers and /
- clear the numbers without the words and / to looks like url path
- use the hint of POST Method 

#### Tools Used
- curl
### Step 2: Approach and Execution

1. **Step-by-Step Process**
    - using the curl tool and make POST Request with web path and add the numbers 
        ```
        curl -X POST https://earthlink-iq-little-endian-quest.chals.io/62616768646164
        ```
