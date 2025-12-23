# Formwear
`README.md` 
> this is most definitely going to ring some bells for those who attended the "router hijacking" workshop L0L

## Solution:
- Extracting the `formwear.zip` reveals 3 files `fwu_ver` `hw_ver` and `rootfs`
- using `file` to analyse
```
 aditya@Dell-Inspiron:~$ file fwu_ver
fwu_ver: ASCII text
aditya@Dell-Inspiron:~$ file hw_ver
hw_ver: X1 archive data
aditya@Dell-Inspiron:~$ file rootfs
rootfs: Squashfs filesystem, little endian, version 4.0, zlib compressed, 10936182 bytes, 910 inodes, blocksize: 131072 bytes, created: Sun Oct  1 07:02:43 2023
```

- check the contents with `cat`
```
aditya@Dell-Inspiron:~$ cat fwu_ver
3.0.5
aditya@Dell-Inspiron:~$ cat hw_ver
X1
```

- the `rootfs` file is the firmware's root file system
- now we can mount it to see the file system contents
```
aditya@Dell-Inspiron:~$ mkdir mnt
aditya@Dell-Inspiron:~$ sudo mount -o ro,loop rootfs mnt
```

```
aditya@Dell-Inspiron:~/mnt$ ls -la
total 4
drwxrwxr-x 14 root   root    257 Aug 10  2022 .
drwxr-x--- 16 aditya aditya 4096 Nov 29 14:20 ..
-rw-rw-r--  1 root   root      0 Aug 10  2022 .lstripped
drwxrwxr-x  3 root   root   3225 Aug 10  2022 bin
lrwxrwxrwx  1 root   root     13 Aug 10  2022 config -> ./var/config/
drwxrwxr-x  2 root   root   3091 Aug 10  2022 dev
drwxrwxr-x  7 root   root    926 Oct  1  2023 etc
drwxrwxr-x  3 root   root     31 Oct  1  2023 home
drwxrwxr-x  2 root   root      3 Oct  1  2023 image
drwxrwxr-x  6 root   root   2580 Aug 10  2022 lib
lrwxrwxrwx  1 root   root      8 Aug 10  2022 mnt -> /var/mnt
drwxrwxr-x  2 root   root      3 Aug 10  2022 overlay
drwxrwxr-x  2 root   root      3 Aug 10  2022 proc
drwxrwxr-x  2 root   root      3 Aug 10  2022 run
lrwxrwxrwx  1 root   root      4 Aug 10  2022 sbin -> /bin
drwxrwxr-x  2 root   root      3 Aug 10  2022 sys
lrwxrwxrwx  1 root   root      8 Aug 10  2022 tmp -> /var/tmp
drwxrwxr-x  3 root   root     28 Aug 10  2022 usr
drwxrwxr-x  2 root   root      3 Aug 10  2022 var
```

- a config file is symlinked to ./var/config/
- but apparently the /var directory is empty
- so I searched for config in the directory
<img width="1349" height="549" alt="image" src="https://github.com/user-attachments/assets/655dfe25-d820-4961-8072-20a8424a29f7" />

- opening the `config_default.xml` reveals a whole bunch of data about the firmware in written in XML
- I searched for `{` to check if there was anything resembling the flag and there was!
```
<Value Name="SUSER_NAME" Value="admin"/>
<Value Name="SUSER_PASSWORD" Value="HTB{N0w_Y0u_C4n_L0g1n}"/>
```

## Flag:

```
HTB{N0w_Y0u_C4n_L0g1n}
```

## Concepts Learnt: 
- mounting a firmware's filesystem using sudo mount
- exploring directories in linux
- revision about symlinks

## Notes:
- exploring /home directory gave some hope - "almost there"

***


# Speed Thrills But Kills
`desc.txt'
> i recently got involved in a hit and run case in pune, that kids porsche was going wayy too fast, if only i knew what the VIN of the car was :(


## Solution:

- open `trace_captured.sal` file Logic2
- Since car's OBD port communicates with a CAN signal we can use CAN analyser in Logic2
<img width="485" height="387" alt="image" src="https://github.com/user-attachments/assets/43705c97-5d79-45b6-997e-c7d70c154fdc" />


- The measurement gives a pulse width of approximately 8 µs. Hence the bitrate is 125000 bits/s
<img width="1919" height="1011" alt="image" src="https://github.com/user-attachments/assets/8995ec7e-ccd3-4d5a-9e4b-2fe867f80bab" />


- selecting `ASCII` mode give us this
<img width="1905" height="1019" alt="image" src="https://github.com/user-attachments/assets/76f66b93-daa5-4441-b4e3-73b2c50a2659" />

- Here we can directly see something that looks like the flag HTB{ ....
- exporting the file as CSV to clearly read the flag
<img width="687" height="678" alt="image" src="https://github.com/user-attachments/assets/cbac629c-777e-4db3-aa55-efadace73f09" />


- hence the flag can be read as HTB{v1n_c42_h4ck1n9_15_1337!*0^}


## Flag:
```
HTB{v1n_c42_h4ck1n9_15_1337!*0^}
```

## Concepts learnt:
- Car communicates it's data through OBD port with the CAN bus
- What is a CAN bus and other types of communication protocalls
- how to find the bitrate of a signal trace


## Notes:
I was correct when I initally analysed using CAN but didn't realise that I had to change the bitrate. I kept trying with other analysers and began my research about how it basically all works. Then I learnt how to find the bitrate and voila!

## Resources:
- (https://www.csselectronics.com/pages/obd2-explained-simple-intro)
- (https://walkertexaslawyer.com/your-car-probably-has-computer-data-on-your-accident/)
- (https://www.youtube.com/watch?v=IyGwvGzrqp8)
- (https://en.wikipedia.org/wiki/CAN_bus)
- (https://www.youtube.com/watch?v=9sas4uW4-Vg&t=808s)

*** 

# Red Devil
`README.md`
> this is the worst football team ever(i dont even watch football lmeow)

## Solution: 

- the signal.cf32 file is a 32-bit floating-point data
- we need to decode and analyze the captured signal
- I used inspectrum to analyse the waveform
- to further understand what's happening I used the utitlity called `Universal Radio Hacker`
- <img width="1530" height="947" alt="image" src="https://github.com/user-attachments/assets/a39a1fc5-6f0b-41a5-9e6d-73ae8071a21e" />

- In the waveform presented in URH we can see the amplitude shifting. This means that it is using ASK modulation.
- I selected ASK and clicked autodetect. Then I moved to the analysis tab.
- <img width="1520" height="942" alt="image" src="https://github.com/user-attachments/assets/d9ab2534-be72-42dd-82ac-b8c39adba3ee" />
- View data as ASCII and assumed the decoding was either of the manchester since the desc gave away a hint of football team
- Now we can simply read the flag

## flag:

```
HTB{RF_H4ck1n6_1s_c00l!!!}
```

## Concepts learnt:
- .cf32 files are radio frequency data in 32-bit complex floating-point.
- analysing these files involves software-defined radio (SDR) captures
  
## Resources:
- (https://www.youtube.com/watch?v=JdzVIjKA68o)
- (https://github.com/merbanan/rtl_433)
- (https://github.com/jopohl/urh?tab=readme-ov-file)

***

# I like logic more

## Solution:

- MicroSD cards communicate using the SPI protocol
- We need to identify which channel corresponds to which line of SPI

- Clock (channel 3): line with regular, rapid square wave pulses. These often appear in bursts (typically groups of 8) 

- Enable (channel 2): signal that stays in one state (usually High) and drops (Active Low) to "frame" the activity on the Clock and Data lines. It usually stays low for the duration of the data transfer.

- MISO / MOSI (Data): These lines will switch states (High/Low) in sync with the clock pulses.
- MISO: Chanel 0
- MOSI: Channel 1
<img width="1919" height="1019" alt="image" src="https://github.com/user-attachments/assets/52b1ee48-940f-4592-b1ca-3c41c395d1fc" />

- Now we can export the data in ASCII format into a CSV
<img width="440" height="708" alt="image" src="https://github.com/user-attachments/assets/ab6244cd-5013-47b2-97ec-50a872506751" />

- now we search for "H" hoping to find HTB{ i.e. the flag format
<img width="715" height="719" alt="image" src="https://github.com/user-attachments/assets/47abf233-218e-44e4-9726-628085ac6719" />


- Here we find the flag that is `HTB{unp2073c73d_532141_p2070c015_0n_53cu23_d3v1c35}`


## Flag:

```
HTB{unp2073c73d_532141_p2070c015_0n_53cu23_d3v1c35}
```

## Concepts learnt:

- SD cards communicate with SPI protocol
- How SPI signals can be interpretted from the channels
- Converting the output data to ASCII and exporting as CSV


## Resources:

- (https://electronics.stackexchange.com/questions/232885/how-does-an-sd-card-communicate-with-a-computer)
- (https://support.saleae.com/product/user-guide/protocol-analyzers/analyzer-user-guides/using-spi)
- (https://zeroalpha.com.au/services/data-recovery-blog/sd/sd-and-micro-sd-pinout-description-including-spi-protocol#:~:text=MicroSD%20SPI%20Protocol%20Pinout,is%20not%20used%20for%20SPI.)

***


# Gates of Mayhem
> iqtest but its on steriods and you have weird aah inputs aswell.

## Solution:
- We have a set of 6 inputs in the csv file
- We need to find the logic to operate on the inputs to get the output
- I looked into how transistors can be used as logic gates and analyzed the given gates 
<img width="2480" height="2055" alt="gates_of_mayhem" src="https://github.com/user-attachments/assets/8acb1855-fdcf-4abd-a02b-572d1bd74caf" />

- Now we can write a python script to read each column from the csv and compute the output
- The output is also in binary so we need to convert it to string 
```
import csv

output_bits = ""

with open("input_sequence.csv", mode="r") as csvfile:
    reader = csv.DictReader(csvfile)

    for row in reader:
        in1 = int(row["IN1"])
        in2 = int(row["IN2"])
        in3 = int(row["IN3"])
        in4 = int(row["IN4"])
        in5 = int(row["IN5"])
        in6 = int(row["IN6"])

        part1 = in1 & in2
        part2 = (in3 & in4) & (in5 | in6)
        result = part1 ^ part2

        output_bits += str(result)

print(output_bits)

# Convert bits → ASCII
text = "".join(
    chr(int(output_bits[i:i+8], 2))
    for i in range(0, len(output_bits), 8)
)
print(text)

```

- The output from this script gives

```
0110001101101001011101000110000101100100011001010110110001111011001100010101111101101100001100000111011000110011010111110111010000110000010111110011001101111000011100000110110000110000001100010111010001011111011011000011000001100111001100010110001101111101
citadel{1_l0v3_t0_3xpl01t_l0g1c}
```


## Flag:
```
citadel{1_l0v3_t0_3xpl01t_l0g1c}
```

## Concepts learnt:

- converting transistors to logic gates
- how to analyse transistors to find a logic
- how to read from a csv file and script in python


## Notes:

I identified the first four gates quite easily. The last XOR gate was a bit confusion since it wasnt explicitly mentioned anywhere. So, I had to manually look for what happened in each case for a particular input and make a truth table. The analysis helped me realise how it worked and is an XOR gate. 

## Resources:

- (https://youtu.be/OWlD7gL9gS0?si=09IodCWZqtj7mhic)
- (https://youtu.be/eYGM3XEIpHg?si=ObEMaoFRrgwY7mp-)

  
 
