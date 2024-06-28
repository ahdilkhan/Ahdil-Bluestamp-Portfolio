
For my optical character recognition project, I utilized a Raspberry Pi + PiCamera to develop an efficent device capable of converting an image of text into machine-readable text format. For instance, if you were to scan a receipt, your computer saves the scan as an image file and then uses OCR technology to read the text. This technology is especially useful for self-driving cars since it is able to read number plates and road signs.


| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Ahdil K | Dougherty Valley | Computer Science | Incoming Senior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone



<iframe width="560" height="315" src="https://www.youtube.com/embed/SR7ORd67jkE?si=JdpPDmQDnqgwwdRc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


Hi my name is Ahdil and this is my milestone 3 video. For my milestone 3, I was able to make two modifications to my base project to make it have multiple features. The first modification I did was that I plugged in a speaker to my raspberry pi which was able to say out loud each word that was detected. The first issue I ran into while trying to do this was that the volume on the speaker was very low so after watching a few youtube videos, I was able to figure out that you can change the volume through VNC viewer which is what I did. The second modification I did was that I added an image generation feature which was able to create a pixelated image of the detected text. How it worked was when it prints the detected text on the screen, the algorithm would print a URL of the image and you can copy paste that URL on google to see the image. One issue I ran into while trying to do this was that I needed an efficient way to be able to generate the images so with the help of my instructor, I was able to get an Open AI API key which allowed the algorithm to run smoothly. Thank you.   



# Second Milestone



<iframe width="560" height="315" src="https://www.youtube.com/embed/1VhNpJ6V-xc?si=ig2Elgns4MIzpBMB" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Hi my name is Ahdil Khan and this is my milestone 2 video. For my milestone 2, I was able to set up my Ardu Raspberry PiCamera that was shipped to me and be able to remotely connect it to it using OpenCV and running a few commands/code to get it up and running. Then, I used the Picamera 2 library to process images which required me to use code to write an image from the pi camera to the folder which would then be able to be seen through selecting it in the VNC viewer. This worked successfully as I was able to see the area around me through the PiCamera. One issue I ran into was that I wasn’t sure how to get the camera display on the laptop but with a few tutorials on youtube, I learned that I had to run the code on vscode and then run a few commands on the VNC viewer terminal to actually connect back to that code. Another issue I ran into was that my camera would shut off after a couple seconds and I wasn’t sure why. After help from my instructor and looking back at the code, I figured out that I needed to change the sleep time of the camera so it stays on for longer. My next step in the process was for my PI to recognize text in live images via the PiCamera. To accomplish this, I created a new file with sample code that my instructor provided me as well as various commands on the terminal which did the trick and the camera was able to recognize texts. For my milestone 3, I am going to use a speaker and my current raspberry pi to convert the recognized text into verbal speech. Thank you. 

# First Milestone



<iframe width="560" height="315" src="https://www.youtube.com/embed/sm7fES8VpNg?si=NbQ9KfNLbPfxW1GS" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


For my first milestone, I set up the hardware components of my Raspberry Pi such as the case, USB wires and power cable connecting to the PI. Problems I ran into while doing this was setting up the fan to make sure the heatsinks on the motherboard don't overheat. I wasn't sure how/where to set up the fan in the Raspberry Pi but through youtube tutorials, I was able to figure out how to connect it and have it start working. After the hardware setup, I was able to flash the SD card with the Raspberry Pi OS and wifi configuration. Then, I was able to remotely connect the Raspberry Pi through SSH and with the connection via VNC viewer, we did not need a keyboard or mouse connection. Originally, I was not able to connect through VNC viewer due to a error that said connection refused. I watched a youtube tutorial that mentioned that you have to enable VNC server on the Raspberry Pi before the VNC viewer connection. Lastly, I used VS code to write a small python script to test if I can push the code into Raspberry Pi which worked successfully. My next steps in completing the project is getting the PiCamera set up as well as get to the begenning stages of setting up the OCR software. 

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code


```python
import cv2
import pytesseract
from pytesseract import Output
from picamera2 import MappedArray, Picamera2, Preview
from espeak import espeak
from time import sleep
from openai import OpenAI
import os
 
#cap = cv2.VideoCapture(0)
#cap.set(cv2.CAP_PROP_BUFFERSIZE, 1)
picam2 = Picamera2()
picam2.configure(picam2.create_preview_configuration({"size": (1024, 768)}))
picam2.start_preview(Preview.QTGL)
picam2.start()
 
i = 0
client = OpenAI(
    # This is the default and can be omitted
    
)

openai = OpenAI()


while True:
    #ret, frame = cap.read() # from the tutorial but outdated
    frame = picam2.capture_array()
 
    d = pytesseract.image_to_data(frame, output_type=Output.DICT)
    n_boxes = len(d['text'])
    #print(n_boxes)
    for i in range(n_boxes):
        if int(d['conf'][i]) > 60:
            (text, x, y, w, h) = (d['text'][i], d['left'][i], d['top'][i], d['width'][i], d['height'][i])
            # don't show empty text
            if text and text.strip() != "":
                frame = cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
                frame = cv2.putText(frame, text, (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 1.0, (0, 0, 255), 3)
                cv2.imwrite("/home/ahdilkhan/Documents/ocr"+str(i)+".png", frame) # exact file path will vary by user
                i += 1
                print(text)
                
              
                espeak.set_voice('english')
                espeak.synth(text)
                while espeak.is_playing():
        # espeak is asynchronous, so wait politely until it's finished
                    sleep(0.25)

                prompt = text
                model = "dall-e-2"
                
    # Generate an image based on the prompt
                response = openai.images.generate(prompt=prompt, model=model, size = "256x256", quality = 'standard', n = 1)

    # Prints response containing a URL link to image
                print(response)

 
    # Display the resulting frame
    cv2.imshow('frame', frame) # only works for VNC Viewer
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cv2.destroyAllWindows()
```



# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|

| Raspberry Pi4 Starter Kit | Used to run the OCR part of the project | $84.99 | <a href="https://www.amazon.com/Vemico-Raspberry-Starter-Heatsinks-Screwdriver/dp/B09QGZ94M8/ref=sr_1_20?crid=3EO43F1FEOV0O&dib=eyJ2IjoiMSJ9.ualQ11OTmi8CjjOvKHbGSeoxaNIDkPpfJZtZyzHol1FvvEglqiwyyx0fx51TZiZ1aBaberSc_J42HaMFrKc4G4wzNZJZEEpXeUe5uPFLGOfeZg1JiJOFIRBWErdjcMSJmt9OiXh3fiaEklJDqjCH8YliLFrOMz2gDd9N61tXfyLELptXi2DxstQVgqAxXv9vJnZG8mc1uPn3ogYVtajj4NEvfs_J7KSsaUplZk2LAkNdh8C7VZAVCj9KGMicav_2LQeL3moiJ-zNLr78csIQdysoo0AdWn9HClXObBEGA2A.R84P9iUHYtjBbieyHy8tFMnaqBUBiYhKHI1MA9uJP1A&dib_tag=se&keywords=raspberry%2Bpi%2Bkit&qid=1716420057&s=electronics&sprefix=raspberry%2Bpi%2Bki%2Celectronics%2C105&sr=1-20&th=1"> Link </a> |

| Ardu PiCamera | To take pictures and detect the text | $6.99 | <a href="https://www.amazon.com/Arducam-Megapixels-Sensor-OV5647-Raspberry/dp/B012V1HEP4/ref=asc_df_B012V1HEP4/?tag=hyprod-20&linkCode=df0&hvadid=693620629591&hvpos=&hvnetw=g&hvrand=15006871956907933718&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9032043&hvtargid=pla-820020083673&mcid=75fa5b0480f739c29fa68a28d965eee9&gad_source=1&th=1"> Link </a> |

| Mini External Speaker | To say the detected text out loud | $15.49 | <a href="https://www.amazon.com/Sanpyl-External-Speaker-NSP-100-Microphone/dp/B0816F2R56"> Link </a> |



# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
