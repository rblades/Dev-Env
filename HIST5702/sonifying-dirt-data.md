<p><strong>NOTE:</strong> This is a work in progress. Stay tuned in the next few days for a final product.</p>
<h1 id="sonifying-dirty-data">Sonifying Dirty Data</h1>
<p><img src="https://i.imgur.com/DOmpxuk.png" alt="The plan mapped out"></p>
<p>This project is an attempt to sonify dirty data. So much of Optical Character Recognition (OCR) focuses on, surprise, visuality. Digital Humanists are no strangers to cleaning up dirty data. We see it presented before us on screens. The OCR misses words, or replaces letters with numbers. We then use cleaning methods, regular expressions, and the like to get the text back to its original informational state. We clean away the ‘nonsense’ data that becomes entangled in our ‘pure’ data. But what happens if we sonify that dirty data? Take it a step further: what happens if we then convert that sonification back into a text file? We begin to mix the digital voice into our work. Sound literally changes our data as it becomes entangled in our association of OCR and visuality.</p>
<h2 id="bare-necessities">Bare Necessities</h2>
<ul>
<li>Raspberry Pi 2 (preferably with plastic case)</li>
<li>Raspberry Pi camera module</li>
<li>Internet connection (only for the development stage) with ethernet cord or Wifi dongle</li>
<li>Micro SD card with adapter</li>
<li>Raspbian Jessie operating system image</li>
<li>HDMI cable (<strong>NOT</strong> a <a href="https://en.wikipedia.org/wiki/VGA_connector">VGA cable since these do not output sound or connect to the Pi</a>)</li>
<li>Headphones, speakers (that can plug into standard 1/8 inch jack), or TV/monitor (with speakers)</li>
<li>Spare cardboard box, electrical and/or technical tape, exacto knife/box cutter</li>
</ul>
<p>We will install the following software:</p>
<ul>
<li>Tesseract (Optical Character Recognition software)</li>
<li>Alsa (Plays audio)</li>
<li>Espeak (Speech recognition software for Text to Speech)</li>
<li>Bison (Compiles Sphinx packages)</li>
<li>SphinxBase (Speech recognition software for Speech to Text)</li>
<li>PocketSphinx (Sphinx addon)</li>
<li>Sox (Audio converter)</li>
</ul>
<h2 id="getting-started">Getting Started</h2>
<p>Let’s start from scratch. If you already have your Pi running, or are using a preinstalled SD card, you can skip to the <a href="#the-screen-isnt-showing-your-pi-and-hdmi">next section</a>.</p>
<p>For this tutorial we are going to use the official Raspberry Pi operating system, Raspbian. You can install NOOBS, but all that does is give you a choice between several operating systems.</p>
<p>On the Raspberry Pi <a href="https://www.raspberrypi.org/downloads/raspbian/">downloads page</a> select Raspbian and choose a version to download. If you have an 8GB Micro SD card, stick with Rapsbian Jessie. If you have a much smaller Micro SD card, you might choose Raspbian Jessie Lite since it is a more bare-bones version of Raspbian and thus uses less space. <strong>This tutorial uses Raspbian Jessie 4.1</strong> so use Lite at your own caution (though you should stil be able to download packages and do most of the heavy lifting). Micro SD cards are pretty darn cheap, though, so think about purchasing a larger one if need be.</p>
<p><strong>NOTE:</strong> A Micro SD card is the small little chip. You usually cannot connect a Micro SD directly to your computer. Most Micro SD cards, however, come with an adapter. Plug the Micro SD into the adapter and make sure the little locking mechanism is unlocked (or else you cannot write anything to the card).</p>
<p>Once your image has downloaded, insert your Micro SD card in the adapter to your computer, and follow Raspberry Pi’s <a href="https://www.raspberrypi.org/documentation/installation/installing-images/">documentation on how to mount the image to your Micro SD card</a>. Each platform has its own method. Remember to safely eject the card.</p>
<h3 id="the-screen-isnt-showing-your-pi-and-hdmi">The Screen isn’t Showing! Your Pi and HDMI</h3>
<p>I am devoting an entire sub-section to connecting your Pi to a monitor via and HDMI cable because <strong>this can be the most annoying first step for any Pi beginner.</strong></p>
<p>Before plugging your Pi into the power, plug in your Micro SD card - the gold stripes should face the Pi and you will hear a click, locking the Micro SD card in place.</p>
<p>Then turn your monitor on. Wait until it is running. Make sure your HDMI cable is connected to your monitor. <strong>Now</strong> plug in the HDMI cable to your Pi. If you do not have a device that supports HDMI, fear not.<a href="https://www.raspberrypi.org/blog/use-your-desktop-or-laptop-screen-and-keyboard-with-your-pi/"> You can connect you Pi to your computer via an ethernet cable</a>. <strong>YOU CANNOT CONNECT YOUR PI TO YOUR COMPUTER VIA HDMI</strong>, since your computer’s HDMI is usually an output, not an input.</p>
<p><strong>NOTE:</strong> Your monitor, TV, etc. may have multiple HDMI inputs. Thankfully, these are usually numbered to correspond with the inputs HDMI channel on, say your TV. Make sure you note which input you are using on the monitor. Switch to that input. For example, I have plugged my HDMI cable into my TV into the HDMI 2 input, so now I have to change the TV’s input to HDMI 2 (usually by hitting the source or input button on your remote until you hit it.</p>
<p>Plug your Pi into its power source and wait for it to boot. If all goes well, you will see a rainbow image and then a ton of code running on the screen while it boots.</p>
<p><strong>NOTE:</strong> If you see nothing and you followed the set-up, you may have to change some code in the <code>config.txt</code> file. See Raspberry Pi’s documentation for <a href="https://www.raspberrypi.org/documentation/configuration/config-txt.md">more information on</a> <code>config.txt</code>.</p>
<p>You should now see your Pi’s desktop. Make sure you have a mouse and keyboard plugged into your Pi. Explore around a bit. You can play games, use some interesting tools, etc.</p>
<p>Lastly, make sure to connect to the internet. If you have purchased a Wifi dongle with your Pi (it looks like a little USB stick), plug that into your Pi. To connect to Wifi, select the network symbol in the top right of the screen. Select your Wifi network and enter your password.</p>
<p>Otherwise, connect your Pi directly to one of your router’s (usually 4) ethernet ports with your ethernet cable.</p>
<h2 id="update-your-pi">Update your Pi</h2>
<p>Now that your Pi is up and running, open your Pi’s terminal (command line).</p>
<p><strong>NOTE:</strong> You can also run your Pi directly from your own computer using a secure shell or SSH. This is a very simple method to run your Pi from your own computer’s command line. See Raspberry Pi’s <a href="https://www.raspberrypi.org/documentation/remote-access/ssh/">documentation on SSH</a>. This is purely preference. SSH-ing is pretty darn cool and can help if you don’t want cords running every which way out of your Pi. I prefer using the Pi’s terminal.</p>
<p>Although you have the latest operating system image, you will still need to upgrade your packages since many have had updates since the original operating system was written and published. To do this, you will need to run two commands integral to Linux operating systems (which Pi is based off of):</p>
<p><code>sudo apt-get update</code> grabs all of the packages to keep your system updated.<br>
<code>sudo apt-get upgrade</code> extracts and installs the downloaded packages.</p>
<p><strong>NOTE:</strong> You will be running these commands as the administrator or ‘superuser’ using the UNIX command <code>sudo</code>. If your Pi prompts you for a password, the default password is <code>raspberry</code>.</p>
<p>You may at this point receive an error telling you that the Pi has failed to fetch certain items. Simply run <code>sudo reboot</code> to restart your Pi and then run <code>sudo apt-get upgrade</code> again.</p>
<p><strong>NOTE:</strong> If your Pi ever fails to download any program or packages, it is usually good practice to <code>sudo reboot</code> and then to re-download whatever you were trying to install. This tends to fix the problem a majority of the time.</p>
<p>The problem with <code>sudo apt-get update</code> is that it saves the packages to your system regardless of whether you have enough space or not. Remember, your Pi is nowhere nearly as powerful as a laptop or desktop computer, nor does it have the same storage capacity. Everything is run on your Micro SD card (usually 4 or 8GB). You can always check your disk partition, size, and usage by typing <code>df -h</code> (view in GB) or <code>df -m</code> (view in MB) into the terminal.</p>
<p>Therefore, you will want to keep your Pi clean and lean. Once your packages have upgraded successfully, run <code>sudo apt-get clean</code> to dump your apt-get cache. <strong>Be Aware</strong> that this method dumps any memory from your <code>apt-get</code> folder. You may have other downloaded items. So be sure that you are willing to dump this information and that you are not currently trying to install or download anything else using <code>sudo apt-get install</code>.</p>
<h2 id="configure-the-camera">Configure the Camera</h2>
<p>To install the camera into your Pi, <a href="https://www.raspberrypi.org/help/camera-module-setup/">follow Raspberry Pi’s instructional video</a>.</p>
<p><strong>NOTE:</strong> Be careful touching any Pi components. If you have built up static electricity, you can short the electronics and ruin your Pi. The easy solution is to touch a non-coated pure metal to ground yourself. While it is not hugely likely you will short your Pi, especially if you are not directly touching components, it is safe practice to ground yourself. <strong>Remember</strong> you accept full responsibility if you have to replace parts.</p>
<p>To configure your camera, run <code>sudo raspi-config</code>. Using your up and down keys, scroll down to <code>Enable Camera</code> and hit enter to turn the camera on. This is also a good time to expand your SD card partition. This is optional, but it allows you to use the entire SD card to store your files. If you do not expand your partition, you may run into problems installing programs in the future. To do this, within <code>raspi config</code> scroll to <code>Expand Filesystem</code> and hit enter to partition the card. See Raspberry Pi’s <a href="https://www.raspberrypi.org/documentation/configuration/raspi-config.md">official documentation</a> for more options on configuring your Pi (for instance, you may want to overclock your Pi to get the most power out of it).</p>
<p>Now scroll down to the bottom with your down key, hit the right side key, and then hit enter to finish to configuration. Your Pi will now reboot.</p>
<p>To test if you Pi camera is working, open the terminal again and type <code>raspistill -v -o test.jpg</code>. This turns the camera on, captures the image, and saves it with the specified name (i.e. in the above example, it saves it as <code>test.jpg</code>). The camera will turn on and <strong>after 5 seconds</strong> will capture an image. This gives you enough time to focus the camera on your subject.</p>
<p>Keep an eye on your working directory in the terminal. Unless you have used the <code>cd</code> method to change directories to this point, you should still be in the main directory. you can always type <code>ls</code> into the terminal to view a list of the current directory’s files. On your desktop, navigate to the main directory with you Downloads, Desktop, Music, Pictures, etc. folders. You will see <code>test.jpg</code>. Double click it to view your new photo.</p>
<h3 id="download-the-ocr-engine">Download the OCR Engine</h3>
<p>Now that we can take photos, we will download the OCR technology to convert our photos of text to actual text files. We will be using <a href="https://github.com/tesseract-ocr/tesseract">Tesseract</a>, a free open source OCR engine sponsored by Google.</p>
<p>In the terminal type <code>sudo apt-get install tesseract-ocr</code>.  This will install Tesseract to your Pi, allowing you to run the program from your terminal.</p>
<p>At this point, we will not attempt to take a photo of a document and convert it to a text file using Tesseract. It is very difficult to center and focus the camera on your paper document without some contraption to help you. We will build a simple holder at the end of this lesson.</p>
<p>If you would like to try it out or you have already built a case for you Pi camera, capture an image using <code>raspistill -v -o ocr.jpg</code> and then run <code>tesseract ocr.jpg ocr.txt</code> to convert the image to an OCRd text file. <strong>Remember</strong> that when running a command on a file in the terminal, you must to either run that command in the same directory as the file, or specify the path to the file. For example, if I run <code>raspistill -v -o ocr.jpg</code> in the <code>Pictures</code> directory, but attempt to use Tesseract in the <code>Desktop</code> directory, I will have to run <code>tesseract ~/Pictures/ocr.jpg ocr.txt</code>. This will convert <code>ocr.jpg</code> found in the <code>Pictures</code> directory and output the <code>ocr.txt</code> file to the <code>Desktop</code> directory.</p>
<p><strong>NOTE:</strong> Converting images to text files using Tesseract takes time since your Pi does not have nearly as much processing power as a laptop or desktop computer.</p>
<h2 id="configuring-sound-and-speech">Configuring Sound and Speech</h2>
<p>Next we will want to configure our Pi’s sound output, text to speech, and then speech to text software. To begin, let’s make sure you have audio working on your Pi.</p>
<p>The core engine of our audio is <a href="http://www.alsa-project.org/main/index.php/Main_Page">Alsa, Linux’s sound system</a>. Run <code>sudo apt-get install alsa-utils</code> to either download or update Alsa.</p>
<p><strong>Remember</strong> if you run into any errors downloading, first check your internet connection and then run <code>sudo reboot</code> to restart your Pi.</p>
<p>Next, load your sound drivers by running <code>sudo modprobe snd-bcm2835</code> (you can make sure the drivers have loaded by running <code>sudo lsmod | grep 2835</code>).</p>
<p><strong>NOTE:</strong> This tutorial assumes that you want to output sound to your TV using HDMI. If you are instead using headphones or speakers plugged into your Pi, skip the following method to edit the system configuration file.</p>
<p>Now let’s make sure your pi can output sound to your TV via the HDMI cable. Open the system configuration file with <code>sudo nano /boot/config.txt</code>. We can now edit this file within the terminal. The configuration files is simply a list of commands your Pi uses. You will notice that most of these commands have a hashtag <code>#</code> in front of them. The <code>#</code> is a commenting convention in the text file. Any line with a <code>#</code> in front of it is treated by the computer as a comment for humans to read. This allows programmers to explain code in plain English without the computer reading it. However, it also allows programmers to input actual code into a file but to make it unreadable by the machine. This allows other programmers to choose whether or not they want to use that code. We want to use a line in that code. Search for <code>hdmi_drive=2</code> and uncomment that line by removing the <code>#</code>. To save <code>config.txt</code>, hit <code>control</code> and the letter <code>o</code> together. Now to exit the configuration file and return to the main terminal view, hit <code>control</code> and the letter <code>x</code>.</p>
<p>To test audio on your Pi, run <code>speaker-test -t sine -f 440 -c 2 -s 1</code> in the terminal. This will output a long beeping sound in the form of a sine wave. To test the speech audio output of your Pi, run <code>aplay /usr/share/sounds/alsa/Front_Center.wav</code>. You will hear a voice saying “Front and Center” on your TV.</p>
<p><strong>NOTE:</strong> You may experience a 2-3 second delay in sound via HDMI. Unfortunately, this is an issue with the Pi connected via HDMI. A possible workaround is to edit your text file to include a few nonsense words at the beginning.</p>
<h3 id="configure-text-to-speech">Configure Text to Speech</h3>
<p>Now let’s install <a href="http://espeak.sourceforge.net/">Espeak, a speech synthesizer</a> we will use for our text to speech method. In the terminal, run <code>sudo apt-get install espeak</code>. Once it has installed, run <code>espeak "Initialize espeak! Hello, it's me. I was wondering if after all these years you'd like espeak"</code> to test espeak.</p>
<h3 id="configure-speech-to-text">Configure Speech to Text</h3>
<p>Lastly, we want to install a program to convert our computer-spoken audio file back into a text file. For this tutorial, we will be using <a href="http://cmusphinx.sourceforge.net/wiki/">PocketSphinx</a> since we can run it on our Pi offline. See <a href="https://raspberrypi.stackexchange.com/questions/10384/speech-processing-on-the-raspberry-pi/10392">this StackOverflow thread</a> on other great options for your Pi.</p>
<p>We will have to install the Sphinx base in order to get Pocket Sphinx to work. To begin, open your terminal and download both Sphinx <code>wget http://sourceforge.net/projects/cmusphinx/files/sphinxbase/0.8/sphinxbase-0.8.tar.gz</code> and then PocketSphinx <code>wget http://sourceforge.net/projects/cmusphinx/files/pocketsphinx/0.8/pocketsphinx-0.8.tar.gz</code>.</p>
<p>Now extract Sphinx <code>tar -zxvf pocketsphinx-0.8.tar.gz</code> and then PocketSphinx <code>tar -zxvf sphinxbase-0.8.tar.gz</code>. Once you have successfully extracted these files, you can remove the two downloads by running <code>sudo rm -rf pocketsphinx-0.8.tar.gz</code> and then <code>sudo rm -rf sphinxbase-0.8.tar.gz</code>.</p>
<p>We need <a href="https://en.wikipedia.org/wiki/GNU_bison">Bison</a> to compile the Sphinx packages. Install Bison by running <code>sudo apt-get install bison libasound2-dev</code> in the terminal.</p>
<p>Once Bison has successfully installed, change directories to the extracted files. To check what they are called exactly, type <code>ls</code> into the terminal to show the contents of your main directory. They should be called <code>sphinxbase-0.8</code> and <code>pocketsphinx-0.8</code>.</p>
<p>Let’s fully compile Sphinx base first. First <code>cd sphinxbase-0.8</code>. Then <code>./configure --enable-fixed</code>. Now run <code>sudo make</code>. This may take several minutes so be patient. Once that is successful, run <code>sudo make install</code>. Allow Sphinx to fully install.</p>
<p>Now change back to the main directory by running <code>cd ..</code>. Now let’s compile PocketSphinx. First <code>cd pocketsphinx-0.8</code>. Now run the same commands as above:<br>
<code>./configure --enable-fixed</code>, <code>sudo make</code>, and then <code>sudo make install</code>.</p>
<p>Sphinx will take a bit of time to compile. Be patient and watch ther terminal for any errors from the compiler.</p>
<h2 id="run-ocr-convert-text-to-speech----speech-to-text">Run OCR, Convert Text to Speech --&gt; Speech to Text</h2>
<p>To begin,</p>
<p>To play your wav file type <code>aplay ocr.wav</code>.</p>
<p>pocketsphinx audio to text requires the wav file to be 16000 hertz so we will <code>sudo apt-get install sox</code>. Once sox install, type <code>sox ocr.wav -r 16000 16000.wav</code> to create a wav file called <code>16000.wav</code>  from <code>ocr.wav</code>.</p>
<p>Next run <code>pocketsphinx_continuous -infile 16000.wav</code> to generate text from our wav audio file. This will run speech recognition on our wav file in the command line. It may take 5-10 minutes so sit back! Copy the output text from the command line. Open the text editor by selecting <code>Menu &gt; Accessories &gt; Text Editor</code> and paste the text into the editor. Save this file as convert.txt.</p>
<h2 id="compare-text-files">Compare Text Files</h2>
<p>We will be using a python script to compare our two text files. Open the Pi text editor and paste in the following code:</p>
<pre><code>from difflib import SequenceMatcher

import RPi.GPIO as GPIO

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)
GREEN_LED = 18
RED_LED = 23
GPIO.setup(GREEN_LED, GPIO.OUT)
GPIO.setup(RED_LED, GPIO.OUT)

text1 = 'ocr.txt'
text2 = 'conversion.txt'
m = SequenceMatcher(None, text1, text2)
print m.ratio()*100

weight = m.ratio()*100

if weight &gt; 70:
      GPIO.output(GREEN_LED, True)
      GPIO.output(RED_LED, False)
  else:
      GPIO.output(GREEN_LED, False)
      GPIO.output(RED_LED, True)
</code></pre>
<p>This code compares <code>ocr.txt</code> and <code>conversion.txt</code>. If they are 70% or more similar, flash the green LED light on the breadboard. But if they are less than 70% similar, flash the red LED light on the breadboard. Save the file as <code>ocr.py</code> in <strong>the same directory as your two text files.</strong></p>
<p>Now with all the files in the same directory, in your terminal run <code>sudo python ocr.py</code>. Depending on the ratio of your texts, you will see a green or red light.</p>
<p>You will notice that the LED has stayed on. To turn off your LED, we will create another python script called <code>off.py</code></p>
<pre><code>import RPi.GPIO as GPIO

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)
GREEN_LED = 18
RED_LED = 23
GPIO.setup(GREEN_LED, GPIO.OUT)
GPIO.setup(RED_LED, GPIO.OUT)
GPIO.output(GREEN_LED, False)
GPIO.output(RED_LED, False)
</code></pre>
<p>Now run <code>sudo python off.py</code> to turn off the LED.</p>
<p>Text to OCR works very well. OCR’d text to audo works well. Audio to text is horrible</p>
<p>Done!</p>
