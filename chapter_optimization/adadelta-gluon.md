# Adadelta——使用`Gluon`

在`Gluon`里，使用Adadelta很容易。我们无需重新实现它。

```{.python .input}
%config InlineBackend.figure_format = 'retina'
%matplotlib inline
import mxnet as mx
from mxnet import autograd
from mxnet import gluon
from mxnet import nd
import numpy as np
import sys
sys.path.append('..')
import utils
```

```{.python .input  n=1}
# 生成数据集。
num_inputs = 2
num_examples = 1000
true_w = [2, -3.4]
true_b = 4.2
X = nd.random_normal(scale=1, shape=(num_examples, num_inputs))
y = true_w[0] * X[:, 0] + true_w[1] * X[:, 1] + true_b
y += .01 * nd.random_normal(scale=1, shape=y.shape)

# 线性回归模型。
net = gluon.nn.Sequential()
net.add(gluon.nn.Dense(1))
```

我们需要在`gluon.Trainer`中指定优化算法名称`adadelta`并设置rho参数。

使用Adadelta，最终学到的参数值与真实值较接近。

```{.python .input  n=3}
net.collect_params().initialize(mx.init.Normal(sigma=1), force_reinit=True)
trainer = gluon.Trainer(net.collect_params(), 'adadelta', {'rho': 0.9999})
utils.optimize(batch_size=10, trainer=trainer, num_epochs=3, decay_epoch=None,
               log_interval=10, X=X, y=y, net=net, print_lr=False)
```

## 小结

* 使用`Gluon`的`Trainer`可以方便地使用Adadelta。

## 练习

* 如果把试验中的参数rho改小会怎样，例如0.9？观察实验结果。


## 讨论

欢迎扫码直达[本节内容讨论区](https://discuss.gluon.ai/t/topic/2278)：

![](../img/qr_adadelta-gluon.svg)
