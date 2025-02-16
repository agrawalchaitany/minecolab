<p align="center"><a href="https://github.com/agrawalchaitany/minecolab"><img src="Minecolab_logo.png" alt="Logo" height="450" width="900"/></a></p>
<h1 align="center">MineColab</h1>
<p align="center">Run Minecraft Server on Google Colab</p>
<p align="left">This project sets up a <b>Minecraft server</b> (Paper, Fabric, Forge, or Purpur) on <b>Google Colab</b> using a cloud-based environment. The setup includes Google Drive integration for persistent storage, Java installation, and tunneling for external access.</p>
<a href="https://colab.research.google.com/github/agrawalchaitany/minecolab/blob/main/minecolab.ipynb" target="_parent"><img align="right" src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a>
<meta name="google-site-verification" content="jEiXc2hgMeg4S4JORpONRnUZ9HpwK_IsA45XV6Z7QVY" />


## 🙉 what is google Colab?
As the official FAQ says, colaboratory, or “Colab” for short, is a product from Google Research. Colab allows anybody to write and execute arbitrary python code through the browser, and is especially well suited to machine learning, data analysis and education. More technically, Colab is a hosted Jupyter notebook service that requires no setup to use, while providing free access to computing resources including GPUs.
In short, it is a vm provided for learning, running python code, machine learning or for general purpose.

---

## 💰 Is it really free to use?
Yes, Colab is free to use. But there are some points which, according to me one should keep in mind:
1. Though colab is a free service, it shouldn't be exploited indiscriminately or without any care. One should value that its a resource offered for no cost and can get depleted/restricted if the demand increases out of control.
2. If it isn't obvious, one shouldn't run mission-critical services (like large and important servers/databases/python programs) on it. Its resources are not guaranteed and not unlimited, and the usage limits sometimes fluctuate. Also, the notebook has a maximum runtime of 12 hours, after which, it should be manually restarted.
3. If you need to use it pretty often for intensive tasks, consider purchasing a vps server. A heavy increase in server load would force google to close the service.

---

## 📌 Features
- Supports multiple server types: **Paper, Fabric, Forge, and Purpur**
- **Google Drive integration** for persistent storage
- **Customizable server properties**
- **Tunneling support** to allow external connections

---

## 🔧 Setup Instructions

1. Open the notebook in google colab.
2. Read through the notebook, most of the code is self explanatory. Run the cells which are useful for your use-case.
3. Run the second cell which runs the Minecraft server.
4. Now you have two options. You can either use Nglocalhost or Pinggy. 
    - Nglocalhost:  Provides a simple and secure way to expose localhost to the internet with minimal setup.
    - Pinggy: Offers lightweight, fast, and disposable public URLs for quick local server exposure.
  * Nglocalhost:
      1. Change `tunnel_service` variable .
      2. You must register the fingerprint if you want a longer duration of tunneling service.
    
    **Note:** Please include only your key fingerprint. For example: `A27Jcya7a+IE+qAEUZBWExENVnwug0IWgskGqcH1zU0`.  
    - Do **not** include the "SHA256:" prefix or the "User@DESKTOP" suffix. 
    - Register the key [here](https://nglocalhost.com/subdomain/registration)  
    - The key can be found at the following location:
      ```
      /content/drive/My Drive/Minecraft-server/ssh_keys/id_rsa_fingerprint.txt
      ```
  * Pinggy:
      1. Change `tunnel_service` variable and follow the script.

---

## ⚡ So, how does it actually work?
As Google Colab is a VM running Ubuntu server as base OS, it can be easily used as a Minecraft server. Here are the steps which the notebook performs to setup the server:
1. Update the system's apt cache.
2. Install Openjdk-21 (Java) through apt-get.
3. Mount Google Drive to access the minecraft folder (Drive is used here to provide persistent storage).
4. Setup Nglocalhost or Pinggy Tunnel (Opening a tunnel at port 25565) depending on the `tunnel_service` variable.
5. Change directory to the minecraft-server folder on google drive ("Minecraft-server" is the default, located in the root directory of my Google Drive.)
6. List/Print the file list on the screen to indicate succesful directory change.
7. Startup the Minecraft server (with optimized JVM parameters from [Aikar's guide)](https://aikar.co/2018/07/02/tuning-the-jvm-g1gc-garbage-collector-flags-for-minecraft/)

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

## 👍 Tips
- If you are using server fabric , Most mods will also require you to install [Fabric API](https://www.curseforge.com/minecraft/mc-mods/fabric-api) into the mods folder.
- If Failed to download server , try reading [PaperMc](https://docs.papermc.io/misc/downloads-api) | [Fabricmc](https://fabricmc.net/use/server/)
- If Tunnel_service does not work, try reading [Nglocalhost](https://nglocalhost.com/) or [pinggy](https://pinggy.io/)
- Switch between the different tunnel providers and see which works best for you.
- Make regular backups of your world.
- For more Tunneling alternatives read [here](https://pinggy.io/blog/best_ngrok_alternatives/)
---

## 📜 License
This project is open-source. Feel free to modify and improve!

---

## 🙌 Credits
Developed by Chaitany Agrawal 🚀

