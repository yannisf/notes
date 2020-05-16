# Wireguard

Wireguard is a really simple VPN. It is so simple, that initially I could not capture its conceptual model. I will describe the most common setup where a peer will assume the role of the server, while other peers will connect to it. While on that, I will expose a couple of gotchas.

## Installation

Since installation can be quite different from platform to platform, I will only outline the Ubuntu 20.04 installation. For the rest, consult the Wireguard [site](https://www.wireguard.com/install/).

    $ sudo apt install wireguard

## Client configuration

Here, I make the assumption that a Wireguard server exists somewhere and you just want to connect there. Server setup is discussed later on.

The first thing to do is create a keypair, _private_ and _public_.

    $ umask 077
    $ wg genkey > private
    $ wg pubkey < private > public

After executing the previous commands, you should end up with two files, `private` and `public` each holding the respective key.

Send **your public** key to the Wireguard VPN server administrator. They should send you back **their public key**, **the IP** you will get assigned when connecting on the network, as well as the **server IP and port**. You will need these 3 pieces of information in the next step.

Next, create the following file and name it `wg0.conf`

```ini
[Interface]
Address = <the IP>
PrivateKey = <your private key>

[Peer]
PublicKey = <their public key>
Endpoint = <server:port>
AllowedIPs = 0.0.0.0/0
```

**Note**: Your public key is not required anywhere in the file. As a matter of fact the only time you need it, is to send it to the peers you want to connect to.

**Note**: The actual private key needs to be included in the previous file and not the filename that has the private key.

**Note**: The IP you will need to enter in the `Address` field should include a netmask as well (e.g. `10.100.1.50/32`).

**Note**: There is a lot to say about `AllowedIPs`. The value used here, `0.0.0.0/0`, establishes the server as the default gateway. This means all your traffic will be routed through the server. Your WAN IP will be the server's and you will have access to the server LAN, provided that the server allows that. If you want to only get point access, use the IP of the peer interface, e.g `10.100.1.1/32`. You may want to have access to just the LAN behind the VPN gateway. In that case use something like `192.168.1.0/24`. Whatever the case, you should discuss this with the administrator.

Finally, bring the interface up as follows:

    $ sudo wg-quick up ./wg0.conf

Perform a couple of pings, to your local wireguard interface, to the remote wireguard interface, to the remote network, to the Internet to see what is your state. Also the `wg` command provides useful information.

    $ sudo wg

## Server configuration

This section tackles the other side of the network. On this side too, a keypair needs to be created (follow the instructions outlines in the previous section).
Next create the following file and name it `wg0.conf`:

```ini
[Interface]
Address = <the IP you want to assign to your wireguard interface with netmask>
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eno1 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eno1 -j MASQUERADE
ListenPort = <port>
PrivateKey = <your private key>

[Peer]
PublicKey = <their public key>
AllowedIPs = <the IP you want to assign to this peer in `xxx.xxx.xxx.xxx/32`>
```

**Note**: The server Wireguard interface IP should be in the form xxx.xxx.xxx.xxx/xx, e.g. `10.100.1.1/24`.

**Note**: If the server if behind a firewall do not forget to open the respective port. If it is behind NAT do not forget to port forward (UDP).

**Note**: In `PostUp`, `PostDown` you can enter specialized `iptables` commands. Here, the Wireguard server assumes the role of a NAT gateway, allowing any peer to access network resources as if being the VPN server itself.

**Note**: In `PostUp`, `PostDown` use the proper network interface. Here `eno1` is used, but in your case it might be e.g. `eth0`.

**Note**: You will have to enable forwarding on the VPN server to forward to other interfaces (`sudo sysctl -w net.ipv4.ip_forward=1`). Mind you if you want this to persist you need to take a few extra steps, not included here.

**Note**: You may have several `[Peer]` sections.

Finally, bring the interface up as before:

    $ sudo wg-quick up ./wg0.conf

This was a very basic guide on how to setup a rudimentary Wireguard, secure and performant VPN.
