# media-server
A repo to store config files, etc., for my media server. For easy future setup.

While it's a few years out of date, I started with this as a rough guide:
https://nick-rondeau.medium.com/creating-the-ultimate-media-server-with-docker-portainer-plex-and-ubuntu-server-f47a4503897f

# Server Hardware
Using a small Intel N150-based server. Specifically the BeeLink EQ14:
https://www.amazon.com/dp/B0B73QB6HK

It's connected to a 20TB external drive. Eventually will likely replace with a DAS in RAID configuration.

# Server OS
Using Ubunty Server, presently version 24.10.

# Configuring the OS

## Network Interfaces

### List Network Interfaces

```bash
ip link show
```

### Add Interface for DHCP

Edit `/etc/netplan/<plan file>`

For a wifi device, add something like the following:

```yaml
  wifis:
    <interface name>:
      dhcp4: true
      access-points:
        <wifi AP broadcast name>:
          auth:
            key-management: "psk"
            password: <wifi password>
```

For an ethernet device, add something like the following:

```yaml
  ethernets:
    <interface name>:
      dhcp4: true
```

### Add Interface for Static IP

Edit `/etc/netplan/<plan file>`

For a wifi device, add something like the following:

```yaml
  wifis:
    <interface name>:
      dhcp4: false
      addresses: [<ip address>/24]
      routes:
        - to: default
          via: <router IP>
      nameservers:
        addresses: [8.8.8.8,8.8.4.4,<router ip>]
```

For an ethernet device, add something like the following:

```yaml
  ethernets:
    <interface name>:
      dhcp4: false
      addresses: [<ip address>/24]
      routes:
        - to: default
          via: <router ip>
      nameservers:
        addresses: [8.8.8.8,8.8.4.4,<router ip>]
```
