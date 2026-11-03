<div align="center">

# 🔐 Wireshark SOC Lab Projects

**Building practical SOC investigation skills through packet capture and network traffic analysis**

![Wireshark](https://img.shields.io/badge/Tool-Wireshark-1679A7?style=flat-square&logo=wireshark&logoColor=white)
![Networkwalks](https://img.shields.io/badge/Training-Networkwalks-C00000?style=flat-square)
![Labs](https://img.shields.io/badge/Labs-3–8-6f42c1?style=flat-square)
![Blue Team](https://img.shields.io/badge/Skill-Blue_Team-404040?style=flat-square&labelColor=C00000)
![TCP Analysis](https://img.shields.io/badge/Skill-TCP_Analysis-404040?style=flat-square&labelColor=C00000)
![Security Analysis](https://img.shields.io/badge/Skill-Security_Analysis-E87500?style=flat-square)

</div>

---

## 📌 Project Overview


The exercises developed my ability to examine captured traffic, identify communicating systems, reconstruct conversations, investigate authentication activity and explain the security implications of exposed data.


> **Evidence note:** Scores and completed tasks are documented in the result images. Example filters and analysis guidance below support future practice; original captures and exact investigation answers are not included.

---

## 🎯 Objectives

The main objectives of these projects were to:

- Capturing and filtering network traffic.
- Identifying source and destination addresses and ports.
- Interpreting TCP connection behaviour.
- Following streams to reconstruct application activity.
- Investigating exposed credentials and repeated login attempts.
- Examining VoIP signalling and media.
- Connecting technical observations to security recommendations.

---

## 🛡️ Purpose of the Labs

These exercises provide practical experience in examining suspicious network activity and explaining findings from packet evidence. They cover:

- Network reconnaissance and port scanning.
- Reconstruction of application conversations.
- Exposure of sensitive data in unencrypted traffic.
- Investigation of repeated authentication attempts.
- VoIP call analysis and recovery of lab audio.

## ⚙️ Project Configuration

| 🧩 Component | ⚙️ Details |
| --- | --- |
| Analysis tool | Wireshark |
| Training provider | Networkwalks |
| Participant | Tebogo Mokone |
| Projects | Wireshark SOC Labs 3–8 |


---

## 🏆 Assessment Results

| Lab | Investigation | Result |
| --- | --- | --- |
| 3 | Identify the Attacker’s IP | 100/100 |
| 4 | Follow TCP Stream | 100/100 |
| 5 | Follow TCP Stream, Part 2 | 100/100 |
| 6 | HTTP Plaintext Credentials | 100/100 |
| 7 | Brute Force Catch | 100/100 |
| 8 | VoIP Call Replay | 100/100 |
| **Total** | **Six assessments** | **600/600 — 100%** |

---

# 🪜 Lab Investigation Procedures

## 🌐 Lab 3 — Identify the Attacker’s IP

### 🎯 Objective

Investigate scanning activity and determine which system initiated it, which system was targeted and how the target responded.

### 🪜 Investigation Tasks Completed

- Captured the traffic and isolated SYN packets.
- Identified the attacker and target IP addresses.
- Identified the source port and destination ports probed.
- Determined which ports were open and closed.
- Examined replies associated with closed ports.
- Identified the attack type in the exercise.

### 🔎 Analysis Approach & Example Filters

A useful starting point is to isolate initial TCP connection attempts, then compare source addresses, destination addresses and destination ports. Repeated probes to different ports on one destination can help identify a scanning pattern.

Suggested display filters:

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

Shows initial SYN packets without the ACK flag.

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 1
```

Shows SYN-ACK responses, which can indicate that a TCP port is accepting connections.

```wireshark
tcp.flags.reset == 1
```

Shows reset packets. A reset in response to a connection attempt can indicate a closed port; interpret it alongside packet direction and the surrounding exchange.

### 💡 What I Learned

- Source and destination fields establish the direction of an exchange.
- TCP flags help explain how a system responds to probes.
- A missing response alone is insufficient to classify a port as closed.
- Identifying scanning behaviour requires a pattern of evidence across packets.

**Result: 100/100**

![Lab 3 assessment result](Lab%203%20Result%20-%20Tebogo%20Mokone.png)

---

## 💬 Lab 4 — Follow TCP Stream

### 🎯 Objective

Reconstruct a chat conversation and identify the participants, connection details and sensitive information exchanged.

### 🪜 Investigation Tasks Completed

- Captured the traffic and identified the chat port.
- Determined who initiated the conversation.
- Identified the TCP stream number and chat size.
- Identified who needed help and the drive discussed.
- Located a temporary password and guest Wi-Fi password.
- Identified the text colour requested by the assessment.

### 🔎 Analysis Approach & Example Filters

Select a packet from the relevant connection and use **Follow → TCP Stream**. Reviewing both directions of the stream provides conversational context and makes it easier to distinguish questions from responses.

Suggested filter template:

```wireshark
tcp.stream eq <stream_number>
```

Replace `<stream_number>` with the stream index found in the capture. This limits the view to one TCP conversation.

### 💡 What I Learned

- Stream reconstruction makes application conversations easier to interpret than isolated packets.
- Unencrypted chat may expose credentials and internal operational information.
- Temporary passwords still require protection.
- A stream index identifies a conversation within that capture; it is not a network port.

**Result: 100/100**

![Lab 4 assessment result](Lab%204%20Result%20-%20Tebogo%20Mokone.png)

---

## 🧩 Lab 5 — Follow TCP Stream, Part 2

### 🎯 Objective

Trace web activity and reconstruct the exchanges needed to locate information, identify a download and understand its context.

### 🪜 Investigation Tasks Completed

- Captured traffic and identified the relevant pages.
- Located the hidden information and associated stream.
- Identified the code word and backup server.
- Determined the direction of the relevant exchange.
- Identified the status code and downloaded file.
- Identified the report author.

### 🔎 Analysis Approach & Example Filters

Review the relevant web requests and responses, then follow the corresponding TCP streams. Connect the requested pages, response codes and transferred content to build a consistent account of the activity.

Suggested filters for readable HTTP traffic:

```wireshark
http.request
```

```wireshark
http.response
```

```wireshark
tcp.stream eq <stream_number>
```

Where applicable, **File → Export Objects → HTTP** can help inspect objects transferred in the capture. Availability depends on the captured content and protocol decoding.

### 💡 What I Learned

- Requests and responses should be examined together.
- Transfer direction helps distinguish information sent by the client from content returned by the server.
- Status codes provide context, but content is needed to explain what was transferred.
- Reconstructing an activity sequence can reveal relationships that individual packets do not show.

**Result: 100/100**

![Lab 5 assessment result](Lab%205%20Result%20-%20Tebogo%20Mokone.png)

---

## 🔓 Lab 6 — HTTP Plaintext Credentials

### 🎯 Objective

Investigate a login exchange and understand how credentials become visible when transmitted without transport encryption.

### 🪜 Investigation Tasks Completed

- Captured traffic and identified the protocol and request method.
- Located the login page and identified the client that signed in.
- Counted the login attempts.
- Identified the username and password in the lab traffic.
- Located the relevant packet-detail line.
- Identified the corrective measure requested by the exercise.

### 🔎 Analysis Approach & Example Filters

Locate login requests, inspect their headers and body, and follow the relevant stream. Link each attempt to its source and response rather than treating every packet as a separate login.

Suggested filters:

```wireshark
http.request
```

```wireshark
http.request.method == "POST"
```

The second filter is useful when the login uses POST; the assessment sheet does not disclose the actual method used.

### 💡 What I Learned

- Plain HTTP can expose credentials to an observer who can capture the traffic.
- POST places data in a request body but does not encrypt it.
- HTTPS with correctly configured TLS protects credentials in transit.
- Packet references make findings easier to verify and explain.

**Result: 100/100**

![Lab 6 assessment result](Lab%206%20Result%20-%20Tebogo%20Mokone.png)

---

## 🚨 Lab 7 — Brute Force Catch

### 🎯 Objective

Investigate repeated authentication attempts and distinguish failed attempts from a successful login in the lab scenario.

### 🪜 Investigation Tasks Completed

- Captured traffic and identified the protocol.
- Identified the attacker and target.
- Identified the username being attempted.
- Identified the failure code and counted failed logins.
- Identified the success code and password that worked.
- Classified the attack type.

### 🔎 Analysis Approach & Example Filters

Narrow the capture to the relevant endpoints and authentication service. Compare requests and responses, count distinct login attempts, and examine whether repeated failures are followed by an application-level success response.

Suggested filter templates:

```wireshark
ip.addr == <attacker_ip> && ip.addr == <target_ip>
```

```wireshark
tcp.stream eq <stream_number>
```

Replace the placeholders with values from the capture. The correct protocol filter and response-code interpretation depend on the service under investigation.

### 💡 What I Learned

- Repeated failures followed by a success deserve investigation.
- Protocol response codes help distinguish authentication outcomes.
- TCP retransmissions should not be counted as additional login attempts.
- Rate limiting, MFA and authentication monitoring can help reduce brute-force risk, depending on the service.

**Result: 100/100**

![Lab 7 assessment result](Lab%207%20Result%20-%20Tebogo%20Mokone.png)

---

## 📞 Lab 8 — VoIP Call Replay

### 🎯 Objective

Investigate a VoIP call, identify its signalling and media details, and understand the exposure created by recoverable voice content.

### 🪜 Investigation Tasks Completed

- Captured the call traffic.
- Identified the call setup and voice protocols.
- Identified the caller extension and name.
- Identified the codec and who answered.
- Recovered the card number and expiry date used in the lab conversation.
- Identified the corrective measure requested by the assessment.

### 🔎 Analysis Approach & Example Filters

Wireshark's **Telephony → VoIP Calls** and **Telephony → RTP → RTP Streams** views can assist with supported captures. Inspect call setup separately from media traffic, then use playback where the capture and codec support it.

Common starting filters, where those protocols are present:

```wireshark
sip
```

```wireshark
rtp
```

These are investigation suggestions; the result sheet does not reveal the actual protocol names or codec selected as answers.

### 💡 What I Learned

- Call signalling and voice media have different roles and should be examined separately.
- Recoverable unencrypted audio can disclose sensitive information.
- Media protection such as SRTP and signalling protection such as TLS address different parts of a VoIP session.
- Recovered personal and payment information should be handled carefully and excluded from public write-ups.

**Result: 100/100**

![Lab 8 assessment result](Lab%208%20Result%20-%20Tebogo%20Mokone.png)


---

# 🐞 Problems Encountered & Investigation Checks

The result sheets do not record troubleshooting incidents. The following checks are useful when repeating these investigations:

| Investigation issue | Check |
| --- | --- |
| Filter returns no packets | Check the selected capture, protocol decoding and filter values. |
| Conversation appears incomplete | Confirm the stream and whether both directions were captured. |
| Login count seems inflated | Separate distinct attempts from retransmissions. |
| Content is unreadable | Establish whether encryption prevents payload inspection. |
| VoIP playback is unavailable | Check whether media packets are captured and the codec is supported. |

---

# 💡 What I Learned

These labs strengthened my ability to move from captured packets to a clear explanation of network activity. The central lesson was to connect endpoints, protocol behaviour, application content and outcomes before drawing a conclusion.

The exercises also reinforced that identifying an issue is only part of an investigation: findings should explain the exposure and connect it to a practical corrective action.


---

# 🔐 Security & Ethical Use

These exercises were completed for training. Traffic capture and analysis should be limited to authorized environments. Passwords, payment information and private conversations recovered during exercises are intentionally omitted from this public documentation.

---

# 🔗 Tools & Resources

Thank you to **Networkwalks** and **[Waqas Karim (CCIE)](https://www.linkedin.com/in/waqaskarim/)** for the practical learning resources and guidance.

- [Networkwalks](https://www.networkwalks.com/)
- [Wireshark](https://www.wireshark.org/)

---

# 👤 Author

**Tebogo Mokone**  
[LinkedIn — MokoneTebogo](https://www.linkedin.com/in/tebogo-mokone-ba2b35261/)


---

## 📌 Project Information

**Programme:** Cybersecurity at Networkwalks | **Projects:** Wireshark SOC Labs 3–8 | **Repository:** GitHub
