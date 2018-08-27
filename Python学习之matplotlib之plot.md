
#pyplot介绍

>matplotlib.pyplot  
>Provides a MATLAB-like plotting framework.



#正态分布图
```

mu, sigma = 0, 1 # mean and standard deviation 正态分布（Normal distribution）又名高斯分布（Gaussian distribution）
# 若随机变量X服从一个数学期望为μ、标准方差为σ2的高斯分布，记为：X∼N(μ,σ2), 正态分布的期望值μ决定了其位置，其标准差σ决定了分布的幅度。
# 我们通常所说的标准正态分布是μ = 0,σ = 1的正态分布（见右图中绿色曲线）。


s = np.random.normal(mu, sigma, 100000)
abs(mu - np.mean(s)) < 0.01
abs(sigma - np.std(s, ddof=1)) < 0.01

# matplotlib.pyplot.hist(
#    x, bins=10, range=None, normed=False,
#    weights=None, cumulative=False, bottom=None,
#    histtype=u'bar', align=u'mid', orientation=u'vertical',
#    rwidth=None, log=False, color=None, label=None, stacked=False,
#    hold=None, **kwargs)
#    参数讲解,参数个数很多，后面还是个边长参数，真是变态
# 　Compute and draw the histogram of x. The return value is a tuple (n, bins, patches) or ([n0, n1, ...], bins, [patches0, patches1,...]) if the input contains multiple data.
# x 这个参数是指定每个bin(箱子)分布的数据,对应x轴
# bins : integer or array_like, optional 这个参数指定bin(箱子)的个数,也就是总共有几条条状图

count, bins, ignored = plt.hist(s, 300, density=True) # hist　是　Plot a histogram.　柱状图的意思
x = bins
y = 1/(sigma * np.sqrt(2 * np.pi)) * np.exp( - (bins - mu)**2 / (2 * sigma**2) )

plt.plot(x, y, linewidth=1, color='r')

```




#正弦函数图
```
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(0, 50, 0.01);
y = np.sin(x)
plt.plot(x, y)
```



