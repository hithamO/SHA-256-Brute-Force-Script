# SHA-256 Brute Force Script
This Python script is designed to perform a brute-force attack on a given SHA-256 hash by comparing it to the hashes of passwords from the rockyou.txt wordlist. It uses the Pwn library for logging and process management.

## Features
- Hash Comparison: The script compares the SHA-256 hash of each password from the wordlist with the given hash.
- Logging: Provides detailed logging of each attempt, including the password and corresponding hash.
- Efficiency: Stops the brute force attempt when a matching hash is found.
- 
### Prerequisites:

- Install the required libraries:
```
pip install pwntools
```
Ensure you have the rockyou.txt wordlist file. This file can be downloaded from various sources online.

### Run the Script:

Execute the script with the SHA-256 hash you want to crack as an argument:

```
from pwn import *
import sys

if len(sys.argv) != 2:
    print("invalid arguments!")
    print(">> {} <sha256sum>".format(sys.argv[0]))
    exit()

wanted_hash = sys.argv[1]
password_file = "rockyou.txt"
attempts = 0

with log.process("Attempting to hack: {}!\n".format(wanted_hash)) as p:
    with open(password_file, "r", encoding='latin-1') as password_list:
        for password in password_list:
            password = password.strip("\n").encode('latin-1')
            password_hash = sha256sum(password)
            p.status("[{}] {} == {}".format(attempts, password.decode('latin-1'), password_hash))
            if password_hash == wanted_hash:
                p.success("password hash found after {} attempts! {} hashes to {} !".format(attempts, password.decode('latin-1'), password_hash))
                exit()
            attempts += 1
        p.failure("password hash not found!")
```
