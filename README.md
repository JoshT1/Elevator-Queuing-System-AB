# Elevator-Queuing-System-AB

This program's purpose is a parking garage elevator queuing system. This was programmed in RSLogix in the Allen Bradley Studio 5000 software. RSLogix utilizes ladder logic programming. It was implemented and tested on an Allen Bradley PLC and HMI. This was my final project/exam for my mechatronics class at the University of Toledo.

---

Inputs:
| Sensor           | Function/State | Signal Assignment |
|------------------|----------------|-------------------|
| EntireGarageFull | Switch         | 1                 |
| DoorButton       | Button         | 1                 |
| RequestButton1   | Button         | 1                 |
| RequestButton2   | Button         | 1                 |
| RequestButton3   | Button         | 1                 |
| RequestButton4   | Button         | 1                 |
| RequestButton5   | Button         | 1                 |
| CarInElevator    | Switch         | 1                 |

Outputs:
| Actuator     | Function/State                          | Signal Assignment |
|--------------|-----------------------------------------|-------------------|
| MotorUp      | Motor                                   | 1                 |
| MotorDown    | Motor                                   | 1                 |
| Brakes       | Braking mechanism                       | 1                 |
| DoorOpen     | Door status                             | 1                 |
| CurrentFloor | Location of elevator                    | 1-5               |
| InProgress   | Floor has a request  in progress        | 1                 |
| Requested    | Floor is requested and waiting in queue | 1                 |

---
I first started by doing a very rough draft of a state diagram, I/O tables, and the HMI display. With these done, I had a good grasp of how to progress further and the major hurdles I had to overcome with this project. There were 2 major parts of this program:

1) The first part was queuing up requests. I had solved this by putting an array called RequestArray in my program. Whenever a request is made, a value is put into RequestArray[index] and then the index is incremented. The value put into the array is dependent on the floor that the request is being made from, so 1 for street level, 2 for the next, etc. Whenever a request is finished, the index is decremented and every value in the array is pushed down 1.

2) The 2nd major part was the step sequence. The main purpose of this is to ensure that the right processes are happening at the correct time. The sequence branches off depending on whether the request was made from the street level or levels 2,3,4, or 5. The upper levels requests have 1 purpose, and that is to get the car to the street level and make sure they leave the garage. The street levels step sequence logic is a bit different. Once the car is in the elevator and the door button is pressed then the elevator goes up 1 floor, checks for any open parking spots and if there is any then the door opens and the user goes there to park. If not, then the floor goes up another level and repeats the same process. If there are no open spots in the entire garage then the street level users will not be able to make a request, there will be a light to show that the garage is full.

For this project, there are some inputs/outputs that I did not use. I used all of the inputs required for the elevator logic to work correctly, and the output/inputs required on the HMI to monitor the statuses of the parking garage. Some outputs like the RequestOutput lights I didn’t implement into this, they were programmed into the HMI. 

For the naming schemes, I decided to go with the street level as floor 1, the next as floor 2, all the way to floor 5. This was much easier for me to understand as in the request array I started it as 1 for the street level. I couldn’t start it at 0, as 0 is used to indicated that no request is currently being made.

This project was challenging to me but at the end I was happy with how the project turned out. The biggest issue I had was getting the door to open at the right time. I originally had it so that during step sequence 20 (which is the step that the user is supposed to enter the elevator and press the button to go up/down floors) the door would not open. After troubleshooting this, I remembered in previous labs it was important to not have different rungs lead to the same output. I then put all the rungs that had OpenDoor as the output into 1 and it worked correctly.

I also added a timer between floors, and a 30 second wait timer for when the elevator door opens awaiting a user. If no car enters the elevator after the 30 seconds then the request is finished and it moves to the next one. Each floor can hold 10 cars (excluding the street level). By default, I had put 8 cars in each floor. To test my program I exited a few cars from the top floors, then filled the entire garage from street level requests. After the garage was full, no cars were able to enter from the street level. After that I made a queue of requests to leave the garage and it seemed to work fine. I only enabled 5 total requests to be entered, 1 from each floor. In a real world situation, if a car was waiting for their request to be made, only 1 car per floor would be close enough to hit the request button, the rest would be waiting behind them.
