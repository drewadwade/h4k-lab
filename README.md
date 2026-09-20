# H4K Student Lab

This repository contains the setup information for the H4K Web Application and Server Vulnerabilities labs.

The lab environment runs:

- **DVWA** — the deliberately vulnerable web application used in the exercises
- **Kali Linux** — provides the command-line security tools used in the exercises
- **MariaDB** — provides DVWA's database

You will also use **Wireshark installed directly on your laptop** for the packet-capture exercises.

---

## 1. Download the H4K lab

Download the current lab package here:

**https://github.com/drewadwade/h4k-lab/releases/latest/download/H4K-Lab.zip**

Extract `H4K-Lab.zip` somewhere easy to find, such as your Desktop or Downloads folder.

Do **not** use GitHub's **Code → Download ZIP** option. Use the link above.

---

## 2. Install Docker Desktop

The H4K lab runs inside Docker containers.

If Docker Desktop is already installed, you can skip to the next section.

Download Docker Desktop from:

**https://www.docker.com/products/docker-desktop/**

Install the version appropriate for your computer.

### Windows

Docker Desktop may need to install or update **Windows Subsystem for Linux (WSL)**.

Follow any prompts from the Docker installer. Windows may require a restart before Docker will work.

### macOS

Choose the Docker Desktop download appropriate for your Mac:

- Apple silicon
- Intel

After installation, open **Docker Desktop** and wait until it reports that Docker is running.

The H4K launcher will also detect if Docker is missing and direct you to the installation instructions.

---

## 3. Install Wireshark

The first H4K lab uses **Wireshark on your own laptop** to capture network traffic.

Wireshark does **not** run inside the Kali container.

Download Wireshark from the official site:

**https://www.wireshark.org/download.html**

### Windows

1. Download the Windows installer appropriate for your computer.
2. Run the installer.
3. Use the normal/default installation options.
4. When the installer offers to install **Npcap**, leave it selected.
5. Finish the installation and start Wireshark.

Npcap provides the packet-capture support Wireshark needs on Windows.

For the first lab, you will need to capture traffic on your computer's **loopback interface**. On Windows this may appear as an Npcap loopback adapter or a similarly named loopback interface rather than `lo0`.

### macOS

1. Download the installer appropriate for your Mac.
2. Open the downloaded disk image.
3. Install Wireshark.
4. Also install **ChmodBPF** from the Wireshark disk image.
5. Start Wireshark.

ChmodBPF gives Wireshark the permissions it needs to capture network traffic.

On macOS, the loopback interface used in the first lab is normally:

`lo0`

### Check Wireshark before continuing

Make sure you can:

- open Wireshark;
- see a list of network interfaces;
- start a packet capture;
- stop a packet capture.

If Wireshark cannot see or capture from your network interfaces, ask an instructor before continuing.

---

## 4. Start the H4K lab

Make sure **Docker Desktop is running** first.

Open the extracted `H4K-Lab` folder.

### Windows

Double-click:

`START-H4K.cmd`

### macOS

Open **Terminal**.

Type:

```text
bash 
```

Include the space after `bash`.

Then:

1. Drag `START-H4K.command` from Finder into the Terminal window.
2. Press **Return**.

---

## 5. First launch

The launcher will:

1. check that Docker is available;
2. download the H4K Kali, DVWA, and database images if necessary;
3. start the lab environment;
4. wait for DVWA to become available;
5. open DVWA in your browser;
6. open a Kali Linux shell.

The **first launch can take several minutes** because Docker has to download the lab environment.

Later launches should be much faster because the downloaded images remain on your computer.

If the download fails, check your Internet connection and run `START-H4K` again.

---

## 6. DVWA

When the lab is ready, DVWA is available at:

**http://localhost:8080**

If DVWA displays its setup page, choose:

**Setup DVWA → Create / Reset Database**

Then log in using:

**Username:** `admin`  
**Password:** `password`

Unless a lab tells you otherwise, make sure the DVWA **Security Level** is set to:

**Low**

---

## 7. Kali Linux

The H4K launcher opens a Kali Linux command-line shell.

The lab exercises use Kali for tools such as:

- Hydra
- John the Ripper
- Nmap
- Nikto
- `dig`
- Nano
- other Linux command-line utilities

The `rockyou.txt` password list used by the exercises is already available at:

```text
/rockyou.txt
```

You do **not** need to install Kali tools yourself.

To leave the Kali shell, type:

```text
exit
```

Leaving the shell does not stop the Docker lab.

You can run `START-H4K` again to reopen it.

---

## 8. Wireshark and the first lab

Wireshark runs on your **normal Windows or macOS desktop**, not inside Kali.

The first exercise captures HTTP traffic between your browser and DVWA.

When instructed to capture local traffic:

- **macOS:** select `lo0`
- **Windows:** select the loopback/Npcap loopback interface

Later in the exercise you will switch Wireshark to your normal Wi-Fi interface.

Only capture network traffic when instructed to do so.

---

## 9. Stop the lab

When you are finished, use the stop launcher in the H4K folder.

### Windows

Double-click:

`STOP-H4K.cmd`

### macOS

From Terminal, run the same way as the start script:

```text
bash 
```

then drag:

`STOP-H4K.command`

into the Terminal window and press **Return**.

Stopping the lab does **not** delete the downloaded Docker images.

---

## 10. Reset the lab

If the lab environment becomes badly broken, or your instructor tells you to start again, use:

### Windows

`RESET-H4K.cmd`

### macOS

Run:

`RESET-H4K.command`

through Terminal using the same `bash ` + drag-and-drop method described above.

Resetting removes the DVWA database and creates a clean lab environment.

Your downloaded Docker images are retained, so they do not normally need to be downloaded again.

---

## Troubleshooting

### Docker is installed but the launcher says it is not running

Open **Docker Desktop** manually.

Wait until Docker reports that its engine is running, then run `START-H4K` again.

### Windows reports a WSL problem

Follow the Windows/Docker instructions to install or update WSL.

A Windows restart may be required.

If this happens during class, tell an instructor rather than spending the lab troubleshooting it on your own.

### The lab images will not download

Check that you have working Internet access and run `START-H4K` again.

If several students are seeing the same problem, tell the instructor.

### DVWA does not open

The normal address is:

**http://localhost:8080**

If the launcher reports an error, leave the error window open and show it to an instructor.

### Wireshark cannot capture traffic

Check that:

- Windows installed **Npcap** with Wireshark; or
- macOS installed **ChmodBPF** from the Wireshark installer.

If it still does not work, ask an instructor.

### Something in DVWA no longer works

Check:

1. that you typed the command or input exactly as shown;
2. that the DVWA Security Level is set to **Low**, unless the exercise tells you to change it.

If necessary, use `RESET-H4K` to recreate a clean DVWA environment.

---

## Safety

DVWA is deliberately vulnerable and the Kali environment contains real security-testing tools.

Use these tools **only against the H4K lab environment or another system your instructor has explicitly authorized you to test**.

Do not use Wireshark to capture other people's network traffic and do not use the Kali tools against systems, websites, or networks that you do not have permission to test.
