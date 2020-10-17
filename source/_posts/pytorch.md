---
title: PyTorch
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-10-15 11:52:15
---
> 
<!-- more -->

# 简介
## 介绍
- [官网](https://pytorch.org/)

## 安装
- 在[官网](https://pytorch.org/)选择自己的环境安装
`conda install pytorch torchvision cudatoolkit=10.1 -c pytorch`

# 变量
## 数据类型
cpu类型|gpu类型|说明
-|-|-
torch.ShortTensor|torch.cuda.ShortTensor|16-bit
torch.IntTensor|torch.cuda.IntTensor|32-bit
torch.LongTensor|torch.cuda.LongTensor|64-bit
torch.FloatTensor|torch.cuda.FloatTensor|32-bit
torch.DoubleTensor|torch.cuda.DoubleTensor|64-bit
torch.ByteTensor|torch.cuda.ByteTensor|8-bit
torch.CharTensor|torch.cuda.CharTensor|8-bit
torch.MalfTensor|torch.cuda.MalfTensor|16-bit

## 方法

函数|说明
-|-
类型推断|`a.type()`
类型判断|`isinstance(a,torch.FloatTensor)`
gpu转化|`data=data.cuda()`
张量形状|`a.shape`
张量尺寸|`a.size()`
内存|`a.numel()`
维度|`a.dim()`


## 标量

函数|说明
-|-
创建标量| `a = torch.tensor(1.)`

## 向量/张量

创建矩阵|说明
-|-
list矩阵| `a = torch.tensor([1.])`
numpy矩阵| `torch.from_numpy(np.ones(2))`
给定维度| `a = torch.FloatTensor(1)`
list矩阵| `a = torch.FloatTensor([2,3])`
全一矩阵| `torch.ones(2)`
全零矩阵|`torch.zeros(3)`
对角矩阵|`torch.eye(3)`
随机正态分布矩阵| `torch.randn(2,3)`<br>`torch.normal(mean=torch.full([10],0),std=torch.arange(1,0,-0.1))`
随机均匀[0,1]矩阵| `torch.rand(2,3)`
随机聚云分布|`torch.randint(1,10)`
给定值的矩阵|`torch.full([2,3],7)`
递增矩阵|`torch.arange(0,10,2)`
等分数列|`torch.linspace(0,10,steps=4)`
等指数列|`torch.logspace(0,-1,steps=4)`
随机打散|`torch.randperm(10)`

## 初始化

创建矩阵|说明
-|-
未初始化数据| `torch.empty(2,3)`<br>`a = torch.FloatTensor(2,3)`,易出现nan，inf等数据
随机均匀初始化|`torch.rand(2,3)`
根据传入的tensor随机初始化|`torch.rand_like(a)`
随机种子|`torch.manual_seed(23);np.random.seed(23)`


## 索引和切片

函数|说明
-|-
索引|`a[0,0]`
切片|`a[1:,2:,:,::2]`
索引|`a.index_select(1,[1,2])`
索引|`a[:,0,...]`
掩码索引|`mask = x.ge(0.5)`<br>`torch.masked_select(a,mask)`
打平后根据索引取|`torch.take(a,torch.torch.tensor([0,2,0]))`


## 维度变换

函数|说明
-|-
更改维度|view/reshape两者相同，`a.view(4,28*28)`
减少、增加维度|squeeze/unsqueeze，squeeze挤压维度不为1则返回原值。`a.unsqueeze(0)`
扩展|expand扩展数据/repeat扩展数据并复制数据，`a.expand(-1,32,14,14)`,`a.repeat(1,32,14,14)`拷贝次数
转置|transpose/t/permute，t只适应于2维，transpose即可实现转置，还可维度交换。`a.transpose(1,3).contiguous().view(4,3*32*32).view(4,3,32,32)`维度污染<br>`a.transpose(1,3).contiguous().view(4,3*32*32).view(4,32,32,3).transpose(1,3)`<br>a.permute(0,2,3,1)

## broadcasting自动扩展
- broadcasting是一种机制，可以维度自动扩展，以适应大小维度不同的向量的运算或者叠加

## Merge Split

函数|说明
-|-
cat|`torch.cat([a,b],dim=0)`，不支持broadcasting时，需要除0维度外的col维度相同
stack|`torch.stack([a,b],dim=0)`，与cat不同，会在dim=0新增维度，组合ab，ab维度必须完全一致
split|根据长度拆分a.size(5,1,1)，长度相同`a.split(2,dim=0)`，长度不同`a.split([1,1,3],dim=0)`
chunk|根据数量拆分`a.chunk(2,dim=0)`


# 运算

## 基本算数
函数|说明
-|-
加减乘除|add,sub,mul,div`torch.all(torch.eq(a-b,torch.sub(a,b)))`
矩阵乘法|2维：`torch.mm`<br>2维或3维：`torch.matmul`、`@`
平方|`a.pow(2)`，`a**2`
根号|`a.sqrt()`，`a**0.5`
平方根的导数|`a.rsqrt()`
指数|`torch.exp(a)`
对数|`torch.log(a)`，默认以e为底
近似解|向下取整`floor`<br>向上取整`ceil`<br>四舍五入`round`<br>整数部分`trunc`<br>小数部分`frac`
裁剪|`a.clamp(10)`小于10的变为10<br>`a.clamp(0,10)`小于0的变为0，大于10取10

## 统计运算

函数|说明
-|-
分布|范数：`norm(n=1)`,`mean`,`sum`,累乘`prod`
最值|`max`,`min`,`argmin`,`argmax`,参数`dim`,`keepdim`
最值|`kthvalue`,前几名的top-5的值`topk`
比较|判断每个元素`eq`,判断是否相等`equal`,`gt`,`>,>=,<,<=,!=,==`

## 条件操作
- 为了更好的利用gpu的功能，pytorch的很多操作都进行了封装，实现高度并行

函数|说明
-|-
条件|`torch.where(condition,x,y)`condition用来表示是从x还是y取值
查表|`gather(input,dim,index,out=None)`


## 导数和梯度
- 导数derivate
- 偏微分partial derivate、梯度gradient(偏微分的集合，是一个标量)

```py
x = torch.ones(1);w=torch.full([1],2.);w.requires_grad_();mse=F.mse_loss(torch.ones(1),x*w)
```

函数|说明
-|-
手动导数|`torch.autograd.grad(mse,[w])`，这里需要对w配置求导，`w.requires_grad_()`，求导一次会对生成图进行清理，可以设置`retain_graph=True`多次求导
自动化|对mse进行反向传播`mse.backword()`，在`w.grad`中查看w的梯度

# 网络
## 激活函数
- 除了torch库，`from torch.nn import funtional as F`也有sigmoid函数

函数|说明
-|-
sigmoid|`torch.sigmoid(a)`
tanh|`torch.tanh(a)`
ReLU|`torch.relu(a)`<br>`F.leaky_relu`<br>`F.selu`<br>`F.softplus`

## 损失函数
- MSE(Mean Squared Error)均方差
- Cross Entropy Loss：可用于二分类和多分类，与softmax搭配使用
- Hinge Loss：$\sum_i{}{max(0,1-y_i*h_\theta(x_i))}$


函数|说明
-|-
MSE|`F.mse_loss(y,pred)`与`torch.norm(y,pred).pow(2)`相同
Cross Entropy|`F.cross_entropy(x,y)`与`pred=F.softmax(y,dim=1);pre_log=torch.log(pred);F.nil_loss(pre_log,y)`相同
softmax|`F.softmax(y,dim=1)`


## 网络层
- 模型类创建过程
  - 引入`import torch.nn as nn`
  - 类继承`nn.Module`
  - 完成init、forward函数，pytorch会自动推导反向传播的函数

```py
class MLP(nn.Module):
    def __init__(self):
        super(MLP, self).__init__()
        self.model = nn.Sequential( # 容器，可以传入任意继承nn.module的类
            nn.Linear(784, 200),nn.ReLU(inplace=True),
            nn.Linear(200, 200),nn.ReLU(inplace=True),
            nn.Linear(200, 10),nn.ReLU(inplace=True),
        )
    def forward(self, x):
        x = self.model(x)
        return x
```


## GPU加速

```py
device = torch.device('cuda:0')
net = MLP().to(device) # 转为cuda
optimizer = optim.SGD(net.parameters(),lr=learning_rate)
criten = nn.CrossEntropyLoss().to(device)
for epoch in range(epochs):
  for batch_idx,(data,target) in enumerate(train_loader):
    data = data.view(-1,28*28)
    data,target = data.to(device),target.cuda() # cuda是老版本方法，这种方法没有使用配置的device，不方便更改
```

## 验证测试
- 在训练每次迭代时，统计验证集的准确率，查看模型是否过拟合

```py
for epoch in range(epochs):
    for batch_idx, (data, target) in enumerate(train_loader):
        data = data.view(-1, 28*28)
        data, target = data.to(device), target.cuda()
        logits = net(data)
        loss = criteon(logits, target)
        optimizer.zero_grad()
        loss.backward()
        # print(w1.grad.norm(), w2.grad.norm())
        optimizer.step()
        if batch_idx % 100 == 0:
            print('Train Epoch: {} [{}/{} ({:.0f}%)]\tLoss: {:.6f}'.format(epoch, batch_idx * len(data), len(train_loader.dataset),100. * batch_idx / len(train_loader), loss.item()))
    test_loss = 0
    correct = 0
    for data, target in test_loader:
        data = data.view(-1, 28 * 28)
        data, target = data.to(device), target.cuda()
        logits = net(data)
        test_loss += criteon(logits, target).item()
        pred = logits.argmax(dim=1)
        correct += pred.eq(target).float().sum().item()
    test_loss /= len(test_loader.dataset)
    print('\nTest set: Average loss: {:.4f}, Accuracy: {}/{} ({:.0f}%)\n'.format(test_loss, correct, len(test_loader.dataset),100. * correct / len(test_loader.dataset)))
```

# 可视化工具
## tensorboardX
- 安装`pip install tensorboardX`
- 使用tensorboardX，主要监听的是numpy数据，使用torch是，必须`param.clone().cpu().data.numpy()`转换才能完成可视化过程
```py
from tensorboardX import SummaryWritter
writer = SummaryWriter()
writer.add_scalar('data/scalar1',dummy_s1[0],n_iter)#监听一个数据
writer.add_scalars('data/scalar_group',{'xsinx':n_iter*np.sin(n_iter),'xcosx':n_iter*np.cos(n_iter),'arctanx':np.arctan(n_iter)},n_iter)
writer.add_image('image',x,n_iter)#监听多个数据
writer.add_text('Text','text logged at step:'+str(n_iter),n_iter)
for name,param in resnet18.named_parameters():
  writer.add_histogram(name,param.clone().cpu().data.numpy(),n_iter)
writer.close()
```

## visdom from facebook
- visdom简化了转化过程，直接就可以展示torch的数据
- 安装`pip install visdom`
- 开启服务：`python -m visdom.server`
- 使用visdom
```py
from visdom import Visdom
viz=Visdom()
# 一条线
viz.line([0.],[0.],win='train_loss',opts=dict(title='train loss'))
viz.line([loss.item()],[global_step],win='train_loss',update='append')
# 多条线
viz.line([0.,0.],[0.],win='test',opts=dict(title='test loss'),legend=['loss','acc'])
viz.line([[test.loss,correct/len(test_loader.dataset)]],[global_step],win='test',update='append')
viz.images(data.view(-1,1,28,28),win='x')
viz.text(str(pre.detach().cpu().numpy()),win='pred',opts=dict(title='pred'))
```

# 优化工具
## 过拟合和欠拟合
- 欠拟合：即使数据增加，acc和loss都无法得到更好的结果的时候，考虑是否模型表征能力不足。可以通过小量的数据训练模型，查看是否会过拟合来判断。
- 过拟合：训练数据表现越来越好，测试数据表现变得更差。

## 数据划分
- 数据划分：`train,val = torch.utils.data.random_split(train_db,[50000,100o0])`
- 训练数据：`train_loader = torch.utils.data.DataLoader(train,batch_size=1,shuffle=True)`
- 验证数据：`val_loader = torch.utils.data.DataLoader(val,batch_size=1,shuffle=False)`

## 正则化
### L2正则化
- $J(\theta)=J(\theta)+\frac{\lambda}{2n}\sum_{w_i^2}$
- 在pytorch只需设置`weight_decay`即可实现L2正则化，代表$\lambda$
```py
net=MLP()
optimizer = optim.SGD(net.paramters(),lr=0.01,weight_decay=0.01)
criteon=nn.CrossEntropyLoss()
```

### L1正则化
- L1正则化需要手动实现
```py
regularization_loss=0
for param in model.parameters():
    regularization_loss=torch.sum(torch.abs(param))
classify_class=criteon(logits,target)
loss=classify_class+0.01*regularization_loss
optimizer.zero_grad()# 调用backward()函数之前都要将梯度清零，因为如果梯度不清零，pytorch中会将上次计算的梯度和本次计算的梯度累加。这样逻辑的好处是，当我们的硬件限制不能使用更大的bachsize时，使用多次计算较小的bachsize的梯度平均值来代替，更方便，坏处当然是每次都要清零梯度。
loss.backward()
optimizer.step()
```

## 动量
- 梯度更新公式：$w^{k+1}=w^k-\alpha\triangledown f(w^k)$，梯度方向减小
- 增加动量后：$w^{k+1}=w^k-\alpha z^{k+1}$,$z^{k+1}=\beta z^k+\triangledown f(w^k)$，增加了系数为$\beta$的动量

- pytorch的支持，只需更改`momentum`参数
```py
optimizer = optim.SGD(model.paramters(),lr=0.01,momentum=0.78,weight_decay=0.01)
```

## Learning Rate Decay
- 衰减lr在大多数情况下，都可以加快模型的训练
- pytorch实现衰减:
```py
# 方法一
optimizer = optim.SGD(model.paramters(),lr=0.01,momentum=0.78,weight_decay=0.01)
scheduler = torch.optim.lr_scheduler.ReduceLROnplateau(optimizer,'min')`
for epoch in range(epochs):
    train(train_loader,model,criterion,optimizer,epoch)
    avg,loss=validate(val_loader,model,criterion,epoch)
    scheduler.step(loss)
# 方法二
scheduler = StepLR(optimizer,step_size=30,gamma=0.1)`
for epoch in range(epochs):
    train(train_loader,model,criterion,optimizer,epoch)
    avg,loss=validate(val_loader,model,criterion,epoch)
    scheduler.step(loss)
```

## Early Stop和Dropout
- 防止过拟合
```py
# 增加dropout
net_dropped = torch.nn.Sequential(
    torch.nn.Linear(784,200),
    torch.nn.Dropout(0.5),
    torch.nn.ReLU(),
    torch.nn.Linear(200,10),
)
```

- 在test时，不需要进行dropout，需要设置网络为evaluation
```py
net_dropout.train5()
net_dropout.eval()
```

# 卷积神经网络
## 卷积层
```py
# 类实现
layer=nn.Conv2d(1,3,kernal_size=3,stride=1,padding=0)
x=torch.rand(1,1,28,28)
out=layer.forward(x)
out=layer(x)# 与-layer.forward(x)不同，pytorch会执行内置的一些机制
# 函数实现
w=torch.rand(16,3,5,5)
b=torch.rand(16)
out=F.conv2d(x,w,b,stride=1,padding=1)
```

## 池化层
```py
layer=nn.MaxPool2d(2,stride=2)
```

## 上采样
```py
layer=F.interpolate(x,scale_factor=2,mode='nearest')
```

## BN
- Image Normalization，图像数据一般为0~255，在训练时一般归一化
```py
normalize = transform.Normalize(mean=[0.485,0.456,0.406],std=[0.229,0.224,0.225])
```

- Batch Normalization，在必须使用sigmoid函数时，超出0的范围越大，梯度变化越微弱，所以通过BN将数据归一化到N(0,1)的分布中，可以加速数据的收敛。
![BN](/images/pasted-292.png)

```py
x=torch.rand(100,16,784)
layer=nn.BatchNorm1d(16)
out=layer(x)
layer.running_mean
layer.running_var
```

```py
x=torch.rand(100,16,28,28)
layer=nn.BatchNorm2d(16)
out=layer(x)
layer.weight # gamma
layer.bias # beta
vars(layer)
```

- 在进行测试时，$\gamma,\beta$无法获得，一般需要对running_mean、running_var赋值为全局的，同时将网络设定为预测模式
```py
layer.eval()
BatchNorm1d(16,eps=1e-5,momentum=0.1,affine=True,track_running_stats=True)
```


### 网络的属性

函数|说明
-|-
权重|`layer.weight`<br>`layer.weight.shape`
偏置|`layer.bias`<br>`layer.bias.shape`


## nn.Module
- 是layer最基本的父类，linear、BatchNorm2d、conv2d、dropout
- 继承Module可以嵌套，使用容器Sequential()传入即可实现多个层的串联
- 参数可以有效管理，`net.parameter`还可以自动命名
- 方便管理所有的层，`net.0.net`，0号是自己本身
- 方便转义gpu，`net.to(device)`
- 加载和保存，`net.load_state_dict(torch.load('ckpt.mdl'))`，`net.save(net.state_dict(),'ckpt.mdl')`
- train和test状态切换，`net.train()`，`net.eval()`
- 方便自定义类

### nn.Sequential
- 容器Sequential()传入即可实现多个层的串联

### nn.Parameter
- 通过Parameter包装后的变量，才可以加到`nn.parameter()`中进行优化。torch.Tensor则不会被优化
```py
self.w = nn.Parameter(torch.randn(outp,inp))
```

## 数据增强
- 常用的图像增强的手段：flip、rotate、scale、random move、crop、GAN、Noise
- Noise需要自己实现

### transforms.Compose
- 容器`transforms.Compose`可以实现多个操作的串联

```py
train_loader = torch.utils.data.DataLoader(
    datasets.MNIST('../data', train=True, download=True,
    transform=transforms.Compose([
        transforms.RandomHorizontalFlip(),# 随机的水平翻转
        transforms.RandomVerticalFlip(),# 随机的垂直翻转
        transforms.RandomRotate(15),# 随机的在[-15,15]中旋转
        transforms.RandomRotate([90,180,270]),# 随机的在[90,180,270]中旋转
        transforms.Resize([32,32]), # 从28*28，resize为32*32的大小
        transforms.RandomCrop([28,28]), # 随机裁剪
        transforms.ToTensor(),
        transforms.Normalize((0.1307,), (0.3081,)) 
    ])),batch_size=batch_size, shuffle=True)
```


# RNN
## RNN
```py
rnn = nn.RNN(input_size=100,hidden_size=10,num_layers=1)
out,ht = rnn.forward(x,h0)
```

```py
rnn = nn.RNNCell(input_size=100,hidden_size=10,num_layers=1)
out,ht = rnn.forward(x,h0)
```

## LSTM

```py
rnn = nn.LSTM(input_size=100,hidden_size=10,num_layers=1)
out,(ht,ct) = rnn.forward(x,[h0,c0])
```

# 数据集
## 自定义数据集
- 继承dataset类，实现`__len__`和`__getitem__`两个方法

```py
class NumberDataset(Dataset):
    def __init__(self,training=True):
        if training:
            self.samples = list(range(1,1000))
        else:
            self.samples = list(range(1001,1500))
    def __len__(self):
        return len(self.samples)
    def __getitem__(self,idx):
        return self.samples[idx]
```

- 如果文件目录是按照类别存储的，可以使用内置函数，方便的转化

```py
tf = transforms.Compose([
    transforms.Resize((224,224)),
    transforms.ToTensor(),
])
db = torchvision.datasets.ImageFolder(root='image',transform=tf)
loader = DataLoader(db,batch_size=32,shuffle=True,num_works=8)
print(db.class_to_idx)
for x,y in loader:
    viz.images(x,nrow=8,win="batch",opts=dict(title='batch'))
    viz.text(str(y.numpy()),win="batch",opts=dict(title="batch-y"))
```


# 迁移网络

```py
from torchvision.models import resnet18
trained_model = resnet18(pretrained=True)
model = nn.Sequential(
    *list(trained_model.children())[:-1],
    nn.Flatten(),
    nn.Linear(512,5),
)
```


# 代码总结
## 局部极小点获取
```py
import matplotlib.pyplot as plt
def himmelblau(x):
    return (x[0]**2+x[1]-11)**2+(x[0]+x[1]**2-7)**2
a = np.arange(-6,6,0.1)
b = np.arange(-6,6,0.1)
X,Y = np.meshgrid(a,b)
z = himmelblau([X,Y])
fig = plt.figure("himm")
ax = fig.gca(projection='3d')
ax.plot_surface(X,Y,z)
ax.view_init(60,-30)
plt.show()
x = torch.tensor([0.,0.],requires_grad=True)
optimizer = torch.optim.Adam([x],lr=1e-3)
for step in range(20000):
  pred = himmelblau(x)
  optimizer.zero_grad()
  pred.backward()
  optimizer.step()
  if not step%2000:
    print('step{}:x={},f(x)={}'.format(step,x.tolist(),pred.item()))
```

## LR实现多分类
```py
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torchvision import datasets, transforms
batch_size=200
learning_rate=0.01
epochs=10
train_loader = torch.utils.data.DataLoader(datasets.MNIST('../data', train=True, download=True,transform=transforms.Compose([transforms.ToTensor(),transforms.Normalize((0.1307,), (0.3081,)) ])),batch_size=batch_size, shuffle=True)
test_loader = torch.utils.data.DataLoader(datasets.MNIST('../data', train=False, transform=transforms.Compose([transforms.ToTensor(),transforms.Normalize((0.1307,), (0.3081,))])),batch_size=batch_size, shuffle=True)
w1, b1 = torch.randn(200, 784, requires_grad=True), torch.zeros(200, requires_grad=True)
w2, b2 = torch.randn(200, 200, requires_grad=True),torch.zeros(200, requires_grad=True)
w3, b3 = torch.randn(10, 200, requires_grad=True),torch.zeros(10, requires_grad=True)
torch.nn.init.kaiming_normal_(w1)
torch.nn.init.kaiming_normal_(w2)
torch.nn.init.kaiming_normal_(w3)
def forward(x):
    x = x@w1.t() + b1
    x = F.relu(x)
    x = x@w2.t() + b2
    x = F.relu(x)
    x = x@w3.t() + b3
    x = F.relu(x)
    return x
optimizer = optim.SGD([w1, b1, w2, b2, w3, b3], lr=learning_rate)
criteon = nn.CrossEntropyLoss()
for epoch in range(epochs):
    for batch_idx, (data, target) in enumerate(train_loader):
        data = data.view(-1, 28*28)
        logits = forward(data)
        loss = criteon(logits, target)
        optimizer.zero_grad()
        loss.backward()
        # print(w1.grad.norm(), w2.grad.norm())
        optimizer.step()
        if batch_idx % 100 == 0:
            print('Train Epoch: {} [{}/{} ({:.0f}%)]\tLoss: {:.6f}'.format(epoch, batch_idx * len(data), len(train_loader.dataset),100. * batch_idx / len(train_loader), loss.item()))
    test_loss = 0
    correct = 0
    for data, target in test_loader:
        data = data.view(-1, 28 * 28)
        logits = forward(data)
        test_loss += criteon(logits, target).item()
        pred = logits.data.max(1)[1]
        correct += pred.eq(target.data).sum()
    test_loss /= len(test_loader.dataset)
    print('\nTest set: Average loss: {:.4f}, Accuracy: {}/{} ({:.0f}%)\n'.format(test_loss, correct, len(test_loader.dataset),100. * correct / len(test_loader.dataset)))
```

## Flatten
- 自定义展平功能并调用

```py
class Flatten(nn.Module):
    def __init__(self):
        super(Flatten,self).__init__()
    def forward(self,input):
        return input.view(input.size(0),-1)
class TestNet(nn.Module):
    def __init__(self):
        super(TestNet,self).__init__()
        self.net = nn.Sequential(nn.Conv2d(1,16,stride=1,padding=1),
            nn.MaxPool2d(2,2),
            Flatten(),
            nn.Linear(1*14*14,10)
        )
    def forward(self,x):
        return self.net(x)
```

## Linear
- 实现线型层

```py
class MyLinear(nn.Module):
    def __init__(self,inp,outp):
        super(MyLinear,self).__init__()
        self.w = nn.Parameter(torch.randn(outp,inp))
        self.b = nn.Parameter(torch.randn(outp))
    def forward(self,x):
        x = x@self.w.t()+self.b
        return x
```

## MLP
```py
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torchvision import datasets, transforms
batch_size=200
learning_rate=0.01
epochs=10
train_loader = torch.utils.data.DataLoader(datasets.MNIST('../data', train=True, download=True,transform=transforms.Compose([transforms.ToTensor(),transforms.Normalize((0.1307,), (0.3081,))])),batch_size=batch_size, shuffle=True)
test_loader = torch.utils.data.DataLoader(datasets.MNIST('../data', train=False, transform=transforms.Compose([transforms.ToTensor(),transforms.Normalize((0.1307,), (0.3081,))])),batch_size=batch_size, shuffle=True)
class MLP(nn.Module):
    def __init__(self):
        super(MLP, self).__init__()
        self.model = nn.Sequential(
            nn.Linear(784, 200),nn.ReLU(inplace=True),
            nn.Linear(200, 200),nn.ReLU(inplace=True),
            nn.Linear(200, 10),nn.ReLU(inplace=True),
        )
    def forward(self, x):
        x = self.model(x)
        return x
net = MLP()
optimizer = optim.SGD(net.parameters(), lr=learning_rate)
criteon = nn.CrossEntropyLoss()
for epoch in range(epochs):
    for batch_idx, (data, target) in enumerate(train_loader):
        data = data.view(-1, 28*28)
        logits = net(data)
        loss = criteon(logits, target)
        optimizer.zero_grad()
        loss.backward()
        # print(w1.grad.norm(), w2.grad.norm())
        optimizer.step()
        if batch_idx % 100 == 0:
            print('Train Epoch: {} [{}/{} ({:.0f}%)]\tLoss: {:.6f}'.format(epoch, batch_idx * len(data), len(train_loader.dataset),100. * batch_idx / len(train_loader), loss.item()))
    test_loss = 0
    correct = 0
    for data, target in test_loader:
        data = data.view(-1, 28 * 28)
        logits = net(data)
        test_loss += criteon(logits, target).item()
        pred = logits.data.max(1)[1]
        correct += pred.eq(target.data).sum()
    test_loss /= len(test_loader.dataset)
    print('\nTest set: Average loss: {:.4f}, Accuracy: {}/{} ({:.0f}%)\n'.format(test_loss, correct, len(test_loader.dataset),100. * correct / len(test_loader.dataset)))
```

## ResNet
![基本单元](/images/pasted-294.png)

- 基本单元的实现

```py
class ResBlk(nn.Module):
    def __init__(self, ch_in, ch_out):
        super(ResBlk, self).__init__()
        self.conv1 = nn.Conv2d(ch_in, ch_out, kernel_size=3, stride=1, padding=1)
        self.bn1 = nn.BatchNorm2d(ch_out)
        self.conv2 = nn.Conv2d(ch_out, ch_out, kernel_size=3, stride=1, padding=1)
        self.bn2 = nn.BatchNorm2d(ch_out)
        self.extra = nn.Sequential()
        if ch_out != ch_in:
            # [b, ch_in, h, w] => [b, ch_out, h, w]
            self.extra = nn.Sequential(
                nn.Conv2d(ch_in, ch_out, kernel_size=1, stride=1),
                nn.BatchNorm2d(ch_out)
            )
    def forward(self, x):
        out = F.relu(self.bn1(self.conv1(x)))
        out = self.bn2(self.conv2(out))
        out = self.extra(x) + out
        return out
```

- 整体实现

```py
import  torch
from    torch import  nn
from    torch.nn import functional as F
from    torch.utils.data import DataLoader
from    torchvision import datasets
from    torchvision import transforms
from    torch import nn, optim
# from    torchvision.models import resnet18
class ResBlk(nn.Module):
    def __init__(self, ch_in, ch_out):
        super(ResBlk, self).__init__()
        self.conv1 = nn.Conv2d(ch_in, ch_out, kernel_size=3, stride=1, padding=1)
        self.bn1 = nn.BatchNorm2d(ch_out)
        self.conv2 = nn.Conv2d(ch_out, ch_out, kernel_size=3, stride=1, padding=1)
        self.bn2 = nn.BatchNorm2d(ch_out)
        self.extra = nn.Sequential()
        if ch_out != ch_in:
            # [b, ch_in, h, w] => [b, ch_out, h, w]
            self.extra = nn.Sequential(
                nn.Conv2d(ch_in, ch_out, kernel_size=1, stride=1),
                nn.BatchNorm2d(ch_out)
            )
    def forward(self, x):
        out = F.relu(self.bn1(self.conv1(x)))
        out = self.bn2(self.conv2(out))
        # short cut.
        # extra module: [b, ch_in, h, w] => [b, ch_out, h, w]
        # element-wise add:
        out = self.extra(x) + out
        return out
class ResNet18(nn.Module):
    def __init__(self):
        super(ResNet18, self).__init__()
        self.conv1 = nn.Sequential(
            nn.Conv2d(3, 16, kernel_size=3, stride=1, padding=1),
            nn.BatchNorm2d(16)
        )
        # followed 4 blocks
        # [b, 64, h, w] => [b, 128, h ,w]
        self.blk1 = ResBlk(16, 16)
        # [b, 128, h, w] => [b, 256, h, w]
        self.blk2 = ResBlk(16, 32)
        # # [b, 256, h, w] => [b, 512, h, w]
        # self.blk3 = ResBlk(128, 256)
        # # [b, 512, h, w] => [b, 1024, h, w]
        # self.blk4 = ResBlk(256, 512)
        self.outlayer = nn.Linear(32*32*32, 10)
    def forward(self, x):
        x = F.relu(self.conv1(x))
        # [b, 64, h, w] => [b, 1024, h, w]
        x = self.blk1(x)
        x = self.blk2(x)
        # x = self.blk3(x)
        # x = self.blk4(x)
        # print(x.shape)
        x = x.view(x.size(0), -1)
        x = self.outlayer(x)
        return x
def main():
    batchsz = 32
    cifar_train = datasets.CIFAR10('cifar', True, transform=transforms.Compose([
        transforms.Resize((32, 32)),
        transforms.ToTensor()
    ]), download=True)
    cifar_train = DataLoader(cifar_train, batch_size=batchsz, shuffle=True)
    cifar_test = datasets.CIFAR10('cifar', False, transform=transforms.Compose([
        transforms.Resize((32, 32)),
        transforms.ToTensor()
    ]), download=True)
    cifar_test = DataLoader(cifar_test, batch_size=batchsz, shuffle=True)
    x, label = iter(cifar_train).next()
    print('x:', x.shape, 'label:', label.shape)
    device = torch.device('cuda')
    # model = Lenet5().to(device)
    model = ResNet18().to(device)
    criteon = nn.CrossEntropyLoss().to(device)
    optimizer = optim.Adam(model.parameters(), lr=1e-3)
    print(model)
    for epoch in range(1000):
        model.train()
        for batchidx, (x, label) in enumerate(cifar_train):
            # [b, 3, 32, 32]
            # [b]
            x, label = x.to(device), label.to(device)
            logits = model(x)
            # logits: [b, 10]
            # label:  [b]
            # loss: tensor scalar
            loss = criteon(logits, label)
            # backprop
            optimizer.zero_grad()
            loss.backward()
            optimizer.step()
        print(epoch, 'loss:', loss.item())
        model.eval()
        with torch.no_grad():
            # test
            total_correct = 0
            total_num = 0
            for x, label in cifar_test:
                # [b, 3, 32, 32]
                # [b]
                x, label = x.to(device), label.to(device)
                # [b, 10]
                logits = model(x)
                # [b]
                pred = logits.argmax(dim=1)
                # [b] vs [b] => scalar tensor
                correct = torch.eq(pred, label).float().sum().item()
                total_correct += correct
                total_num += x.size(0)
                # print(correct)
            acc = total_correct / total_num
            print(epoch, 'acc:', acc)
if __name__ == '__main__':
    main()
```

## RNN预测sin函数
```py
import  numpy as np
import  torch
import  torch.nn as nn
import  torch.optim as optim
from    matplotlib import pyplot as plt
num_time_steps = 50
input_size = 1
hidden_size = 16
output_size = 1
lr=0.01
class Net(nn.Module):
    def __init__(self, ):
        super(Net, self).__init__()

        self.rnn = nn.RNN(
            input_size=input_size,
            hidden_size=hidden_size,
            num_layers=1,
            batch_first=True,
        )
        for p in self.rnn.parameters():
          nn.init.normal_(p, mean=0.0, std=0.001)
        self.linear = nn.Linear(hidden_size, output_size)
    def forward(self, x, hidden_prev):
       out, hidden_prev = self.rnn(x, hidden_prev)
       # [b, seq, h]
       out = out.view(-1, hidden_size)
       out = self.linear(out)
       out = out.unsqueeze(dim=0)
       return out, hidden_prev
model = Net()
criterion = nn.MSELoss()
optimizer = optim.Adam(model.parameters(), lr)
hidden_prev = torch.zeros(1, 1, hidden_size)
for iter in range(6000):
    start = np.random.randint(3, size=1)[0]
    time_steps = np.linspace(start, start + 10, num_time_steps)
    data = np.sin(time_steps)
    data = data.reshape(num_time_steps, 1)
    x = torch.tensor(data[:-1]).float().view(1, num_time_steps - 1, 1)
    y = torch.tensor(data[1:]).float().view(1, num_time_steps - 1, 1)
    output, hidden_prev = model(x, hidden_prev)
    hidden_prev = hidden_prev.detach()
    loss = criterion(output, y)
    model.zero_grad()
    loss.backward()
    # for p in model.parameters():
    #     print(p.grad.norm())
    # torch.nn.utils.clip_grad_norm_(p, 10)
    optimizer.step()
    if iter % 100 == 0:
        print("Iteration: {} loss {}".format(iter, loss.item()))
start = np.random.randint(3, size=1)[0]
time_steps = np.linspace(start, start + 10, num_time_steps)
data = np.sin(time_steps)
data = data.reshape(num_time_steps, 1)
x = torch.tensor(data[:-1]).float().view(1, num_time_steps - 1, 1)
y = torch.tensor(data[1:]).float().view(1, num_time_steps - 1, 1)
predictions = []
input = x[:, 0, :]
for _ in range(x.shape[1]):
  input = input.view(1, 1, 1)
  (pred, hidden_prev) = model(input, hidden_prev)
  input = pred
  predictions.append(pred.detach().numpy().ravel()[0])
x = x.data.numpy().ravel()
y = y.data.numpy()
plt.scatter(time_steps[:-1], x.ravel(), s=90)
plt.plot(time_steps[:-1], x.ravel())
plt.scatter(time_steps[1:], predictions)
plt.show()
```































