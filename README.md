# Minecraft Server on Google Colab

This project sets up a **Minecraft server** (Paper, Fabric, Forge, or Purpur) on **Google Colab** using a cloud-based environment. The setup includes Google Drive integration for persistent storage, Java installation, and tunneling for external access.

---

## 📌 Features
- Supports multiple server types: **Paper, Fabric, Forge, and Purpur**
- **Google Drive integration** for persistent storage
- **Customizable server properties**
- **Tunneling support** to allow external connections

---

## 🔧 Setup Instructions

### 1️⃣ Prerequisites
- A **Google account** to use Google Colab
- A **Minecraft server JAR file** (Paper, Fabric, Forge, or Purpur)
- **Google Drive storage** (optional but recommended)

### 2️⃣ Steps to Run the Server
#### **1. Open Google Colab**
- Navigate to [Google Colab](https://colab.research.google.com/)
- Upload the Python script containing the server setup

#### **2. Mount Google Drive (Optional, but recommended)**
```python
from google.colab import drive
drive.mount('/content/drive')
```
This ensures that server data is saved persistently.

#### **3. Install Java**
```python
!apt-get update && apt-get install openjdk-17-jdk -y
```

#### **4. Download and Set Up the Server**
Choose the server type:
```python
!wget -O server.jar <URL_TO_SERVER_JAR>
```
For example, for **Paper**:
```python
!wget -O server.jar https://papermc.io/api/v2/projects/paper/versions/1.20.1/builds/latest/downloads/paper-1.20.1.jar
```

#### **5. Accept EULA**
```python
echo 'eula=true' > eula.txt
```

#### **6. Start the Server**
```python
!java -Xms1G -Xmx2G -jar server.jar nogui
```

#### **7. Generate SSH key if missing**
```python
key_path = "/content/drive/My Drive/Minecraft-server/ssh_keys/id_rsa"
def generate_ssh_key(key_path):
    
    if not os.path.isfile(key_path):
        os.makedirs(os.path.dirname(key_path), exist_ok=True)
        os.system(f"ssh-keygen -t rsa -b 4096 -f '{key_path}' -N '' -q")
        os.system(f"chmod 400 '{key_path}'")
        os.system(f"ssh-keygen -lf '{key_path}.pub' > '{key_path}_fingerprint.txt'")
```
---

#### **8. Connect to the tunnel**
Use a tunneling service such as **nglocalhost** , **pinggy**:
```python
# 🌍 Setup tunneling service
tunnel_service = "nglocalhost"  # Can be changed to "pinggy" if needed
print(f"\n🌍 Using {tunnel_service} for server tunneling...")

# 🔌 Establishing the tunnel
if tunnel_service == "nglocalhost":    
    
    # ⏳ Generate SSH key if missing
    generate_ssh_key(key_path)

    # Establish tunnel
    print("⏳ Establishing tunnel, please wait...")
    os.system("nohup ssh -T -o StrictHostKeyChecking=no -i '/content/drive/My Drive/Minecraft-server/ssh_keys/id_rsa' -R minecraft03127:25565:localhost:25565 nglocalhost.com > tunnel_log.txt 2>&1 &")
    url_pattern=r"(nglocalhost:[^\s]+)"
    # Wait for tunnel to establish
    time.sleep(10)

elif tunnel_service == "pinggy":    

    # ⏳ Generate SSH key if missing
    generate_ssh_key(key_path)

    # Establish tunnel
    print("⏳ Establishing tunnel, please wait...")
    os.system("nohup ssh -p 443 -o StrictHostKeyChecking=no -o ServerAliveInterval=30 -R0:127.0.0.1:25565 tcp@ap.a.pinggy.io > tunnel_log.txt 2>&1 &")
    url_pattern=r"(tcp://[^\s]+)"
    # Wait for tunnel to establish
    time.sleep(10)

else:
    print("⚠️ No tunneling service selected. Server will run locally.")
```
---

## 🔌 Register Key SHA256 Fingerprint

⚠️ **Important:** You must register the fingerprint if you want a longer duration of tunneling service on **nglocalhost**.

**Note:** Please include only your key fingerprint. For example: `A27Jcya7a+IE+qAEUZBWExENVnwug0IWgskGqcH1zU0`.  
Do **not** include the "SHA256:" prefix or the "User@DESKTOP" suffix.

### Find Your Key:
- The key can be found at the following location:
  ```
  /content/drive/My Drive/Minecraft-server/ssh_keys/id_rsa_fingerprint.txt
  ```
- Register the key at if tunnel_service is **nglocalhost** :
  ```
  https://nglocalhost.com/subdomain/registration
  ```
---

## 🎮 Server Configuration
You can edit `server.properties` to customize settings like:
- `difficulty=hard`
- `max-players=10`
- `online-mode=false` (for cracked servers)
- `server-port=25565`(⚠️must)
---

## ❓ Troubleshooting
- **Server not starting?** Check Java version and server logs.
- **Tunneling not working?** Restart Colab ,rerun the script and check tunnel_log.txt.
- **High latency?** Google Colab may throttle resources.
- **Note:** tunnel_log.txt can be found at following location:
  ```
  /content/drive/My Drive/Minecraft-server/tunnel_log.txt
  ```
---

## 📜 License
This project is open-source. Feel free to modify and improve!

---

## 🙌 Credits
Developed by Chaitany Agrawal 🚀

