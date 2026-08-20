# Ph4nt0m Intrud3r
| **Platform** | picoCTF 2025 |
| **Category** | Forensics |
| **Difficulty** | Easy |
| **Author** | Prince Nyonsuthu N. |
| **Topic** | PCAP / Network Forensics / Packet Analysis / Base64 |

## Challenge

The challenge provides a network traffic PCAP file and asks us to find the hidden flag.

The hints are:

```text
Filter your packets to narrow down your search.
Attacks were done in timely manner.
Time is essential.
```

The important clue is that the attack traffic is time-sensitive, so packet timestamps can help identify the relevant packets.

## Step 1 – Download the PCAP

Download the provided network traffic file.

The file used in this challenge is:

```text
myNetworkTraffic.pcap
```

Check the file:

```bash
ls -lh myNetworkTraffic.pcap
```

## Step 2 – Inspect the PCAP

We can use Wireshark or command-line tools to inspect the packets.

A quick way to inspect packet timestamps and data is:

```bash
tshark -r myNetworkTraffic.pcap
```

The challenge hints that timing is important, so packet timestamps should be examined carefully.

## Step 3 – Extract Packet Time and Data

The packet data can be extracted using:

```bash
tshark -r myNetworkTraffic.pcap -T fields -e frame.time_epoch -e data
```

This gives output containing:

```text
TIMESTAMP    HEX_DATA
```

The data field contains hexadecimal bytes.

## Step 4 – Convert Hex Data

The hexadecimal packet data can be converted into raw bytes and then decoded.

For example:

```bash
echo "HEX_DATA" | xxd -r -p
```

The decoded data contains Base64-encoded fragments.

Because the fragments are transmitted across multiple packets, they need to be collected in the correct time order.

## Step 5 – Sort the Packets by Time

Since the hint says:

```text
Time is essential
```

the packet timestamps can be used to preserve the correct order.

A useful approach is:

```bash
tshark -r myNetworkTraffic.pcap -T fields -e frame.time_epoch -e data > packets.txt
```

Then process each packet in timestamp order and decode its hexadecimal data.

Example:

```bash
while read -r time hex; do
    decoded=$(echo "$hex" | xxd -r -p 2>/dev/null | base64 -d 2>/dev/null)
    [ -n "$decoded" ] && printf '%s\t%s\n' "$time" "$decoded"
done < packets.txt > decoded.txt
```

## Step 6 – Search for the Flag

Search the decoded output for `picoCTF`:

```bash
grep -i "picoCTF" decoded.txt
```

The output reveals that the flag is split across several packets.

To inspect the surrounding packets:

```bash
grep -i -B 10 -A 20 "picoCTF" decoded.txt
```

The relevant pieces can then be reconstructed in timestamp order.

## Step 7 – Reconstruct the Flag

The packet data contains fragments such as:

```text
{1t_w4s
D'3
rX
_34sy_t
;^?C
8
)
ZY<o
...
```

and later:

```text
picoCTF
bh_4r_9
g09
(:C
}
```

The fragments must be interpreted in their original packet order to reconstruct the complete flag.

The reconstructed flag is:

```text
picoCTF{1t_w4snt_th4t_34sy_tbh_4r_966d0bfb}
```

## Why This Works

The challenge hides information inside network packets.

The important observations are:

1. The file is a PCAP network capture.
2. Packet timestamps are important.
3. Packet payloads contain encoded data.
4. The payload is first represented as hexadecimal data.
5. The hexadecimal data can be converted back to bytes.
6. The resulting data contains Base64-encoded information.
7. The decoded fragments must be ordered using their timestamps.
8. Combining the fragments reveals the flag.

## Important Concepts

### PCAP

PCAP files contain captured network traffic.

They can be analyzed using:

```bash
wireshark myNetworkTraffic.pcap
```

or:

```bash
tshark -r myNetworkTraffic.pcap
```

### Packet Timestamps

Every captured packet has a timestamp.

The timestamps are especially important in this challenge because the hidden message is transmitted in pieces.

### Hexadecimal Data

Network packet payloads may be displayed as hexadecimal bytes.

For example:

```text
7069636f
```

can be converted back to bytes with:

```bash
echo "7069636f" | xxd -r -p
```

### Base64

After converting the packet data from hexadecimal, Base64-encoded fragments can be recovered.

Base64 can be decoded using:

```bash
echo "BASE64_DATA" | base64 -d
```

## Commands Used

Check the PCAP:

```bash
ls -lh myNetworkTraffic.pcap
```

Inspect packets:

```bash
tshark -r myNetworkTraffic.pcap
```

Extract timestamps and packet data:

```bash
tshark -r myNetworkTraffic.pcap -T fields -e frame.time_epoch -e data > packets.txt
```

Convert hexadecimal data:

```bash
echo "HEX_DATA" | xxd -r -p
```

Decode Base64:

```bash
echo "BASE64_DATA" | base64 -d
```

Process the packet data:

```bash
while read -r time hex; do
    decoded=$(echo "$hex" | xxd -r -p 2>/dev/null | base64 -d 2>/dev/null)
    [ -n "$decoded" ] && printf '%s\t%s\n' "$time" "$decoded"
done < packets.txt > decoded.txt
```

Search for the flag:

```bash
grep -i "picoCTF" decoded.txt
```

Show surrounding data:

```bash
grep -i -B 10 -A 20 "picoCTF" decoded.txt
```

## One-Line Solution

```bash
tshark -r myNetworkTraffic.pcap -T fields -e frame.time_epoch -e data > packets.txt && while read -r time hex; do decoded=$(echo "$hex" | xxd -r -p 2>/dev/null | base64 -d 2>/dev/null); [ -n "$decoded" ] && printf '%s\t%s\n' "$time" "$decoded"; done < packets.txt > decoded.txt && grep -i -B 10 -A 20 "picoCTF" decoded.txt
```

## Complete Solution

```text
Download myNetworkTraffic.pcap
        ↓
Inspect the PCAP
        ↓
Extract packet timestamps and data
        ↓
Convert hexadecimal payloads
        ↓
Decode Base64 fragments
        ↓
Keep packets in timestamp order
        ↓
Search for picoCTF
        ↓
Reconstruct the fragments
        ↓
Recover the complete flag
```

## Flag

```text
picoCTF{1t_w4snt_th4t_34sy_tbh_4r_966d0bfb}
```

## Tools Used

- Wireshark
- tshark
- Linux Terminal
- `xxd`
- `base64`
- `grep`
- PCAP analysis

## Key Learning

- PCAP files can contain hidden information inside packet payloads.
- Packet timestamps can be important when reconstructing transmitted data.
- Hexadecimal payloads can be converted back to raw bytes using `xxd`.
- Base64 data can be decoded using `base64 -d`.
- Network forensic investigations often require reconstructing data from multiple packets.
- Filtering and searching packet data makes PCAP analysis much easier.
- Always pay attention to challenge hints such as "Time is essential."

## Final Solution

```text
myNetworkTraffic.pcap
        ↓
tshark
        ↓
Packet timestamps + payload
        ↓
Hex decoding
        ↓
Base64 decoding
        ↓
Order fragments by time
        ↓
Search for picoCTF
        ↓
picoCTF{1t_w4snt_th4t_34sy_tbh_4r_966d0bfb}
```
