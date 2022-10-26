# Manuel The Grit

created by:
Anwar Bouzoubaa
26-10-2022

## About this project

At the moment I’m creating a IoT device called The Grit. The Grit is a smart cat litter box that can help you with cleaning and filling it automatically. At the moment I’m creating a prototype with a NodeMCU, some sensors and i’m connecting it with some API using the internet. In this document I will show you my process of connecting a sensor and a API. The Grit will have a reservoir which you can fill with litter. Via the app the user could add a Google account and an event would be added when the sensor detects that the litters is almost finished. The event would state that the user needs to buy litter tomorrow. For this prototype I want to push a button for 4 times and on the 4th time the system send this event.

## What you need

1. NodeMCU ESP8266 (You can use something else like a Arduino but pay attention to the ports and need of wifi)
2. A hotspot with 4g (I used my phone. Make sure to put it on 4G not 5G)
3. A button switch

## Create a zap ⚡️

For this prototype I wanted to use Zapier. Zapier can automate workflows by using triggers to connect to actions. I created a feed on Adafruit.io called TheGrid and used this as a trigger in Zapier. I followed the steps but I got this message when I wanted to test the trigger.

img

Inside my feed in [Adafruit.io](http://Adafruit.io) I added a value ‘0’ I when I tested it again it worked. This was the data I got back in Zapier .

img

I created an action with Google Calendar. So if a Value is been added in my feed I wanted to create a new event on my Agenda. I selected “Create Detailed Events” and connected my Google account. I filled all the information that I needed. For time I looked up what was supported. I filled in ‘Tomorrow 5pm’ because after the litter is almost out I want to buy it tomorrow. This way I don’t have to check which day it is today and add 1 day to it. When I tested the zap I got something I didn’t expect. I wanted to add an event **for** tomorrow but instead it created an event for today **till** tomorrow 😲.

img

img

When I looked back to what I filled I saw that I filled it incorrect. I added the location at the starting date and I added the dat at the ending date. I fixed the issue and when I tested it again it worked 😎! I published the Zap and did the next step.

img

## Connect NodeMCU to Feed 🔍

In the library manager in Arduino IDE I installed 'Adafruit IO Arduino'. I openend the example file 'Adafruitio_20_shared_feed_write.in' and filled in my credentials in the config file. I changed the ports that where corresponding with click button on my NodeMCU. I filled in the name of the feed I created earlier and published the code. After 1 minuted I checked my agenda and I got this.

img

It works but every time I push the button It creates a new event and when I release the button it creates another event. So If I keep pressing I would create too many events. In order to test some new functionalities I created a new feed called ‘GritTesting’ so no new events would be created.

## If 4 times pressed add event 🎉

I created a new variable number with the value of ‘0’ and every time I click the button I wanted the number to add 1 and if the number returns 4 I wanted to send that value to the Adafruit Feed so It will create an event automatically. But the code that i wrote didn’t do what I expected. With the first attempt my code got send every second and I didn’t know why. I also got an error in Adafruit because I was sending too much data. I would have a big problem if I still connected my Google agenda with this test 😅.

img

I changed the position of the ‘number++’ to only add if the button is being pressed. I got an error.

img

I tried to look online but couldn’t find any quick answer ioso I tried to write this code how I would write javascript. I placed brackets where I wanted to have my function instead of only a tab. Later on I understood that if you write an If statement you can leave the brackets if you only execute one thing. Because I did two I got this error. I tried many different ways to write the code but they did not what I expected because what i mentioned above. You wont get an error if you write only an is statement where you write multiple execution underneath each other. Here you can see al my trials.

Eventually it worked 🤗!!

## Create a server 🏄🏻‍♂️

I wanted the information also to been displayed on a user interface. So I created a server on the NodeMCU to display a very simple bar that would increase if I click on the button. I found this youtube video made by [Binary Updates](https://www.youtube.com/watch?v=pqaaPSRiYec&ab_channel=BINARYUPDATES) , explaining what I should do. But when I tried to implement the code It didn’t work. I got this in my serial monitor.

img

I looked for another youtube video and I found one made by [Anas Kuzechie](https://www.youtube.com/watch?v=eHxkZ7poKHc&t=267s&ab_channel=AnasKuzechie). I implemented his code and everything worked!! Than I tried to implement the other code with pushing the button to send it to Adafruit if I pressed 4 times. Everything worked fine!! Now I wanted to create a simple webpage with a bar. I created 2 other pages that I wanted to show if I click on the button. I named them the percentage that the reservoir would be empty. When I pushed to code I got this error 😫.

img

I had to also rename the variable inside of the file and when I changes it to the correct name It worked 🤗. Then in the function that pushed the html file I created a condition. If the number was 0; show page with full reservoir, if the number was 1 because I pressed ones on the button; show the page with 75% full reservoir and if pressed twice the number should be 2 and I wanted to show page with 50% full reservoir. When I tested it I noticed that I did’t show the pages. But when I refresh the page It would show the new page. I tried to run the function in the loop. But it didn’t refresh automatically. This is something I want to to next time. Here are the pages that will be show if you press the button.

img img img
