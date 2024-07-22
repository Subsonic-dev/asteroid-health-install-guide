# asteroid-health-install-guide
A quick install guide for Asteroid OS watched on how to install pre made packages from the Asteroid server, specifically asteroid-health using SSH over USB.

Background: This is a quick guide of the steps I used for a Ticwatch pro 2020 (Catfish) using a laptop running Opensuse Tumbleweed and using SSH over USB. As such these steps are written with Linux users in mind but the general principals still apply. 

# Step 1: Preparing the laptop 

On Linux installs with a newer kernal RDNIS (the protocol we need to use) is disabled by default. We need to run the following commands to enable it:
```
sudo nano  /usr/lib/modprobe.d/50-blacklist-rndis.conf
```
comment out each line so that it looks like this:

#RNDIS is considered insecure (bsc#1205767, jsc#PED-5731)

#blacklist rndis_wlan

#blacklist usb_f_rndis

#blacklist rndis_host

# Step 2: Test you can now SSH onto the watch 

Plug in the watch and run the following command to verify you can connect: 
```
ssh root@192.168.2.15
```
If it does not connect see <link> for troubleshooting, otherwise proceed to step 3. 


# Step 3: Giving the watch a connection to the internet
Before we can download packages to the watch we need to connect it to the internet. 

Run in a terminal on your **computer**: 
```
echo 1 > /proc/sys/net/ipv4/ip_forward
```
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

Make sure to replace **eth0** with the name of the interface that you use to connect to the internet, i.e wlps20 was mine because I conneced via wifi, so it would look like 

sudo iptables -t nat -A POSTROUTING -o wlps20 -j MASQUERADE

#Step 4: Checking the watch can connect to the internet and downloading the latest package list
To download the latest package list we first need to connect to the watch again:

```
ssh root@192.168.2.15
```

And run:
```
opkg update 
```

If this comes up with an error make sure you connected with the user "root" and that the watch can reach the internet. Try running:
```
ping 8.8.8.8 
```

# Step 5: Installing the package
Now we have an internet connection we can install any premade package from https://github.com/AsteroidOS/meta-asteroid-community/tree/master using opkg install <package> 

So to install asteroid health we want: 
```
opkg install asteroid-health
```

# Step 6: Enjoy your new app!
And that's it. Assuming you did not get any errors you have now installed Asteroid health. 

If you encounter any problems that this guide cannot solve I have linked my source material below. If you are still struggling join the Asteroid OS community on Matrix (https://asteroidos.org/contact/) and a helpful member of the community might be able to help you. 

If you found this guide helpful please star it so others can find it and if you have any suggetions please open a pull request or issue. 


# Useful resources 

| Resource  | Link |
| ------------- | ------------- |
| Asteroid SSH guide | https://wiki.asteroidos.org/index.php/SSH|
| Asteroid Installing packages  | https://wiki.asteroidos.org/index.php/Watchface_and_Package_Installation#Installation_of_prebuilt_packages |
