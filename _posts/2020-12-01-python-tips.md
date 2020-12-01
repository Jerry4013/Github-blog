---
title:  "Python -- Download all the files from a Github folder"
tags: Python
---

Github上，只提供了下载单个文件的“download"按钮。但作业要求下载某个文件夹下所有的emoji图片。

最后使用正则表达式解决了：

1. 用文件夹的url获取html文本
2. 用正则表达式匹配文本中的图片名称
3. 把图片名称拼接到下载的链接后面即可

```python
import requests
import re

def download(remoteurl: str, localfile: str):
    """
    Download remoteurl to localfile, unless localfile already exists.
    Returns the localfile string.
    """
    if not os.path.exists(localfile):
        print("Downloading %s..." % localfile)
        filename, headers = urllib.request.urlretrieve(remoteurl, localfile)
    return localfile


def download_emoji_data():
    url = "https://github.com/iamcal/emoji-data/raw/master/sheets-clean"
    html = requests.get(url).text
    re_file_names = re.compile(r"title=\"sheet_(\w+)\.png\"")
    for file_name in re_file_names.findall(html):
        download("https://github.com/iamcal/emoji-data/raw/master/sheets-clean/sheet_" + file_name + ".png", "emoji_data/sheets_" + file_name + ".png")

download_emoji_data()
```