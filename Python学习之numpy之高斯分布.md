
#介绍

正态分布（Normal distribution）又名高斯分布（Gaussian distribution），
是一个在数学、物理及project等领域都很重要的概率分布，在统计学的很多方面有着重大的影响力。

若随机变量X服从一个数学期望为μ、标准方差为σ2的高斯分布，记为：X~N(μ,σ2),


正态分布的期望值μ决定了其位置，其标准差σ决定了分布的幅度。因其曲线呈钟形，因此人们又常常称之为钟形曲线。
我们通常所说的标准正态分布是μ = 0,σ = 1的正态分布（见右图中绿色曲线）。

numpy random类中，看名称就是产生随机数的模块。

#numpy.random.normal() 高斯分布随机数

```
normal(...) method of mtrand.RandomState instance
    normal(loc=0.0, scale=1.0, size=None)

    Draw random samples from a normal (Gaussian) distribution.

    The probability density function of the normal distribution, first
    derived by De Moivre and 200 years later by both Gauss and Laplace
    independently [2]_, is often called the bell curve because of
    its characteristic shape (see the example below).

    The normal distributions occurs often in nature.  For example, it
    describes the commonly occurring distribution of samples influenced
    by a large number of tiny, random disturbances, each with its own
    unique distribution [2]_.

    Parameters
    ----------
    loc : float or array_like of floats
        Mean ("centre") of the distribution.
    scale : float or array_like of floats
        Standard deviation (spread or "width") of the distribution.
    size : int or tuple of ints, optional
        Output shape.  If the given shape is, e.g., ``(m, n, k)``, then
        ``m * n * k`` samples are drawn.  If size is ``None`` (default),
        a single value is returned if ``loc`` and ``scale`` are both scalars.
        Otherwise, ``np.broadcast(loc, scale).size`` samples are drawn.
    loc：均值（数学期望为μ） ，scale：标准差 （标准方差为σ2，标准差就是去掉平放），size：抽取样本的size
    通过文档可以看到size参数可以是一个数字，或者是一个元组。他来决定了输出结果的形状。
    如果传入的是一个单个数字 30，返回就会是30个长度的列表.

    Returns
    -------
    out : ndarray or scalar
        Drawn samples from the parameterized normal distribution.
    返回是一个数组和这种类型数组和list很像。

    See Also
    --------
    scipy.stats.norm : probability density function, distribution or
        cumulative density function, etc.

    Notes
    -----
    The probability density for the Gaussian distribution（高斯分布） is

    .. math:: p(x) = \frac{1}{\sqrt{ 2 \pi \sigma^2 }}
                     e^{ - \frac{ (x - \mu)^2 } {2 \sigma^2} },
        这个是高斯分布是函数式。
    where :math:`\mu` is the mean（平均数） and :math:`\sigma` the standard
    deviation. The square of the standard deviation, :math:`\sigma^2`,
    is called the variance.

    The function has its peak at the mean, and its "spread" increases with
    the standard deviation (the function reaches 0.607 times its maximum at
    :math:`x + \sigma` and :math:`x - \sigma` [2]_).  This implies that
    `numpy.random.normal` is more likely to return samples lying close to
    the mean, rather than those far away.

    References
    ----------
    .. [1] Wikipedia, "Normal distribution",
           http://en.wikipedia.org/wiki/Normal_distribution
    .. [2] P. R. Peebles Jr., "Central Limit Theorem" in "Probability,
           Random Variables and Random Signal Principles", 4th ed., 2001,
           pp. 51, 51, 125.

    Examples
    --------
    Draw samples from the distribution:

    >>> mu, sigma = 0, 0.1 # mean and standard deviation
    >>> s = np.random.normal(mu, sigma, 1000)

    Verify the mean and the variance:

    >>> abs(mu - np.mean(s)) < 0.01
    True

    >>> abs(sigma - np.std(s, ddof=1)) < 0.01
    True

    Display the histogram of the samples, along with
    the probability density function:

    >>> import matplotlib.pyplot as plt
    >>> count, bins, ignored = plt.hist(s, 30, density=True)
    >>> plt.plot(bins, 1/(sigma * np.sqrt(2 * np.pi)) *
    ...                np.exp( - (bins - mu)**2 / (2 * sigma**2) ),
    ...          linewidth=2, color='r')
    >>> plt.show()


n = np.random.normal(loc=0.0, scale=1, size=(3,3,3))
print(n)

上面输出下面的一个列表。可以看到size参数 如果是3个数字的元组，就返回3*3*3的数组，
[[[ 1.0172058  -0.44269518  0.26462677]
  [-0.35355925  0.6063244   1.26014832]
  [-0.18538448 -0.49259078 -0.62822534]]

 [[ 0.95726339 -0.06239384 -1.56474133]
  [ 1.42287373 -0.50173702  2.1642026 ]
  [-0.54096807  0.2472884  -1.1990265 ]]

 [[ 0.15663884 -0.18501496 -1.80360639]
  [ 0.81581949 -2.73858599  0.34537614]
  [-0.50873844  0.0351258   0.14204044]]
  ]



n = np.random.normal(loc=0.0, scale=1, size=(2,3))
print(n)
size参数 如果是2个数字的元组，就返回2*3的 数组，
[[-0.45735578 -0.53921269 -0.67449221]
 [-0.98068719 -0.37125721 -0.43999013]]




import pprint
n = np.random.normal(loc=0.0, scale=1, size=30)
pprint.pprint(n)
array([ 0.59407443, -1.36867189, -0.32986369,  0.80101075,  0.72632235,
        0.347779  ,  0.10520544,  0.9492837 ,  2.19274468,  1.59970721,
       -0.95508177, -1.12671986, -0.53202767,  0.25783216, -1.1101487 ,
        0.78002647, -0.14404636, -1.50865102,  1.29681861, -0.67255912,
       -0.97184549, -0.30896753,  0.94493543,  0.686387  ,  0.89299833,
       -0.17019804, -0.12766749, -0.30600834, -0.0332422 , -0.05667029])



```

# 正态分布 图
我们可以借助matplotlib模块来画出正态分布的图
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
# Compute and draw the histogram of x. The return value is a tuple (n, bins, patches) or ([n0, n1, ...], bins, [patches0, patches1,...]) if the input contains multiple data.
# x 这个参数是指定每个bin(箱子)分布的数据,对应x轴
# bins : integer or array_like, optional 这个参数指定bin(箱子)的个数,也就是总共有几条条状图

count, bins, ignored = plt.hist(s, 300, density=True) # hist　是　Plot a histogram.　柱状图的意思
x = bins
y = 1/(sigma * np.sqrt(2 * np.pi)) * np.exp( - (bins - mu)**2 / (2 * sigma**2) )

plt.plot(x, y, linewidth=1, color='r')
```

# 代码例子
```
"""
=========================================================
Demo of the histogram (hist) function with a few features
=========================================================

In addition to the basic histogram, this demo shows a few optional
features:

    * Setting the number of data bins
    * The ``normed`` flag, which normalizes bin heights so that the
      integral of the histogram is 1. The resulting histogram is an
      approximation of the probability density function.
    * Setting the face color of the bars
    * Setting the opacity (alpha value).

Selecting different bin counts and sizes can significantly affect the
shape of a histogram. The Astropy docs have a great section on how to
select these parameters:
http://docs.astropy.org/en/stable/visualization/histogram.html
"""

import numpy as np
import matplotlib.mlab as mlab
import matplotlib.pyplot as plt

np.random.seed(0)

# example data
mu = 100  # mean of distribution
sigma = 15  # standard deviation of distribution
x = mu + sigma * np.random.randn(437)

num_bins = 50

fig, ax = plt.subplots()

# the histogram of the data
n, bins, patches = ax.hist(x, num_bins, normed=1)

# add a 'best fit' line
y = mlab.normpdf(bins, mu, sigma)
ax.plot(bins, y, '--')
ax.set_xlabel('Smarts')
ax.set_ylabel('Probability density')
ax.set_title(r'Histogram of IQ: $\mu=100$, $\sigma=15$')

# Tweak spacing to prevent clipping of ylabel
fig.tight_layout()
plt.show()

```