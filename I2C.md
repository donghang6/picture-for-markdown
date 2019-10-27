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

![signal](https://github.com/donghang6/picture-for-markdown/blob/dev/iic2.png?raw=true "signal")

![gpio](https://pic1.zhimg.com/50/61588ca0d53f17ad99362cbaea4136a0_hd.jpg "gpio")

* **master** device: initiates and terminates bus transaction
* **servant** device: listens for address and responds as necessary
* each  device can be a transmitter and a receiver, and can switch rates during a bus transaction.

---

#### Signaling waveforms

![wave](https://github.com/donghang6/picture-for-markdown/blob/dev/iic6.png?raw=true "wave")

![stop](https://github.com/donghang6/picture-for-markdown/blob/dev/iic7.png?raw=true "stop")

---

#### Data transfer examples

![example](https://github.com/donghang6/picture-for-markdown/blob/dev/iic9.png?raw=true "example")

---

#### Read/Write MPU6050

![mpu6050](https://github.com/donghang6/picture-for-markdown/blob/master/1.png?raw=true "mpu6050")
![mpu6050](https://github.com/donghang6/picture-for-markdown/blob/master/2.png?raw=true "mpu6050")
![mpu6050](https://github.com/donghang6/picture-for-markdown/blob/master/3.png?raw=true "mpu6050")

---

#### Example Code

``` C
/******************************************
*函数原型： void IIC_Start(void)
*功能： 产生IIC起始信号
******************************************/
void IIC_Start(void)
{
    SDA_OUT();
    IIC_SDA=1;
    IIC_SCL=1;
    delay_us(4);
    IIC_SDA=0; //START:when CLK is high,SDA change from hig to low
    delay_us(4);
    IIC_SCL=0; //Ready Transmit or Receive
}

/******************************************
*函数原型： void IIC_Start(void)
*功能： 产生IIC结束信号
******************************************/
void IIC_Stop(void)
{
    SDA_OUT();
    IIC_SDA=0;
    IIC_SCL=0; 
    delay_us(4);
    IIC_SDA=1; //STOP:when CLK is low,SDA change from low to high
    IIC_SCL=1; //发送I2C总线结束信号
    delay_us(4);
}
/******************************************
*函数原型： void IIC_Wait_Ack(void)
*功能： 等待应答信号到来
*输出； 1，接收应答失败
       0，接收应答成功
******************************************/
u8 IIC_Wait_Ack(void)
{
    u8 ucErrTime = 0;
    SDA_IN();
    IIC_SDA=1;
    delay_us(1);
    IIC_SCL=1;
    delay_us(1);
    while(IIC_SDA) //最多等待50us
    {
        ucErrTime++;
        if(ucErrTime>50)
        {
            IIC_Stop();
            return 1;
        }
        delay_us(1);
    }
    IIC_SCL=0; //时钟输出0
    return 0;
}

/******************************************
*函数原型： void IIC_Ack(void)
*功能： 产生ACK应答信号SDA=0
******************************************/
void IIC_Ack(void)
{
    IIC_SCL=0;
    SDA_OUT();
    IIC_SDA=0;
    delay_us(1);
    IIC_SCL=1;
    delay_us(1);
    IIC_SCL=0;
}

/******************************************
*函数原型： void IIC_Ack(void)
*功能： 产生ACK应答信号SDA=0
******************************************/
void IIC_NAck(void)
{
    IIC_SCL=0;
    SDA_OUT();
    IIC_SDA=1;
    delay_us(1);
    IIC_SCL=1;
    delay_us(1);
    IIC_SCL=0;
}
/****************************************************
*函数原型： u8 IICwriteBytes(u8 dev, u8 reg, u8 length, u8 *data)
*功能： 将多个字节写入指定设备 指定寄存器
*输入： dev 目标设备地址
* reg 寄存器地址
* length 要写的字节数
* *data 将要写的数据的首地址
*返回： 返回是否成功，1成功
****************************************************/
u8 IICwriteBytes(u8 dev, u8 reg, u8 length, u8 *data)
{
    u8 count = 0;
    IIC_Start();
    IIC_Send_Byte(dev); //发送写命令
    IIC_Wait_Ack();
    IIC_Send_Byte(reg); //发送写入的地址
    IIC_Wait_Ack();
    for(count=0;count<length;count++)
    {
        IIC_Send_Byte(data[count]);
        IIC_Wait_Ack();
    }
    IIC_Stop(); //发送停止信号
    
    return 1;
}

/******************************************
*函数原型： u8 IIC_Read_Byte(unsigned char ack)
*功能： 读取一个Byte的字节
*输入： 读取一个字节,ack=1,发送ACK,ack=0,发送nACK
*返回： 读取到的Byte
******************************************/
u8 IIC_Read_Byte(unsigned char ack)
{
    unsigned char i, receive = 0;
    SDA_IN(); //设置为输入
    for(i=0;i<8;i++)
    {
        IIC_SCL=0;
        delay_us(1);
        IIC_SCL=1;
        receive<<=1;
        if(READ_SDA) receive++;
        delay_us(1);
    }
    if(ack)
        IIC_Ack(); //发送ACK
    else
        IIC_NAck(); //发送NACK
    
    return receive;    
}
```
