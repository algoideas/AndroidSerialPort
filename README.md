## AndroidSerialPort


### ˵��

AndroidSerialPort ����Android����ͨ�ţ����߲���Ҫroot��ͬʱ����ҪNDK���������ں�������
������� Android USB Host API, ������֧��API Level 14+.


��Ҫ����github��������Ŀ������AndroidSerialPort�������޸Ĵ����������֡�
<br>
[AndroidSerialPort](https://github.com/Deemonser/AndroidSerialPort) 

[usb-serial-for-android](https://github.com/mik3y/usb-serial-for-android)

AndroidSerialPort ֧�����ò����ʡ�У��λ������λ��ֹͣλ.

����ͨ�ż��

���нӿ���һ�ֿ��Խ���������CPU�Ĳ��������ַ�ת��Ϊ�����Ĵ������������ͳ�ȥ��ͬʱ�ɽ����ܵĴ���������ת��Ϊ���е������ַ�����CPU��������һ��������ֹ��ܵĵ�·�����ǳ�Ϊ���нӿڵ�·��
����ͨ�ţ�Serial Communications����ָ����ͼ�����䣬ͨ�������ź��� �����ߡ������ߵȣ���λ���д������ݵ�һ��ͨѶ��ʽ��
����ͨ���Ǽ�����зǳ�������ͨ�ŷ�ʽ������һЩ������ꡢ���̡���ӡ���ȶ���ͨ�����ڽ���ͨ�ŵġ�
���ڵ�ͨ��һ��ʹ��3������ɣ��ֱ��ǵ��ߡ������ߣ�tx���������ߣ�rx����

���ڵĲ���

�������������Ҫ�Ĳ����������豸���������ʡ���żУ��λ������λ��ֹͣλ��
�豸���ƣ����ڵ����ơ�
�����ʣ��������ʵĲ����������ʺʹ������ɷ��ȡ�
У��λ���ڴ���ͨ����һ�ּ򵥵ļ��ʽ�������ּ��ʽ��ż���桢�ߺ͵ͣ�������У��λ��
����λ��ͨ����ʵ������λ�Ĳ���
ֹͣλ�����ڱ�ʾ�����������һλ��
���м���λһ��Ĭ��λNONE������λһ��Ĭ��Ϊ8��ֹͣλĬ��Ϊ1��У��λ��Ϊ�˼������Ļ�����桢ż���в�λ������
������������ͨ�ŵĶ˿ڣ���Щ��������ƥ�䣬�������˲��������շ���

<br>

### APK����

[����apk](https://github.com/algoideas/AndroidSerialPort/)
<br>


### API

#### �򿪴���

```java

mUsbManager = (UsbManager) getSystemService(Context.USB_SERVICE);
UsbDeviceConnection connection = mUsbManager.openDevice(mUsbSerialPort.getDriver().getDevice());
mUsbSerialPort.open(connection);

```
<br>

�������Ҫ���ø����������ʹ�����¹���setParameters����

```java
/**
     * Sets various serial port parameters.
     *
     * @param baudRate baud rate as an integer, for example {@code 115200}.
     * @param dataBits one of {@link #DATABITS_5}, {@link #DATABITS_6},
     *            {@link #DATABITS_7}, or {@link #DATABITS_8}.
     * @param stopBits one of {@link #STOPBITS_1}, {@link #STOPBITS_1_5}, or
     *            {@link #STOPBITS_2}.
     * @param parity one of {@link #PARITY_NONE}, {@link #PARITY_ODD},
     *            {@link #PARITY_EVEN}, {@link #PARITY_MARK}, or
     *            {@link #PARITY_SPACE}.
     * @throws IOException on error setting the port parameters
     */
    public void setParameters(
            int baudRate, int dataBits, int stopBits, int parity) throws IOException;
```

����λһ��Ĭ����0��NONE��������λһ��Ĭ��Ϊ8��ֹͣλĬ��Ϊ1��

<br>

#### ��д����

##### ������

```java
// ��� Rxjava2 �����㴦���쳣
mReceiveDisposable = Flowable.create((FlowableOnSubscribe<byte[]>) emitter -> {
    if (mUsbSerialPort != null) {
        int len = 0;
        while (!isInterrupted
            && mUsbSerialPort != null) {
            len = mUsbSerialPort.read(mReadBuffer.array(), READ_WAIT_MILLIS);
            if (len > 0) {
                byte[] bytes = new byte[len];
                mReadBuffer.get(bytes, 0, len);
                emitter.onNext(bytes);
                mReadBuffer.clear();
            }
        }
    }
}, BackpressureStrategy.MISSING)
```


##### д����

```java
mUsbSerialPort.write(isHexSend ? ByteUtils.hexStringToBytes(s)
            : ByteUtils.stringToAsciiBytes(s), READ_WAIT_MILLIS));
```

<br>

#### �رմ���

```java
 mUsbSerialPort.close();
```

