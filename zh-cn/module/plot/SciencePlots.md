> SciencePlots 是科研样式绘图库

[Github传送门](https://github.com/garrettj403/SciencePlots)

## 安装

```bash
pip install SciencePlots
```

**tips**:

SciencePlots 库需要电脑安装 `LaTex` ，但你可以选择不安装并在代码中添加 `'no-latex'` ，其中

- MacOS 用户安装 [MacTex](https://www.tug.org/mactex/) 
- Windows 用户安装 [MikTex](https://miktex.org/) 

## 初始化绘图样式

在 SciencePlots 库中，科研绘图样式都是用的 science

```python
import matplotlib.pyplot as plt
 
plt.style.use('science')
```

可以同时设置多个样式

```python
plt.style.use(['science', 'ieee'])
```

在上面的代码中， **ieee** 会覆盖掉 **science** 中的某些参数（列宽、字号等），以达到符合 **IEEE** 论文的绘图要求

**临时使用**

```python
# 不使用latex渲染
with plt.style.context(['science', 'ieee', 'no-latex']):
    plt.figure()
    plt.plot(x, y)
    plt.show() 
    
# 使用latex渲染
with plt.style.context(['science', 'ieee']):
    plt.figure()
    plt.plot(x, y)
    plt.show()
```

## 案例

定义函数曲线， 准备数据

```python
import numpy as np
import matplotlib.pyplot as plt

#plt.rcParams['font.sans-serif']=['SimHei'] #显示中文标签
#plt.rcParams['axes.unicode_minus']=False   #这两行需要手动设置
 
def function(x, p):
    return x ** (2 * p + 1) / (1 + x ** (2 * p))
 
pparam = dict(xlabel='Voltage (mV)', ylabel='Current ($\mu$A)')
 
x = np.linspace(0.75, 1.25, 201)
```

### science样式

```python
with plt.style.context(['science', 'no-latex']):
    fig, ax = plt.subplots()
    for p in [10, 15, 20, 30, 50, 100]:
        ax.plot(x, function(x, p), label=p)
    ax.legend(title='Order')
    ax.autoscale(tight=True)
    ax.set(**pparam)
    fig.savefig('figures/fig1.pdf')
    fig.savefig('figures/fig1.jpg', dpi=300)
```

![science](https://cdn.jsdelivr.net/gh/choya-lee/PyAIO@master/zh-cn/module/plot/imgs/science.png)

### science+ieee样式

针对 **IEEE** 论文准备的 **science+ieee** 样式

```python
with plt.style.context(['science', 'ieee']):
    fig, ax = plt.subplots()
    for p in [10, 20, 40, 100]:
        ax.plot(x, function(x, p), label=p)
    ax.legend(title='Order')
    ax.autoscale(tight=True)
    ax.set(**pparam)
    
    # Note: $\mu$ doesn't work with Times font (used by ieee style)
    ax.set_ylabel(r'Current (\textmu A)')  
    
    fig.savefig('figures/fig2a.pdf')
    fig.savefig('figures/fig2a.jpg', dpi=300)
```

![science_ieee](https://cdn.jsdelivr.net/gh/choya-lee/PyAIO@master/zh-cn/module/plot/imgs/science_ieee.png)

### science+scatter样式

**IEEE** 要求图形以黑白打印时必须可读。**ieee** 样式还可以将图形宽度设置为适合 **IEEE** 论文的一列。

```python
with plt.style.context(['science', 'scatter']):
    fig, ax = plt.subplots(figsize=(4, 4))
    ax.plot([-2, 2], [-2, 2], 'k--')
    ax.fill_between([-2, 2], [-2.2, 1.8], [-1.8, 2.2],
                    color='dodgerblue', alpha=0.2, lw=0)
    for i in range(7):
        x1 = np.random.normal(0, 0.5, 10)
        y1 = x1 + np.random.normal(0, 0.2, 10)
        ax.plot(x1, y1, label=r"$^\#${}".format(i+1))
    ax.legend(title='Sample', loc=2)
    xlbl = r"$\log_{10}\left(\frac{L_\mathrm{IR}}{\mathrm{L}_\odot}\right)$"
    ylbl = r"$\log_{10}\left(\frac{L_\mathrm{6.2}}{\mathrm{L}_\odot}\right)$"
    ax.set_xlabel(xlbl)
    ax.set_ylabel(ylbl)
    ax.set_xlim([-2, 2])
    ax.set_ylim([-2, 2])
    fig.savefig('figures/fig3.pdf')
    fig.savefig('figures/fig3.jpg', dpi=300)
```

![science_scatter](https://cdn.jsdelivr.net/gh/choya-lee/PyAIO@master/zh-cn/module/plot/imgs/science_scatter.png)

### dark_background +science+high-vis

可以将这些样式与Matplotlib随附的其他样式结合使用。例如，dark_background +science+high-vis样式：

```python
with plt.style.context(['dark_background', 'science', 'high-vis']):
    fig, ax = plt.subplots()
    for p in [10, 15, 20, 30, 50, 100]:
        ax.plot(x, function(x, p), label=p)
    ax.legend(title='Order')
    ax.autoscale(tight=True)
    ax.set(**pparam)
    fig.savefig('figures/fig5.pdf')
    fig.savefig('figures/fig5.jpg', dpi=300)
```

![dark](https://cdn.jsdelivr.net/gh/choya-lee/PyAIO@master/zh-cn/module/plot/imgs/dark.png)

## 报错及解决

- **报错**

```bash
# 字体错误
findfont: Font family ['serif'] not found. Falling back to DejaVu Sans.
# 中文字符报错
RuntimeWarning: Glyph 30005 missing from current font.
Font 'default' does not have a glyph for '\u7535' [U+7535], substituting with a dummy symbol.
```

- **解决**
  
  *字体错误*

  
  1. 前往指定目录 `your_python_path/site-packages/matplotlib/mpl-data/fonts/ttf`，将下载好的 `SimHei `字体移动到该目录下。
  
  2. 修改 `matplotlibrc` 文件 `your_python_path/site-packages/matplotlib/mpl-data/matplotlibrc`，修改内容如下：
  
     ```
     font.family         : sans-serif   
     # 去掉前面的#     
     font.sans-serif     : SimHei, Bitstream Vera Sans, Lucida Grande, Verdana, Geneva, Lucid, Arial, Helvetica, Avant Garde, sans-serif  
     # 去掉前面的#，并在冒号后面添加SimHei
     axes.unicode_minus  : False
     # 去掉前面的#，并将True改为False
     ```
  
  3. 重启 python 环境

​      *中文字符报错*

​           解决操作步骤同上，只是安装字体为[指定字体](https://github.com/garrettj403/SciencePlots/wiki/FAQ#installing-cjk-fonts)

