# Easy-Editor

--> to line 165 for readme
---------------------------------------------------------------------------------------------------------------
#create the Easy Editor photo editor here!
import os
from PyQt5.QtWidgets import (
   QApplication, QWidget,
   QFileDialog, # Dialogue for opening files (and folders)
   QLabel, QPushButton, QListWidget,
   QHBoxLayout, QVBoxLayout
)

from PyQt5.QtCore import Qt # needs a Qt.KeepAspectRatio constant to resize while maintaining proportions
from PyQt5.QtGui import QPixmap # screen-optimised
from PIL import Image

from PIL import ImageFilter
from PIL.ImageFilter import (
BLUR, CONTOUR, DETAIL, EDGE_ENHANCE, EDGE_ENHANCE_MORE,
EMBOSS, FIND_EDGES, SMOOTH, SMOOTH_MORE, SHARPEN,
GaussianBlur, UnsharpMask
)

app = QApplication([])
win = QWidget()      
win.resize(700, 500)
win.setWindowTitle('Easy Editor')

lb_image = QLabel("Image")
btn_dir = QPushButton("Folder")
lw_files = QListWidget()
btn_left = QPushButton("Left")
btn_right = QPushButton("Right")
btn_flip = QPushButton("Mirror")
btn_sharp = QPushButton("Sharpness")
btn_bw = QPushButton("B/W")
btn_blur = QPushButton("Blur")
row = QHBoxLayout()          # Main line
col1 = QVBoxLayout()         # divided into two columns
col2 = QVBoxLayout()
col1.addWidget(btn_dir)      # in the first - directory selection button
col1.addWidget(lw_files)     # and file list
col2.addWidget(lb_image, 95) # in the second - image
row_tools = QHBoxLayout()    # and button bar
row_tools.addWidget(btn_left)
row_tools.addWidget(btn_right)
row_tools.addWidget(btn_flip)
row_tools.addWidget(btn_sharp)
row_tools.addWidget(btn_bw)
row_tools.addWidget(btn_blur)
col2.addLayout(row_tools)
row.addLayout(col1, 20)
row.addLayout(col2, 80)
win.setLayout(row)
win.show()

workdir = ''

def filter(files, extensions):
   result = []
   for filename in files:
       for ext in extensions:
           if filename.endswith(ext):
               result.append(filename)
   return result

def chooseWorkdir():
   global workdir
   workdir = QFileDialog.getExistingDirectory()

def showFilenamesList():
    extensions = ['.jpg','.jpeg', '.png', '.gif', '.bmp']
    chooseWorkdir()
    filenames = filter(os.listdir(workdir), extensions)
    lw_files.clear()
    for filename in filenames:
        lw_files.addItem(filename)

btn_dir.clicked.connect(showFilenamesList)

class ImageProcessor():
    def __init__(self):
        self.image = None
        self.dir = None
        self.filename = None
        self.save_dir = "Modified/"
    def loadImage(self, dir, filename):
        '''When loading, remember the path and file name'''
        self.dir = dir
        self.filename = filename
        image_path = os.path.join(dir, filename)    # not needed now but become useful
        self.image = Image.open(image_path)
    def showImage(self, path):
        lb_image.hide()
        pixmapimage = QPixmap(path)
        w, h = lb_image.width(), lb_image.height()
        pixmapimage = pixmapimage.scaled(w, h, Qt.KeepAspectRatio)
        lb_image.setPixmap(pixmapimage)
        lb_image.show()
    def saveImage(self):
        path = os.path.join(self.dir, self.save_dir)
        os.path.exists(path)
        if not( (os.path.exists(path)) or (os.path.isdir(path)) ):
            os.mkdir(path)
        image_path = os.path.join(path, self.filename)
        self.image.save(image_path)
    
    def do_bw(self):
        lb_image.hide()
        self.image = self.image.convert("L")
        self.saveImage()
        image_path = os.path.join(self.dir, self.save_dir, self.filename)
        self.showImage(image_path)
    def do_left(self):
        lb_image.hide()
        self.image = self.image.transpose(Image.ROTATE_90)
        self.saveImage()
        image_path = os.path.join(self.dir, self.save_dir, self.filename)
        self.showImage(image_path)
    def do_right(self):
        lb_image.hide()
        self.image = self.image.transpose(Image.ROTATE_270)
        self.saveImage()
        image_path = os.path.join(self.dir, self.save_dir, self.filename)
        self.showImage(image_path)
    def do_flipped(self):
        lb_image.hide()
        self.image = self.image.transpose(Image.FLIP_LEFT_RIGHT)
        self.saveImage()
        image_path = os.path.join(self.dir, self.save_dir, self.filename)
        self.showImage(image_path)
    def do_sharp(self):
        lb_image.hide()
        self.image = self.image.filter(SHARPEN)
        self.saveImage()
        image_path = os.path.join(self.dir, self.save_dir, self.filename)
        self.showImage(image_path)
    def do_blur(self):
        lb_image.hide()
        self.image = self.image.filter(BLUR)
        self.saveImage()
        image_path = os.path.join(self.dir, self.save_dir, self.filename)
        self.showImage(image_path)

workimage = ImageProcessor()
btn_bw.clicked.connect(workimage.do_bw)
btn_left.clicked.connect(workimage.do_left)
btn_right.clicked.connect(workimage.do_left)
btn_flip.clicked.connect(workimage.do_flipped)
btn_sharp.clicked.connect(workimage.do_sharp)
btn_blur.clicked.connect(workimage.do_blur)

def showChosenImage():
   if lw_files.currentRow() >= 0:
       filename = lw_files.currentItem().text()
       workimage.loadImage(workdir, filename)
       image_path = os.path.join(workdir, filename)
       workimage.showImage(image_path)

lw_files.currentRowChanged.connect(showChosenImage)

app.exec()
--------------------------------------------------------------------------------------------------
You can edit any picture file with this: blur, sharpen, smooth, edge-enhance and more!
Made using PyQt and PIL and os.
You can edit files by doing the following:

1. Opening the Easy-Editor.py file
2. Click on the "Folder Button"
3. Select any folder that contains any picture files
4. All the picture files will appear in the list widget at the left side of the application
5. Select one of the picture files
6. Apply any one of the modifications to the picture by clicking on the various buttons at the bototm of the screen
7. Done! The modified picture(s) will be saved inside a folder called "Modified" in the folder that conatined the original picture files. These original files will still be in the original file, you will not have lost them!


e.g. I did this with Waluigi's face:



--BEFORE--
![image](https://user-images.githubusercontent.com/85016636/120067386-3ea98080-c073-11eb-86b2-6033f9d4b99e.png) copy and paste the link inside the brackets

--AFTER--
![image](https://user-images.githubusercontent.com/85016636/120067460-98aa4600-c073-11eb-89dc-1684f22c6e82.png) do the same for this

Thanks for reading, hope u enjoy ;)
