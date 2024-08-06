## Localize Anomaly Detection

Due to a plugin called `jekyll-titles-from-headings` which is supported by GitHub Pages by default. The above header (in the markdown file) will be automatically used as the pages title.

If the file does not start with a header, then the post title will be derived from the filename.

This is a sample blog post. You can talk about all sorts of fun things here.

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
