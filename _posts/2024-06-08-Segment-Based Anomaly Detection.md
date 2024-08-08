# Segment-Based Anomaly Detection


In today's data-driven world, the ability to automatically detect and localize anomalies in images is crucial across a wide range of applications. Traditionally, anomaly detection has focused on identifying differences between entire images. However, a more granular approach that detects anomalies within segments of the same image can provide even greater value and insights. Here's why this localized approach to anomaly detection is so important:

1. **Quality Control in Manufacturing:** Ensuring product quality is crucial. Classic anomaly detection might miss small defects. Segment-level detection identifies minor issues, enhancing quality control and reducing waste.

2. **Medical Imaging:** Early detection of anomalies in medical images can save lives. Segment-level detection highlights small abnormalities in X-rays, MRIs, or CT scans, aiding accurate diagnoses and improving patient outcomes.

---

**In this work, we focus on segment-level anomaly detection. By analyzing smaller regions within an image, we can pinpoint anomalies that might be overlooked when considering the entire image.**

By leveraging the capabilities of the Fastdup library, we aim to enhance the capability of anomaly detection systems to operate at a finer granularity, delivering better insights and outcomes.

---

## Dataset

We conducted the experiments on the [MVTec Anomaly Detection benchmark](https://www.mvtec.com/company/research/datasets/mvtec-ad/downloads), a renowned dataset in the anomaly detection and localization field. MVTec AD contains 10 object categories from manufacturing with a total of 5354 images. The dataset is composed of normal images for training and both normal and anomaly images with various types of defects for testing. It also provides pixel-level annotations for defective test images.

<p align="center">
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/006.png" alt="Image 006" width="200" />
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/037.png" alt="Image 037" width="200" />
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/263.png" alt="Image 263" width="200" />
</p>

The dataset also includes images with defects.

<p align="center">
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/007.png" alt="Image 007" width="200" />
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/024.png" alt="Image 024" width="200" />
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/016.png" alt="Image 016" width="200" />
</p>

**The last three images are examples of defective images.**


## Method

In this project, we utilize a segment-level anomaly detection approach to identify outliers within an image. The method involves several key steps:

1. **Image Segmentation**: 
   - The uploaded image is divided into smaller segments based on the specified number of parts. Here, "parts" refers to the grid divisions of the image; for example, if the image is divided into 4 parts, it will be split into a 4x4 grid. The number of parts can be adjusted according to the desired granularity of the segmentation.
   - This segmentation is achieved using a function that calculates the dimensions of each part and crops the image accordingly. By breaking down the image into smaller parts, we can focus on detecting anomalies in localized regions, which is more precise than evaluating the entire image as a whole.

2. **Saving Image Segments**: 
   - Each segment is saved as a separate image file in a temporary directory. This organization is crucial for the subsequent analysis.

3. **Anomaly Detection with Fastdup**:
   - The `fastdup` library is employed to detect outliers among the saved image segments. 
   - The library processes the segments, compares them, and identifies which segments are outliers based on their visual characteristics.

4. **Highlighting Outliers**: 
   - Once the outliers are detected, the original image is revisited, and rectangles are drawn around the segments identified as anomalies. This visual representation helps in easily locating the problematic areas within the image.

## Implementation
Below are the parts of the code used in our segment-level anomaly detection system.

#### Setting Up the Environment

We start by importing all necessary libraries. These include `os` and `shutil` for file operations, `ipywidgets` for creating interactive widgets within Jupyter Notebook, `IPython.display` for displaying images and HTML content, `PIL` for image processing, `fastdup` for detecting outliers, `contextlib` for handling output redirection, and `uuid` for generating unique directory names.

```pyhton
import os
import shutil
import ipywidgets as widgets
from IPython.display import display, Image, HTML
from PIL import Image as PILImage, ImageDraw
from io import BytesIO
import fastdup
import sys
import contextlib
import uuid  # To generate a unique directory name
```

#### Creating Interactive Widgets

This part creates the user interface elements. We define the title, description, file upload widget, slider for specifying the number of parts, an output area to display results, and a loading message widget.


```pyhton
# Create widgets for the UI
def create_widgets():
    title = widgets.HTML(value="<h2 style='color: #2E86C1;'>Image Outlier Detection</h2>")
    description = widgets.HTML(value="<p style='font-size: 16px;'>You can load an image with an outlier, and the model will return the segment with the outlier:</p>")
    file_upload = widgets.FileUpload(accept='image/*', multiple=False, style={'description_width': 'initial'})
    # Create an IntSlider widget to allow the user to specify the number of parts to divide the image into
    num_parts = widgets.IntSlider(value=7, min=1, max=10, step=1, description='Num Parts:', style={'description_width': 'initial'})
    output = widgets.Output()
    loading_message = widgets.Label(value="")
    return title, description, file_upload, num_parts, output, loading_message

```

#### Segmenting the Image

We define a function to divide the uploaded image into segments. This function calculates the dimensions of each segment and returns both the segments and their coordinates.

```pyhton
# Function to divide an image into a specified number of parts and return segments with coordinates
def divide_image_with_coordinates(image, num_parts):
    img_width, img_height = image.size  # Get the width and height of the image
    segments = []
    part_width = img_width // num_parts  # Calculate the width of each part
    part_height = img_height // num_parts  # Calculate the height of each part
    
    for y in range(num_parts):
        for x in range(num_parts):
            left = x * part_width  # Calculate the left coordinate of the part
            upper = y * part_height  # Calculate the upper coordinate of the part
            right = (x + 1) * part_width if (x + 1) * part_width < img_width else img_width  # Calculate the right coordinate
            lower = (y + 1) * part_height if (y + 1) * part_height < img_height else img_height  # Calculate the lower coordinate
            segment = image.crop((left, upper, right, lower))  # Crop the image to create the segment
            segments.append((segment, (left, upper, right, lower)))  # Append the segment and its coordinates to the list
    
    return segments
```

#### Saving Image Segments

This function saves the image segments to a temporary directory. It first clears the directory if it exists, then saves each segment.

```python
# Function to save segments to a temporary directory
def save_segments(segments_with_coords, temp_dir):
    if os.path.exists(temp_dir):
        shutil.rmtree(temp_dir)  # Remove the temporary directory if it exists
    os.makedirs(temp_dir)  # Create a new temporary directory
    
    segment_files = []
    for i, (segment, (left, upper, right, lower)) in enumerate(segments_with_coords):
        segment_file = os.path.join(temp_dir, f"segment_{i}.png")
        segment.save(segment_file)  # Save each segment
        segment_files.append(segment_file)
    
    return segment_files
```

#### Running Fastdup and Getting Outliers

This function runs the `fastdup` library on the saved segments to detect outliers. It suppresses the output to avoid cluttering the display.

```python
# Function to run fastdup and get outliers
def run_fastdup_and_get_outliers(temp_dir):
    with contextlib.redirect_stdout(open(os.devnull, 'w')), contextlib.redirect_stderr(open(os.devnull, 'w')):
        fd = fastdup.create(input_dir=temp_dir)
        fd.run(overwrite=True)
    outliers_df = fd.outliers()
    outlier_files = set(outliers_df['filename_outlier'].values) if not outliers_df.empty else set()
    return outlier_files
```

#### Drawing Rectangles Around Outliers

This function draws rectangles around the detected outliers on the original image, making it easy to visualize the anomalies.

```python
# Function to draw rectangles around detected outliers on the original image
def draw_outliers_on_image(img, segments_with_coords, outlier_files):
    draw = ImageDraw.Draw(img)
    outlier_basenames = {os.path.basename(outlier_file) for outlier_file in outlier_files}
    for i, (segment, (left, upper, right, lower)) in enumerate(segments_with_coords):
        segment_file = f"segment_{i}.png"
        if segment_file in outlier_basenames:
            draw.rectangle([left, upper, right, lower], outline="red", width=5)
    return img
```
#### Handling File Uploads

This function handles the file upload event. It processes the uploaded image, segments it according to the specified number of parts, saves the segments, runs `fastdup` to detect outliers, and highlights the detected outliers on the original image.
```python
# File upload event handler
def on_file_upload(change):
    with output:
        output.clear_output()  # Clear previous outputs
        loading_message.value = "Processing the image, please wait..."
        display(loading_message)  # Show loading message
        
        for name, file_info in file_upload.value.items():
            img = PILImage.open(BytesIO(file_info['content']))  # Process the uploaded image
            segments_with_coords = divide_image_with_coordinates(img, num_parts.value)
            temp_dir = f"/tmp/temp_segments_{uuid.uuid4()}"
            save_segments(segments_with_coords, temp_dir)
            outlier_files = run_fastdup_and_get_outliers(temp_dir)
            img_with_outliers = draw_outliers_on_image(img, segments_with_coords, outlier_files)
            
            loading_message.value = ""  # Remove loading message
            
            img_display_with_outliers = BytesIO()
            img_with_outliers.save(img_display_with_outliers, format='PNG')
            display(Image(data=img_display_with_outliers.getvalue()))  # Display the original image with outliers highlighted
```

#### Creating and Displaying the UI

Finally, we set up and display the user interface. The file upload widget is observed for changes, and all the widgets are arranged in a vertical box layout for display.

```python
# Create and display UI
title, description, file_upload, num_parts, output, loading_message = create_widgets()

file_upload.observe(on_file_upload, names='value')

ui = widgets.VBox([
    title,
    description,
    num_parts,
    file_upload,
    output
])

display(ui)
```

Below is an image showing how the UI looks like:

<p align="center">
  <img src="https://github.com/EN555/EN555.github.io/blob/main/images/UI.JPG" alt="Image UI" />
</p>

## Results

We selected a few defective images from the MVTec Anomaly Detection benchmark dataset and processed them using our segment-level anomaly detection system. The results show relatively good performance in detecting and highlighting local anomalies within the images. Below are the original images alongside the images with detected anomalies.

<div style="display: flex; justify-content: center;">
  <div style="margin-right: 20px; text-align: center;">
    <p><b>Original Images</b></p>
    <img src="https://github.com/EN555/EN555.github.io/raw/main/images/007.png" alt="Image 007" width="200" />
    <img src="https://github.com/EN555/EN555.github.io/raw/main/images/024.png" alt="Image 024" width="200" />
    <img src="https://github.com/EN555/EN555.github.io/raw/main/images/016.png" alt="Image 016" width="200" />
  </div>
  <div style="text-align: center;">
    <p><b>Images Highlighting Detected Anomalies</b></p>
    <img src="https://github.com/EN555/EN555.github.io/raw/main/images/007-defect.png" alt="Image 007 Defect" width="200" />
    <img src="https://github.com/EN555/EN555.github.io/raw/main/images/024-defect.png" alt="Image 024 Defect" width="200" />
    <img src="https://github.com/EN555/EN555.github.io/raw/main/images/016-defect.png" alt="Image 016 Defect" width="200" />
  </div>
</div>

### Conclusion

In this blog post, we demonstrated how to implement a segment-level anomaly detection system using Python and Jupyter Notebook. By leveraging interactive widgets, image processing libraries, and the Fastdup library, we created a tool that can efficiently detect and localize anomalies within images.



