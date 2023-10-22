# AI-Eye-Detection-Model
Created AI Eye detection model using AI and different python frameworks.

import cv2
import tkinter as tk
from tkinter import filedialog
from PIL import Image, ImageTk  # Import the necessary module from Pillow

# Load the pre-trained eye detection model
eye_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_eye.xml')

# Function to detect eyes in an image
def detect_eyes(image):
    # Convert the image to grayscale (required for eye detection)
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

    # Detect eyes in the grayscale image
    eyes = eye_cascade.detectMultiScale(gray, scaleFactor=1.1, minNeighbors=5, minSize=(30, 30))

    # Draw rectangles around the detected eyes
    for (x, y, w, h) in eyes:
        cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 2)

    return image

# Function to open a file dialog and select an image
def open_file_dialog():
    file_path = filedialog.askopenfilename(filetypes=[("Image files", "*.jpg *.jpeg *.png *.bmp *.gif *.tiff")])
    if file_path:
        image = cv2.imread(file_path)
        output_image = detect_eyes(image)
        display_image(output_image)

# Function to display the image in the tkinter window
def display_image(img):
    img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)  # Convert image format for tkinter
    img = Image.fromarray(img)  # Create a PIL Image object
    img = ImageTk.PhotoImage(img)  # Convert PIL Image to ImageTk format

    # Display the image on the tkinter window
    image_label.config(image=img)
    image_label.image = img  # Keep a reference to prevent image from being garbage collected

# Main function
def main():
    root = tk.Tk()
    root.title("Eye Detection App")

    # Create a button to open the file dialog
    open_button = tk.Button(root, text="Upload Image", command=open_file_dialog)
    open_button.pack()

    # Create a label to display the image
    global image_label
    image_label = tk.Label(root)
    image_label.pack()

    root.mainloop()

if __name__ == "__main__":
    main()
 
