## Segment-Based Anomaly Detection for Greater Accuracy


In today's data-driven world, the ability to automatically detect and localize anomalies in images is crucial across a wide range of applications. Traditionally, anomaly detection has focused on identifying differences between entire images. However, a more granular approach that detects anomalies within segments of the same image can provide even greater value and insights. Here's why this localized approach to anomaly detection is so important:

1. **Quality Control in Manufacturing:** Ensuring product quality is crucial. Classic anomaly detection might miss small defects. Segment-level detection identifies minor issues, enhancing quality control and reducing waste.

2. **Medical Imaging:** Early detection of anomalies in medical images can save lives. Segment-level detection highlights small abnormalities in X-rays, MRIs, or CT scans, aiding accurate diagnoses and improving patient outcomes.

---

**In this work, we focus on segment-level anomaly detection. By analyzing smaller regions within an image, we can pinpoint anomalies that might be overlooked when considering the entire image.**

By leveraging the capabilities of the Fastdup library, we aim to enhance the capability of anomaly detection systems to operate at a finer granularity, delivering better insights and outcomes.

---

### Dataset

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


#### Some T-SQL Code

```tsql
SELECT This, [Is], A, Code, Block -- Using SSMS style syntax highlighting
    , REVERSE('abc')
FROM dbo.SomeTable s
    CROSS JOIN dbo.OtherTable o;
```

#### Some PowerShell Code

```powershell
Write-Host "This is a powershell Code block";

# There are many other languages you can use, but the style has to be loaded first

ForEach ($thing in $things) {
    Write-Output "It highlights it using the GitHub style"
}
```

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
