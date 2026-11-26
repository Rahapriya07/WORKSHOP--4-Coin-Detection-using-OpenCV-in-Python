# WORKSHOP 4 – COIN DETECTION USING OPENCV IN PYTHON
## Name : Raha Priya Dharshini M
## Reg No.: 212224240124

## Aim
To detect and count coins in an image using image processing techniques such as grayscale conversion, Gaussian Blur, Canny Edge Detection, and contour detection.

## Software Requirements
- Python
- OpenCV
- NumPy
- Matplotlib
- Jupyter Notebook




## Procedure

1. Import the required libraries: OpenCV, NumPy, Matplotlib, and OS.
2. Load the input coin image using `cv2.imread()`.
3. Convert the original image into grayscale using `cv2.cvtColor()`.
4. Apply Gaussian Blur using `cv2.GaussianBlur()` to reduce noise.
5. Perform Canny Edge Detection using `cv2.Canny()` with threshold values 30 and 150.
6. Detect the contours in the edge image using `cv2.findContours()`.
7. Filter out small contours based on their area.
8. Find the minimum enclosing circle for each contour using `cv2.minEnclosingCircle()`.
9. Count the detected coins and draw green circles around them.
10. Display the coin numbers using `cv2.putText()`.
11. Print the total number of coins detected.
12. Display the original image, detected coin image, and all image processing stages using Matplotlib.


## Code
```

import cv2
import numpy as np
import matplotlib.pyplot as plt
import os

# Step 1: Specify the image path
image_path = "coin.jpeg"

# Step 2: Load the image
image = cv2.imread(image_path)

if image is None:
    raise FileNotFoundError(f"Image not found at {image_path}")

# Step 3: Convert the image to grayscale
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# Step 4: Apply Gaussian Blur
blurred = cv2.GaussianBlur(gray, (11, 11), 0)

# Step 5: Apply Canny Edge Detection
edges = cv2.Canny(blurred, 30, 150)

# Step 6: Find contours
contours, _ = cv2.findContours(
    edges.copy(),
    cv2.RETR_EXTERNAL,
    cv2.CHAIN_APPROX_SIMPLE
)

# Step 7: Create a copy of the original image
output = image.copy()

coin_count = 0

# Step 8: Detect and count coins
for contour in contours:

    # Filter out small contours
    area = cv2.contourArea(contour)

    if area < 300:
        continue

    # Find the enclosing circle
    ((x, y), radius) = cv2.minEnclosingCircle(contour)

    if radius > 15:
        coin_count += 1

        # Draw the detected coin
        cv2.circle(
            output,
            (int(x), int(y)),
            int(radius),
            (0, 255, 0),
            2
        )

        # Display coin number
        cv2.putText(
            output,
            f"{coin_count}",
            (int(x - 10), int(y + 10)),
            cv2.FONT_HERSHEY_SIMPLEX,
            0.6,
            (0, 0, 255),
            2
        )

# Step 9: Print total number of coins
print(f"Total coins detected: {coin_count}")

# Step 10: Display original and detected coin images
plt.figure(figsize=(12, 6))

plt.subplot(1, 2, 1)
plt.title("Original Image")
plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
plt.axis("off")

plt.subplot(1, 2, 2)
plt.title(f"Detected Coins: {coin_count}")
plt.imshow(cv2.cvtColor(output, cv2.COLOR_BGR2RGB))
plt.axis("off")

plt.tight_layout()
plt.show()

# Step 11: Display all processing stages
fig, axs = plt.subplots(1, 5, figsize=(20, 6))

axs[0].imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
axs[0].set_title("Original Image")
axs[0].axis("off")

axs[1].imshow(gray, cmap="gray")
axs[1].set_title("Grayscale Image")
axs[1].axis("off")

axs[2].imshow(blurred, cmap="gray")
axs[2].set_title("Blurred Image")
axs[2].axis("off")

axs[3].imshow(edges, cmap="gray")
axs[3].set_title("Edge Detection (Canny)")
axs[3].axis("off")

axs[4].imshow(cv2.cvtColor(output, cv2.COLOR_BGR2RGB))
axs[4].set_title(f"Detected Coins: {coin_count}")
axs[4].axis("off")

plt.tight_layout()
plt.show()
```

## Output

### 1. Original Image and Detected Coins


<img width="1167" height="581" alt="image" src="https://github.com/user-attachments/assets/1cfdb0e5-bf50-4ce6-bf50-0c50c9c7d1b4" />


### 2. Image Processing Stages


<img width="1362" height="291" alt="image" src="https://github.com/user-attachments/assets/e4a1300e-ae51-4e51-b08d-c2951e0e7e88" />


## Result
Thus, coin detection and counting were successfully implemented using OpenCV and Python by applying grayscale conversion, Gaussian Blur, Canny Edge Detection, and contour detection.
