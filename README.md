## Detecting border vectors with Teensy 4 + Pixy 2

### The Pixy 2

The Pixy 2 is a camera with many built-in functionalities such as object tracking and line detection. You can find many details and documentations at [Pixy2](https://pixycam.com/pixy2/). We decided to use it in this project because it allow us (in a certain level) to abstract all the image processing part and work directly with the border vectors of the track.

The following images show our first test setup and results, using directly the Pixy2.

![pixy2_pointing](https://raw.githubusercontent.com/Enseirb-NXP-Cup-2019/nxpcup.enseirb.github.io/master/img/pixy2-pointing.png)

![pixy2_with_vectors](https://raw.githubusercontent.com/Enseirb-NXP-Cup-2019/nxpcup.enseirb.github.io/master/img/pixy2-vector.png)

### The Teensy 4
The Teensy 4 is a powerfull microcontroller which can be programmed using the [Arduino IDE add-on](https://www.pjrc.com/teensy/td_download.html). In order to have the Teensy working we use a 5V micro-usb. For a first test, we connected it to the arduino IDE and tried to upload the simple example, which worked flawlessly.

### Mixing Teensy 4 + Pixy 2
Having both parts working together, now we must mix them in order to have the Pixy vectors being send to the Teensy. The key element to do it is the [Pixy 2 API for Arduino](https://docs.pixycam.com/wiki/doku.php?id=wiki:v2:full_api) which provides an easy way to access all data from the camera inside the microprocessor.

Using this API, we can make some very slight changes to the example of "line_get_all" which looks like:

```c
#include <Pixy2.h>

Pixy2 pixy;

void setup() {
  Serial.begin(115200);
  Serial.print("Starting...\n");

  // we need to initialize the pixy object
  pixy.init();
  // Change to line tracking program
  pixy.changeProg("line");
}

void loop() {
  int8_t i;
  char buf[128];

  pixy.line.getAllFeatures();

  // print all vectors
  for (i=0; i<pixy.line.numVectors; i++)
  {
    sprintf(buf, "line %d: ", i);
    Serial.print(buf);
    pixy.line.vectors[i].print();
  }

}
```

This is the very basic code that only displays what vectors are being readed from the Pixy 2. The following images show the final setup and the results.

![pixy2_with_vectors](https://raw.githubusercontent.com/Enseirb-NXP-Cup-2019/nxpcup.enseirb.github.io/master/img/final-setup.jpeg)

![pixy2_with_vectors](https://raw.githubusercontent.com/Enseirb-NXP-Cup-2019/nxpcup.enseirb.github.io/master/img/final-results.png)

If we take a look at the the Pixy 2 results, we can see it's detecting three vectors there are more or less always the same like:

```json
{
  line0: [[54, 15], [55, 0]],
  line1: [[68, 17], [70, 0]],
  line2: [[23, 10], [24, 0]]
}
```

To be sure the results look like we would expect (three more or less vertical bars) we can plot the results in the Pixy 2 vector capture format and check that we have three vertical vectors:

![pixy2_with_vectors](https://raw.githubusercontent.com/Enseirb-NXP-Cup-2019/nxpcup.enseirb.github.io/master/img/image-365.png)

---

## Simultaneous localization and mapping (SLAM)

A brief history of SLAM and it's future can be grasped with this papper : [C. Cadena and L. Carlone and H. Carrillo and Y. Latif and D. Scaramuzza and J. Neira and I. Reid and J.J. Leonard,
“Past, Present, and Future of Simultaneous Localization And Mapping: Towards the Robust-Perception Age”,
in IEEE Transactions on Robotics 32 (6) pp 1309-1332, 2016](https://arxiv.org/abs/1606.05830).

A brief overview of SLAM and it's issues : [Frese, Udo. (2006). A Discussion of Simultaneous Localization and Mapping. Auton. Robots. 20. 25-42. 10.1007/s10514-006-5735-x.](https://www.researchgate.net/publication/220474326_A_Discussion_of_Simultaneous_Localization_and_Mapping)

# Papers
* [Oussama El Hamzaoui. Localisation et cartographie simultanées pour un robot mobile équipé d’un
laser à balayage : CoreSLAM. Autre [cs.OH]. Ecole Nationale Supérieure des Mines de Paris, 2012.
Français. NNT : 2012ENMP0103. pastel-00935600](https://pastel.archives-ouvertes.fr/pastel-00935600)
* [](https://ieeexplore.ieee.org/document/1570091)

# Others

* A list of algorithms : [Open-Slam](https://openslam-org.github.io/)
