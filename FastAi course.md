# Fast AI course

[Fast AI course](https://course.fast.ai/)

CPU - central processing unit [10s]
GPU - Graphics processing unit [10,000s]
TPU - Tensor processing unit [up to 128,000s]

# Set up

1. Get set up in [Google Colab (free)](https://course.fast.ai/start_colab.html) / AWS / Google Cloud Platform. Compatible with Github..
2. Head on to the Colab Welcome Page and click on ‘Github’. In the ‘Enter a GitHub URL or search by organization or user’ line enter ‘fastai/course-v3’. You will see all the courses notebooks listed there. Click on the one you are interested in using.
3. Cicking on the ‘Runtime’ tab and selecting ‘Change runtime type’. A pop-up window will open up with a drop-down menu. Select ‘GPU’ from the menu and click ‘Save’.
4. Install the necessary packages. You can do this by creating a code cell, and running:
```  !curl -s https://course.fast.ai/setup/colab | bash```
5. save your work to Google Drive. You can do this by clicking on ‘File’ and then ‘Save’. Your notebook will be saved in a folder called Colab Notebooks in your Google Drive by default.
6. To save files, you need to permit your Colaboratory instance to read and write files to your Google Drive. Add the following code snippet at the beginning of every notebook.


```
from google.colab import drive
drive.mount('/content/gdrive', force_remount=True)
root_dir = "/content/gdrive/My Drive/"
base_dir = root_dir + 'fastai-v3/'
```