# Nginx reverse proxy with multiple mdns domains

The simplest example to show mdns, an nginx reverse proxy, and multiple example web apps
on a single docker compose file. This allows multiple `.local` domain names to be advertised
and internally routed without much effort. It also can enable HTTPS for HTTP servers.

## Running

Edit `services.mdns.environment.IP` in `compose.yaml` to your actual external IP address.

Run `docker compose up -d`.

## Windows users

> [!IMPORTANT]
> This does not work in Docker Desktop. You _must_ install docker directly in WSL2.

```
sudo apt update
sudo apt install docker.io
sudo service docker start
```

You also have to configure WSL2:

- Open "WSL Settings" from start menu
- Networking > Networking mode: Mirrored
	- Host Address Loopback: On
	- DNS Tunneling enabled: false (if networking mode: NAT)


> [!TIP]
> Alternatively you can manually edit `C:\\Users\\{username}\\.wslconfig`.

```
[wsl2]
networkingMode=Mirrored
dnsTunneling=false
[experimental]
hostAddressLoopback=true
```

> [!WARNING]
> Due to an unresolved hairpin NAT issue, the Windows WSL2/Docker host will not be able to
> see your mdns broadcast, but everyone else on the network will. 

## Make a self-signed cert in linux

```
sudo apt install mkcert
mkcert -install
mkdir -p ./certs
cd ./certs
mkcert "*.myserver.local"
mv wildcard.myserver.local.pem myserver.local.crt
mv wildcard.myserver.local-key.pem myserver.local.key
```

Then make it available to nginx at `/etc/nginx/certs` by adding another volume to `services.reverse-proxy.volumes`.

## Docs

https://hub.docker.com/r/jwilder/nginx-proxy/

https://learn.microsoft.com/en-us/windows/wsl/troubleshooting#resolve-local-names-in-wsl
