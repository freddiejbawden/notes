# IOT Case Studies

## Laparoscopic Simulator

A Laparoscopy is a low risk, minimally invasive procedure that requires only small incisions.

We use inertial sensors attached to instruments to provide feedback on performance on a standard set of surgical skills.

We can analyse the measurements from the sensors against cohort of professionals. 

To develop this, we could try different placements of the sensors, for example on the tools or on the hand. Placement on the tools has concerns with size (surgical tools are very small), hands would be less accurate. 

We can verify the performance of a participant by consulting experts and creating a formula. 

Possible metrics includes:

* Task Duration
* Average Speed
* Average Acceleration
* Smoothness

We can verify the effectiveness of the new system by taking the correlation coefficient to the old data. 

We use a box plot to evaluate if we see a difference in the expert and novice surgeons.  

## IoT Tutor for Cellists (Ch-ell-ists)
* Take home tutor for students learning to play the cello
* Students practice with an IMU sensor attached ti the wrist of the playing hand and to the frog in the bow
* Recognise correct bowing for a selection of techniques and provides feedback. 

Goal to recognise to bowing techniques. 

The orient data segmented based on a time window between 1 and 2 seconds. 

We can look at:

* Average acceleration
* Variance in the acceleration
* Average smoothness of acceleration
* Average angular velocity
* Variance in angular velocity
* Average angular acceleration

Using various classifiers (SVM, Logisitic Regression) we can get >95% accuracy.

## Fast Moving Consumer Goods

* We want to understand how people use goods in real life
* Communicates data to an App on the phone

Normally we use interviews and questionnaires to find data. For example advantages, low tech -> low cost. Disadvantages; noise, inaccurate, intrusive.

Using K-NN we can define different actions on the product, such as squeeze and shake. 