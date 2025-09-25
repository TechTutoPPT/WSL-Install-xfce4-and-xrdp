# WSL-Install-xfce4-and-xrdp

[![](https://github.com/TechTutoPPT/WSL-Install-xfce4-and-xrdp/blob/main/cover.PNG)](https://youtu.be/fSu2mU0W1Q8)

承接上一影片, 既然已安裝了WLS的Ubuntu CLI環境, 何不再為它添上友善的使用者介面.
我們先快速確認已執行以下步驟安裝上WSL及Ubuntu(以系統管員身份開啟PowerShell並執行):
```
wsl --install
wsl --update
wsl --shutdown
```

然後按Win+R, 輸入ubuntu開啟Ubuntu CLI環境, 再執行以下指令去安裝相關套件:
```
sudo apt update
sudo apt install -y xfce4 xfce4-goodies xrdp 
```

再輸入以下指令去配置以下檔案:
```
sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini 
sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini 
```
```
echo xfce4-session > ~/.xsession
```
```
sudo sed -i 's/test -x/#test -x/g' /etc/xrdp/startwm.sh
sudo sed -i 's/exec/#exec/g' /etc/xrdp/startwm.sh
echo "unset DBUS_SESSION_BUS_ADDRESS" >> /etc/xrdp/startwm.sh
echo "unset XDG_RUNTIME_DIR" >> /etc/xrdp/startwm.sh
echo "startxfce4" >> /etc/xrdp/startwm.sh
```

然後啟動xrdp服務:
```
sudo /etc/init.d/xrdp start
```

先查看一下現時Ubuntu的IP地址:
```
ip a | grep eth0
```
然後按Win+R, 輸入mstsc開啟遠端桌面連線並輸入上述IP地址再點擊連線並登入便可獲得桌面介面了.

再分享個小小的技巧, 如果直接執行sudo apt install firefox安裝Firefox瀏覽器的話會失敗的, 
因為WSL會使用Snap套件管理去安裝firefox, 但WSL又未能使用Snap的完整功能，特別是缺少systemd服務，會導致安裝的firefox後無法正常啟動.
所以請改以以下方式安裝Firefox的ESR版本:
```
sudo add-apt-repository ppa:mozillateam/ppa
sudo apt update
sudo apt install firefox-esr
```


