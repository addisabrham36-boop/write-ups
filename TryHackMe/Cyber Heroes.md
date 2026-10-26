# CyberHeroes

Welcome to the CyberHeroes room walkthrough on TryHackMe!

{% embed url="https://tryhackme.com/room/cyberheroes" %}

***

CyberHeroes is an easy web application challenge centered around client-side authentication bypass. Our objective is to analyze the login page, identify the flaw in the authentication mechanism, recover the credentials, and retrieve the flag.

---

## 1. Initial Reconnaissance

We start by navigating to the target web application:

```text
http://<TARGET_IP>/
```

The home page introduces the "CyberHeroes" squad. Looking through the navigation bar, there is a **Login** link leading to:

```text
http://<TARGET_IP>/login.html
```

We are presented with a clean login form asking for a username and password.

---

## 2. Inspecting the Page Source

Before attempting any brute-forcing or automated scans, we inspect the page source (`Ctrl + U` or Right-Click -> *View Page Source*).

Inside `login.html`, we find an embedded `<script>` tag containing the following `authenticate()` JavaScript function:

```javascript
function authenticate() {
    a = document.getElementById('uname')
    b = document.getElementById('pass')
    const RevereString = str => [...str].reverse().join('');
    if (a.value=="h3ck3rBoi" & b.value==RevereString("54321@terceSrepuS")) {
        var xhttp = new XMLHttpRequest();
        xhttp.onreadystatechange = function() {
            if (this.readyState == 4 && this.status == 200) {
                document.getElementById("flag").innerHTML = this.responseText;
                document.getElementById("todel").innerHTML = "";
                document.getElementById("rm").remove();
            }
        };
        xhttp.open("GET", "RandomLo0o0o0o0o0o0o0o0o0o0gpath12345_Flag_"+a.value+"_"+b.value+".txt", true);
        xhttp.send();
    } else {
        alert("Incorrect Password, try again.. you got this hacker !");
    }
}
```

### Analyzing the Flaw:
The authentication decision is implemented entirely on the **client side** within the user's browser:
1. **Username (`a.value`)**: Directly checked against `"h3ck3rBoi"`.
2. **Password (`b.value`)**: Compared against `RevereString("54321@terceSrepuS")`.
3. Reversing `"54321@terceSrepuS"` yields:
   ```text
   SuperSecret@12345
   ```

Because client-side validation is completely controllable by the user, we have the valid credentials without interacting with any backend database.

---

## 3. Exploitation & Retrieving the Flag

We have two ways to retrieve the flag:

### Method 1: Form Login
1. Navigate to `http://<TARGET_IP>/login.html`.
2. Enter the recovered credentials:
   - **Username:** `h3ck3rBoi`
   - **Password:** `SuperSecret@12345`
3. Click **Login**. The page dynamically executes the `XMLHttpRequest` and prints the flag.

### Method 2: Direct URL Request
Observing the `XMLHttpRequest` endpoint:
```text
RandomLo0o0o0o0o0o0o0o0o0o0gpath12345_Flag_h3ck3rBoi_SuperSecret@12345.txt
```
We can directly fetch the flag using `curl`:

```bash
curl http://<TARGET_IP>/RandomLo0o0o0o0o0o0o0o0o0o0gpath12345_Flag_h3ck3rBoi_SuperSecret@12345.txt
```

### Output:
```text
flag{edb0be532c540b1a150c3a7e85d2466e}
```

---

## 🏁 Questions & Answers

| Task / Question | Answer |
| :--- | :--- |
| **Give the flag that was displayed!** | `flag{edb0be532c540b1a150c3a7e85d2466e}` |

***
