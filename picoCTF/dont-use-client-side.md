# don't-use-client-side

**Platform:** picoCTF 2019

**Category:** Web Exploitation

**Difficulty:** Easy

---

## Challenge

Can you break into this super secure portal?

http://fickle-tempest.picoctf.net:60674

The challenge presents a simple login page that asks for a password to access a secure portal.

The objective is to discover the correct password (flag) without guessing it.

## Hint
The only hint provided is:

> Never trust the client.

This hint suggests that the client-side code (HTML and JavaScript running in the browser) may contain sensitive information.

## Approach

Instead of trying random passwords, I followed the hint and inspected the web page.
Using the browser's **Developer Tools**, I viewed the page source and analyzed the JavaScript responsible for password verification.
The JavaScript code revealed that the password was being checked entirely on the client side, making it possible to recover the flag without interacting with the server.

## JavaScript Analysis
The `verify()` function split the password into multiple parts using the `substring()` function and compared each part separately.
```javascript
function verify() {
    checkpass = document.getElementById("pass").value;
    split = 4;

    if (checkpass.substring(0, split) == 'pico') {
        if (checkpass.substring(split*6, split*7) == 'eb02') {
            if (checkpass.substring(split, split*2) == 'CTF{') {
                if (checkpass.substring(split*4, split*5) == 'ts_p') {
                    if (checkpass.substring(split*3, split*4) == 'lien') {
                        if (checkpass.substring(split*5, split*6) == 'lz_2') {
                            if (checkpass.substring(split*2, split*3) == 'no_c') {
                                if (checkpass.substring(split*7, split*8) == 'b45}') {
                                    alert("Password Verified");
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}
```

Instead of storing the complete flag in one string, it was divided into several 4-character pieces.

| Position | Value |
|---------:|-------|
| 0 | `pico` |
| 1 | `CTF{` |
| 2 | `no_c` |
| 3 | `lien` |
| 4 | `ts_p` |
| 5 | `lz_2` |
| 6 | `eb02` |
| 7 | `b45}` |

Combining these parts gives:

```text
picoCTF{no_clients_plz_2eb02b45}
```
## Flag

```text
picoCTF{no_clients_plz_2eb02b45}
```

---

## Tools Used

- Browser Developer Tools
- View Source
- JavaScript Analysis

---

## Key Learning

- Never trust client-side validation.
- Sensitive information should never be stored inside client-side JavaScript.
- Browser Developer Tools can reveal hidden application logic.
- Authentication and security checks should always be performed on the server.
