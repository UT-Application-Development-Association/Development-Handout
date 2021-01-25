# Git-Secret
This post is about giving a introduction to git secret which is a bash tool for encrypting and protecting files in a git repository

## GPG
Git secret is using **gpg** an open source project for encrypting files.

Below is a list of commonly used commands of gpg

- `gpg --list-key`: List out all public keys

- `gpg --list-secrey-keys`: List out all secret keys

- `gpg --gen-key`: Generate new private key
    - gpg requires a name of the key and a email for creating a key
    
    - The email will be used as the user id of the key
    
    - Also, it requires a passphrase which will be used to export and for revealing the encrypted file

- `gpt --armor --output <output file> --export <user id>`: Export **public key**

- `gpg --armor --output <output file> --export-secret-keys <user id>`: Export **secret key**

- `gpg --import <private key>`: Import the key

## Git Secret
1. Initialize a git secret repository
    - `git-secret init`  This initializes a .gitsecret folder under the repository
    
2. Add private key for encryption the <user> is used to locate the private key in the local
    - `git-secret tell <user>`
    
3. Add files for encryption
    - `git-secret add <file>`
    
    - Meanwhile, git secret will try to detect whether the files are mentioned in the .gitignore， if not, it will
    automatically add the file to the .gitignore for preventing accidentally pushing the file to the repo 
    
4. Hide files
    - `git-secret hide`
    
    - After hide, the original file will be encrypted and hidden from the local 
      
    - Meanwhile, an encrypted version end with .secret will be created
    
5. Push to the repo
    - This step is same as you usually do
    
    - But, you should only push the encrypted version to your repo
    
6. Reveal file
    - `git-secret reveal`
    
    - The above command will prompt you to enter the passphrase for revealing the original file
