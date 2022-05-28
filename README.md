# Geekman | 28 May | Saturday
# twitter : https://twitter.com/geekman_28

There was a recent room in Tryhackme called "CyberHeroes", Which was flagged as medium ["But it is not actually"].

Description says that we should navigate to http://Target_IP and do some authentication bypass stuff,

Step 1 : nmap scan
    '''
    # Nmap 7.92 scan initiated Sat May 28 02:36:01 2022 as: nmap -sC -sT -A -p22,80 -oN nmap/initial 10.10.13.82
    Nmap scan report for 10.10.13.82
    Host is up (0.25s latency).

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 8d:dc:f0:6d:cb:8f:f4:6e:d8:e7:d3:36:8e:13:bf:58 (RSA)
    |   256 a5:e8:c0:bd:5d:c9:bf:93:94:b1:8b:bf:50:df:e4:da (ECDSA)
    |_  256 49:8b:8e:7a:72:32:e8:31:83:53:76:0c:ef:c0:79:3a (ED25519)
    80/tcp open  http    Apache httpd 2.4.48 ((Ubuntu))
    |_http-server-header: Apache/2.4.48 (Ubuntu)
    |_http-title: CyberHeros : Index
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    # Nmap done at Sat May 28 02:36:32 2022 -- 1 IP address (1 host up) scanned in 31.35 seconds
    '''

# nmap scan shows that there is not much more intresting stuff than http {port 80}

Step 2 : looking at the source code of login page
    1. go to login page and view the source
    (You can look at the source code by right-clicking the web page and selecting "View page source" or
    Ctrl+U works just fine)
    
# you can notice that authenticate() function is loaded when you click on login button
# and there is a javascript right after it which defines the authenticate() function

Step 3 : Going through authenticate() function code

    function authenticate() {
      a = document.getElementById('uname')
      b = document.getElementById('pass')
      const RevereString = str => [...str].reverse().join('');
      if (a.value=="h3ck3rBoi" & b.value==RevereString("54321@terceSrepuS")) { 
        var xhttp = new XMLHttpRequest();
        xhttp.onreadystatechange = function() {
          if (this.readyState == 4 && this.status == 200) {
            document.getElementById("flag").innerHTML = this.responseText ;
            document.getElementById("todel").innerHTML = "";
            document.getElementById("rm").remove() ;
          }
        };
        xhttp.open("GET", "RandomLo0o0o0o0o0o0o0o0o0o0gpath12345_Flag_"+a.value+"_"+b.value+".txt", true);
        xhttp.send();
      }
      else {
        alert("Incorrect Password, try again.. you got this hacker !")
      }
    }

# here it says that 'a' variable will be username and 'b' variable will be password, so lets take look at that

# you can see a.value = h3ck3rBoi and b.value = RevereString("54321@terceSrepuS")
# Now you know that username is h3ck3rBoi and password looks wierd
# ReverseString is a function in javascript which reverses the string
for example : if the strings is "abc" and reverse will be "cba", like that reversing the password will give you the real password

paste this command to get password

echo "54321@terceSrepuS" | rev
# then the output will be SuperSecret@12345

login with the creds
username : h3ck3rBoi
pasword : SuperSecret@12345

# you will get the flag in the format of
flag{REDACTED}