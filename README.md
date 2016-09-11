# Mjpg streamer for Rpi Jessie
## How to get an mjpg streamer working on a Raspberry Pi 3 with Debian Jessie OS in plain English. 
![1367-07](https://cloud.githubusercontent.com/assets/21986609/18231653/2b8c782c-72b7-11e6-92dc-f0bc6c6606a5.jpg)

Setup:
* RPi 3
* Rasbian Jessie OS
* Camera module attached and enabled
* ssh enabled

Start with ssh into your Pi and update:
```
sudo apt-get update
```
Pull a couple of libraries:

```
sudo apt-get install libjpeg62-turbo-dev
```
and
```
sudo apt-get install cmake
```
Then clone the mjpg streamer from [Jacksonliam's Github site](https://www.github.com/jacksonliam "Jacksonliam's Homepage") and create a folder for it, all within in the same command:
```
git clone https://github.com/jacksonliam/mjpg-streamer.git ~/mjpg streamer
```
Change the directory from home to the mjpg-streamer:
```
cd mjpg-streamer
```
Then into the mjpg-streamer-experimental:
```
cd mjpg-streamer-experimental
```
and build it:
```
sudo make clean all
```
Make a new directory in the opt directory. Opt holds optional software and packages that you install that are not required for the system to run. In the old days, "/opt" was used by UNIX vendors like AT&T, Sun, DEC and 3rd-party vendors to hold "Option" packages;
```
sudo mkdir /opt/mjpg-streamer
```
Move everything into the new new directory:
```
sudo mv * /opt/mjpg-streamer
```
Now comes the tricky bit, change to the home directory:
```
cd8
```
Copy this line:
```
LD_LIBRARY_PATH=/opt/mjpg-streamer/ /opt/mjpg-streamer/mjpg_streamer -i "input_raspicam.so -vf -hf -fps 15 -q 50 -x 640 -y 480" -o "output_http.so -p 9000 -w /opt/mjpg-streamer/www" > /dev/null 2>&1&
```
Open the file from the command line:
```
sudo nano /etc/rc.local
```
and paste the code before the exit 0 line at the bottom. Save and exit the file:
![params](https://cloud.githubusercontent.com/assets/21986609/18417320/0e049218-7824-11e6-9d23-06426b74cd0e.jpg)

Change the parameters to your needs. The defaults are **-vf** and **-hf** that flips the picture upside down. **-fps 15** frames per second is set to 15, streaming pretty good video quality with low bandwidth. **-x 640 -y 480** is the screen resolution. The port of the web server is going to **-p 9000**.

9. Reboot the Rpi:
```
sudo reboot
```
10. Go to your ip address:9000 and the mjpg-streamer will be up and running. That's it.

![stream](https://cloud.githubusercontent.com/assets/21986609/18417210/62823dde-7821-11e6-8b69-de9f5b38d912.jpg)
Enjoy.