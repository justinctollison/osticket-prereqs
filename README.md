<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

# osTicket - Prerequisites and Installation

In this tutorial, I’ll go through the full process of setting up **osTicket**, a help desk ticketing system, on a **Windows 10 Virtual Machine** using **Microsoft Azure**. This includes creating the Virtual Machine, installing the necessary dependencies like **IIS**, **PHP**, and **MySQL**, and configuring everything to get osTicket up and running.

The goal is to build a working help desk environment from scratch so you can get hands-on experience with how ticketing systems are installed and managed. By the end, you’ll have a complete osTicket setup that you can log into, create tickets, and explore both the admin and end-user sides of the system.

---

## Video Demonstration

- [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)

---

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

---

## Operating Systems Used

- Windows 10 (21H2)

---

## List of Prerequisites

- Azure Virtual Machine
- [osTicket Installation Files](https://drive.usercontent.google.com/download?id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD&export=download&authuser=0)

---

## Installation Steps

### Step 1: Create the Azure Virtual Machine

Start by creating a resource group in the Azure Portal and let's name it **osTicket** just for the sake of this lab. After creating the Resource Group, let's create a Virtual Machine inside it so we can keep everything contained. We will be using our Virtual Machine to install osTicket and will be using Remote Desktop Protocol (RDP) to use it. When you create your Virtual Machine, make sure to use Windows 10 in the image and to have at least 2 CPUs and 8 GB of memory. In my instance, I am using Standard D2s v6 (2 vCPUs, 8 GiB memory).

<br>
<img width="1640" height="744" alt="image" src="https://github.com/user-attachments/assets/03745795-f018-4d64-8e13-1e18e9a38722" />
<br>

Once your VM is deployed and ready, use Remote Desktop to connect to it. You're going to use the public IPv4 address to sign in to it and whatever username you chose when you set up the Virtual Machine.

<br>
<img width="397" height="480" alt="image" src="https://github.com/user-attachments/assets/1d89cd4f-2e8d-4efb-992f-2222499c5620" />
<br>

---

### Step 2: Enabling Internet Information Services (IIS)

After connecting to your Virtual Machine, you will have to enable Internet Information Services (IIS) and CGI. This can be accessed by searching for the **Control Panel** in the Search Bar and then going to **Control Panel** → **Uninstall a Program**. From inside here, there will be an option in the left panel called **Turn Windows features on or off**, with a shield icon. Select this option and then a list will appear titled **Windows Features**. Scroll down this list until you find **Internet Information Services** and enable it. From inside Internet Information Services, go to **World Wide Web Services → Application Development Features** and then scroll down to **CGI** and enable it. This will enable us to use a web server with osTicket.

<br>
<img width="1141" height="639" alt="image" src="https://github.com/user-attachments/assets/bbeaa54d-e434-4d64-a24b-e0909a84d871" />
<br>

You can check this is working by going to `127.0.0.1` in your web browser, this is the default webpage for IIS.

<br>
<img width="1180" height="725" alt="image" src="https://github.com/user-attachments/assets/a946e197-85c0-4fd3-9c25-ac9a6c3c4040" />
<br>

---

### Step 3: Download and Extract Required Files

Next, we're going to follow this [Google Drive Link](https://drive.usercontent.google.com/download?id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD&export=download&authuser=0) and download a folder that contains osTicket and all the dependencies to get osTicket working such as PHP, MySQL, and HeidiSQL. Once the folder is done downloading, extract the entire folder. Feel free to extract it anywhere, but I like to extract it to the Desktop so I don't lose the folder and it's easy to access. The folder should be called **osTicket-Installation-Files**.

<br>
<img width="1121" height="632" alt="image" src="https://github.com/user-attachments/assets/7dadedce-6287-4977-a928-3b87824ce1ef" />
<br>

---

### Step 4: Installing PHP Manager and Rewrite Module

Now we're going to install the PHP Manager and Rewrite Module. PHP is a backend language that osTicket runs on, so we'll need to install it to get osTicket working. Go into the **osTicket-Installation-Files** folder and look for the **PHPManager**. Click next through all the menus and install it. Next, find the **Rewrite Module** and do the same. For the weblinks to work correctly, osTicket needs Rewrite.

<br>
<img width="1295" height="796" alt="image" src="https://github.com/user-attachments/assets/d7a410d1-e87e-40a8-8329-188c9e1de5bb" />
<br>

---

### Step 5: Create a PHP Directory in the C: Drive

We're going to create a new directory in our C: drive called **PHP**. Navigate to **This PC → C: drive**, then right-click and add a **New Folder** named **PHP**. After creating the folder, navigate back to the **osTicket-Installation-Files** folder. Inside here, take **PHP 7.3.8 (php-7.3.8-nts-Win32-VC15-x86.zip)** and unzip it into the PHP folder we just created. When the **Select Destination** option comes up, enter `C:\PHP` and extract. These are the binaries needed for the osTicket server to use.

<br>
<img width="1591" height="644" alt="image" src="https://github.com/user-attachments/assets/cf412c20-1251-4fcb-98b9-c8292c541e32" />
<br>

---

### Step 6: Installing VC Redistributable and MySQL

Go back into the **osTicket-Installation-Files** folder and find **VC_redist** and install it. Next, find MySQL (`mysql-5.5.62-win32.msi`) and install it. Pick **Typical** and then launch the **MySQL Configuration Wizard**. Choose **Standard Configuration** and set the password to **root**. The username is also **root**. Make sure to save this password somewhere; you will not be able to access it later.

<br>
<img width="1000" height="696" alt="image" src="https://github.com/user-attachments/assets/d259c447-51a0-4a6e-b766-8b3653685b3c" />
<br>

---

### Step 7: Configure PHP

Open **IIS Manager** as an Administrator. In **PHP Manager**, select **Register new PHP version**, then navigate to the PHP folder in C: drive (`C:\PHP\php-cgi.exe`). Afterwards, restart the server by selecting **Restart Server** on the right-side menu or manually stopping and starting it.

<br>
<img width="1239" height="512" alt="image" src="https://github.com/user-attachments/assets/9d027fb4-6d81-4abd-99bf-b364e6649e46" />
<img width="1211" height="747" alt="image" src="https://github.com/user-attachments/assets/46ef0de9-da50-40f7-a969-4a9d4b80926e" />
<br>

---

### Step 8: Installing osTicket

Extract the **osTicket** folder from the **osTicket-Installation-Files** folder. Copy the **upload** folder into `C:\inetpub\wwwroot` and rename it to **osTicket**. Restart IIS.

<br>
<img width="1368" height="413" alt="image" src="https://github.com/user-attachments/assets/514afc1b-b4ba-4f82-bb16-03d91f7b2ec8" />
<br>

---

### Step 9: Open osTicket Web Page and Enable PHP Extensions

In IIS Manager, navigate to **Sites → Default Web Site → osTicket**. Select **Browse: *.80** to open the web browser to your osTicket installation. In PHP Manager, enable the following extensions:

- php_imap.dll  
- php_intl.dll  
- php_opcache.dll  

<br>
<img width="1742" height="761" alt="image" src="https://github.com/user-attachments/assets/0dcafae4-0493-44bf-b06d-b29bd9c4b40e" />
<img width="936" height="640" alt="image" src="https://github.com/user-attachments/assets/2c8b852d-0c02-4681-ae6f-66ee69f20dc3" />
<br>

---

### Step 10: osTicket Configurations

Navigate to `C:\inetpub\wwwroot\osTicket\include` and rename **ost-sampleconfig.php** to **ost-config.php**. Right-click the file → Properties → Security → Advanced → Disable Inheritance → Remove all inherited permissions. Add a new permission for **Everyone** and give **Full Control**. Apply changes.

<br>
<img width="1344" height="784" alt="image" src="https://github.com/user-attachments/assets/314b6ac4-a17e-4291-b63f-099b5cb762ae" />
<br>

---

### Step 11: osTicket Setup and HeidiSQL Installation

Open the osTicket web page and continue the installation. Fill in the help desk details and credentials (example: username `adminuser`, password `Password1`). Next, install **HeidiSQL** from the **osTicket-Installation-Files** folder. Connect using root credentials and create a database named **osTicket**. Enter this database info into osTicket and click **Install Now**.

<br>
<img width="1282" height="476" alt="image" src="https://github.com/user-attachments/assets/92955a61-50f4-4295-bdb5-9c224942edd5" />
<img width="941" height="592" alt="image" src="https://github.com/user-attachments/assets/2ea6871d-db47-43e8-b2bd-1dc25013af5b" />
<img width="1033" height="884" alt="image" src="https://github.com/user-attachments/assets/829adf25-2f82-4471-a92f-f932658f4f13" />
<br>

---

## Conclusion

Congratulations! You now have your very own help desk set up and installed on your Virtual Machine using osTicket. This is a great start to practicing with ticketing services.  

You can log in and explore both the admin and end-user portals using the links below:

- **Admin/Login Page:** [http://localhost/osTicket/scp/login.php](http://localhost/osTicket/scp/login.php)  
- **End User Page:** [http://localhost/osTicket/](http://localhost/osTicket/)

---

### Optional Cleanup

- Delete: `C:\inetpub\wwwroot\osTicket\setup`  
- Set permissions to **Read Only**: `C:\inetpub\wwwroot\osTicket\include\ost-config.php`
