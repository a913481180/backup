---
title: Python
date: 2023-01-01
categories:
  - python
---

python 国内常用镜像源

```txt
清华大学 ：https://pypi.tuna.tsinghua.edu.cn/simple/
阿里云：http://mirrors.aliyun.com/pypi/simple/
中国科学技术大学 ：http://pypi.mirrors.ustc.edu.cn/simple/
华中科技大学：http://pypi.hustunique.com/
豆瓣源：http://pypi.douban.com/simple/
腾讯源：http://mirrors.cloud.tencent.com/pypi/simple
华为镜像源：https://repo.huaweicloud.com/repository/pypi/simple/
```

1、临时使用

```bash
pip install [包名] -i [pip源URL]

# 示例
pip install pytest -i https://pypi.tuna.tsinghua.edu.cn/simple
# 或
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pytest
# 或
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```

2、永久使用
前提 pip 版本 (>=10.0.0) 方可使用此命令进行配置：

升级 pip 到最新的版本

```bash
pip install pip -U

# 或

python -m pip install --upgrade pip
```

修改 pip 源为清华源

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
