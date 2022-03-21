# Create your PiHole Container using Portainer!

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
- Go to **Settings** and under **App Templetes**
- Check **Use external templetes**
> Paste this: 
> `https://raw.githubusercontent.com/SelfhostedPro/selfhosted_templates/master/Template/portainer-v2.json`
> - ### Taken from this [github page](https://github.com/SelfhostedPro/selfhosted_templates)

## Go to App Temeplets and look for `pihole`

### Click and Edit Advanced Settings 
```
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      # "67:67/udp"  If you want PiHole to act as your DHCP server you will need to uncomment this
      - "1010:80/tcp" 
      - "4443:443/tcp"
      # Changing the port 80 to 83 becausse im gonna use port 80 for something else (homer)
    # Add the path that you created here to store the pihole data inbetween upgrades 
    # Replace the username with yours
    volumes:
      - '/home/banjo/portainer/etc-pihole/:/etc/pihole/'
      - '/home/banjo/portainer/etc-dnsmasq.d/:/etc/dnsmasq.d/'
 ```
 - I have commented UDP port 67 out. I let my router handle DHCP
 > After editing click on `deploy the container`

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
> You can login to your router and edit the DHCP settings to asign the IP address to the devices on your network
