# LegoUniverseServer

## 0.0.0 Introduction
Lego Universe is a massively multiplayer online game (MMOG) that was developed by NetDevil and released by the LEGO Group in October 2010. As of 2012, the game was unfortunately shut down. Darkflame Universe (DLU) is a server emulator for LEGO® Universe. Development started in 2013 and has gone through multiple iterations and is now able to present a near-perfect emulation of the game server. For information about their licensing, please reference the following: https://github.com/DarkflameUniverse/DarkflameServer/blob/main/LICENSE

### 0.1.0 Requirements
### 0.1.1 Basic requirements
This tutorial will require patience, primarily because this process is tedious. It is **HIGHLY** recommended that you have some experience with the Linux CLI, although it isn't strictly necessary for this tutorial. 
### 0.1.2 Dealing with problems during setup
Depending on the unique state of your setup, you will have to navigate the Linux filesystem and mySQL database. But don't let this scare you! In this case, chatGPT, the Lego Universe discord (https://discord.com/invite/DBt9h8Q), and Google are your friends. Work slowly, and try to understand whatever problem you are having in the context of where you are within the setup process.
### 0.1.3 Recording
When setting up this server, you will need to create various usernames and passwords. For the love of God, write these down. **DO NOT** try to remember each and every one of your usernames and passwords.
### 0.1.4 Necessary Links:
#### DarkFlameUniverse
https://github.com/DarkflameUniverse/DarkflameServer/tree/main
#### NexusDashboard
https://github.com/DarkflameUniverse/NexusDashboard
#### Azure Cloud (Hosting Service)
https://azure.microsoft.com/en-us/products/virtual-machines/dedicated-host/ef_id=_k_7bb213f4da5918e0feba768f5aba644b_k_&OCID=AIDcmm5edswduu_SEM__k_7bb213f4da5918e0feba768f5aba644b_k_&msclkid=7bb213f4da5918e0feba768f5aba644b
#### PuTTY (SSH Client)
https://putty.org/

## 1.0.0 Azure Cloud
### 1.1.0 The Azure Cloud
Microsoft Azure, commonly referred to as Azure, is a cloud computing service created by Microsoft for building, testing, deploying, and managing applications and services through Microsoft-managed data centers. It provides a range of cloud services, including those for computing, analytics, storage, and networking. Users can pick and choose from these services to develop and scale new applications or run existing applications in the public cloud.
### 1.1.1 A note on cloud services
The Azure cloud environment provides a perfect space for hosting your DarkFlameUniverse server. Of course, this process can be replicated with any other cloud service. For the purpose of this tutorial, Azure Cloud will be the hosting platform.
### 1.1.2 Paywalls, student emails, and .edu
#### Student emails and .edu
If you are a student with a .edu email or are a university recognized by Azure Cloud Services, you can sign up for a **FREE** Azure Cloud Account. This allows you to host a number of resources at no cost for one year. This is, in essence, **VERY** nice. Please take advantage of this.
#### Paywalls
If you don't have a .edu account, don't worry. Simply sign up for an Azure Cloud account. For one year (from what I understand), you are given 200 Azure Cloud credits to work with each month. This will be the "currency" that is burned through when running your server.

## 2.0.0 Setting up your first VM
### 2.1.0 Selecting your resource
Once you have an Azure Cloud account, navigate to https://portal.azure.com/. Here, you will be faced with your Azure Portal homepage. From here, select **Create a resource** and then select **Virtual machine**. If **Virtual machine** isn't shown under **Popular Azure services**, in the search bar, to the right of **Get Started**, type and select **Virtual machine**.
### 2.2.0 VM Specifics
After selecting to create a virtual machine, you must select the specific characteristics of your machine. Blow and to the right of **Subscription** you will see a dropdown menu named **Resource Group**. Select **Create New** and name this something general and recognizable (I named mine DarkFlameUniverse). Next, to the right of **Virtual machine name**, create a name for your server. Scroll down to where you see the characteristic **Image** on the left-hand side of the screen. Select one of the Ubuntu server images. Under size, select any  size other than **Standard_B1s**. The default size doesn't have enough processing power to actively compile the game code. Continue to scroll down to where **Administrator account** is seen on the left-hand side of your screen. First, beside **Authentication type** select password. Next, type a username, and password, and confirm the password. **DO NOT FORGET THIS**, as this is the login you will use to SSH into your server. Finally, scroll to the top of the page and select **Networking**. On the left-hand side, you will see **Virtual network**. Here, select **Create new** and create a name for your network. Lastly, at the bottom, click **Review + create** and then **Create** to create your virtual machine.
### 2.3.0
Congratulations, you have just set up your first virtual machine!

## 3.0.0 Connecting to your server
### 3.1.0 Install PuTTY
Using the link provided above, download, install, and open the application PuTTY
### 3.1.0 Finding your server's public IP
At the Azure Portal Homepage, you will see a menu named **Resources**. For here, select your server by clicking on it's name (to the right of the name, it will have the **Type: Virtual machine**. Under the **Essentials** dropdown menu, you will see the descriptor **Public IP address**. Copy this IP address (it will look something like this, with different numbers: 123.456.789.123)
### 3.2.0 Connecting to your server
#### 3.2.1 Saving your session
Returning to your open instance of the PuTTY application, paste your server's IP into the box below **Host Name** (or IP address). From here, in the menu for **Load, save, or delete a stored session**, under **Saved Sessions**, type the name of your server and select the **Save** button. Doing this step stops you from manually having to type your IP into the **Host Name (or IP address)** box every time you want to connect to your server.
### 3.3.0 Connecting and logging in
Select **Open** near the bottom of the application. You will be greeted with an Ubuntu Command Line Interface. Upon your first connect, you will be greeted with a panel asking you if you are sure you'd like to connect. Please select **Accept**, allowing your device to trust this SSH connection now and in the future.
### 3.4.0
Congratulations, you are now connected to your server!

## 4.0.0 DarkFlame Setup 1
### 4.1.0 Important notes moving Forewards/Problems solved in this tutorial
At this point, it is possible to follow the tutorial provided in the Git Repository for the DarkFlameUniverse server. For the most part, this tutorial will guide you through a majority of the installation. But, there are a few problems that I ran into when going through said tutorial using an Azure VM. These problems (solved/attempted to being solved in this tutorial) are as follows.
#### 4.1.1 Virtual maching compute power isn't enough
**Solved** through an upgrade of compute powers i.e. using a vm of size > than Standard B1s.
#### 4.1.2 MySQL symlinks throwing errors during setup
error: sudo systemctl enable --now mysql Synchronizing state of mysql.service with SysV service script with /lib/systemd/systemd-sysv-install. Executing: /lib/systemd/systemd-sysv-install enable mysql Failed to enable unit: Refusing to operate on alias name or linked unit file: mysql.service.
**Solved** through the following commands: `sudo rm /etc/system/mysql.service` and `sudo rm /etc/system/mysqld.service`
#### 4.1.3 Being unable to sign into the game
This problem seemingly arises from the admin account (which is supposedly setup when running `./masterserver -a`) a. not being added to the MySQL database and subsequently not being recognized in the nexus dashboard. Or b. being put into the MySQL database, but not being "acitvated" on the NexusDashboard application. My solution changes the settings of both of these programs, rather than changing the code running them. Of course, this does make the server a bit more unsafe (i.e anyone can play because they don't need a playkey to register). This shouldn't be a problem though, as these servers are only meant for a few people to play on at a time.
**Solved** through editing the configuration of the Nexus Dashboard in `/NexusDashboard/app/settings.py`. Setting `USER_ENABLE_REGISTER = True` and  `REQUIRE_PLAY_KEY = False`. This allows individuals to register on the NexusDashboard without using a playkey. In tandem, going into `/DarkflameServer/build/authconfig.ini` and setting `dont_use_keys=1`. This allows the server to accept player accounts that haven't used playkeys to register
#### 4.1.4 The SQL database setup in the DarkflameServer not having the proper table columns
