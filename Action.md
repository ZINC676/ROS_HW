# action-server/action-client
下面是最后一种通信机制action-server/action-client，尽管与service/client通信机制很像，它与service/client通信机制还是有很关键的不同点的：

那就是多对多的action-server与action-client的交流机制

## 区别
service/client通信机制中service和client通信是单对单的，当client发送一个“请求”后，只有当它接收到service的“响应”信息才能执行其他操作，可以理解为这是一种单通道的封闭式通信，而
action-server与action-client通信就不受这种限制，action-client对server发出一个“请求”后，还可以执行其他操作。

## 相似点
与service/client通信相似的是：

action-client需要知道它出发的“请求”的接收者的名称（serviceName）
action-server也需要知道“响应”信息的接收对象是“谁”
当然，action-servers和action-clients的框架里面还有很多功能和变量，可以在**http//wiki.ros.org/actionlib**中查看，这里只列出一个简单的应用。

# 创建一个action-server的包
1. 依然使用catkin_simple功能来创建package

``catkin_create_pkg example_action_server roscpp actionlib``

2. 然后在新建的package中创建一个**action**package用来存储action messages的信息，方法与创建service messages一样。

```
cd example_action_server
mkdir action
```

3. 惯例，在package.xml文件中添加两行生成消息文件的语句：

```
<build_depend>message_generation</build_depend>
<run_depend>message_runtime</run_depend>
```

## 定义action-server消息
与之前定义msg消息和service消息很相似，都需要建立一个对应格式的消息文件

*publishers/subscribers的消息文件格式是\*.msg*
*service/client的消息文件格式是\*.srv*
*action-server/action-client的消息文件格式是\*.action*

其中，service messages比起简单的messages要复杂一些，而action的消息跟service的消息很类似，也有分多个区域，总体上要稍微复杂一点
service消息有两个消息区域

```
request
---
response
```

action消息有三个消息区域

```
goal
---
result
---
feedback
```

## 实例
以下是本例要创建的一个action消息
1. 先进入到action的文件夹，创建一个名称为example.action的文件：

```
cd action
touch demo.action
```

2. 然后写入如下内容：

```
#goal definition

---
#result definition

---
#feedback

```

三个消息区域通过三个连字符分隔开，最上面的是goad，中间的是result，最下面的feedback，“#”开头的注释语句不是必须的，可以省略。
**注意**:顺序不可以打乱，严格按照以上顺序排列

3. 跟着就可以进行编译生成action消息类型的头文件了
``catkin_make``

生成的头文件在

``～/ros_ws/devel/include/packageName``

这里的packageName是example_action_server，里面有7个头文件

其中，以demoAction.h为主，其他六个头文件都包含在该头文件中，要调用该包中的action消息类型，只需要知道demoAction.h这个头文件的名称即可。


