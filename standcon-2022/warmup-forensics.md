# Warmup Forensics

#### Learning Point

Learned about the fields of PNG IHDR chunk.

![](<../.gitbook/assets/image (7).png>)

![broken (Original png file)](<../.gitbook/assets/image (34).png>)

We were given a broken png image. We notice the file signature of the png image is replaced by STANDCON22. Hence, we replace it with the file signature of 89 50 4E 47 0D 0A 1A 0A.&#x20;

Next, we find an IHDR chunk type code. Then, we modified 4 bytes before the IHDR code which is the length of the IHDR chunk to 00 00 00 0D as the chunk data is only 0xD in size. The 2 bytes of the size field were modified to be 22.

Next, we noticed the width and the height fields of the original IHDR chunk were set to be 0 pixels, but the CRC checksum is not set to 0 pixels, leading us to think it should not be modified.

The width and height fields define the image width and height respectively. According to the guide mentioned below, modification to the width and height would cause the image to not be rendered properly.

By opening the image, it notifies us of the IHDR CRC error as the CRC checksum does not match the checksum computed of the IHDR data, meaning the data of the IHDR chunk has been modified.

Since we know that the CRC checksum of E8 D3 C1 43 was computed from the original width and height of the image, we can brute force and guess the possible width and height values that result in the CRC checksum.



![Fixed Image](<../.gitbook/assets/image (37).png>)

![](<../.gitbook/assets/image (15).png>)

I modified the script that I found below to include the computing of the height field as well.

We found that computing the CRC checksum of the width of 1920 and height of 1080 matches the original CRC checksum.

&#x20;

![](<../.gitbook/assets/image (14).png>)



Credits of the script: [https://ctf-wiki.mahaloz.re/misc/picture/png/](https://ctf-wiki.mahaloz.re/misc/picture/png/)
