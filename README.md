### beton brut
### noah tanner
### summer 2024

PHOTO, CENTERED
beton brut is a brutalist inspired, 66pc keyboard based on the Leopold FC660M layout complete with a concrete case designed from scratch and built by noah tanner.
this readme file will outline the design and build process, while highlighting technical details in a way that hopefully is not too boring. if i do this all correctly, this document should be guiding and inspiring enough for people to embark on their own rewarding journey of making a keyboard from scratch (it is so satisfying typing this document on my own keyboard).

#### table of contents
1. [preliminary-design](#preliminary-design)
2. [electronics](#electronics)
3. [firmware](#firmware)
4. [case](#case)
5. [other-items](#other-items)
7. [build](#build)
8. [reflection](#reflection)
9. [license](#license)
10. [acknowledgements](#acknowledegments)

### preliminary-design
the motivation for this project was enhance my electronics and product design skills by designing an aesthetic keyboard that pulled visual influence from 1950s [brutalist](https://en.wikipedia.org/wiki/Brutalist_architecture) architecture. starting with youtube videos and some internet browsing, i was able to breakdown the keyboard build process into a few parts: electronics, case, keys, and then the inbetween "joinery" stuff like gaskets and peripheral items to secure the pcb in the case. i have a background in mechanical engineering so i was familiar with micro controllers and electronics, but i had not designed a pcb before so i knew that would be the most difficult part of the project. 

for the keyboard design and layout, i went on keyboard layout editor ([kle](http://www.keyboard-layout-editor.com/())) and searched through default layouts. i was really hooked by the layout of the leopold fc660 with the two offset keys and the compact layout wihtout sacrificing the arrow keys. since it was going to be my first full custom pcb, i decided to move forward with the leopold layout rather than trying to create a new one for myself. as for the look of the case, i wanted it to be a simple slab of concrete, very minimally finished and decorated in order to align with the principles of brtualism. i chose a 6 degree angle to keep the case relatively flat, but still offer me some angle to align with my natural wrist positioning.

### electronics
i used kicad for pcb design. it is a great product and extremely easy to get familiar with. for the first iteration, i referenced a [video](https://www.youtube.com/watch?v=iznKltVU1yw) by noah kiser on youtube that showed the process of creating a keyboard pcb that used a teensy microcontroller unit and through hole diodes. noah detailed all of the footprints that would be used in keyboard making and where to find them online which was really helpful. that guide worked great up until the point where i started routing my martrix to the teensy and realized that the 18 gpio pinouts on the teensy were not enough to meet my 20 needed (5 rows and 15 columns). a quick reddit post about my dilemma and a few hour wait brought me into contact with markus knutsson ([github](https://github.com/TweetyDaBird/)). markus recommended that i alter my matrix layout in a way that would work, but would be quite ugly, or alternatively replace the teensy with an rp2040. from the start of the project, i was aware of using an on board mcu, but i was too nervous to setup the peripheral devices and risk messing it up. after reading marcus' reply, i checked online about the rp2040 mcu and decided to give it a try. having used raspberry pi devices in the past, i know their documentation is great, and that proved to be the case with the rp2040 as well. the [datasheet](https://datasheets.raspberrypi.com/rp2040/rp2040-datasheet.pdf?_gl=1*18x0kee*_ga*MTU3NjQ0Mzc0Ny4xNzI0NTQ0NTU5*_ga_22FD70LWDS*MTcyNDU0NDU1OS4xLjEuMTcyNDU0NDU4MC4wLjAuMA..) and the [hardware-design-with-the-rp2040](https://datasheets.raspberrypi.com/rp2040/hardware-design-with-rp2040.pdf?_gl=1*18x0kee*_ga*MTU3NjQ0Mzc0Ny4xNzI0NTQ0NTU5*_ga_22FD70LWDS*MTcyNDU0NDU1OS4xLjEuMTcyNDU0NDU4MC4wLjAuMA..) were all i needed to set up the rp2040 on my pcb. the hardware design manual had a very well written out process for setting up the rp2040 with qpsi flash storage, a usb port, a crystal, and information on sizing capacitors and resistors as well. thank you so much raspberry pi for making a datasheet that 1. doesn't suck, and 2. is't boring to read. 

ITERATION 1 PHOTO

after reading through these two manuals and with a few hours of work i was able to setup my first iteration of the pcb board. i was pretty hyped. i sent it over to marcus to get some feedback and he let me know that it was pretty terrible and problematic. the problems came in two forms. the first was that i routed every line to the outsides of the board, thinking that it would make sense to keep things away from the center and not crowd the board. while this worked out for the crwoding issue, it was a terrible design choice. routing traces to the edges of a board is bad for electromagnetic interference and could lead to issues with other electronics on my desk near the keyboard. the second issue with the board was that i did not use any vias. my reasoning for this came down to a lack of experience and pcb familiarity. i thought vias were a last resort thing and justified that, if i can put everything on one side, why not do that? inversely though, why not divide things on both sides of the board using vias and route N->S one side, and E->W on the other. 

ITERATION 2 PHOTO 

building off the feedback from marcus, i started over from scratch and rerouted the whole board. as you can see in the photos, it looks so much better than the first iteration. routing across the two sides was way quicker than the first time thanks to the use of vias. after routing, i ran the kicad design rule checker (drc) and found a comically high number of warnings, 188 to be exact. luckily there were 0 errors, and the warnings were all about silkscreen overlaps. at this point i was just tidying up the loose ends. i fixed the silkscreen overlaps by moving the silkscreened component names around on the board and added a border for the board production. the 3d model looked great now, no errors, and marcus verified everything looked correct and was ready for production. 

i ordered everything off of jlpcb, and this was esaily the most confusing part of the project. jlpcb makes it really easy to go through the steps of uploading the files that you need like the gerbers, positioning, and bom. despite this, the component selection was difficult to navigate and find what piece you needed. i ended up having to reorder this a few times and ran into some errors of footprint sizing which meant i had to go back into kicad, select new footprints, and then re-export the gerbers again. luckily, the jlpcb staff is incredibly helpful and always avaiable online. no joke, they have a nigh shift team so any question i had, i would ask in the live chat and have a response in minutes. 

that wraps up the electronics.

### firmware
shoutout to [qmk](https://qmk.fm/). without them, this would not be possible. qmk makes the firmware sooo much easier, and they have extensive support for many mcus, including the rp2040. going through the qmk setup is pretty straightforward and i won't dive into it too much because i am not an expert on it. the main issue i had with qmk was the lack of documentation they had for their new config.json file. the setup through the config.json file is supposed to make the keyboard setup easier than it previously was. unfortunately, there is not a lot of good documentation about the config.json file itself and the current tutorial setup referes to the rules.mk file and the "old" way of doing things, so it got a bit tricky to navigate. regardless, the cli tool and qmk firmware as a whole are incredible projects and i cannot wait to work more with them in future keyboards. 

### case
to design the case, i needed to finalize the pcb to have information on the dimensions to build around. sticking to my original design, i made a really simple slab case in fusion 360 with a 6 degree angle. the pcb laid in side of the case, contacting with the case on only the edges of the pcb board. a tricky part of this design was going to be the usb-c cable bceause i did not want to leave the area above it exposed. to make the mold in fusion, i made another component and made a rectangular body a bit bigger than the case. aligning the bottom face of the case with a face of the new body, i then used the subtract tool to create a negative of the case. i split this negative into 4 components so that they could be 3d printed and then fastened together. i fastened them using sikaflex which was a mistake because the pieces were so tightly bound that when it was time to remove the mold, they would not budge from eachother. in short, this mold did not work. i used a grout mix for the first casting and that part worked well, but i could not get the case out of the mold when it had dried. i expected this to be somewhat of a problem, but it was much more severe than i anticipated. the case had no drafted edges, so each of the corners effectively "sucked" in the concrete. i tried using a pneumatic powered saw to cut away the sides of the case and pull it out slowly, but it ended up breaking the case. the other massive issue with this case was usb-c port design. it was an exposed hole through the wall of the case, so the mold would have concrete around it, and no easy way to remove the mold. this was not going to work and i needed a new solution. see the photos down below. 

MOLD 1 PHOTO

for the second attempt, i used a cement mix instead of the grout bceause the cement would look a lot better. for the new mold design, i added filleted corners on all of the previosuly 90 degree joints, hoping that it would pop out a lot easier. i also put some draft angles in the center parts of the mold that can be seen in the photos below. for the usb-c port, i came up with a great design that left the area above the port open, and had a separate insert piece to fit over that, and rest across the board. this solved the usb-c issue and also occupied the blank space on the pcb between the main keys and the offset two piece keys. lastly, i used clamps rather than sikaflex so that when the case had dried, i could remove each quadrant with ease. despite all these changed, it was still not quite there! the case was too fragile and broke again when i was taking it out. i think this was due to the design angles again, as well as the material. the outer walls of the case were too short and did not cover the key switches as much as i wanted. the top case piece broke as well because there were too many 90 degree angles, some drafts and fillets should fix that easily. time for a third iteration.

MOLD 2 PHOTO

in the third attempt, i removed the fillets and added more draft angles on every inner wall of the mold. i raised the wall height as well and was ready to go. for the third mix, i used the same cement, but added a structural adhesive to help out with the strength. after coating the mold in wd40 to help with the release, casting, and waiting, i took it apart and the case came out great! the insert piece worked great as well, but i forgot to invert the mold so that the exposed part of the cast was the piece that would be facing up in the final design. you an see this in the way that it contrasts a bit with the case in the final version, but i will keep it like that, i kind of enjoy it. for finishing, i coated the case in laquer a few times and then added a few coats of johnson paste wax. since the cement is so porous, the final result is really smooth and not sticky. i am really impressed with the finish.

MOLD 3 PHOTO

### other-items
here is everything else i purchased, it is all self-explanatory:
1. [keycaps](https://www.aliexpress.us/item/3256806228987028.html?spm=a2g0o.order_list.order_list_main.5.7f441802ZPgCd5&gatewayAdapt=glo2usa)
2. [butyl](https://www.aliexpress.us/item/3256806034781549.html?spm=a2g0o.order_list.order_list_main.10.7f441802ZPgCd5&gatewayAdapt=glo2usa)
3. [usb-c](https://www.aliexpress.us/item/3256805246064043.html?spm=a2g0o.order_list.order_list_main.15.7f441802ZPgCd5&gatewayAdapt=glo2usa)
4. [stabilizers](https://www.aliexpress.us/item/3256801499984864.html?spm=a2g0o.order_list.order_list_main.20.7f441802ZPgCd5&gatewayAdapt=glo2usa)
5. [cherry-mx-black](https://www.aliexpress.us/item/3256806069646359.html?spm=a2g0o.order_list.order_list_main.25.7f441802ZPgCd5&gatewayAdapt=glo2usa)
6. [foam-pads](https://www.aliexpress.us/item/3256804440346820.html?spm=a2g0o.order_list.order_list_main.30.7f441802ZPgCd5&gatewayAdapt=glo2usa)
7. [tactile-switch](https://www.aliexpress.us/item/3256806891393231.html?spm=a2g0o.order_list.order_list_main.35.7f441802ZPgCd5&gatewayAdapt=glo2usa)

LIST

### build
when all of the items came in, i soldered the switches and buttons onto the board, making sure to put the foam patches underneath the switches. after flashing the firmware and testing the board, the rest of the assembly was pretty straightforward. throughout the case build, i had been periodically placing the pcb in the failed builds to make sure that it was sitting in the case properly. to protect the edges of the board and secure it in the case, i used butyl tape and put it on the edges of walls next to where the board sat, as well as a tiny bit underneath the board to grip it well. the one thing to note is that the final design of the top insert piece that covers the usb-c nudges up a bit too closely to the right arrow key and i had to sandpaper it down a lot to make it fit. i doubt anyone will be replicating this build. but if one were to, this would need to be changed or sanded down again. otherwise, the build worked like a dream and the case is really solidly held together.

### reflection
i am so happy with how this build came out. i learned so much throughout this project and it would not have been possible without marcus knutsson, noah kiser, raspberry pi, the kicad forum, and my father. reflecting upon this project, i think the biggest success was the pcb design itself, but the case was relatively rudimentary and would have benefitted from some stand off mounting posts, and more professional mounting hardware than just the butyl tape. that being said, i would need to include screw post holes on the pcb as well, something i did not even vaguely consider at the time. once again, massive shout out to everyone that helped me, if you have read through this far, please take a minute to go check out some of their work, linked in [acknowledgements](acknowledgments). lastly, if you are building a keyboard and somehow found yourself here, feel free to contact me with any questions. it seems like a daunting task, but its fun and **extremely** rewarding and i would love to help out in the way that others helped me. 

### license
MIT License

Copyright (c) [2024] [noah tanner]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

### acknowledegments
- [marcus knutsson](https://github.com/TweetyDaBird/)
- [noah kiser](https://www.youtube.com/watch?v=iznKltVU1yw)
- [qmk](https://qmk.fm/)
