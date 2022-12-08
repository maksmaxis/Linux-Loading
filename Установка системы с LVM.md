# Установить систему с LVM и переименовать VG.

```
VirtalBox 6.1
CentOS Linux release 7.9.2009 (Core)
```
Посмотреть текущее состояние системы:
```
vgs
```
Output:
```
  VG     #PV #LV #SN Attr   VSize   VFree
  centos   1   2   0 wz--n- <25.14g 4.00m
```
Переименовать `Volume Group` командой:
```
vgrename VolGroup00 OtusRoot
```
Output:
```
Volume group "centos" successfully renamed to "otusroot"
```

Далее необходимо править следующие файлы `/etc/fstab; /etc/default/grub; /boot/grub2/grub.cfg;`
```
sed -i 's/rd.lvm.lv\=centos/rd.lvm.lv\=otusroot/g' /boot/grub2/grub.cfg
sed -i 's/centos-root/otusroot-root/g' /boot/grub2/grub.cfg
sed -i 's/centos/otusroot/g' /etc/fstab /etc/default/grub
```
Пересоздаем `initrd image`:
```
mkinitrd -f -v /boot/initramfs-$(uname -r).img $(uname -r)
```
Output:
```
*** Creating image file done ***
*** Creating initramfs image file '/boot/initramfs-3.10.0-1160.el7.x86_64.img' done ***
```

Перезагружаем виртуальную машину:
```
reboot
```

![image](https://user-images.githubusercontent.com/60498923/206454171-b84b9e35-ba19-4fbd-acf0-2a30a89634e6.png)


