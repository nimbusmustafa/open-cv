# Description
implementation of a computer vision task that involves real-time video processing and circle detection. A yellow tennis ball detection program in python using OpenCV.

# Explaination
1) ``` import numpy as np ```

   Imports the NumPy library, which provides support for large, multi-dimensional arrays and mathematical functions to operate on these arrays efficiently.

2) ```import cv2 as cv```

    Imports the OpenCV library, which is a popular computer vision library used for image and video processing.

3) ```cap = cv.VideoCapture(0)```

    Creates a VideoCapture object 'cap' to capture video from the default camera (index 0).

4) ```while True:```

    Starts an infinite loop, which will continue until the loop is explicitly broken.

5) ```ret, frame = cap.read()```

    Reads the video frame from the capture object 'cap'. The function returns 'ret' as a boolean value indicating whether the frame was successfully read, and 'frame' as the captured image.

6) ```lower = np.array([20, 70, 80])```

    Creates a NumPy array 'lower' representing the lower bound of the color range for thresholding in HSV color space.

7) ```upper = np.array([38, 255, 255])```

    Creates a NumPy array 'upper' representing the upper bound of the color range for thresholding in HSV color space.

8) ```lower1 = np.array([25, 0, 140])```

    Creates a NumPy array 'lower1' representing the lower bound of the color range for thresholding in LAB color space.

9) ```upper1 = np.array([255, 140, 220])```

    Creates a NumPy array 'upper1' representing the upper bound of the color range for thresholding in LAB color space.

10) ```gauss = cv.GaussianBlur(frame, (9, 9), 10)```

    Applies a Gaussian blur to the captured frame to reduce noise. The function takes the 'frame' as input, kernel size of (9, 9), and a standard deviation of 10.

11) ```median = cv.medianBlur(gauss, 9, 0)```

     Applies a median blur to further reduce noise in the frame. The function takes the 'gauss' frame as input, kernel size of (9, 9), and no additional parameters.

12) ```framehsv = cv.cvtColor(median, cv.COLOR_BGR2HSV)```

     Converts the 'median' frame from the BGR color space to the HSV color space using the cvtColor function.

13) ```framelab = cv.cvtColor(median, cv.COLOR_BGR2LAB)```

     Converts the 'median' frame from the BGR color space to the LAB color space using the cvtColor function.

14) ```maskinglab = cv.inRange(framelab, lower1, upper1)```

     Creates a binary mask by thresholding the 'framelab' frame using the 'lower1' and 'upper1' color range. Pixels within the specified range are set to 255, while others are set to 0.

15) ```maskinghsv = cv.inRange(framehsv, lower, upper)```

     Creates a binary mask by thresholding the 'framehsv' frame using the 'lower' and 'upper' color range. Similar to the previous line, it sets pixels within the range to 255 and others to 0.

16) ```mask = cv.bitwise_or(maskinglab, maskinglab, mask=maskinghsv)```

     Combines the binary masks obtained from the previous two steps using a bitwise OR operation. This creates the final mask, where pixels within either of the two color ranges are set to 255.

17) ```cv.imshow('mask', mask)```

     Displays the 'mask' image in a window with the title 'mask'.
    ![Screenshot from 2023-07-07 11-41-04](https://github.com/nimbusmustafa/open-cv/assets/117943931/56763c9f-2b5a-4917-83d1-82bd09552c5a)


18) ```mask_erode = cv.erode(mask, None, iterations=1)```

     Performs erosion on the 'mask' image to remove small white regions. The 'iterations' parameter controls the number of times erosion is applied.

19) ```mask_dilate = cv.dilate(mask_erode, None, iterations=1)```

     Performs dilation on the eroded mask to expand the remaining white regions. This helps to fill in gaps and connect nearby contours.

20) ```gauss2 = cv.GaussianBlur(mask_dilate, (11, 11), 20)```

     Applies a Gaussian blur to the dilated mask to further smoothen the image. The function takes the 'mask_dilate' image, kernel size of (11, 11), and a standard deviation of 20.

21) ```circles = cv.HoughCircles(gauss2, cv.HOUGH_GRADIENT, 1, 50, param1=60, param2=30, minRadius=10, maxRadius=100)```

     Applies the Hough circle transform to detect circles in the 'gauss2' image. The function searches for circles with a specific range of radii defined by 'minRadius' and 'maxRadius'. Other parameters control the sensitivity and accuracy of the circle detection.

22) ```if circles is not None:```

     Checks if any circles were detected by verifying if the 'circles' variable is not None.

23) ```circles = np.uint16(np.around(circles))```

     Converts the floating-point circle coordinates obtained from the Hough circle transform to integer values for drawing circles on the frame.

24) ```for circle in circles[0, :]:```

     Iterates over each detected circle.

25) ```x, y, r = circle[0], circle[1], circle[2]```

     Extracts the center coordinates 'x' and 'y', and the radius 'r' of the current circle.

26) ```cv.circle(frame, (int(x), int(y)), int(r), (255, 25, 255), 3)```

     Draws a circle on the 'frame' image using the center coordinates, radius, and specified color (255, 25, 255) and thickness 3.

27) ```cv.circle(frame, (int(x), int(y)), 3, (0, 0, 255), -1)```

     Draws a small filled circle at the center of the detected circle to mark it.

28) ```cv.imshow("Frame", frame)```

     Displays the 'frame' image with the drawn circles in a window titled "Frame".

29) ```if cv.waitKey(1) == ord('q'):```

     Waits for a keyboard event for 1 millisecond. If the key pressed is 'q', the loop is exited.

30) ```cap.release()``

     Releases the VideoCapture object, releasing the camera.

31) ```cv.destroyAllWindows()```

    Closes all windows created by OpenCV.
    

## Testing in Different Conditions
### Well Lit
https://youtu.be/GAjb88efxeE

### Dim light 
https://youtu.be/t7mACFk8AVk

### Pitch black room with a Flash light
https://youtu.be/sdsSkw5ayXY

### Well Lit area with Yellow Background 
https://youtu.be/O-qD8Sz5ZNk
    
