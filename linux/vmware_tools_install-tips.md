```
$ sudo apt-get install git
$ git clone https://github.com/rasa/vmware-tools-patches.git
$ cd vmware-tools-patches
$ sudo ./patched-open-vm-tools.sh

```

* vmware中安装的ubuntu需要全屏的化，需保证下面的进程启动
```
/usr/lib/vmware-tools/sbin64/vmtoolsd -n vmusr --blockFd 3
```
* vmware中需要与外面的windows共享文件的化，需要保证下面的昵称启动
```
/usr/bin/vmhgfs-fuse -o subtype=vmhgfs-fuse,allow_other /mnt/hgfs
```
* 修复Failed to mount VMware vmblock fuse mount Error
```
sudo mkdir /var/run/vmblock-fuse
sudo systemctl enable run-vmblock\\x2dfuse.mount
sudo systemctl start run-vmblock\\x2dfuse.mount
```
