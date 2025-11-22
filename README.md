# OpenPanoramaMaker
A Google Colab-powered tool for automatically downloading images from Google Drive and stitching them into a seamless panorama using OpenCV. Perfect for easy, code-free panorama creation and visualization, directly in your browser.  Let me know if you want a more technical or creative version!

🖼️ Image Stitching Panorama with OpenCV & Google Colab

Create seamless panoramas by stitching multiple images together using OpenCV, fully integrated with Google Colab and Google Drive. This project makes it easy to generate panoramas from image collections in your Drive, with a focus on simplicity, usability, and reproducibility.

✨ Features

Automatic image download from Google Drive via gdown

Supports JPEG and PNG formats

Robust panorama stitching using OpenCV

Panorama display directly in Colab notebooks

Step-by-step workflow with user-friendly error messages

🛠️ Requirements

Google Colab account

Google Drive folder containing your images (JPG or PNG)

Python 3.7+

OpenCV (opencv-python)

gdown

🚀 Getting Started

Clone this repository and open the notebook in Google Colab
. Follow the steps below:

1. Install Dependencies
!pip install opencv-python gdown

2. Download Images from Google Drive

Get your folder's ID from the shareable link:
https://drive.google.com/drive/folders/<FOLDER_ID>

Replace YOUR_FOLDER_ID_HERE in the code:

import os

folder_id = "YOUR_FOLDER_ID_HERE"  # Replace with your folder ID
output_dir = "/content/images"
os.makedirs(output_dir, exist_ok=True)

!gdown --folder https://drive.google.com/drive/folders/{folder_id} -O {output_dir}

3. Load Images
import cv2

image_paths = sorted([
    os.path.join(output_dir, fname)
    for fname in os.listdir(output_dir)
    if fname.lower().endswith(('.jpg', '.png'))
])

images = [cv2.imread(p) for p in image_paths]

if any(img is None for img in images):
    raise ValueError("One or more images couldn't be read. Check the folder and file formats.")

4. Stitch Images
stitcher = cv2.Stitcher_create()
status, pano = stitcher.stitch(images)

if status != cv2.Stitcher_OK and status != 0:
    raise RuntimeError(f"Stitching failed with status {status}")

output_path = "/content/panorama_result.jpg"
cv2.imwrite(output_path, pano)
print(f"Panorama saved as {output_path}")

5. Display Panorama
from google.colab.patches import cv2_imshow
cv2_imshow(pano)

📝 Quick Workflow
Step	Action	Code Reference
1	Install Dependencies	!pip install opencv-python gdown
2	Download Images	gdown --folder <folder_id>
3	Load Images	cv2.imread()
4	Stitch Images	cv2.Stitcher_create()
5	Save & Display Panorama	cv2.imwrite() / cv2_imshow()
💡 Usage Tips

Ensure images have sufficient overlap for successful stitching.

Only JPG and PNG formats are supported.

Processing large image sets may take longer.

Error messages are descriptive and help identify issues quickly.

⚠️ Troubleshooting

Images won’t read: Check file formats and Drive permissions.

Stitching fails: Ensure images have enough overlap and similar resolution.

Notebook must run in Colab due to Google Drive integration.

📂 Example
folder_id = "https://drive.google.com/drive/folders/1qknsdFp0YnPNIbBJi1HcZdL65mas2BJQ?usp=sharing"
# Run the workflow sequentially to generate the panorama.

🎉 Happy Stitching!
