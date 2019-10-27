# Introduce I2C

## How I2C communication works

---

### Serial Communication: I2C-bus

---

#### I2C = Inter IC = Inter Integrated Circuit

* Developed by Phillips Semiconductors
  * Now is NXP Semiconductors
* Two signal wires
  * serial data(SDA)
  * serial clock(SCL)
* Synchronous Communication
    ![Synchronous](https://img-blog.csdn.net/20150819163425283?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center "Synchronous")


* Bidirection data transfer(not simultaneous)

* Multi-master support

* Byte-oriented transfers(8 bits)

* Wide range of bus speeds
  * Standard mode -> 100kbps
  * Fast mode -> 400kbps

---

#### Signal definitions

![signal](https://github.com/donghang6/picture-for-markdown/blob/master/iic2.png?raw=true "signal")

![gpio](https://pic1.zhimg.com/50/61588ca0d53f17ad99362cbaea4136a0_hd.jpg "gpio")

* **master** device: initiates and terminates bus transaction
* **servant** device: listens for address and responds as necessary
* each  device can be a transmitter and a receiver, and can switch rates during a bus transaction.

---

#### Signaling waveforms

![wave](https://github.com/donghang6/picture-for-markdown/blob/master/iic6.png?raw=true "wave")

![stop](https://github.com/donghang6/picture-for-markdown/blob/master/iic7.png?raw=true "stop")

---

#### Data transfer examples

![example](https://github.com/donghang6/picture-for-markdown/blob/master/iic9.png?raw=true "example")

---

#### Read/Write MPU6050

![mpu6050](https://github.com/donghang6/picture-for-markdown/blob/master/1.png?raw=true "mpu6050")
![mpu6050](https://github.com/donghang6/picture-for-markdown/blob/master/2.png?raw=true "mpu6050")
![mpu6050](https://github.com/donghang6/picture-for-markdown/blob/master/3.png?raw=true "mpu6050")