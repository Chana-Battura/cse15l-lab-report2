# CSE 15L Lab Report 2
### by Charan Battula A17750911

---

This lab report will describe the creation process of ChatServer.  With this website, the queries are added to the chat log as messages and the following sections will describe this process. 
---
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
---
## `/add-message` query
> The `/add-message` query works with two parts. The basic template for the query that can be run is `?s=MESSAGE&user=USERNAME` where the message and username can be switched with this.

#### Example One
Here is the screenshot of the query run `http://localhost:4000/add-message?s=Hello&user=jpolitz`
> ![image](https://github.com/Chana-Battura/cse15l-lab-report2/assets/39713790/0a2674cd-76f1-41e2-b496-c8e2b7adefed)
- When this query is run, the `handleRequest(URI url)` method is run, specifically the section when path contains `/add-message`.
- The relevant argument to this method is the URL.  This method call adds the message from the URI object to the field `ArrayList<String> vals`
- When this method is called, the `ArrayList<String> vals` gets updated to the values of the message.  The values that are passed are the message and the username.  The message is `Hello` and the username is `jpolitz`.  

#### Example Two
Here is the screenshot of the query run `http://localhost:4000/add-message?s=Hello&user=jpolitz`
> ![image](https://github.com/Chana-Battura/cse15l-lab-report2/assets/39713790/d4d72123-2a83-42ff-9b43-5aba2b360ba6)
- When this query is run, the `handleRequest(URI url)` method is run, specifically the section when path contains `/add-message`.
- The relevant argument to this method is the URL.  This method call adds the message from the URI object to the field `ArrayList<String> vals`
- When this method is called, the `ArrayList<String> vals` gets updated to the values of the message.  The values that are passed are the message and the username.  The message is `Hello` and the username is `jpolitz`.  
