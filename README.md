# 2024-12-26-Earthlink-CTF

## 1- horseCode

### Challenge Details

- **Challenge Name**: exitTool
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

    ```
    [Example of a command or code snippet]
    ```

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

## 2- pyCrypter

### Challenge Details

- **Challenge Name**: pyCrypter
- **Category**: ReverseEngineering
- **Points**: 70
- **Description**: [Provide the official description of the challenge]

---

### Step 1: Initial Analysis

#### Observations
- two files flag.txt.nc,python 
- edit on the python file to make the code work will wih the contint of flag.txt.en 

#### Tools Used
- Python


---

## Step 2: Approach and Execution

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
---

## 1- Mother Of All Math

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
    - get the Letters as [P E E P 0 P 3 3 P O P]

---

### Step 3: Solution
