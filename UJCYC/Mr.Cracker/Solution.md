# Overview

This challenge was created by our CyberSecurity Club, its category is Steganography.

# Description
We are given a rar archive which is protected by a password.
The password policy is given in the challenge, and our task is to create a wordlist from this policy to crack the password.

The policy was :

1. The first character is lowercase
2. The second character is uppercase
3. Third is a symbol
4. Fourth is a number
5. then append three names: fares, salem, and basurrah.

# Solution

I will use **crunch**** to create the wordlist for me and specify the pattern, we can also use Python but for the sake of the time crunch is faster.
```
crunch 9 9 -t @,^%feras -o feras.lst
crunch 9 9 -t @,^%salem -o salem.lst
crunch 12 12 -t @,^%basurrah -o basurrah.lst
```
now we have three wordlists as follows :
![image](https://github.com/0xiNAIF/CTF-Writeups/assets/86973917/bbf717c2-a387-4288-a0ce-18aeadbbb5cb)

let us combine them into one wordlist
```
cat basurrah.lst feras.lst salem.lst > wordlist.lst
```
Now we need the hash of the RAR Archive to crack it, using John to extract the hash :
```
rar2john Mr.Cracker.rar
```
![image](https://github.com/0xiNAIF/CTF-Writeups/assets/86973917/ec7676c9-b165-4fcc-856e-decc5cd3fb55)

Using hashcat, we can crack the hash but remove the first part ( File name ) because hashcat doesn't accept it.
$rar5$16$b54d5846474d833c4681aee5a31cfea5$15$1dec08dd38245b4438e83bc4c215b6e8$8$113f6c537575bf09

```
echo "$rar5$16$b54d5846474d833c4681aee5a31cfea5$15$1dec08dd38245b4438e83bc4c215b6e8$8$113f6c537575bf09" > hash
```

Now we can start cracking using **hashcat**, hashcat is much faster using GPU, let us use our Windows Host machine to crack the file.
```
hashcat -m 13000 testHash wordlist --force
```

And sure enough, we get the password in seconds :
![image](https://github.com/0xiNAIF/CTF-Writeups/assets/86973917/413e14b9-20ff-4fbf-8eba-68502d4b3652)
Password: jY$2salem

Unzipping the archive and getting the flag : UJCYC{pAsSw0rD_Cr4cK3r_04583408973460}

