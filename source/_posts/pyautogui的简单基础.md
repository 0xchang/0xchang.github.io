---
title: pyautogui的简单基础
date: 2021-11-24 17:35:56
tags: 
    - python
    - pyautogui
---

## pyautogui的简单基础

### 安装pyautogui

```powershell
pip install pyautogui
```

### 查看屏幕大小

```python
import pyautogui as pag
print(pag.size())            #左上角坐标为(0,0)
```

### 输出鼠标位置

```python
import pyautogui as pag
print(pag.position())
```

### 鼠标点击

```python
import pyautogui as pag
pag.click(0,200,clicks=3,interval=1,button='left')
#前面两个是坐标，clicks是点击次数，默认为1，interval是间隔时间，默认为0，button值为left是鼠标左键，right是右键
```

### 使用键盘的按键

```python
import pyautogui as pag
pag.press('enter')
```

### 鼠标移动

```python
import pyautogui as pag
pag.moveTo(600,800,duration=1)  
#前面两个参数是坐标，duration是花多少时间移动到目标位置
```

### 简单写一个实时打印鼠标位置的脚本

```python
import pyautogui as pag
from time import sleep
for i in range(1000):
    print(pag.position())
    sleep(0.1)
```

### 输入文字

```python
import pyautogui as pag
pag.typewrite('nihao ')
pag.press('enter')
#typewrite里面不能是中文，这个是模仿人在按键盘的情况，如果默认输入法是中文，就会模拟人打字拼音一样，最后打出汉字。最后的enter是模拟发送
```

