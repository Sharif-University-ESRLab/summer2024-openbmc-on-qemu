
![Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/6/66/OpenBMC_logo.png/1200px-OpenBMC_logo.png)


# OpenBMC on qemu

This project aims to emulate a BMC system by running openBMC on qemu and controlling fan speed based on temperature.

## Tools
In this section, you should mention the hardware or simulators utilized in your project.
- Qemu
- OpenBMC


## Implementation Details

In this section, you will explain how you completed your project. It is recommended to use pictures to demonstrate your system model and implementation.


Feel free to use sub-topics for your projects. If your project consists of multiple parts (e.g. server, client, and embedded device), create a separate topic for each one.

## How to Run

In this part, you should provide instructions on how to run your project. Also if your project requires any prerequisites, mention them. 

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
In this section, you should present your results and provide an explanation for them.

Using image is required.

## Related Links
Some links related to your project come here.
 - [OpenBMC](https://github.com/openbmc/openbmc)
 - [Qemu](https://github.com/qemu/qemu)


## Authors
Authors and their github link come here.
- [@HosseinGhaasemi](https://github.com/HosseinGhaasemi)
- [@Ali-Dehghani](https://github.com/Ali-Dehghani)
- []()

