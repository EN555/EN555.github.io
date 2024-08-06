## Segment-Based Anomaly Detection for Greater Accuracy


In today's data-driven world, the ability to automatically detect and localize anomalies in images is crucial across a wide range of applications. Traditionally, anomaly detection has focused on identifying differences between entire images. However, a more granular approach that detects anomalies within segments of the same image can provide even greater value and insights. Here's why this localized approach to anomaly detection is so important:

### Quality Control in Manufacturing
In manufacturing, ensuring the quality of products is paramount. Anomalies such as defects in materials or assembly errors can lead to product failures, costly recalls, and damage to a brand's reputation. While classic anomaly detection can identify defective products by comparing entire images, it often misses subtle defects that only affect small areas of the product. Localized anomaly detection within segments of an image allows for the identification of minor defects that might otherwise go unnoticed, ensuring a higher level of quality control and reducing waste.

### Medical Imaging
In healthcare, early detection of anomalies in medical images can save lives. For instance, identifying unusual patterns in X-rays, MRIs, or CT scans can help diagnose diseases such as cancer at an early stage. Traditional methods compare entire scans to detect anomalies, but this approach can miss small, yet critical, abnormalities. By dividing images into segments and analyzing each part individually, localized anomaly detection can highlight suspicious areas with greater precision, aiding radiologists in making more accurate diagnoses and improving patient outcomes.

---

### Dataset

We conduct the experiments on the MVTec Anomaly Detection benchmark, that is, a famous dataset in the anomaly detection and localization field. MVTec AD
contains 5 texture and 10 object categories stemming from manufacturing with a total of 5354 images. The dataset
is composed of normal images for training and both normal and anomaly images with various types of defect for
test. It also provides pixel-level annotations for defective test images.

<p align="center">
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/006.png" alt="Image 006" width="200" />
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/037.png" alt="Image 037" width="200" />
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/263.png" alt="Image 263" width="200" />
</p>

In this dataset they too proveide defective images.

<p align="center">
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/007.png" alt="Image 007" width="200" />
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/024.png" alt="Image 024" width="200" />
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/016.png" alt="Image 016" width="200" />
</p>

---

<p align="center">
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/007-defect.png" alt="Image 007" width="200" />
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/024-defect.png" alt="Image 024" width="200" />
  <img src="https://github.com/EN555/EN555.github.io/raw/main/images/016-defect.png" alt="Image 016" width="200" />
</p>


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
