# Segment-Based Anomaly Detection for Greater Accuracy


In today's data-driven world, the ability to automatically detect and localize anomalies in images is crucial across a wide range of applications. Traditionally, anomaly detection has focused on identifying differences between entire images. However, a more granular approach that detects anomalies within segments of the same image can provide even greater value and insights. Here's why this localized approach to anomaly detection is so important:

1. **Quality Control in Manufacturing:** Ensuring product quality is crucial. Classic anomaly detection might miss small defects. Segment-level detection identifies minor issues, enhancing quality control and reducing waste.

2. **Medical Imaging:** Early detection of anomalies in medical images can save lives. Segment-level detection highlights small abnormalities in X-rays, MRIs, or CT scans, aiding accurate diagnoses and improving patient outcomes.

---

**In this work, we focus on segment-level anomaly detection. By analyzing smaller regions within an image, we can pinpoint anomalies that might be overlooked when considering the entire image.**

By leveraging the capabilities of the Fastdup library, we aim to enhance the capability of anomaly detection systems to operate at a finer granularity, delivering better insights and outcomes.

---

## Dataset

We conducted the experiments on the MVTec Anomaly Detection benchmark, a renowned dataset in the anomaly detection and localization field. MVTec AD contains 10 object categories from manufacturing with a total of 5354 images. The dataset is composed of normal images for training and both normal and anomaly images with various types of defects for testing. It also provides pixel-level annotations for defective test images.

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

---

## Code
Below are the main parts of the code used in our segment-level anomaly detection system.

#### Segmenting the Image

```pyhton
# Function to divide an image into a specified number of parts and return segments with coordinates
def divide_image_with_coordinates(image, num_parts):
    # Get the width and height of the image
    img_width, img_height = image.size
    # Initialize an empty list to store the segments and their coordinates
    segments = []
    # Calculate the width and height of each segment
    part_width = img_width // num_parts
    part_height = img_height // num_parts
    
    # Loop through the number of parts vertically
    for y in range(num_parts):
        # Loop through the number of parts horizontally
        for x in range(num_parts):
            # Calculate the left, upper, right, and lower coordinates for each segment
            left = x * part_width
            upper = y * part_height
            right = (x + 1) * part_width if (x + 1) * part_width < img_width else img_width
            lower = (y + 1) * part_height if (y + 1) * part_height < img_height else img_height
            # Crop the image to create the segment
            segment = image.crop((left, upper, right, lower))
            # Append the segment and its coordinates to the list
            segments.append((segment, (left, upper, right, lower)))
    
    # Return the list of segments and their coordinates
    return segments
```

#### Run Fastdup

```python
# Function to run Fastdup on segmented images
def run_fastdup_on_segments(temp_dir):
    # Run fastdup to detect outliers with suppressed output
    with contextlib.redirect_stdout(open(os.devnull, 'w')), contextlib.redirect_stderr(open(os.devnull, 'w')):
        fd = fastdup.create(input_dir=temp_dir)
        fd.run(overwrite=True)
    
    # Get the outliers
    outliers_df = fd.outliers()
    return outliers_df
```

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

### How to use?

