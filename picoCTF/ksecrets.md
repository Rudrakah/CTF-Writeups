# KSECRETS
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Author** | Darkraicg492 |
| **Topic** | Kubernetes / Secrets |

## Challenge
The challenge is about finding a flag stored inside a Kubernetes Secret.

The hints are:

- Where are secrets usually stored in Kubernetes?
- How are Kubernetes secrets stored internally? Can you decode them?
- Please ignore TLS

These hints tell us that the flag is stored inside a Kubernetes Secret and that the value needs to be decoded.

## Step 1 – Find the Kubernetes Secret

First, check the available Kubernetes secrets:

    kubectl get secrets

We are looking for:

    flag-secret

## Step 2 – View the Secret

Run:

    kubectl get secret flag-secret -o yaml

The output contains the Secret information.

The important part is:

    data:
      flag: <BASE64_VALUE>

The flag is stored in Base64 format.

## Step 3 – Extract the Flag

We can use JSONPath to extract only the flag value:

    kubectl get secret flag-secret -o jsonpath='{.data.flag}'

This gives us the Base64 encoded value.

## Step 4 – Decode the Flag

Use `base64 -d` to decode it:

    kubectl get secret flag-secret -o jsonpath='{.data.flag}' | base64 -d

This directly prints the flag.

## Step 5 – TLS Hint

The challenge gives the hint:

    Please ignore TLS

If TLS certificate verification causes a problem, use:

    kubectl --insecure-skip-tls-verify get secret flag-secret -o yaml

Or:

    kubectl --insecure-skip-tls-verify get secret flag-secret -o jsonpath='{.data.flag}' | base64 -d

## Step 6 – Connection Refused

If you get:

    The connection to the server 127.0.0.1:6443 was refused

the Kubernetes API server is not currently available.

Restart the challenge instance and then try:

    kubectl get secrets

again.

## Important Concept

Kubernetes Secrets are used to store sensitive information such as:

- Passwords
- API keys
- Tokens
- Certificates
- Credentials

The values inside the `data` field are commonly Base64 encoded.

Base64 is encoding, not encryption.

Therefore, if we can access the Secret, we can decode the value easily.

For example:

    echo '<BASE64_VALUE>' | base64 -d

## Commands Used

Check Kubernetes nodes:

    kubectl get nodes

List secrets:

    kubectl get secrets

View the Secret:

    kubectl get secret flag-secret -o yaml

Extract the encoded flag:

    kubectl get secret flag-secret -o jsonpath='{.data.flag}'

Decode the flag:

    kubectl get secret flag-secret -o jsonpath='{.data.flag}' | base64 -d

If TLS verification causes an issue:

    kubectl --insecure-skip-tls-verify get secret flag-secret -o jsonpath='{.data.flag}' | base64 -d

## Flag

    picoCTF{kS3cr375_41n7_s4f3_52f603c4}

## Tools Used

- Kubernetes
- `kubectl`
- Linux Terminal
- Base64
- JSONPath
- YAML

## Key Learning

- Kubernetes uses Secret objects to store sensitive information.
- Secret values are commonly Base64 encoded.
- Base64 is not encryption.
- `kubectl get secret` can be used to inspect Kubernetes Secrets.
- JSONPath can extract a specific field.
- `base64 -d` can decode the extracted value.
- TLS verification may need to be skipped in some CTF environments when instructed by the challenge.

## Final Solution

    Start the Kubernetes challenge
            ↓
    Check Kubernetes secrets
            ↓
    Find flag-secret
            ↓
    View the Secret
            ↓
    Find the Base64 encoded flag
            ↓
    Extract .data.flag
            ↓
    Decode using base64 -d
            ↓
    Get the flag

## One-Line Solution

    kubectl get secret flag-secret -o jsonpath='{.data.flag}' | base64 -d
