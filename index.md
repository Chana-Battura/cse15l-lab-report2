# CSE 15L Lab Report 2
### by Charan Battula A17750911

---
### Part One: 
This lab report will describe the creation process of ChatServer.  With this website, the queries are added to the chat log as messages and the following sections will describe this process. 

## The Code:
Here is the code that runs all of the Chat Server:
```java
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.

    ArrayList<String> vals = new ArrayList<String>();

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format("Welcome to Charan's Chat Server");
        } else {
            if (url.getPath().contains("/add-message")) {
                String[] queries = url.getQuery().split("&");
                String[] messageParams = queries[0].split("=");
                String[] userParams = queries[1].split("=");
                if (userParams[0].equals("user") && messageParams[0].equals("s")) {
                    vals.add(String.format("%s: %s", userParams[1], messageParams[1]));
                }
                String message = "";
                for (String i: vals){
                    message += i + "\n";
                }
                return String.format(message);
            }
            return "404 Not Found!";
        }
    }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}

```

## `/add-message` query
> The `/add-message` query works with two parts. The basic template for the query that can be run is `?s=MESSAGE&user=USERNAME` where the message and username can be switched with this.

#### Example One
Here is the screenshot of the query run `http://localhost:4000/add-message?s=Hello&user=jpolitz`
> ![image](https://github.com/Chana-Battura/cse15l-lab-report2/assets/39713790/0a2674cd-76f1-41e2-b496-c8e2b7adefed)
- When this query is run, the `handleRequest(URI url)` method is run, specifically the section when path contains `/add-message`.
- The relevant argument to this method is the URL.  This method call adds the message from the URI object to the field `ArrayList<String> vals`
- When this method is called, the `ArrayList<String> vals` gets updated to the values of the message of `jpolitz: Hello`.  The values that are passed are the message and the username.  The message is stored in the variable `String userParams[1]` and the username is in the variable `String userParams[0]`.  

#### Example Two
Here is the screenshot of the query run `http://localhost:4000/add-message?s=Hello&user=jpolitz`
> ![image](https://github.com/Chana-Battura/cse15l-lab-report2/assets/39713790/d4d72123-2a83-42ff-9b43-5aba2b360ba6)
- When this query is run, the `handleRequest(URI url)` method is run, specifically the section when path contains `/add-message`.
- The relevant argument to this method is the URL.  This method call adds the message from the URI object to the field `ArrayList<String> vals`
- When this method is called, the `ArrayList<String> vals` gets updated to the values of the message of `CharanB: Good Morning!`.  The values that are passed are the message and the username.  The message is stored in the variable `String userParams[1]` and the username is in the variable `String userParams[0]`.
---
### Part Two:
In this section of the lab, we will go over the process of using the `ls` command line when accessing our server from our computer

- Private Key
```
C:\Users\chara\.ssh>dir
 Volume in drive C is OS
 Volume Serial Number is 4C4E-A1B9

 Directory of C:\Users\chara\.ssh

01/25/2024  02:34 PM    <DIR>          .
01/28/2024  04:25 AM    <DIR>          ..
01/25/2024  02:34 PM             2,610 id_rsa
01/25/2024  02:34 PM               576 id_rsa.pub
01/25/2024  02:14 PM               225 known_hosts
               3 File(s)          3,411 bytes
               2 Dir(s)  288,206,143,488 bytes free
```
> This cannot be accessed from the server.  Only the public key is sent to the server to complete our authentication, but the private key stays with the computer.  There for here is the private key.  The private key is the id_rsa.
- Public Key Location
``` bash
[srbattula@ieng6-203]:~:64$ cd .ssh/
[srbattula@ieng6-203]:.ssh:65$ ls
authorized_keys
```
> Public key is stored in the authorized_keys as shown with `ls`.  The public key works as it can be stored in the server and is used to authenticate the connection by comparing the private key on my computer.
- Terminal Interaction Without Password
```bash
C:\Users\chara\OneDrive\Desktop\CSE 15L\Lab Report 2>ssh srbattula@ieng6.ucsd.edu
Last login: Mon Jan 29 20:03:14 2024 from 128.54.150.240
quota: Cannot resolve mountpoint path /home/linux/ieng6/cs120wi24/public/.snapshot/daily.2023-12-28_0010: Stale file handle
Hello srbattula, you are currently logged into ieng6-203.ucsd.edu

You are using 0% CPU on this system

Cluster Status 
Hostname     Time    #Users  Load  Averages  
ieng6-201   20:10:01   2  0.87,  0.42,  0.35
ieng6-202   20:10:01   2  0.54,  0.24,  0.17
ieng6-203   20:10:01   4  0.02,  0.22,  0.22

 

To begin work for one of your courses [ cs15lwi24 ], type its name       
at the command prompt.  (For example, "cs15lwi24", without the quotes).  

To see all available software packages, type "prep -l" at the command prompt,
or "prep -h" for more options.
[srbattula@ieng6-203]:~:67$ 
```
> This shows how without even using a password step, I can log into the server without such authetification.
---
### Part 3:
During the labs from week 2 and week 3, I learned how to use `ssh`.  This was a command that I had heard of before but never used to connect to a server.  I found the most interesting part to be adding the public key to the server as I never knew computers had such a system to connect server to server.
