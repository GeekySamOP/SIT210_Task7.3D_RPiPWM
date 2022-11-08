#Libraries
import RPi.GPIO as GPIO
import time
 
#GPIO Mode which is set to board
GPIO.setmode(GPIO.BOARD)
 
#setting GPIO Pins
GPIO_TRIGGER = 8
GPIO_ECHO = 10

#setting PWM Pins
LED = 12

#Setting maximum distance 
SET_DISTANCE = 25

#set GPIO direction (IN / OUT)
GPIO.setup(GPIO_TRIGGER, GPIO.OUT)
GPIO.setup(GPIO_ECHO, GPIO.IN)

GPIO.setup(LED, GPIO.OUT)
 
#set LED pin to PWM that will vary 1 to 100
led = GPIO.PWM(LED,100)

#led will start from the off position
led.start(0)


def distance():
    # set Trigger pin to HIGH when it will triggers a signal
    GPIO.output(GPIO_TRIGGER, True)
 
    # set Trigger after 0.01ms to LOW that it will not trigger the signal again
    time.sleep(0.00001)
    GPIO.output(GPIO_TRIGGER, False)
 
    #initialising two variables for time diffrence
    StartTime = time.time()
    StopTime = time.time()
 
    # store StartTime
    while GPIO.input(GPIO_ECHO) == 0:
        StartTime = time.time()
 
    # store time of arrival
    while GPIO.input(GPIO_ECHO) == 1:
        StopTime = time.time()
 
    # time difference between start and arrival
    TimeElapsed = StopTime - StartTime

    # multiply with the sonic speed (34300 cm/s)
    # and divide by 2, because there and back

    distance = (TimeElapsed * 34300) / 2
 
    return distance

try:
	while True:
		dist = distance()       # call distance function that will calculate distance 
		print ("Measured Distance = %.1f cm" % dist)    #it will display distance
		if dist <= SET_DISTANCE:           #if distance is under the max distance that it will cange led's brightness
			led.ChangeDutyCycle(100 - (dist/SET_DISTANCE * 100)) #change duty cycle will set LED's brightness accorgin to distance
			time.sleep(0.1)
		else:
			led.ChangeDutyCycle(0)      #if distance is higher than max distance than LED will off
 
        # Reset by pressing CTRL + C
except KeyboardInterrupt:
    print("Measurement stopped by User")
    
led.stop()
GPIO.cleanup()
