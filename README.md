# ProxMox Portainer Install Guide

## Description
In this guide, I will provide a step-by-step walkthrough on how to install **Portainer** inside a virtual machine (VM) on your ProxMox system.

---

## Program Walk-through

1. **Log into your ProxMox homepage.**

2. **Download an ISO file** from a trusted source (e.g., Ubuntu Server, Debian) or upload an existing ISO file from your own machine.

   - **If downloading an ISO file**:
     - Click on the ProxMox node in the left sidebar.
     - Navigate to `local` → `ISO Images`.
     - Click **Download from URL** and paste the ISO URL.
   
   - **If uploading an existing ISO file**:
     - Navigate to `local` → `ISO Images`.
     - Click **Upload**, then select the ISO from your computer.

3. **Click "Create VM"** in the top-right corner of the ProxMox dashboard.

4. **Follow the VM creation wizard** and adjust settings based on your needs.

   - **Page 1 – General**:
     - Enter a name for your VM (e.g., `portainer-vm`).
     - Set the VM ID or leave it as default.

   - **Page 2 – OS**:
     - Choose the ISO image you uploaded/downloaded.
     - Select the correct guest OS type.

   - **Page 3 – System**:
     - Choose BIOS (typically `OVMF` or `SeaBIOS`).
     - Leave other settings as default unless you need special hardware options.

   - **Page 4 – Hard Disk**:
     - Choose your storage location (e.g., `local-lvm`).
     - Set the disk size (e.g., 10GB or more depending on container usage).

   - **Page 5 – CPU**:
     - Allocate CPU cores (1–2 is fine for most basic Portainer setups).

   - **Page 6 – Memory**:
     - Allocate RAM (1–2GB minimum recommended).

   - **Page 7 – Network**:
     - Attach to bridge `vmbr0`.
     - Leave model as `VirtIO` or use `E1000` if needed.

5. **Click "Finish"** to create the VM.

6. **Start the VM** and install your chosen OS (e.g., Ubuntu Server).

7. Once inside the VM:
   - Update packages:  
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```
   - Install Docker:  
     ```bash
     curl -fsSL https://get.docker.com -o get-docker.sh
     sudo sh get-docker.sh
     ```
   - Run Portainer container:
     ```bash
     sudo docker volume create portainer_data
     sudo docker run -d -p 8000:8000 -p 9443:9443 \
       --name portainer --restart=always \
       -v /var/run/docker.sock:/var/run/docker.sock \
       -v portainer_data:/data \
       portainer/portainer-ce:latest
     ```

8. Access Portainer via your browser:
      https://<your-vm-ip>:9443
   
