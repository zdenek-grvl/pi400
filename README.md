
# Install steps

## install kodi
```sh
apt install kodi kodi-inputstream-adaptive
```

## install docker
```sh
curl -fsSL https://get.docker.com -o get-docker.sh
chmod +x get-docker.sh
./get-docker.sh
```

### rootless kit
```sh
dockerd-rootless-setuptool.sh install
usermod -aG docker pi
```
* if user is named `pi`

### stop and disable the system-wide root Docker service
```sh
systemctl disable --now docker.service docker.socket
systemctl disable --now containerd.service
```

## enable bind to 1-1023 ports
```sh
setcap cap_net_bind_service=+ep $(which rootlesskit)
```
## create system units
* /etc/systemd/system/adguard.service
* /etc/systemd/system/kodi.service

```sh
cp ./etc/systemd/system/*.service /etc/systemd/system/
```


## build SledovaniTV PVR addon
```sh
apt install cmake

git clone --branch Omega --depth=1 https://github.com/xbmc/xbmc.git
git clone --branch Omega --depth=1 https://github.com/palinek/pvr.sledovanitv.cz.git
cd pvr.sledovanitv.cz && mkdir -p build && cd build
cmake -DADDONS_TO_BUILD=pvr.sledovanitv.cz -DADDON_SRC_PREFIX=../.. -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=xbmc/addons -DPACKAGE_ZIP=1 -DADDONS_DEFINITION_DIR="$(pwd)/../xbmc/cmake/addons/addons" ../../xbmc/cmake/addons
make
make package-addons

cp /home/pi/pvr.sledovanitv.cz/build/pvr.sledovanitv.cz-prefix/src/pvr.sledovanitv.cz-build/*.zip /home/pi/
```

## ivysilani
Install custom repo from `custom_repos`

## YouTube
1. Install **YouTube** from standard repos
2. Restore settings by copying from `dot_kodi` to `~/.kodi`
    
# AdGuard
Go to http://192.168.55.100:3000/

