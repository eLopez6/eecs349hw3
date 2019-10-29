### Emilio Lopez, eil11

### EECS 349; 18 OCT 2019

# HW3 OllyDBG Tools

#### Q1

Refer to the CRACKME_Y.exe contained in this repository. The code has been changed so that any serial number should work.

#### Q2

Instead of using OllyDBG, I used Ida. While looking at the assembly line by line, I saw something called a WndProc. I reasoned it probably means WindowsProcess and figured it might be important.

I think within this WndProc, the username is entered into EAX. There is a `cmp eax, 0` command to ensure that the username entered is not blank. 

After that, this sub_40137E function is called. Here, it seems to convert the each character in the username to uppercase form. Then, it sums the ASCII value of the userename. The program then invokes `XOR edi, 5678h`, which is then moved to the EAX result.  

Then, a sub_4013D8 function is invoked. Here, the username is XORed with 0x1234. 

Finally, the program then compares that the serial number entered matches the value calculated so far: the capital letter ASCII representation of the username string XORed with 0x5678 and 0x1234


For actually solving what the serial number should be, I wrote the following basic script:

import sys

```python
import sys

def serial(username):
    s = 0
    for i in username.upper():
        s += ord(i)
    return s ^ 0x5678 ^ 0x1234

if __name__ == '__main__':
    print ("Crackme Serial: " +  str(serial(sys.argv[1])))
```

`$ python serial.py emilio`

`=> 17907`


`$ python serial.py shifu`

`=> 17715`