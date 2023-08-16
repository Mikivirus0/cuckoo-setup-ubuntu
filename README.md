# **Mastering Cuckoo Sandbox: My Installation Journey**

Greetings, tech enthusiasts! Ever found yourself intrigued by the inner workings of malware analysis? I sure did, and after a week of intense efforts, I've conquered the challenge of installing Cuckoo Sandbox. Join me in reliving the highs, lows, and eventual victory as I share how I navigated through the installation process. If you've ever thought about diving into the world of cybersecurity, this journey might just be your inspiration. Let's dive in!

## Requirements:

1. Ubuntu 18.0 (Preferred) 
    
    Link: 
    
    [](https://releases.ubuntu.com/18.04/ubuntu-18.04.6-desktop-amd64.iso)
    
2. Window 7 ova file. 
    
    Link:
    
    [Google Drive - Quota exceeded](https://drive.google.com/u/0/uc?id=1uGxNwvSuSIhokeuX9N61D8VtyFDoK0-2&export=download)
    
3. Python 2.7 (By Default Present In ubuntu 18)
4. Remaining Requirements will be installed using pip and apt. (Given Below)
5. Check Errors Section, If you face any issue. It will redirect you towards solution :) 

## My Scenario:

I was using kali as my host, so i installed ubuntu in VMware and then setup the Cuckoo Sandbox in ubuntu.

## Installing Dependencies:

To lay the foundation for our Cuckoo Sandbox adventure, I embarked on a journey of dependency installation. Armed with determination and a series of commands, I paved the way for smooth sailing through the setup process.

First things first, I ensured my system was up to date by executing the following:

```bash
sudo apt update
sudo apt upgrade -y
```

With the basics covered, I moved on to installing the required packages:

```bash
sudo apt-get install python python-pip python-dev libffi-dev libssl-dev -y
sudo apt-get install python-virtualenv python-setuptools -y
sudo apt-get install libjpeg-dev zlib1g-dev swig -y
sudo apt-get install mongodb -y
sudo apt-get install postgresql libpq-dev -y
sudo apt-get install tcpdump apparmor-utils -y
```

The installation of TCP Dump called for a few additional steps to ensure seamless functionality:

```bash
sudo groupadd pcap
sudo usermod -a -G pcap cuckoo
sudo chgrp pcap /usr/sbin/tcpdump
sudo setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump
getcap /usr/sbin/tcpdump
sudo aa-disable /usr/sbin/tcpdump
sudo usermod -a -G vboxusers cuckoo
```

No cybersecurity journey would be complete without tackling cryptographic libraries:

```bash
sudo apt-get install swig
sudo pip install m2crypto
```

And with that, the stage was set for the grand finale – the installation of Cuckoo Sandbox itself!

## Setting Up Virtualenv:

Creating a virtual environment where Cuckoo can thrive. Here's how I set the stage for a successful installation:

1. **Update and Install:** I kicked things off by updating and installing the necessary tools with the following commands:
    
    ```bash
    sudo apt-get update && sudo apt-get -y install virtualenv
    ```
    
2. **Virtualenvwrapper:** Next, I made sure to get virtualenvwrapper on board, a useful tool for managing virtual environments. I used:
    
    ```bash
    sudo apt-get -y install virtualenvwrapper
    ```
    
3. **Shell Integration:** To seamlessly integrate virtualenvwrapper into my shell, I added this line to my **`.bashrc`** file:
    
    ```bash
    echo "source /usr/share/virtualenvwrapper/virtualenvwrapper.sh" >> ~/.bashrc
    ```
    
4. **Python3-pip and Completion:** I installed **`python3-pip`** and enhanced it with bash completion:
    
    ```bash
    sudo apt-get -y install python3-pip
    pip3 completion --bash >> ~/.bashrc
    ```
    
5. **Virtualenvwrapper Setup:** Installing virtualenvwrapper itself was a breeze with pip3:
    
    ```bash
    pip3 install --user virtualenvwrapper
    ```
    
6. **Python Version for Virtualenv:** I specified the Python version for virtualenvwrapper:
    
    ```bash
    echo "export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3" >> ~/.bashrc
    ```
    
7. **Sourcing the Script:** To ensure virtualenvwrapper works its magic, I sourced its script:
    
    ```bash
    echo "source ~/.local/bin/virtualenvwrapper.sh" >> ~/.bashrc
    ```
    
8. **Environment Location:** I set the location for virtual environments:
    
    ```bash
    export WORKON_HOME=~/.virtualenvs
    echo "export WORKON_HOME=~/.virtualenvs" >> ~/.bashrc
    echo "export PIP_VIRTUALENV_BASE=~/.virtualenvs" >> ~/.bashrc
    ```
    

With these steps, my system was now equipped with a virtual environment tailored for Cuckoo Sandbox. 

Dont forget to add pip packages to path variable, so you can access pip installed packages using cli. By default you cannot access pip packages from cli.

```bash
export PATH="$HOME/.local/bin:$PATH"
```

Note: `I faced few errors while setting it up, but stackoverflow helped me a lot.` 

## Installing Cuckoo:

Installing Cuckoo Sandbox itself. Brace yourself for the main step of our journey as I guide you through this step to bring Cuckoo to life:

1. **Upgrading pip and setuptools:** First, I ensured my tools were up to date by executing these commands:
    
    ```bash
    pip install -U pip setuptools
    ```
    
    Keeping your tools updated is like having a well-tuned instrument before a grand performance.
    
2. **Installing Cuckoo:** The crescendo of our journey – installing Cuckoo Sandbox. With the following command, Cuckoo would soon be at our fingertips:

```bash
pip install -U cuckoo
```

1. Initializing Cuckoo
    
    ```bash
    cuckoo init
    ```
    
    After running this command a dir named “.cuckoo” will be spawned in the /home/user directory. We will access and edit some conf files in this dir later on. 
    
    A wave of anticipation washed over me as Cuckoo Sandbox took shape within my virtual environment. The tools, the dependencies, the setup – all had led us to this defining moment.
    

## **Setting Up VirtualBox Inside Ubuntu: Connecting the Pieces**

With Cuckoo Sandbox ready to flex its muscles, we need the perfect companion to put our analysis into motion – a Windows 7 virtual machine. Here's how I got VirtualBox in on the action:

1. **Installing VirtualBox:** The first step was to get VirtualBox on board. With the following command, I secured the foundation for our Windows 7 machine:
    
    ```bash
    sudo apt-get install virtualbox
    ```
    
    VirtualBox would be our stage, where Cuckoo's analytical prowess would meet the Windows 7 specimen.
    
2. **Installing Extensions:** To unlock VirtualBox's full potential, I also installed its extension pack:
    
    ```bash
    sudo apt-get install virtualbox—ext–pack
    ```
    
    This pack was like a key, opening doors to enhanced features and compatibility.
    

With VirtualBox and its extension pack firmly in place, I could almost see the Windows 7 virtual machine taking shape. The gears were turning, and the stage was set for our malware analysis journey to reach its pinnacle.

## **Fine-Tuning VirtualBox: Getting Ready**

As our Cuckoo journey advances, we're taking a moment to optimize VirtualBox for a smooth experience. Here's how I did it, in a nutshell:

1. **Creating a Network Adapter:** To ensure our systems communicate effortlessly, I used **`VBoxManage`** to craft a host-only network adapter named **`vboxnet0`**:
    
    ```bash
    vboxmanage hostonlyif create && vboxmanage hostonlyif ipconfig vboxnet0 --ip 192.168.56.1 --netmask 255.255.255.0
    ```
    
    This adapter would link our elements together, like a bridge between worlds.
    
2. **Setting Up Storage:** I established a storage home for VirtualBox VMs, smoothing the way for a well-organized workspace:
    
    ```bash
    sudo mkdir /data && sudo mkdir /data/VirtualBoxVms && sudo chmod 777 /data/VirtualBoxVms
    ```
    
    It was like creating a cozy backstage area for our virtual performers.
    
3. **Directing VM Storage:** I directed VirtualBox to utilize our new storage location, making sure everything was in order:
    
    ```bash
    vboxmanage setproperty machinefolder /data/VirtualBoxVms
    ```
    
    With this step, the stage was set for the grand act to unfold.
    

With VirtualBox all set up and Cuckoo Sandbox eagerly waiting, we're on the brink of unleashing the analytical prowess of our Windows 7 machine. The finale is near – stay tuned for the last chapter of our installation journey!

## **Merging Windows with Cuckoo: The Snapshot Dance**

In this electrifying phase, we're blending our Windows machine into Cuckoo Sandbox. Here's how the magic unfolds:

1. **Welcoming Windows:** I kick-started the action by importing the Windows OVA file. This step gave life to our virtual Windows:
    
    ```bash
    vboxmanage import ~/Downloads/Agent.ova 
    ```
    
2. **Rename** The VM Agent → cuckoo1
    
    ```bash
    vboxmanage modifyvm "Agent" --name "cuckoo1"
    ```
    
3. **Starting** The VM to get Snap.
    
    ```bash
    vboxmanage startvm "cuckoo1"
    ```
    
4. **Freeze-Frame:** I captured a snapshot of this pristine Windows state, freezing it in time for analysis:
    
    ```bash
    vboxmanage snapshot "cuckoo1" take "snap1" && vboxmanage controlvm "cuckoo1" powerof
    ```
    
    Now we have snapshot named “snap1” and then we will powerof the VM.
    
5. **Joining the Band:** Our Windows machine harmonized with Cuckoo Sandbox using these steps:
    
    ```bash
    cuckoo machine --add cuckoo1 {IP-Without these bracks} --platform windows --snapshot snap1
    ```
    
    Note: “This IP address is of win7 machine, you can get this by running the machine from VirtualBox and then”:
    `cmd → ipconfig`
    
    It's like our Windows star found its place in the Cuckoo ensemble, ready for the malware analysis performance.
    

As the curtains rise, the Cuckoo Sandbox and Windows 7 machine are set for their malware analysis duet. The crescendo approaches – brace for the thrilling climax of our installation saga!

## **Enabling IP Forwarding: Clearing the Path**

To ensure seamless communication and analysis, we need to enable IP forwarding and make some critical tweaks. Here's how I did it:

1. **Setting Up IP Forwarding:** I enabled IP forwarding with these iptables rules:
    
    ```bash
    sudo iptables -A FORWARD -i vboxnet0 -s 192.168.56.0/24 -m conntrack --ctstate NEW -j ACCEPT && sudo iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT && sudo iptables -A POSTROUTING -t nat -j MASQUERADE
    ```
    
    It's like ensuring a clear path for communication between systems.
    
2. **Tweaking sysctl.conf:** I opened the sysctl configuration file and made an adjustment for IP forwarding:
    
    ```bash
    sudo vim /etc/sysctl.conf
    # Look for net.ipv4.ip_forward = 0, change to 1
    ```
    
    This was like adjusting the light settings before a performance.
    
3. **Applying Changes:** To bring these tweaks to life, I applied the changes and saved them:
    
    ```bash
    sudo sysctl -p /etc/sysctl.conf && sudo netfilter-persistent save
    ```
    
    It's akin to flipping the switch to illuminate the stage.
    

## **Fine-Tuning Cuckoo:**

To wrap things up, I modified the Cuckoo configuration files to enable key components:

```bash
nano ~/.cuckoo/conf/reporting.conf
# Change mongodb status from "off" to "on"

nano ~/.cuckoo/conf/processing.conf
# Change virustotal status from "off" to "on"

nano virtualbox.conf
************************************************************************************# edit like machines = cuckoo1 (cuckoo1 is your win7 vm name)

nano routing.conf
************************************************# internet = your default network interface. Mostly ubuntu have ens33
```

This step ensured Cuckoo had all the tools it needed for a stellar performance.

## Running Cuckoo: **Lights, Camera, Action!**

The moment of truth has arrived – it's time to launch Cuckoo Sandbox and witness its malware analysis prowess in action. Here's how I set the stage:

`Note: Use 2 terminals here, start rooter on 1, and start cuckoo on second.`

1. **Starting Cuckoo Rooter:** To kick things off, I fired up Cuckoo Rooter with these commands:
    
    ```bash
    cuckoo rooter --sudo --group ubuntu
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/10715014-5a70-4fd5-b5ce-1fe7ccc1a350/Untitled.png)
    
    It's like ensuring everyone's in place backstage before the curtain rises.
    
2. **Launching Cuckoo:** The main event is here! I started Cuckoo with a simple command:
    
    ```bash
    cuckoo
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e21bfdd6-1dac-495d-860b-c3994b2a31f7/Untitled.png)
    
    This is where the magic begins – the performance is about to unfold.
    
3. **Cuckoo Web Interface:** To keep an eye on the show, I launched the Cuckoo web interface:
    
    ```bash
    cuckoo web -H yourip -p 8000
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f378cbd-f4d1-4686-8f65-17e49408d675/Untitled.png)
    
    After running this, it will provide you link for the web interface, and you can access that interface from all the devices connected to your network.
    

As the clock ticks, our Cuckoo Sandbox will dive deep into the malware analysis world, unveiling its findings and insights. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/31961b8f-1161-4d49-8265-25e87e4651a6/Untitled.png)

## Closing Remarks:

In our exploration of Cuckoo Sandbox installation, we embarked on a journey that transformed curiosity into mastery. Through twists, turns, and triumphs, we unraveled the intricate threads of malware analysis and cybersecurity.

From the initial struggle of dependencies to the harmonious integration of VirtualBox and Windows 7, we pieced together a robust platform. As we fine-tuned configurations, activated IP forwarding, and set the stage for Cuckoo Sandbox, we were preparing for something remarkable.

With the final curtain rising on our Cuckoo installation saga, we've witnessed the symphony of technology and expertise harmonize. The spotlight now shines on Cuckoo Sandbox, ready to unveil the hidden truths within digital threats.

This journey serves as a testament to determination, problem-solving, and the allure of mastering cutting-edge cybersecurity tools. The road to installation might have been challenging, but it has empowered us to tackle the challenges that lie ahead in the realm of cybersecurity.

As you venture forward, remember that cybersecurity is an ever-evolving landscape, and the skills you've acquired are your compass through it. With Cuckoo Sandbox as your ally, you're equipped to dissect the complexities of malware, enhance your understanding of digital threats, and fortify your digital defenses.

Thank you for joining me on this odyssey. The stage is set, the tools are at your fingertips – now, go forth and explore the boundless horizons of cybersecurity mastery.

Stay vigilant, stay curious, and never stop learning.

Farewell for now,
**Umair Sabir ~ MikiVirus**

## Common Errors:

I faced a lot of errors while setting it up, so here i combined the best solutions from different resources to make things easy for you.

### Initial Dependencies Errors:

You will face errors in beginning, so dont process until you solve them. Just copy the error and paste on google, you will get the solution. All you need to do is struggle and google.

## Virtualenv Erros:

I faced few errors in this part too, but these were easy to solve.

### Virtual Box Errors:

While setting up virtual box, i faced a lot of errors but solution was quiet simple and clean:

Just follow my each step carefully while setting up vbox and installing windows7 in it. 

You need to make sure that Agent.ova is installed in VirtualBox and named as cuckoo1 and you have taken snap named snap 1 and you have provided the IP of machine(window7) in the command where you see the IP is redacted. That's it, if you have achieved this you have done the most complicated part of the Cuckoo Installation.

After this, i didn't faced any error. Just edit those config files the way i have mentioned.

## References:

https://0x0c.cc/2020/03/19/Install-a-Cuckoo-Sandbox-in-12-steps/

https://cuckoo.readthedocs.io/en/latest/

https://dogukankurnaz.com/blog/cuckoo-sandbox-installation/#/

https://www.virtualbox.org/wiki/Linux_Downloads

https://stackoverflow.com/
