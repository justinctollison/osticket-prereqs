<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

# osTicket - Prerequisites and Installation

In this beginner-friendly tutorial, I’ll go through the full process of setting up **osTicket**, a help desk ticketing system, on a **Windows 10 Virtual Machine** using **Microsoft Azure**. This includes creating the Virtual Machine, installing the necessary dependencies like **IIS**, **PHP**, and **MySQL**, and configuring everything to get osTicket up and running.

The goal is to build a working help desk environment from scratch so you can get hands-on experience with how ticketing systems are installed and managed. By the end, you’ll have a complete osTicket setup that you can log into, create tickets, and explore both the admin and end-user sides of the system.

## Video Demonstration

- [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)


## Environments and Technologies Used


- Microsoft Azure (Virtual Machines/Compute)  
- Remote Desktop  
- Internet Information Services (IIS)

## Operating Systems Used

- Windows 10 (21H2)


## List of Prerequisites

- Azure Virtual Machine  
- [osTicket Installation Files](https://drive.usercontent.google.com/download?id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD&export=download&authuser=0)


## Installation Steps

### Step 1: Create the Azure Virtual Machine

Start by creating a resource group in the Azure Portal and let's name it **osTicket** just for the sake of this lab. After creating the Resource Group, let's create a Virtual Machine inside it so we can keep everything contained. We will be using our Virtual Machine to install osTicket and will be using Remote Desktop Protocol (RDP) to use it. When you create your Virtual Machine, make sure to use Windows 10 in the image and to have at least 2 CPUs and 8gbs of memory. In my instance, I am using Standard D2s v6 (2 vcpus, 8 GiB memory).

<br>
<img width="1640" height="744" alt="image" src="https://github.com/user-attachments/assets/03745795-f018-4d64-8e13-1e18e9a38722" />
<br>

Once your VM is deployed and ready, use Remote Desktop to connect to it. You're going to use the public IPv4 address to sign in to it and whatever username you chose when you set up the Virtual Machine.

<br>
<img width="397" height="480" alt="image" src="https://github.com/user-attachments/assets/1d89cd4f-2e8d-4efb-992f-2222499c5620" />
<br>

---

### Step 2: Enabling Internet Information Services (ISS)

After connecting to your Virtual Machine, you will have to enable the Internet Information Services or ISS and CGI. This can be accessed by searching for the **Control Panel** in the Search Bar and then going to the **Control Panel** → **Uninstall a Program**. From inside here, there will be an option in the left panel called **Turn Windows features on or off**, with a shield icon. Select this option and then a list will appear titled **Windows Features**. Scroll down this list until you find the Internet Information Services and enable it. From inside Internet Information Services, go to **World Wide Web Services → Application Development Features** and then scroll down to **CGI** and enable that. This will enable us to use a web server with osTicket.

<br>
<img width="1141" height="639" alt="image" src="https://github.com/user-attachments/assets/bbeaa54d-e434-4d64-a24b-e0909a84d871" />
<br>

You can check this is working by going to `127.0.0.1` in your Web Bowser, this is the default webpage for ISS.

<br>
<img width="1180" height="725" alt="image" src="https://github.com/user-attachments/assets/a946e197-85c0-4fd3-9c25-ac9a6c3c4040" />
<br>

---

### Step 3: Download and Extract Required Files

Next, we're going to follow this [Google Drive Link](https://drive.usercontent.google.com/download?id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD&export=download&authuser=0) and download a folder that contains osTicket and all the dependencies to get osTicket working such as PHP, mySQL and HeidiSQL. Once the folder is done downloading, extract the entire folder. Feel free to extract it anywhere, but I like to extract it to the Desktop so I don't lose the folder and it's easy to access. The folder should be called **osTicket-Installation-Files**.

<br>
<img width="1121" height="632" alt="image" src="https://github.com/user-attachments/assets/7dadedce-6287-4977-a928-3b87824ce1ef" />
<br>

---

### Step 4: Installing PHP Manager and Rewrite Module

Now we're going to install the PHP Manager and Rewrite Module. PHP is a backend language that osTicket runs on, so we'll need to install it to get osTicket working. Go into the **osTicket-Installation-Files** that we extracted earlier and look for the PHPManager. Click next through all the menus and install it. Next, find the Rewrite Module and same as before, click through all the screens and install it. For the weblinks to work correctly, osTicket needs Rewrite.

<br>
<img width="1295" height="796" alt="image" src="https://github.com/user-attachments/assets/d7a410d1-e87e-40a8-8329-188c9e1de5bb" />
<br>

---

### Step 5: Create a PHP Directory in the C: Drive

We're going to create a new directory in our C: drive called **PHP**, it will look like `C:\PHP`. Navigate to your C: drive by opening up the Windows Explorer, the folder in the task bar. Then navigate to **This PC → C: drive**. Once inside your C: drive, it should list out some folders, such as `inetpub`, `Program Files (x86)`, and `Users`. Right-click inside the C: drive and add a **New Folder** named `PHP`. After you've created the PHP Folder, navigate back to the **osTicket-Installation-Files folder**. Inside here we're going to take the **PHP 7.3.8 (php-7.3.8-nts-Win32-VC15-x86.zip)** and unzip or extract it INTO the PHP folder we just created. When the Select Destination option comes up, you can just enter `C:\PHP` then extract. These are the binaries needed for the osTicket server to use.

<br>
<img width="1591" height="644" alt="image" src="https://github.com/user-attachments/assets/cf412c20-1251-4fcb-98b9-c8292c541e32" />
<br>

---

### Step 6: Installing VC Restribution and MySQL

Go back into the **osTicket-Installation-Files folder** and find **VC_redist** and install it. Next, find **MySQL (mysql-5.5.62-win32.msi)** and let's install it. Pick **Typical** and then when you get to the end, launch the MySQL configuration Wizard, make sure it's checked. Choose **Standard Configuration** and click through until you get to Modify Security Settings. For the Password, pick **root**. Make sure you put it on a notepad somewhere so you don't forget, you will not be able to access the password later. Making the password "root" keeps it as simple as possible for this lab. The Username will also be "root". MySQL will be our database.

<br>
<img width="1000" height="696" alt="image" src="https://github.com/user-attachments/assets/d259c447-51a0-4a6e-b766-8b3653685b3c" />
<br>

---

### Step 7: Configure PHP

Open up Internet Information Services (ISS) as an Admin. Down in the search bar, enter ISS and it should be listed. You want to run this service as an Admin, either right-click and run as administrator or select it in the search menu. From inside the ISS Manager, you'll want to find the PHP Manager and double-click it. Inside the PHP Manager, there will be a "Register new PHP version" select this. Then navigate to the the new PHP Folder in the C: drive that we added and select the executable inside there. The route should look like this: `C:\PHP\php-cgi.exe`. Afterwards we will need to restart the server. This can be done by clicking back to the home menu and selecting **Restart Server** on the right-side bar. Altneratively, you can also manually Stop and Start the server.

<br>
<img width="1239" height="512" alt="image" src="https://github.com/user-attachments/assets/9d027fb4-6d81-4abd-99bf-b364e6649e46" />
<img width="1211" height="747" alt="image" src="https://github.com/user-attachments/assets/46ef0de9-da50-40f7-a969-4a9d4b80926e" />
<br>

---

### Step 8: Installing osTicket

Now that we have all the dependancies set up and installed, we can finally install osTicket. So, back in the "osTicket-Installation-Files folder" we're going to want to extract the folder inside there. So right-click the osTicket zipped folder and select Extract All and click through it. Now the extracted folder should be inside. Open the Folder and you should see two folders, "scripts" and "upload". Copy the upload folder and put it inside the folder `C:\inetpub\wwwroot` which is on your C: drive, where we made the PHP Folder and inside the inetpub folder. This is the root of our web server and how it will display osTicket. Make sure you rename the upload folder to **osTicket** this is very important! Once that's finished, Restart the ISS server again like before.

<br>
<img width="1368" height="413" alt="image" src="https://github.com/user-attachments/assets/514afc1b-b4ba-4f82-bb16-03d91f7b2ec8" />
<br>

---

### Step 9: Open osTicket Web Page and Enabling PHP Extensions

Back in the ISS Manager, navigate to **Sites → Default Web Site → osTicket** and in the right-hand bar should be a "Browse: *.80". Select this and it will open our web browser to our newly installed osTicket. This will be a great way to check if we've done everything correctly. Now if you take a look at the listed items in the osTicket Installer, we have already installed PHP and MySQL but now we need to enable a few PHP extensions. Go back to the ISS Manager and navigate back to the PHP Manager. There will be an option down below called "Enable or Disable an Extension". This should open up a list of extensions. We're going to enable:

- `php_imap.dll`  
- `php_intl.dll`  
- `php_opcache.dll`  

<br>
<img width="1742" height="761" alt="image" src="https://github.com/user-attachments/assets/0dcafae4-0493-44bf-b06d-b29bd9c4b40e" />
<img width="936" height="640" alt="image" src="https://github.com/user-attachments/assets/2c8b852d-0c02-4681-ae6f-66ee69f20dc3" />
<br>

---

### Step 10: osTicket Configurations

Before we can finally install osTicket, we'll need to do some more configurations. First navigate back to the C: drive and into `inetpub → wwwroot → osticket → include` and find the file `ost-sampleconfig.php`. We want to rename this file to `ost-config.php`. It must be named exactly, be careful not to typo it. Next, osTicket needs permission to make changes to that same file. Right-click it → Properties → Security → Advanced → Disable Inheritance → Remove all inherited permissions from this object. Then we're going to add a new permissions, so select Add and then at the top should be a "Select a Principal" option. Click that and it should open a new menu. Inside the "Enter a name..." we're going to enter **Everyone**. You can click "Check Names" on the right for it to autoselect. And then check **Full Control**. Go ahead and click apply and ok to all the menus.

<br>
<img width="1344" height="784" alt="image" src="https://github.com/user-attachments/assets/314b6ac4-a17e-4291-b63f-099b5cb762ae" />
<br>

---

### Step 11: osTicket Setup and HeidiSQL Installation

Back in the osTicket Webpage we can now continue with the Installation, go ahead and click continue. Feel free to name your Help Desk whatever you want and then for the email it doesn't matter, I'll use `helpdesk.ticket@gmail.com`. Fill out the rest of the information, the second email has to be different but can also be a random one. For Username and Password I'll choose `adminuser` and `Password1`. Make sure to save these credentials on Notepad. Now before the last section with the Database settings, we'll need to first install **HeidiSQL**. So navigate back to the "osTicket-Installation-Files folder" and let's install our last dependency. Click next through all the prompts and then open the Session Manager. From here we're going to connect our databases. Click "new" on the bottom left and then enter "root" in the password, remember this is the password we entered for our MySQL.

Open the new Connection and now we're going to create a new database for our osTicket. Right-click the "Unamed" and select Create New → Database. Make sure you name the database exactly as "osTicket" with lowercase "os" and an uppercase "T". Now back into the osTicket installation, fill out the rest of the Database information. The MySQL Database is "osTicket" and the username and password are both "root". With everything entered, you can now click "Install now."

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
