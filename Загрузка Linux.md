# Linux-Loading - Загрузка Linux
```
VirtalBox 6.1
CentOS Linux release 7.9.2009 (Core)
```

# Способ 1. init=/bin/sh

При выборе ядра для загрузки нажимаем на клавиатуре "e"
Далее в появившемся окне консоли добавляем `init=/bin/sh` и нажимаем `сtrl-x` для загрузки в систему.

![image](https://user-images.githubusercontent.com/60498923/206419684-230c62c3-06c8-485e-8350-43ad53da5a5e.png)

Файловая `root` система монтируется в режиме `Read-Only` как видно на скриншоте ниже `(ro)`.

![image](https://user-images.githubusercontent.com/60498923/206419911-8b963f9a-e8df-4a4c-92bf-56c9f2bd3994.png)

Перемонтировать в режим Read-Write командой:

```
mount -o remount,rw /
```
Как видим вывод команды `mount | grep root` показывает, что `root` файловая система в режиме `Read-Write (rw)`.

![image](https://user-images.githubusercontent.com/60498923/206420193-e8755893-4915-4b03-b9c7-ec4239484f04.png)

# Способ 2. rd.break

```
Параметр rd.break прерывает процесс загрузки до того, как управление будет передано ядру.
rd.break - это параметр командной строки ядра Dracut - перейти в оболочку в конце. rd относится к «ramdisk»
```
В конце строки `linux16` добавлāем `rd.break` и нажимаем `сtrl-x` для загрузки в систему.

![image](https://user-images.githubusercontent.com/60498923/206420645-b3f84abf-1b92-46f1-9a4d-e8672f26efde.png)

После попадаем в emergency mode.

![image](https://user-images.githubusercontent.com/60498923/206420816-edb41e28-6830-4fa5-a2d9-50b8b2aea655.png)

Далее необходимо использовать следующие команды для того, чтобы попасть в файловую систему `root` и поменять пароль для пользователя `root`.

Перемонтировать файловую систему для Read-Write:

```
mount -o remount,rw /sysroot
```

Команда `passwd` для установки пароля для пользователя `root`:
```
passwd root
```

Команда для изменения корневого каталога:
```
chroot /sysroot
```
После этого, обновляем параметры SELinux командой:
```
touch /.autorelabel
```
Output:

![image](https://user-images.githubusercontent.com/60498923/206423322-81b173ac-7218-45d6-93da-47ea9197e901.png)

