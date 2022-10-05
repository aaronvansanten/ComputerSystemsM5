# Assignment OS introduction 

Job Logmans – s2331179 

## Assignment: Read about the Raspberry Pi 

I have thought of two things that I personally would use the Raspberry Pi for once we are done with the module. First, I would use it as a high-uptime addition to my home server on which I run a reverse proxy, DNS server, and VPN server. The Raspberry Pi is very suitable for this as it draws very little power and has a very small form factor without a fan. The Raspberry Pi has plenty of horsepower for this use case. I might also host some other services which are light-weight, but still require high up-time. 

Secondly, the Raspberry Pi could be used to make an all-in-one sensor for home automation. Using the GPIO pins and USB ports one could make a presence detection device which can also measure the environment regarding air quality, temperature and more. The Raspberry Pi is quite possibly powerful enough to also run Home Assistant to define and use these automations. 

## Assignment: Get a Raspberry Pi 4 with some accessories 

The kit seems complete. The case was obstructing the SD-card reader a bit, but this seems to be fixed after some tinkering. 

## Assignment: Install a Linux environment on your Laptop computer as well 

I run Pop!_OS by System76 and I'm currently on version 22.04 using a btrfs filesystem. 

## Assignment: Download Raspberry Pi OS and prepare your SD card 

I have installed the 64-bit Raspberry Pi OS and I am able to SSH into the RPi. I have also configured the Pi to connect to my laptop's Wi-Fi hotspot. 

## Assignment: Prepare the network, boot the Pi and login for the first time 

I have configured the Pi to connect to my laptop's Wi-Fi hotspot and my laptop is able to simultaneously connect to a Wi-Fi network and to host a hotspot. Therefore, I do not need to configure network sharing over ethernet. This is also much easier for me as I do not have a full-sized ethernet port on my laptop and would need to bring an adapter. 

I am able to SSH into the Pi and the Pi can reach the internet. 

## Assignment: Get used to the shell on your Pi

1. How can you search through the last commands you executed for a specific one with a certain keyword?

By pressing ctrl+r you can search through your past commands.

2. How can you rename a directory?

By using 'mv <old_directory_name> <new_directory_name>'.

3. How can you easily print the number from 1 to 10?

By running 'seq 10'.

4. How can you run a specific command 10 times?

By using a structure like 'for i in {1..10}; do echo "test"; done', where the echo command can be replaced with anything.

5. How can you redirect the output of a command to a file?

By appending '>> <filename>' to the command.

6. How can you find all manual pages that contain a specific keyword?

By running 'man -k <search-term>'.

7. How can you measure the time it takes to execute a certain command?

By prepending the 'time' command to the command. This will execute various measured times.

