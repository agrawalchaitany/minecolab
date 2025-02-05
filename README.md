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

#### **7. Connect to the Server**
Use a tunneling service such as **nglocalhost**:
```python
# 🌍 Setup tunneling service
tunnel_service = "nglocalhost"  # Can be changed to "playit.gg" if needed
print(f"\n🌍 Using {tunnel_service} for server tunneling...")

# 🔌 Establishing the tunnel
if tunnel_service == "nglocalhost":
    print("\n🔌 Setting up nglocalhost tunneling...")
        # Generate SSH key if missing
    if not os.path.isfile("/content/drive/My Drive/Minecraft-server/ssh_keys/id_rsa"):
        os.system("mkdir -p '/content/drive/My Drive/Minecraft-server/ssh_keys'")  # Create directory
        os.system("ssh-keygen -t rsa -b 4096 -f '/content/drive/My Drive/Minecraft-server/ssh_keys/id_rsa' -N '' -q")  # Generate SSH key
        os.system("chmod 400 '/content/drive/My Drive/Minecraft-server/ssh_keys/id_rsa'")  # Change permissions
        os.system("ssh-keygen -lf '/content/drive/My Drive/Minecraft-server/ssh_keys/id_rsa.pub' > '/content/drive/My Drive/Minecraft-server/ssh_keys/id_rsa_fingerprint.txt'" )

    # Establish tunnel
    print("⏳ Establishing tunnel, please wait...")
    os.system("nohup ssh -T -o StrictHostKeyChecking=no -i '/content/drive/My Drive/Minecraft-server/ssh_keys/id_rsa' -R minecraft03127:25565:localhost:25565 nglocalhost.com > tunnel_log.txt 2>&1 &")

    # Wait for tunnel to establish
    time.sleep(10)

    # Extract and display the tunnel URL
    with open("tunnel_log.txt", "r") as log_file:
        log_content = log_file.read()
        match = re.search(r"(nglocalhost[^\s]+)", log_content)  # Extract first URL
        if match:
            tunnel_url = match.group(0)
            print(f"✅ Tunnel established! Access your Minecraft server at:\n🌍 {tunnel_url}")
        else:
            print("⚠️ Could not extract tunnel URL. Check 'tunnel_log.txt' manually.")

else:
    print("⚠️ No tunneling service selected. Server will run locally.")
```
---

## 🔌 Register Key SHA256 Fingerprint

⚠️ **Important:** You must register the fingerprint if you want a longer duration of tunneling service.

**Note:** Please include only your key fingerprint. For example: `A27Jcya7a+IE+qAEUZBWExENVnwug0IWgskGqcH1zU0`.  
Do **not** include the "SHA256:" prefix or the "User@DESKTOP" suffix.

### Find Your Key:
- The key can be found at the following location:
  ```
  /content/drive/My Drive/Minecraft-server/ssh_keys/id_rsa_fingerprint.txt
  ```
- Register the key at:
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
- **Nglocalhost not working?** Restart Colab and rerun the script.
- **High latency?** Google Colab may throttle resources.

---

## 📜 License
This project is open-source. Feel free to modify and improve!

---

## 🙌 Credits
Developed by Chaitany Agrawal 🚀

