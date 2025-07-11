# AWS IoT Setup for nRF9160 DK via SLM

> **Private Guide** — For internal use only
>
> Author: Hamas Saeed\
> Email: [[your-email@example.com](mailto\:your-email@example.com)]\
> GitHub: [https://github.com/yourgithub](https://github.com/yourgithub)

---

## 🧾 Prerequisites

- Nordic nRF9160 DK (connected via USB)
- nRF Connect for Desktop (Programmer & LTE Link Monitor installed)
- AWS IoT account with generated certificates (Root CA, device cert, private key)
- Prebuilt SLM firmware (`slm.hex`)
- Any serial terminal (e.g. LTE Link Monitor, Tera Term)

---

## ✅ Step 1: Flash SLM Firmware

1. Open **nRF Connect for Desktop** → Launch **Programmer**
2. Connect the **nRF9160 DK** via USB (J2)
3. Click **Erase All**
4. Click **Add HEX File** → Select `slm.hex`
5. Click **Write**

---

## ✅ Step 2: Test AT Commands

1. Launch **LTE Link Monitor** or any serial terminal
2. Set parameters:
   - Baud: `115200`
   - Line ending: `CR+LF`
   - Flow control: None
3. Type:
   ```
   AT
   ```
   ✅ Expected response: `OK`

---

## ✅ Step 3: Upload AWS Certificates

Use `AT#XCMNG` to upload files:

### 🔐 1. Upload Root CA

```bash
AT#XCMNG=0,0,"AmazonRootCA1.pem"
(paste certificate content)
(Ctrl+Z to finish)
```

### 🔐 2. Upload Device Certificate

```bash
AT#XCMNG=0,4,"device-cert.pem"
(paste certificate content)
(Ctrl+Z)
```

### 🔐 3. Upload Private Key

```bash
AT#XCMNG=0,5,"private-key.pem"
(paste key content)
(Ctrl+Z)
```

### 📋 Verify Stored Files

```bash
AT#XCMNG=2
```

### 🗑️ Delete a File (if needed)

```bash
AT#XCMNG=3,0,"filename.pem"
```

---

## ✅ Step 4: Configure and Connect to AWS IoT

### 1. Set AWS Endpoint

Get your endpoint from AWS IoT Console → **Settings**.

```bash
AT#XCLOUDCFG="AWS","<your-endpoint>"
```

Example:

```bash
AT#XCLOUDCFG="AWS","a1234567890-ats.iot.us-east-1.amazonaws.com"
```

### 2. Connect to AWS MQTT

```bash
AT#XMQTTCON=1
```

✅ Expected: `#XMQTTCON: 1`

### 3. Publish MQTT Message

```bash
AT#XMQTTPUB="iot/test/topic","Hello from SLM",0
```

### 4. Subscribe to Topic

```bash
AT#XMQTTSUB="iot/test/topic",0
```

✅ Messages will appear as:

```
#XMQTTMSG: "iot/test/topic","hello from AWS"
```

### 5. Unsubscribe / Disconnect

```bash
AT#XMQTTUNSUB="iot/test/topic"
AT#XMQTTCON=0
```

---

## 🚑 Troubleshooting

| Symptom             | Possible Fix                         |
| ------------------- | ------------------------------------ |
| No `OK` on `AT`     | Check serial COM port & line endings |
| `XMQTTCON: -1`      | Certs invalid or endpoint incorrect  |
| `XMQTTCON: -47`     | SIM not connected to LTE network     |
| No message received | Topic mismatch or publish error      |

---

## 📋 AT Command Cheat Sheet

```bash
# Upload certs
AT#XCMNG=0,0,"AmazonRootCA1.pem"
AT#XCMNG=0,4,"device-cert.pem"
AT#XCMNG=0,5,"private-key.pem"

# Verify certs
AT#XCMNG=2

# Set AWS endpoint
AT#XCLOUDCFG="AWS","<your-endpoint>"

# Connect & Publish
AT#XMQTTCON=1
AT#XMQTTPUB="iot/test","hello",0

# Subscribe / Unsubscribe
AT#XMQTTSUB="iot/test",0
AT#XMQTTUNSUB="iot/test"

# Disconnect
AT#XMQTTCON=0
```

---

## 🧾 License

Private documentation prepared by **Hamas Saeed** for AWS IoT client project using Nordic nRF9160 DK.

---

*End of Guide*

