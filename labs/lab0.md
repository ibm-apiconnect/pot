---
title: Lab 0 - Setup IBM API Connect
toc: true
sidebar: labs_sidebar
permalink: /lab0.html
summary: In this lab, you’ll start from scratch to install Node.js and the components of the API Connect Developer toolkit.  Once you have the toolkit installed, you’ll get a chance to use the APIC command line interface for creating LoopBack applications, the intuitive Web-based user interface, and explore the various aspects associated with solution’s configuration of RESTful based services as well as their operation.
---

## Objective 
In the following lab, you will learn:

+ How to install Node.js on to your machine
+ How to enable API Connect on your Bluemix account
+ How to install the APIC Developer Toolkit

## Case Study used in this tutorial
In this tutorial, you will be starting from scratch to set up your development environment on your local machine.

## Before you begin
For this lab, you will be starting with your local image and installing node.js and the developer toolkit.  After that, if you do not already have a Bluemix account, you will be creating one for you and enabling the API Connect Essentials service on your account. The instructions you will follow will vary by the host operating system you are using. The three host OS's supported by this are `Windows`, `Linux - Intel` and `Linux - Mac`.  Skip to the section for your appropriate operating system.

## Step by Step Lab Instructions

### 0.1 - Install Node.js

#### 0.1a - Windows

1.  If you don't have node already installed on your machine, then please proceed.  Otherwise, move on to section 0.2 (Setup your Bluemix account).  

1.  Installing Node on Windows based machines (Tested on Windows 7 and Windows 10):
	
1.  The following are pre-requisites that need to be installed before installing node:

    1.  Install Python
	
        1.  version 2.7 is required. You can download it here:
        
            [https://www.python.org/download/releases/2.7/](https://www.python.org/download/releases/2.7/)
		
        1.  Note the location where it is installed.  You will need to add this location to your path later on.
		
    1.  Install .Net Frameworks SDK 2.0
    
        [https://www.microsoft.com/en-us/download/confirmation.aspx?id=19988](https://www.microsoft.com/en-us/download/confirmation.aspx?id=19988)
	
    1.  Install Visual Studio Express
    
        [https://www.microsoft.com/en-us/download/confirmation.aspx?id=44914](https://www.microsoft.com/en-us/download/confirmation.aspx?id=44914)
	
    1.  Add Python to the System Path in Windows 7 to the System Path.
	
        1.  Go to `Control Panel` -> `System` -> `Advanced System Settings`

            ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/1.png)

        1.  Click `Environment Variables`.

            ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/2.png)

        1.  Click `Edit` and append `;C:\python27` (or wherever you installed python) to the Path variable.

            ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/3.png)

1.  Install Node.js

    1.  Go to this link to install Node:
    
        [https://nodejs.org/en/download/](https://nodejs.org/en/download/)

    1.  Select the "LTS version - Recommended for most users".  Version at the top should depict v4.5 - includes npm 2.15.9.

        ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/4.png)

    1.  Select the Windows Installer .msi binary and then follow the prompts to install on your machine.
	
    1.  Before moving on, You will need to update your version of `npm` to version `3.8.10`
	
    1.  To do so, perform the following steps:
	
    1.  Click on `Start` and then in the search window type in `Powershell`.  Right click on `Windows PowerShell` and then select `Run as Administrator`

        ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/4a.png)

    1.  Execute these commands in Powershell

        ```bash
        Set-ExecutionPolicy Unrestricted -Scope CurrentUser -Force
        npm install -g npm-windows-upgrade
        npm-windows-upgrade
        ```
        
    1.  Select the `3.10.8` version of npm.  This is the recommended version to run with. Close the Powershell.

1.  Install API Connect on to your machine by starting up a command line window as Administrator and issue this command:

    ```bash
    npm install -g apiconnect --no-optional --ignore-scripts
    ```

1.  Once complete start up a new terminal window and enter in `apic -v`.

    If it returns the version of the platform, and not an error message, then the toolkit should then be properly installed.

1.  You are now ready to move on to section 0.2.

#### 0.1b - Linux

1.  Install the essentials to run node.js:

    ```bash
    sudo apt-get install build-essential libssl-dev curl git-core
    ```

1.  Install Node Version Manager aka "nvm" by issuing this command here:

    ```bash
    curl https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash
    ```

    {% include note.html content="
        When you install `nvm` it will also install `npm` which is the node package manager used to install node.js based software modules, including the API Connect Developer Toolkit.
    " %}

1.  Close and restart your terminal as indicated in the terminal window or run this command `source ~/.profile`.

1.  Run this command to confirm your version `nvm --version`

1.  This command will provide documentation on nvm if you wish to learn more about it `nvm help`.  NVM is exceptionally useful as it will allow you to support multiple installations of node.js on your machine, and select the version you want to use.  For our lab today, we only need the one version as indicated in the steps below.

1.  To check and see which versions of node are available, issue this command `nvm ls-remote`.  It will provide a very long list.  For API Connect, we will use a specific version of node.  That is version 4.5.0

1.  To install node 4.5.0 issue this command `nvm install 4.5.0`

    ```bash
    student@ubuntu:~$ nvm install 4.5.0
    Downloading https://nodejs.org/dist/v4.4.7/node-v4.5.0-linux-x64.tar.xz...
    Now using node v4.5.0 (npm v2.15.9)
    ```

1.  Install API Connect by issuing this command:

    ```bash
    npm install -g apiconnect
    ```

    It will take several minutes to complete.

1.  Once complete start up a new terminal window and enter in `apic -v`.

    If it returns the version of the platform, and not an error message, then the toolkit should then be properly installed.

1.  You are now ready to move on to section 0.2.

#### 0.1c - Mac OS

1.  Check to see if node is already installed.

    ```bash
    node -v
    ```

1.  Check the version, if it is version 4.5.0 then you can move on to section 1.1 - Create your Bluemix account. However if you would prefer to uninstall and reinstall node using Node Version Manager or if node was not found then please continue.

1.  Remove any existing versions of node. You will only need to complete these steps if node currently exists. To remove node completely from your Mac OSX environment run each of the following commands. Open a terminal session and type each of the commands below individually. 
 
    ```bash
    sudo rm /usr/local/bin/npm
    sudo rm /usr/local/share/man/man1/node.1
    sudo rm /usr/local/lib/dtrace/node.d
    sudo rm -rf ~/.npm
    sudo rm -rf ~/.node-gyp
    sudo rm /opt/local/bin/node
    sudo rm /opt/local/include/node
    sudo rm -rf /opt/local/lib/node_modules
    sudo rm -rf /usr/local/include/node/
    ```

1.  Install Xcode Command Line Tools. Open a terminal and type

    ```bash
    xcode-select --install
    ```

1.  This will open a dialog informing you that the command line developer tools are required for install. Press the Install button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-xcode-tools-confirm-install.png)

1.  Agree to the license

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-xcode-tools-confirm-license.png)
	
1.  The software install will begin

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-xcode-tools-progress.png)
	
1.  The install is complete when you see the "The software was installed" message.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-xcode-tools-install-complete.png)

1.  Next install git. You may already have a version, but if you want to have the latest version you will need to download the install from https://git-scm.com/download/mac

1.  Once on the install page the download should start immediately. However if it does not start immediately, click the "click here to download manually" link.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-git-install-download.png)

1.  Once the download has completed, you should have a file named similar to git-2.8.1-intel-universal-mavericks.dmg. Double-click to unpack the disk image.

1.  Once the disk image is unpacked and open, double-click the git-2.8.1-intel-universal-mavericks.pkg file. Note that if you downloaded a different version the file name will be reflected accordingly.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-git-install-package.png)

1.  You may get a dialog box with a message stating that the file can't be opened because it is from an unidentified developer. Click the OK button. Open your System Preferences and select the Security & Privacy component. In the section called "Allow apps downloaded from:" you will notice a message that states '"git-2.8.1-..ericks.pkg" was blocked from opening because it is not from an identified developer.', press the Open Anyway button. This will open the installer for git.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-git-install-unsecure.png)
		
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-git-install-security.png)

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-git-install-start.png)

1.  Follow the wizard to install. The installation is complete when you see a message stating that the installation was successful.
  
1.  Install Node Version Manager aka "nvm" by opening a terminal session and typing the following commands:

    ```bash
    git clone git://github.com/creationix/nvm.git ~/.nvm
    printf "\n\n# NVM\nif [ -s ~/.nvm/nvm.sh ]; then\n\tNVM_DIR=~/.nvm\n\tsource ~/.nvm/nvm.sh\nfi" >> ~/.bashrc
    NVM_DIR=~/.nvm
    source ~/.nvm/nvm.sh
    nvm install 4.5.0
    nvm alias default node
    nvm use node
    ```

    > **Note:** You may want to update your .bash file so you do not have to source the shell script every time. To do that, add the following to your .profile located in your user home directory:
    > 
    > ```bash
    > export NVM_PATH=~/.nvm
    > source ~/.nvm/nvm.sh
    > ```

1.  Install API Connect by issuing the following command:

    ```bash
    npm install -g apiconnect
    ```

    It will take several minutes to complete.

1.  Once complete start up a new terminal window and enter in `apic -v`.

    If it returns the version of the platform, and not an error message, then the toolkit should then be properly installed.

1.  You are now ready to move on to section 0.2.

### 0.2 - Create your Bluemix account

1.  Open a browser and visit:

    [http://www.bluemix.net](http://www.bluemix.net)

1.  Press the SIGN UP button
 	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-bluemix-setup.png)

1.  Complete the form and press the CREATE ACCOUNT button
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-bluemix-setup-account-details.png)

1.  Check your email for your next steps.
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-bluemix-setup-confirmation-email.png)

1.  Open the email and click the Confirm your account link.
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-bluemix-setup-confirmation-email-detail.png)

1.  Once confirmed, you will be taken to a page that says Success! To login, click the Log In link

1.  Enter your email address and press the CONTINUE button

1.  Then enter your password on the next page and press the LOG IN button

1.  Once you are logged in, you can start your 30 day trial by clicking on the launch button.

	![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-bluemix-launch-trial.png)

2. You will be prompted to create an organization. Enter an organization name (notice that there are suggestions for you). Also select an appropriate region. Then press the CREATE button.
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-bluemix-setup-create-org.png)

1.  Next you will be prompted to create a space such as dev, test, prod, etc. You can name it whatever you would like (again notice the recommendations). Then press the CREATE button.
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-bluemix-setup-create-space.png)

1.  Next you will see the Summary page where you can review your entries. Press the I'm Ready button
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-bluemix-setup-summary.png)

1.  You will see a screen then that  Click the `Catalog` button to go to your catalog and create your API Connect instance in section 0.3 below.
 	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/mac-bluemix-setup-complete.png)

### 0.3 - Enable the API Connect Service on your Bluemix Account

1.  Once in the catalog, you can search for the API Connect service by entering in `API Connect` in the search box next to the magnifying glass icon.  Click on the `API Connect` Icon to install a new instance of API Connect into your Bluemix space.

    ![](http://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/28.png)

1.  You can read through some of the details about the service.  You can fill in the information based on your needs.
	- SPACE – The name of your Bluemix Space to deploy the API Connect Service
	- APP – Keep the default “Leave unbound”
	- Service Name – The name you want to give to your API Connect implementation if required
	- Selected Plan – Keep the default  Once you are ready to continue, Click the `Create` button.

    ![](http://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/29.png)
 
1.  The API Connect service is now deployed.  Return to your Dashboard and you will see that your API Connect service is there.  Click on it open.

    ![](http://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/33.png)

1.  It will log into your instance for you automatically.  Once finished, it should drop you in your Drafts window.  

    ![](http://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/34.png)

1.  Click on the Navigate button in the upper left corner and select `Dashboard`.  If you see your `Sandbox` catalog there, then the process to create your instance is complete.

	![](http://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/34a.png)

### 0.4 Create your Developer Portal Instance for your API Connect Catalog

1.  Open a new tab in your web browser by clicking on the new tab `+` button.

    ![](http://github.com/ibm-apiconnect/pot-bluemix-docs/raw/master/img/lab1/new-tab.png)

1.  Log into your Bluemix Account using this url: [https://new-console.ng.bluemix.net/]()
1.  Via the dashboard, click on your API Connect instance.

    ![](http://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab1/33.png)

1.  From the top left, go to the `Dashboard` to bring your list of catalogs.  Click on your `Sandbox` Catalog

    ![](http://github.com/ibm-apiconnect/pot-bluemix-docs/raw/master/img/lab1/19.png)

1.  Click on `Settings`

    ![](http://github.com/ibm-apiconnect/pot-bluemix-docs/raw/master/img/lab1/20.png)

1.  Here you will see that you have no developer portal set up.

    ![](http://github.com/ibm-apiconnect/pot-bluemix-docs/raw/master/img/lab1/21.png)

1.  Select the `IBM Developer Portal` Radio Button to create your own custom portal that is tied to your catalog and then click `Save`. It will automatically generate the portal URL and the portal as well.

    ![](http://github.com/ibm-apiconnect/pot-bluemix-docs/raw/master/img/lab1/22.png)

1.  A pop up screen will let you know that the process to create your portal has started.

1.  It might take some time for your developer portal to get created, but usually the process is pretty quick.  If this piece doesn't work for you right away, move on to the next lab and circle back to this lab in a bit.  Once the portal is done creating, you will receive an email.  If it still doesn't work by the time you get to the later labs, inform your instructor.

1.  Once the process is complete, click on the link of the portal URL as seen in the screenshot above in step #9.  It is highly suggested that you create a bookmark for your developer portal in your browser so you can get back to it easily later.

## Conclusion

**Congratulations!** You have installed node.js, the API Connect developer toolkit!

In this lab you learned:

+ How to install node.js and its prerequisites and the API Connect Developer Toolkit on your native OS.
+ Set up you Bluemix Account.

**Options:** Either

- Proceed to [Lab 1 - Quick Start](lab1.html) **or**
- Proceed to [Lab 2 - Create a LoopBack Application](lab2.html)