# media-server
A repo to store config files, etc., for my media server. For easy future setup.

While it's a few years out of date, I started with this as a rough guide:

https://nick-rondeau.medium.com/creating-the-ultimate-media-server-with-docker-portainer-plex-and-ubuntu-server-f47a4503897f

# Server Hardware
Using a small Intel N150-based server. Specifically the BeeLink EQ14:

https://www.amazon.com/dp/B0B73QB6HK

It's connected to a 20TB external drive. Eventually will likely replace with a DAS in RAID configuration.

# Server OS

## Ubuntu Server (Headless)

### 24.10
Started here, as the kernel version supports the CPU in my machine. (N150 requires Linux kernel 6.11+.) However, I could not, for the life of me, get hardware transcoding to work. Am going to try downgrading to the LTS version and then upgrading the Kernel to a version which support the CPU. More to come.

### Installation Steps
1. Download and install balenaEtcher (https://etcher.balena.io/)
1. Download the OS ISO from Ubuntu. (https://ubuntu.com/download/server#how-to-install-latest)
1. Use balenaEtcher to create a bootable USB drive.
1. Insert USB stick into server, install OS.


# Configuring the OS

## Intel Tools, etc.
MAY NOT BE NECESSARY?

```
sudo apt install intel-gpu-tools
sudo apt install intel-media-va-driver
sudo apt install intel-opencl-icd
```

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

## Directory Structure
Root directory: `~/mediasrv`

Mount external drive into: `~/mediasrv/ext`

Use a large local partition for scratch data. Symlink this into `~/mediasrv/scratch`

### Media Library

#### Films
`~/mediasrv/ext/library/films`

#### Series
`~/mediasrv/ext/library/series`

### Config Files
For persistence, store in the external drive. All permanent storage should be there.

`~/mediasrv/ext/config/<service name>`

### Transcoding
Considered scratch data, should be as fast as possible, hence local, in the scratch space.

`~/mediasrv/scratch/transcode`

### Usenet Downloads
Considered scratch data, as it will be moved into the correct location on the external drive.

`~/mediasrv/scratch/usenet`
