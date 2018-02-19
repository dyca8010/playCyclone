#code to take video

# import the necessary packages
import numpy as np
import argparse
import RPi.GPIO as GPIO

from picamera.array import PiRGBArray
from picamera import piCamera
import time
import cv2

#pin to output signal
GPIO.setup(17,GPIO.OUT)

#initialize the camera
camera = PiCamera()
camera.resolution(400,300)

camera.framerate = 15
rawCapture = PiRGBArray(camera, size=(400, 300))


# construct the argument parse and parse the arguments
ap = argparse.ArgumentParser()
ap.add_argument("-i", "--image", help = "path to the image")
args = vars(ap.parse_args())

# define the list of boundaries, BGR format, numpy is backwards
boundary = [
	([50, 205, 50], [150, 255, 100]), #green in BGR
]


#allow time for camera to warm up
time.sleep(0.1)

# capture frames from the camera
for frame in camera.capture_continuous(rawCapture, format="bgr", use_video_port=True):
	# grab the raw NumPy array representing the image, then initialize the timestamp
	# and occupied/unoccupied text
	image = frame.array

  		#code to detect whether green is present
	lower = np.array(lower, dtype = "uint8")
	upper = np.array(upper, dtype = "uint8")
	mask = cv2.inRange(image, lower, upper)

	for xval in range(0,400):
		for yval in range(0,300):
			if mask[xval,yval] > boundary[0][1] and mask[xval,yval] < boundary[1][1]: #if pixel value is green
				GPIO.output(17,GPIO.HIGH)
				time.sleep(5)

	# show the frame
	cv2.imshow("Frame", image)
	key = cv2.waitKey(5) & 0xFF

	# clear the stream in preparation for the next frame
	rawCapture.truncate(0)

	# if the `q` key was pressed, break from the loop
	if key == 27:
		break

