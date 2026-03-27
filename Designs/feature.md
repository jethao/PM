Product Definition

Product name: AirHealth

Category: Connected Consumer Electronics, Health Product

Short description: AirHealth analyzes your breath and measures your metabolism biometrics in your breath (aka breath-print). The history of your breath-print will give user health indication and track the progress through measureable goals

Core Features: user will deep breath, follow the instructions from app to interact with device. The devie measures breath-print and transfer the data to mobile phone. Phone will show data, show progress and provide suggestions. Suggestion may bring user to purchase related health products. Mobile app provide a 60-day trial period, and after that it will be $5.99 per month.

List all primary features. These should drive the PRD. Even though AirHealth can track various use cases, the features are exclusive, user can only measure one thing at a time. If another measurements needs to be made, user must finish or cancel current session.

UX:
setup: pair devices with phone (for first time)
on home screen, listed all features. Tap on each feature, you can "set goals", "view history", "measure", "get suggestion", "consult professionals"
set goals: open mobile app -> tap on a featre -> set a goal (can use AI to suggest a goal)
Interaction model (buttons, 3-color LED, app, voice, etc.): button on device (on/off), app indication of actions
Low power mode: put system into low power mode when sensor become inactive (change less than 1% from last second's average)
Interaction: user can only do one action at a time.

Feature 1: Measure Oral & Dental Health
User value: maintain oral and dental health
Key behaviors: user set the goal, track over time and see how it progresses, app will provide suggestions to mudulate your oral health
Sensors: Hydrogen sulfide and Methyl mercaptan sensors
Outputs: Measurement normalized to the average of first 5 measurements, progress
Key user flows: 
Daily use: turn on the device -> open mobile app, tap on a feature -> wait till it shows ready (and also show animations how to do the measurement) -> put the device in your mouth and close your mouth -> mobile app will show measureing -> wait until mobile app shows done -> tap on done and browse your visalized measurements and progress -> get advices (with potential purchase suggestion)
    During measurement, if the user stops befoe done, it will be considered as a cancel and no results will be shown or stored

Feature 2: Measure Fat Burning
User value: understands Fat-burning
Key behaviors: user set the goal, tracks if user are burning enough fat
Sensors: Acetone and CO2
Outputs: Measurement normalized to the starting point of the measurement, it's measured multiple times through one session
Key user flows: 
Daily use: turn on the device -> open mobile app, tap on a feature -> wait till it shows ready (and also show animations how to do the measurement) -> Breath and hold for 10 seconds -> Blow into the device -> mobile app will show measureing -> wait until mobile app shows done -> tap on done -> repeat until your session is over -> tap on Finish and browse your visalized measurements and progress -> get advices (with potential purchase suggestion)
    During measurement, if the user stops befoe done, it will be considered as a cancel and no results will be shown or stored
    
Feature 3: Factory mode
User value: good open box experience
Key behavior: test the HW functionality at factory
Key user flow: press the button for 10 seconds to turn on -> firmware automatically runs the HW functionality check, turn LED to orange -> report error logs over BLE -> turn LED to green if no error, turn LED to red on if there is error. -> press the button for 10 seconds to turn off 
One time use

Not User Facing Feature: HW-ID
User value: automatically detect the type of VOC detected with different HW-ID
Key behavior: the output will be organized based on supported HW-ID
Key user flow: none.

Accepetance criteria: consequent measurement from same source should be with 5% error


Platform Scope
Mobile (iOS/Android): support iOS26 and layer, android 16 and later. We shall allow data sharing with 3P health app like health, oura, fitbit etc
Firmware: BLE to communicate with phone, controls sensor and measurement.
Cloud: Stores data, goals, progress, health history, past interactions

Design Constraints: this is a hand hold devie, ID must be as slick as possible. The HW especially the electrical component selection must consider the ID and structure. We want as fewer mechanical part as possible, better to be 0. The air flow needs to be stable before it reaches sensor, air flow cannot be directly blowed on sensors.

Cost targets: $199

Timeline constraints: use after brush teeth, don't use toothpaste with strong scent

Technical limitations

Differentiators: all in one tracker, easy to use, tracks health progress

What makes this product unique? Accuracy through priperitary sensory techonology
