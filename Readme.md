
![Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/6/66/OpenBMC_logo.png/1200px-OpenBMC_logo.png)


# OpenBMC on qemu

This project aims to emulate a BMC system by running openBMC on qemu and controlling fan speed based on temperature.

## Tools
In this section, you should mention the hardware or simulators utilized in your project.
- Qemu
- OpenBMC


## Implementation Details
in order to add sensors to a system we must

1- Add I2C sensor config to devicetree in (openbmc/build/e3c246d4i/workspace/sources/linux-aspeed/arch/arm/boot/dts/aspeed/aspeed-bmc-asrock-e3c246d4i.dts)
   &i2c7 {status = "okay"; lm75@4d {compatible = "national,lm75"; reg = <0x4d>;};};

2- Add sensor recipe to openbmc/meta-asrock/meta-e3c246d4i/recipes-phosphor/sensors/phosphor-hwmon/obmc/hwmon/ahb/apb/bus@1e78a000/i2c-bus@300/lm75@4d.conf and bbappend file.
    we can get the recipe from a similar machine like romulus

3- Build the OS image(as described below).

## How to Run

#### prerequisites
libraries and packages requiered for openbmc
```bash
    sudo apt install git python3-distutils gcc g++ make file wget \
    gawk diffstat bzip2 cpio chrpath zstd lz4 bzip2
```

#### Building the Project
first we must clone openbmc from github
```bash
  git clone https://github.com/openbmc/openbmc
  cd openbmc
```
then we target our hardware
```bash
  . setup e3c246d4i
```
after that we build the image
```bash
  bitbake u-boot-aspeed && bitbake obmc-phosphor-image
```

#### Running the OS
first we must get qemu
```bash
  sudo apt-get install qemu-system-arm
```
then we change our directory to where the openbmc image is stored
```bash
  cd openbmc/build/e3c246d4i/tmp/deploy/images/e3c246d4i/
```
and the we run "image-bmc" with qemu
```bash
  qemu-system-arm -m 256 -M ast2500-evb -nographic \
    -drive file=image-bmc,format=raw,if=mtd \
    -net nic \
    -net user,hostfwd=:127.0.0.1:2222-:22,hostfwd=:127.0.0.1:2443-:443,hostfwd=udp:127.0.0.1:2623-:623,hostname=qemu
```
after the OS is booted we enter the username and password
```bash
  e3c246d4i login: root
  Password: 0penBmc
```
## Results
in order to chech our temperature and fan controller we must check hwmon
```bash
  cd /sys/class/hwmon
  ls
```
here the first folder is our fan cotroller
which has to profiles pwm1 and pwm2 and seven fans  
![image](https://github.com/user-attachments/assets/7aa8e744-0e94-4475-a85c-4dd8377d3bc0)

the second entry is the temperature sensor
which always shows zero because there is no real sensor!
![image](https://github.com/user-attachments/assets/38bc3df2-a562-4ece-9ec1-67a2ce2613fb)


## Related Links
Some links related to your project come here.
 - [OpenBMC](https://github.com/openbmc/openbmc)
 - [Qemu](https://github.com/qemu/qemu)


## Authors
Authors and their github link come here.
- [@HosseinGhaasemi](https://github.com/HosseinGhaasemi)
- [@Ali-Dehghani](https://github.com/Ali-Dehghani)
- []()

