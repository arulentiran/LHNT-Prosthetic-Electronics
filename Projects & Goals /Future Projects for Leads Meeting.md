# Future Projects for Leads Meeting

# Infrared Sensors

Infrared Sensors allow for the detection of obstacles when grabbing objects. The primarily use of infrared sensors would be to detect when an object is nearby and slow the clamping of the hand until it reaches the object. 

Most IR sensors contain and IR LED (emitter) and Photodiode/phototransistor (receiver). The photodiode/phototransistor detects reflected light emitted by the IR LED. The higher the reflection, the stronger the signal and greater indication that an object is nearby.

### Option 1: Detect if Object is Present or Not

Amazon offers cheap infrared sensors that are primarily for detecting whether an object is within a certain threshold distance from the sensor. In doing this, we can slow the hand down once it reaches this threshold distance instead of having the fingers slam on the object. This allows for more natural hand movement.

- [Amazon Link](https://www.amazon.com/EC-Buying-Infrared-Obstacle-Avoidance/dp/B0CWKWWKYL/ref=sr_1_4?crid=2Q9UP1SHPLW4Y&dib=eyJ2IjoiMSJ9.YCsubYKaA-AlUhDagEQaEWQB0tlQrt9INmuWnjLypjtIEB8sdZqmvVUpejPjSCEugDUulBEYnL2pm4w1hLDTvtAsQRU5y1rw2X5O0oZ0t7ySFDrpm6Nt2W8sblG7ZzLC_pE1lSmLVzplL44PuMDKTq7hWgoSKIqRsSR-EERuAD_Mme_9GrLMOxtlhWTzcxdm-_haatrb6oSDE0Z4VYcU_aoIi6TptT2cmL9cXLwiN4c.dp2ntAivM4mjvCdEFOfHu21ptCjIlSThLEPw0vA_qQA&dib_tag=se&keywords=infrared+sensors&qid=1769989484&sprefix=infared+sensors+%2Caps%2C187&sr=8-4) - $8.99 for 20 infrared sensors

### Option 2: Detect Presence of Object + Distance

We can opt for infrared sensors that detect the precise distance from the object and output it. This does not seem as useful or practical as we more care if an object is within a given threshold distance, not how far it is at all times from the hand.

### Implementation

We can implement IR sensors by placing them in the fingertips. By orienting them such that the lights point out of where the fingerprints are and making a casing that surrounds them, you can put 5 IR sensors (or 1 assuming the fingers are even).

# Pressure Sensors

Pressure sensors would be able to detect the moment at which the hand grasps an object. In detecting this, we know that an object is being grabbed and can adjust the grip strength accordingly. Once the pressure sensors detect pressure (meaning the object has been grabbed), then we know to start adjusting grip - either increase the speed at which the motors rotate (thereby increasing grip strength) or decrease the speed and decrease grip strength.

### FSR402 Thin-Film Pressure Sensors

These are pretty expensive on Amazon (~7 per sensor) but very cheap when ordered in bulk off of Chinese websites (~1.50 per sensor). Assuming all fingers are even, then we only need one to detect when an object is being grabbed if all 5 fingers are clasping the object at the same time.

### Implementation

You can implement pressure sensors on fingers that don’t have IR sensors. That way, they don’t interfere with reflected light at all. They would be placed on where fingerprints are as this is where you grab objects.
