# Using Hercules to simulate a mainframe environment on a Windows system

I recently started learning COBOL and came to understand that mainframe systems offer an environment closer to real-world applications. Consequently, the primary challenge I faced was simulating a mainframe operating environment on a Windows system. Below is a brief introduction to this topic.

## First, the essential tools and their download links are as follows:

1. Hercules  http://www.hercules-390.eu/
2. TK5-  https://www.prince-webdesign.nl/tk5
3. Vista TN3270  https://www.tombrennansoftware.com/

## Next, after downloading and extracting the files, the most crucial step is to establish a connection to Tk5- using Vista TN3270.

After extracting the TK5 archive into the Hercules folder, launch the system by clicking 'mvs.bat' - the successful initialization will be indicated by the interface shown in Figure ②.

[![r/cobol - ①](https://preview.redd.it/using-hercules-to-simulate-a-mainframe-environment-on-a-v0-al8iqzhb4eyf1.png?width=895&format=png&auto=webp&s=014e9357122e06035377a8385ea6245310ca1f6f)](https://preview.redd.it/using-hercules-to-simulate-a-mainframe-environment-on-a-v0-al8iqzhb4eyf1.png?width=895&format=png&auto=webp&s=014e9357122e06035377a8385ea6245310ca1f6f)①

[![r/cobol - ②](https://preview.redd.it/using-hercules-to-simulate-a-mainframe-environment-on-a-v0-6o2q5g2c4eyf1.png?width=947&format=png&auto=webp&s=dbf4be49b400274b8224212af218af9719a0a6e8)](https://preview.redd.it/using-hercules-to-simulate-a-mainframe-environment-on-a-v0-6o2q5g2c4eyf1.png?width=947&format=png&auto=webp&s=dbf4be49b400274b8224212af218af9719a0a6e8)②

## Then, open Vista TN3270 and establish the connection by entering the IP address, as shown in Figure ③.

[![r/cobol - ③](https://preview.redd.it/using-hercules-to-simulate-a-mainframe-environment-on-a-v0-b9kuz2qg4eyf1.png?width=927&format=png&auto=webp&s=48a4ce1c07c1b7dcdb6acb49cc7ef622348584a4)](https://preview.redd.it/using-hercules-to-simulate-a-mainframe-environment-on-a-v0-b9kuz2qg4eyf1.png?width=927&format=png&auto=webp&s=48a4ce1c07c1b7dcdb6acb49cc7ef622348584a4)③

## Next, you will see Figure ④, indicating a successful connection to TK5. Press Enter, then enter the username and password. Use either **herc01** or **herc02** as the UserID with the password **CUL8R** to access the main menu, as shown in Figure.

[![r/cobol - ④](https://preview.redd.it/using-hercules-to-simulate-a-mainframe-environment-on-a-v0-k8aos54d5eyf1.png?width=896&format=png&auto=webp&s=44dbf0218fc29dca1e099029956585cc36f411d7)](https://preview.redd.it/using-hercules-to-simulate-a-mainframe-environment-on-a-v0-k8aos54d5eyf1.png?width=896&format=png&auto=webp&s=44dbf0218fc29dca1e099029956585cc36f411d7)④

[![r/cobol - ⑤](https://preview.redd.it/using-hercules-to-simulate-a-mainframe-environment-on-a-v0-62u6jzbh5eyf1.png?width=924&format=png&auto=webp&s=f6df87bdbc916529e777c8af643ed4fc132d260c)](https://preview.redd.it/using-hercules-to-simulate-a-mainframe-environment-on-a-v0-62u6jzbh5eyf1.png?width=924&format=png&auto=webp&s=f6df87bdbc916529e777c8af643ed4fc132d260c)

## Postscript


1."The complete TK5 system is a complete system with Hercules, all the conf files and manuals. It also has Update 4 installed."

Downloading Hercules  for TK5 is not required . 
2. The TN3270 requires payment，you can download the WX3270  as an alternative.

Originally published at ：[Reddit](https://www.reddit.com/r/cobol/comments/1okn9sl/using_hercules_to_simulate_a_mainframe/)