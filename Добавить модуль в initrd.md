# Добавить модуль в initrd

Скрипты модулей хранятся в каталоге `/usr/lib/dracut/modules.d/`

Создадим каталог с названием `01test`
```
mkdir /usr/lib/dracut/modules.d/01test
```
В каталог `/usr/lib/dracut/modules.d/01test` поместим 2 скрипта

```
cat << EOF > /usr/lib/dracut/modules.d/01test/test.sh
#!/bin/bash

exec 0<>/dev/console 1<>/dev/console 2<>/dev/console
cat <<'msgend'
Hello! You are in dracut module!
 ___________________
< I'm dracut module >
 -------------------
   \
    \
        .--.
       |o_o |
       |:_/ |
      //   \ \
     (|     | )
    / \_   _/ \
    \___)=(___/
msgend
sleep 10
echo " continuing...."
EOF

```

```
cat << EOF > /usr/lib/dracut/modules.d/01test/module_setup.sh
#!/bin/bash

check() {
    return 0
}

depends() {
    return 0
}

install() {
    inst_hook cleanup 00 "${moddir}/test.sh"
}
EOF
```
Дадим права на выполнение:
```
chmod +x /usr/lib/dracut/modules.d/01test/*.sh
```

Пересобираем образ `initrd`:
```
mkinitrd -f -v /boot/initramfs-$(uname -r).img $(uname -r)
```
Перезаписать `initramfs`:

```
dracut -f -v
```
Output:
```
*** Creating image file done ***
*** Creating initramfs image file '/boot/initramfs-3.10.0-1160.el7.x86_64.img' done ***
```
Отредактировать grub.cfg убрав опции rghb и quiet: 
```
sed -i 's/rhgb quiet//g' /boot/grub2/grub.cfg
```
