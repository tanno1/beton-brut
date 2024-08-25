### beton brut
### noah tanner
### summer 2024

PHOTO, CENTERED
beton brut is a brutalist inspired, 66pc keyboard based on the Leopold FC660M layout complete with a concrete case designed from scratch and built by noah tanner.
this readme file will outline the design and build process, while highlighting technical details in a way that hopefully is not too boring. if i do this all correctly, this document should be guiding and inspiring enough for people to embark on their own rewarding journey of making a keyboard from scratch (it is so satisfying typing this document on my own keyboard).

#### table of contents
1. [preliminary-design](#preliminary-design)
2. [electronics](#electronics)
3. [case](#case)
4. [keys](#keys)
5. [joinery?](#joinery)
6. [ordering-process](#ordering-process)
7. [build](#build)
8. [reflection](#reflection)
9. [license](#license)
10. [acknowledgements](#acknowledegments)

### preliminary-design
the motivation for this project was enhance my electronics and product design skills by designing an aesthetic keyboard that pulled visual influence from 1950s [brutalist](https://en.wikipedia.org/wiki/Brutalist_architecture) architecture. starting with youtube videos and some internet browsing, i was able to breakdown the keyboard build process into a few parts: electronics, case, keys, and then the inbetween "joinery" stuff like gaskets and peripheral items to secure the pcb in the case. i have a background in mechanical engineering so i was familiar with micro controllers and electronics, but i had not designed a pcb before so i knew that would be the most difficult part of the project. 

for the keyboard design and layout, i went on keyboard layout editor ([kle]http://www.keyboard-layout-editor.com/()) and searched through default layouts. i was really hooked by the layout of the leopold fc660 with the two offset keys and the compact layout wihtout sacrificing the arrow keys. since it was going to be my first full custom pcb, i decided to move forward with the leopold layout rather than trying to create a new one for myself. as for the look of the case, i wanted it to be a simple slab of concrete, very minimally finished and decorated in order to align with the principles of brtualism. i chose a 6 degree angle to keep the case relatively flat, but still offer me some angle to align with my natural wrist positioning.

### electronics
i used kicad for pcb design. it is a great product and extremely easy to get familiar with. for the first iteration, i referenced a [video]https://www.youtube.com/watch?v=iznKltVU1yw) by noah kiser on youtube that showed the process of creating a keyboard pcb that used a teensy microcontroller unit and through hole diodes. that guide worked great up until the point where i started routing my martrix to the teensy and realized that the 18 gpio pinouts on the teensy were not enough to meet my 20 needed (5 rows and 15 columns). a quick reddit post about my dilemma and a few hour wait brought me into contact with markus knutsson ([github](https://github.com/TweetyDaBird/)). markus recommended that i alter my matrix layout in a way that would work, but would be quite ugly, or alternatively replace the teensy with an rp2040. from the start of the project, i was aware of using an on board mcu, but i was too nervous to setup the peripheral devices and risk messing it up. after reading marcus' reply, i checked online about the rp2040 mcu and decided to give it a try. having used raspberry pi devices in the past, i know their documentation is great, and that proved to be the case with the rp2040 as well. the [datasheet](https://datasheets.raspberrypi.com/rp2040/rp2040-datasheet.pdf?_gl=1*18x0kee*_ga*MTU3NjQ0Mzc0Ny4xNzI0NTQ0NTU5*_ga_22FD70LWDS*MTcyNDU0NDU1OS4xLjEuMTcyNDU0NDU4MC4wLjAuMA..) and the [hardware-design-with-the-rp2040](https://datasheets.raspberrypi.com/rp2040/hardware-design-with-rp2040.pdf?_gl=1*18x0kee*_ga*MTU3NjQ0Mzc0Ny4xNzI0NTQ0NTU5*_ga_22FD70LWDS*MTcyNDU0NDU1OS4xLjEuMTcyNDU0NDU4MC4wLjAuMA..) were all i needed to set up the rp2040 on my pcb. the hardware design manual had a very well written out process for setting up the rp2040 with qpsi flash storage, a usb port, a crystal, and information on sizing capacitors and resistors as well. thank you so much raspberry pi for making a datasheet that 1. doesn't suck, and 2. is't boring to read.

### case
### keys
### joinery
### ordering-process
### build
### reflection
### license
### acknowledegments
