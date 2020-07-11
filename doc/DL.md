
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

$`cost(w) = \left(\frac{1}{N}\right)`\sum_{n=1}^N(`\hat y`-y)``$



