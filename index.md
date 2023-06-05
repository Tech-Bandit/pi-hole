---
layout: default
---
[Back](./)

# Create your Pihole Container using Portainer!
## Make folders to hold the data
> ```
> cd
> mkdir portainer
> cd portainer
> mkdir {etc-pihole,etc-dnsmasq.d}
> ```

## On newer Ubuntu distros 
- Disable and stop Ubuntu's DNS resolver using 
```
sudo systemctl disable systemd-resolved.service
sudo systemctl stop systemd-resolved.service
```

## Open Portainer 
- By navigating to your `localhost:9443`
- Login

# Make sure you have your portainer repo templates 
- Go to **Settings** and under **App Templates**
- Check **Use external templates**
> Paste this: 
> `https://raw.githubusercontent.com/SelfhostedPro/selfhosted_templates/master/Template/portainer-v2.json`
> - ### Taken from this [GitHub page](https://github.com/SelfhostedPro/selfhosted_templates)

## Go to App templates and look for `Pihole`

### Click and Edit Advanced Settings 
```
    ports:
      #Domain port 53 for DNS Queries
      - "53:53/tcp"
      - "53:53/udp"
      # "67:67/udp"  If you want Pihole to act as your DHCP server you will need to uncomment this
      #Port 1010 will be the internal port where 80 is the external ( you should change your ports so they don't interfere )
      - "1010:80/tcp"
      #Same as port 80 but this is for https traffic
      - "4443:443/tcp"
      # Changing the port 80 to 83 becausse im gonna use port 80 for something else (homer)
    # Add the path that you created here to store the Pihole data in-between upgrades 
    # Replace the username with yours
    volumes:
      - '/home/banjo/portainer/etc-pihole/:/etc/pihole/'
      - '/home/banjo/portainer/etc-dnsmasq.d/:/etc/dnsmasq.d/'
 ```
 - I have commented UDP port 67 out. I let my router handle DHCP
 > After editing click on `deploy the container`

## Network Create (Optional)
- To create a direct attached macvlan ( edit the parent to your network interface, subnet, gateway and the ip to suit your network )

- sudo docker network create -d macvlan -o parent=enp7s0f1 --subnet=192.168.1.0/24 --gateway=192.168.1.1 --ip-range=192.168.1.5/32 pihole

## Volume Create (Optional)
- Another way to create a volume is to specify the driver, file type, device and the name of the volume

- sudo docker volume create --driver local \
    --opt type=ext4 \
    --opt device=/dev/md1 \
    Portainer-volume
    

## Look for Login credentials
- Go back to **Containers** and open the logs for the pi-hole container 
- Scroll and look for the user and password and go login
- Or use the command line and type `docker logs pihole | grep random`

## Access pihole
- Open a new tab and enter `http://pi.hole/admin` or the ipaddress:port for what you linked to 80
- In my case I could also but `http://localhost:83/admin`

## Add Block lists
> Check out this [Website](https://firebog.net/) for a list of common blocklisted domains 
> There is also this link too from [github](https://www.github.developerdan.com/hosts/)

## Use
> You can edit your pc's network adapter settings to use the IP address of the server you installed pihole on
> You can login to your router and edit the DHCP settings to assign the IP address to the devices on your network

## Documentation
[Docker CLI reference](https://docs.docker.com/engine/reference/run/)

[Portainer Documentation](https://docs.portainer.io/)

[Back](./)
