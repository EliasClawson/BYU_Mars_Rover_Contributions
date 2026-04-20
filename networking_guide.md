# Networking Guide - 3/27/25
Braden Meyers

## What this contains:

- How static IP's are set and why they are important.

- How to connect to the rover computer from the base station

- How to debug the network connection from the base station to the rover

- How to connect the rover computer to the internet. 

- How to setup an SSH key

- How to test your network connection


## Terminology and Setup:

A few terms to know before reading through this:

- Rocket: There are two rockets in our current system - One on the base station side and one on the rover. They connect directly to the antennas and handle the network connection between the two devices. Each of them has an IP address on them that you can visit and see the configuration quickly view the network traffic. 

- POE: Power over ethernet. Only specific ethernet cord have the ability to provide power over the ethernet cord. The rockets are powered using this method. On the base station side the power comes from a POE ethernet switch and on the rover power is injected directly into the ethernet cord from the PCB.


## Static IP's:

Static IP addresses managed by Network Profiles. You can switch between different configurations under the **Wired** section in Network settings.

![network profiles](./media/networkProfiles.png)

Static IP addresses are critical for communication between the Rover and Base station because there is no router to automatically assign IP addresses for the devices. We currently use an IPV4 configuration. The neccessary things to enter is the IP address and the netmask. 

- The rover computer traditionally uses: `192.168.1.120`
- The base station computer traditionally uses: `192.168.1.111`
- The cart rover typically uses `192.168.0.XXX`. Where `XXX` is either `120` for the rover or `111` for the base station.
- The netmask for all of these should be: `255.255.255.0`

> Note for the `marsrover_4.0` repository: The `scripts/ip_config.sh` makes it easy to hot swap IP addresses. See the [IP config readme](../../tutorials/IP_CONFIG_README.md) for more information on how to change the IP addresses that the software uses.

### You can see that both of the computers use the same first 3 numbers. THIS IS CRITICAL!

Here is a decent tutorial on how to set a Static IP:
https://linuxconfig.org/how-to-configure-static-ip-address-on-ubuntu-18-10-cosmic-cuttlefish-linux

<figure>
    <img src="media/StaticIPNetworkingPage.png" alt="Linux Networking GUI in settings - Manual Static IP config" width="600" height="500">  
</figure>

You can check the IP address of the computer by running the command:

ifconfig - look for the ethernet device (eth0) and look for the IPV4 or inet IP address
ip addr show - also shows ip addresses for the network cards

> **WARNING**: We have had these network profiles delete themselves when a computer is restarted, or even change spontaneously when a new connection is made.

## Connecting to the Rover from the Base station

Start with the rover computer powered on (on the orin there is a little green light) as well as power to both of the rocket antenna modules (there are indicator lights). Green lights on the rocket indicate power and the red and orange lights indicate a connection between the rockets. 

Once the static IP addresses are assigned to the rover and base station computer with both computers plugged in with ethernet cables you can attempt to ping the rover using the command: 

ping 192.168.1.120  (or whatever the static IP of the rover is set to)

This will send pings to the rover's computer. When there is a connection there will be a response that says "64 bytes recieved from 192.168.1.120 ...". This is a good sign!

If you are unable to get this connection, double check the IP addresses and that all the cables are plugged in properly. One quick way to bypass the rocket antennas to elimanate that point of failure is to plug the ethernet cord from the base station laptop directly to the rover computer ethernet port. 

### SOMETIMES THINGS JUST TAKE A SECOND TO CONNECT SO GIVE IT SOME TIME

Good rule of thumb... Don't start entering commands from chatgpt to fix your network connection. Make sure all your connections are good and IP addresses are set and things should work. Take the time to learn how this all works. 

Once you get a connection you are good to SSH into the rover computer from the base station.


## Connecting the Rover Computer to the internet:

We currently don't have a great setup for this but a USB wifi antenna could be plugged into the rover computer so we could access the wireless network in the lab. The way we currently download packages is by connecting the rover computer to a monitor with a mouse and keyboard. We plug an ethernet cable into the rover that is connected to a router and the internet. We have to create a new network profile that does not have a static IP address. 

Setting up the USB wifi antenna would be really easy, we just haven't done it. Maybe we will do it by the end of the year

Try to avoid connecting the rover computer to the internet via ethernet because forgetting to change the IP address back is one of the most common issues that we have run into. There are ways to copy files from the base station onto the rover computer as well as pulling changes with git from the base station computer onto the rover computer. 

See the page on Using Git to manage rover code. 

<figure>
    <img src="media/AutomaticIP.png" alt="Linux Networking GUI in settings - Automatic DHCP IP config" width="600" height="500">  
</figure>

## Starting SSH (ONLY For a new Computer)

- Install and enable SSH server:
```
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```

## Mosh vs SSH

Mosh is branch of ssh that handles network dropouts and is lighter than ssh. This has worked really well for us. It has a few downfalls, but for the competition it can be very reliable. 

As long as mosh is installed on BOTH devices the only difference is you just switch ssh for mosh

See: https://mosh.org/#usage

## Setup SSH key

Why do I need an SSH key? 

Well some of our scripts rely on the need to be able to ssh into the rover without putting in the passowrd. 

Everytime you ssh into the rover it needs some kind of identification. We setup a ssh key to be able to do this automatically without the password of the computer.

Here is a great guide to setup an ssh key between the rover and base station:
https://www.ssh.com/academy/ssh/copy-id

Quick overview:
- Make sure you have a SSH key on the device
- Copy the ssh key from the computer to the rover computer using 
ssh-copy-id <user>@<ip_address>
ssh-copy-id marsrover@192.168.1.120


## Testing Network connection

There are many ways to test your network connection. A few important factors that affect your connection is signal strength and throughput. 

You can see quickly see the signal strength with incoming and outgoing traffic by going to the device page for the rocket antenna. 


#   todo get the rocket network page

To monitor the network traffic easily from the base station computer you can use the command nload:
https://www.tecmint.com/nload-monitor-linux-network-traffic-bandwidth-usage/

You can test your network connection throughput by using iperf (don't use iperf3):
https://www.baeldung.com/linux/iperf-measure-network-performance