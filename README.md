# anycast-dns-ntp

## Setup loopbacks
### Ubuntu (netplan)
Add the following to your netplan /etc/netplan/<something>", network and ethernets will likely already be there:
```
network:
  ethernets:
    lo:
      addresses:
      - 1.1.1.1/32
      - 8.8.8.8/32
```
Activate the changes with:
```
sudo netplan apply
```

### Disable stub listener on port 53 (SystemD only needs this)

Edit /etc/systemd/resolved.conf uncommenting and changing the following two lines (change 1.1.1.2 and 9.9.9.9 to your preferred DNS servers):
```
DNS=1.1.1.2,9.9.9.9
DNSStubListener=no
```
Make the /etc/resolve.conf:
```
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

This changes requires a reboot of the machine to activate so "sudo reboot"

## Activate the servers
```
docker compose up -d
```


## Testing Your server:
### Windows:
#### NTP
Open a command prompt:
```
w32tm /stripchart /dataonly /samples:5 /computer:<non-anycast IP of server>
```
