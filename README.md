# hello-lkm

Hello world example for lkm (loadable kernel module)

### Instructions
```shell
$ make
$ sudo insmod hello-lkm.ko
$ sudo rmmod hello-lkm
```

### main_user.c
```shell
$ gcc -o main_user main_user.c
$ ./main_user
```
If you get `Segmentation fault (core dumped)` after running the main_user, it's probably because the rmdpc is not allowed. It might due to the protection principles done by kernal 4.x, please see more information in the [github issue](https://github.com/EEESlab/examon/issues/2#issuecomment-407695762).
For simply remove this protection, do this:
```shell
$ sudo sh -c "echo '2' > /sys/bus/event_source/devices/cpu/rdpmc"
```

### problem about `insmod`
If you get
```shell
insmod: ERROR: could not insert module simple-lkm.ko: Operation not permited
```
when you try `sudo insmod simple-lkm.ko` after you build the module, it is probably because your BIOS setting.
In my case, when I use `dmesg | tail -5` to see what actually happened, I get:
```shell
Lockdown: insmod: Loading of unsigned module is restricted; see man kernel_lockdoen.7
```
