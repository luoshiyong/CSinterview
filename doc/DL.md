
# CUDA加速测试
```
import torch
# 以下代码只有在PyTorch GPU版本上才会执行
import time
print(torch.__version__)
print(torch.cuda.is_available())
a = torch.randn(10000,1000)
b = torch.randn(1000,2000)
t0 = time.time()
c = torch.matmul(a,b)
t1 = time.time()
print(a.device,t1-t0,c.norm(2))
 
device = torch.device('cuda')
a = a.to(device)
b = b.to(device)
t0 = time.time()
c = torch.matmul(a,b)
t1 = time.time()
print(a.device,t1-t0,c.norm(2))
 
t0 = time.time()
c = torch.matmul(a,b)
t1 = time.time()
print(a.device,t1-t0,c.norm(2))
```
# 梯度下降算法（gradient desent)
## 以直线模型y=x*w为例
## 数据集如下：
|x|y|
|-|-|
|1|2|
|2|4|
|3|6|

$cost(w) = \left(\frac{1}{N}\right)\sum_{n=1}^N(\hat y-y)^2$

梯度的方向一定是函数值上升的方向，最小值是0
梯度下降公式：$w = w - α\frac{\partial cost}{\partial w}$（α为学习率）

$\frac{\partial cost}{\partial w}$=$\frac{1}{N}\frac{\partial }{\partial w}\sum_{n=1}^N(x_n*w-y_n)^2$
 
   =$\frac{1}{N}\sum_{n=1}^N2x_n(x_n*w-y_n)$
   
   python代码：

```
x_data = [1.0,2.0,3.0]
y_data = [2.0,4.0,6.0]
w = 1.0

def forward(x):
    return x*w

def cost(xs,ys):
    cost = 0
    for x,y in zip(xs,ys):
        y_pred = forward(x)
        cost +=(y_pred-y) ** 2
    return cost/len(xs)
def gradient(xs,ys):
    grad = 0
    for x,y in zip(xs,ys):
        grad+=2*x*(x*w-y)
    return grad/len(xs)

print('Predict (before training)',4,forward(4))
for epoch in range(100):
    cost_val = cost(x_data,y_data)
    grad_val = gradient(x_data,y_data)
    w-=0.01*grad_val
    print('Epoch:',epoch,'w=',w,'loss=',cost_val)
print('Predict (after training)',4,forward(4))
```
若曲线波动比较大，可以用指数加权均值平滑

$C^*_0 = C_0$

$C^*_i = αC_i+(1-α)C^*_(i-1)$

## 随机梯度下降（Stochastic gradient descent）

梯度下降公式：$w = w - α\frac{\partial loss}{\partial w}$


$\frac{\partial loss}{\partial w}$=$2x_n(x_n*w-y_n)$

注意这里是对每一个样本求梯度，而梯度下降是对全部样本求梯度，这里就有一个很明显的问题，对于梯度下降在计算f(x)的梯度和计算f(x+1)的梯度是可以并行计算的，然而在随机梯度下降中显然是不行的，因为其计算每一个样本的梯度然后更新w，这个w又用于f(x+1)的计算，所以从时间复杂度上来看随机梯度下降差，但是性能上随机梯度下降好一些。




