# Deploy a Secret Node
Guide Version 0.60 | Date Feb 2nd, 2020 | Discovery Testnet Beta

!> If you are reading this, you are a brave beta tester! While you cannot launch a Secret Node today, you can prepare your SGX compatible machine to run Nodes. Also please understand this guide was tested on a NUC 8i7BEK, NUC 8i3BEH, and only with the stock Ubuntu Server 18.04 LTS ISO. If you use anything else your results may differ. Please report any issues [here](https://github.com/secretnodes/cookbook/issues).

!> All tokens discussed here are test tokens on the Kovan network. Do not send any real tokens using this guide!

[![asciicast](https://asciinema.org/a/IkNGIBYYU1qlFtQNaeaRFaiEW.svg)](https://asciinema.org/a/IkNGIBYYU1qlFtQNaeaRFaiEW)

**Estimated Time | 20 - 35 Minutes depending on experience.**

# Intel NUC Overview

The "Next Unit of Computing (NUC)" is a line of small-form-factor barebone computer kits designed by Intel. While [Enigma](https://enigma.co) has [partnered with Intel](https://blog.enigma.co/announcing-enigmas-collaboration-with-intel-43bbf73a86a7) to work on SGX integration, it is not a requirement that you use an Intel NUC for your Secret Node.

**Devices & Cloud Providers Confirmed Compatible**
1. `Intel NUC 8i7BEK`, `NUC8I3BEK`.

Secretnodes.org Recommended Models
8i3BEH, 8i5BEH, 8i7BEH.

Please report your compatible device or provider by opening an issue [here](https://github.com/secretnodes/cookbook/issues) and we'll add it to the list.

# Part 1 - Enabling SGX & Disabling Secure boot in the BIOS

1. When booting your NUC press the "F2" key.

2. Navigate to "Advanced" > "Security" > "Security Features" > "Intel Software Guard Extension (SGX)" and toggle it to "Enabled".

3. Navigate to "Advanced" > "Boot" > "Security Boot" > "Secure Boot" and uncheck the box if it's checked.

4. Press "ESC" key & save be sure to save your settings.

!> Note: We cannot maintain guides for each and every BIOS menu. Try searching for the term "SGX" or "Secure Boot" in addition to the computer you have and find out how to enable SGX and disable "Secure Boot" in the BIOS.

# Part 2 - Installing Ubuntu Server 18.04 LTS
The first step to configuring a Secret Node is making sure your hardware or VPS is running Ubuntu Server 18.04 LTS. If you already have Ubuntu installed but it is not Ubuntu Server 18.04 LTS or you are unsure if it's a vinalla install then proceed with the understanding that this guide should still work but you may need to do additional work that's outside the scope of this guide. If you're using Windows, OSX, or Linux and want general guidance on how to create a flash drive you can use to install Ubuntu, then we recommend the following.
1. First [Download Ubuntu Server 18.04 LTS ISO](https://ubuntu.com/download/server/thank-you?version=18.04.3&architecture=amd64)
2. Download and install [this tool](https://www.balena.io/etcher/) to create the bootable Ubuntu Installer.
3. Run the Etcher software.
4. In the Etcher software for the option "Select Image", Please select the Ubuntu ISO you downloaded.
5. Now in Etcher for the "Select target" option, please select the flash drive you want to turn into a bootable Ubuntu Installer.
6. Lastly, in Etcher click the "Flash" button.
7. You now have a USB bootable Ubuntu Server 18.04 Installer.

## Tell your NUC to boot from your USB drive.

If you created a USB bootable Ubuntu Installer and want to tell your NUC to boot from the newly created drive do as follows.

1. Press "F2" while booting your NUC to enter the bios.
2. Select your newly created flash drive in the "Boot Order"
3. Select "No" when asked if you want to save changes.

> If you can't find this in your bios, just search the term "boot".

# Part 3 - Remote into your Secret Node

If you wish to remotely manage your NUC from another machine within your network, here's how you can.

## Windows Users
If you're using Windows 10 64-bit or newer, launch PuTTY then enter the IP address of your Secret Node into the "Host Name" field. Then click open. This will allow you to remote administrate your Secret Node from a windows machine.

If you don't already have putty then download it.
* Go to [Putty.org](https://www.putty.org/)
* Click "here" where it says "You can download PuTTY here."
* Below the "Package Files" section, see "MSI (‘Windows Installer’)" and download the most recent 64-bit version of Putty.
* Run the Installer.
* Run Putty.
* Enter the IP address of your Secret Node into the "Host Name" field. Then click open.

> NOTE: If asked to add an ECDSA fingerprint, answer yes.

## OSX & Linux Users
On OSX, or Linux open Terminal and login to your node. (Skip if using Windows.)
 This will allow you to remote administrate your Secret Node from an Apple or Linux machine.

* Open a terminal session.
```bash
ssh root@<your-nodes-ip>
```
> NOTE: (1) Be sure to replace <your-nodes-ip> with your nodes ip address. (2) If asked to add an ECDSA fingerprint, answer yes.

# Part 4 - Package Installation and Initial Configuration

Here we will download everything required to run a secret node. That includes scripts to; install Docker, docker compose, the Intel SGX Driver, parity node software for kovan ETH node, and various other scripts to help empower you as a node runner. Please pay attention as the script runs, read any notices it posts, and respond "y" or "yes" to all prompts.

1. Download our node provision script.
```bash
wget https://raw.githubusercontent.com/secretnodes/cookbook/master/provision.sh
```

2. Run the secretnodes.org provision script. Be sure to respond with "y" or "yes" to all inquiries.
```bash
sudo bash provision.sh
```
3. Run the following command:
```bash
sudo gpasswd -a $USER docker
```

4. Run the following command:
```bash
newgrp docker
```

5. Run the following command:
```bash
docker ps
```

!> If the command returns a permission error, then reboot your machine and try again from step 3.

# Part 5 - Configure and launch a Kovan ETH node.

Leave the previous scripts open and running then open a new terminal window and run this script in it to configure a koven ETH node.

```bash
bash eth-kovan.sh
```

# Administer Your Node
How to start a stopped ETH Kovan node

Run the following command:
```bash
bash eth-start.sh
```

Please report any issues on github by clicking "New Issue" [here](https://github.com/secretnodes/cookbook/issues). Be sure to share any errors you are encountering. This has only been tested on an Intel NUC 8i3BEH & 8i7BEK.
