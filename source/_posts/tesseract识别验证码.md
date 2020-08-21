title: tesseract识别验证码
author: hero576
tags:
  - python
categories:
  - programme
date: 2020-08-10 17:51:00
---
> 识别验证码
<!--more-->

# 使用tesseract-ocr
- 安装tesseract软件，[下载地址](https://github.com/UB-Mannheim/tesseract/wiki)
- 安装完毕后要将tesseract.exe路径添加到环境变量中，在cmd中输入`tesseract --version`测试安装是否成功

- 使用`subprocess.Popen`调用tesseract软件
```python
img = Image.open(filename)
img = img.convert('RGB') # 更改为RGB模式
img = img.point(lambda x: 0 if x < 143 else 255) # 二值化图像
borderImg = ImageOps.expand(img, 10, 'white')
borderImg.save(filename)
# 通过subprocess在当前py运行的进程中开启另外一个线程运行tesseract，并且当前线程暂停等待tesseract线程完成识别工作并将识别结果保存到captcha.txt中。
p = subprocess.Popen(['tesseract', 'captcha.jpg', 'captcha'], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
p.wait()
# 读取解析结果
with open('captcha.txt', 'r') as reader:
    catchacode = reader.read().replace(' ', '').replace('\n', '')
    print('验证码为：', catchacode)
```

# 使用pytesseract
- 安装环境
```bash
pip install pillow
pip install pytesseract
```

- 安装tesseract软件，[下载地址](https://github.com/UB-Mannheim/tesseract/wiki)，并配置环境变量

- 在python的安装路径下的修改安装的pytesseract库里面的pytesseract.py，将默认的改成Tesseract-OCR的安装路径
```python
tesseract_cmd=r'tesseract' #如果路径正确，可以不用更改
tesseract_cmd=r'path/tesseract.exe'
```

- 识别图片
```python
img = Image.open(filename)
img = img.convert('RGB') # 更改为RGB模式
text = pytesseract.image_to_string(img,lang='eng')
```

# 读取二进制图片
```python
# 示例代码
import requests
from PIL import Image
content = requests.get("http://my.cnki.net/Register/CheckCode.aspx?id=1563416120154").content
image = Image.frombytes(mode="RGBA",size=(64,25),data=content,decoder_name="raw")
```

- 抛出异常：`ValueError: not enough image data`
- 原因：图片为jpg格式，包括了图片的原始（jpg压缩后的）数据和（jpg）文件头，而frombytes只能读取纯二进制数据
- 解决方法一
```python
from io import StringIO,BytesIO
import requests
from PIL import Image
content = requests.get("http://my.cnki.net/Register/CheckCode.aspx?id=1563416120154").content
image = Image.open(BytesIO(content))
```

- 解决方法二:
```python
imgByteArr = BytesIO()
image.save(imgByteArr,format="PNG")
imgByteArr.getvalue()
```

# tesseract提高识别率

## 下载识别语言训练数据
- [下载地址](https://github.com/tesseract-ocr/tessdata)

## 修改图片的灰度
```python
from PIL import Image
from PIL import ImageEnhance
import pytesseract
img = Image.open('sanyecao.jpg')
img = img.convert('RGB')  #这里也可以尝试使用L
enhancer = ImageEnhance.Color(img)
enhancer = enhancer.enhance(0)
enhancer = ImageEnhance.Brightness(enhancer)
enhancer = enhancer.enhance(2)
enhancer = ImageEnhance.Contrast(enhancer)
enhancer = enhancer.enhance(8)
enhancer = ImageEnhance.Sharpness(enhancer)
img = enhancer.enhance(20)
text=pytesseract.image_to_string(img)
```

## 闭操作，去除噪点
```python
img = cv2.imdecode(np.frombuffer(ret.content, np.uint8), cv2.IMREAD_COLOR)
cv2.imshow("source", img)
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
res, binary = cv2.threshold(gray, 0, 255, cv2.THRESH_BINARY | cv2.THRESH_OTSU)
cv2.imshow("binary", binary)
kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (2, 2))
binary = cv2.morphologyEx(binary, cv2.MORPH_CLOSE, kernel)
cv2.imshow("close result", binary)
image = Image.fromarray(cv2.cvtColor(binary, cv2.COLOR_GRAY2RGB))
code = pytesseract.image_to_string(img)
```

# opencv转pillow
```python
img = cv2.imdecode(np.frombuffer(ret.content, np.uint8), cv2.IMREAD_COLOR)
cv2.imshow("source", img)
image = Image.fromarray(cv2.cvtColor(binary, cv2.COLOR_GRAY2RGB))
code = pytesseract.image_to_string(img)
```


# 问题
## from . import _imaging as core ImportError: DLL load failed: 找不到指定的模块
- 卸载pillow，重新安装
```
pip uninstall pillow
pip install pillow
```




