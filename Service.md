# Services/Clients

1. 与publishers/subscribers的通信机制相比，services/clients通信机制是一种双向、一对一的可靠通信机制。
   当client发送一个“请求”（request）到service处，service会反馈一个“响应”（response）给client，这时的client就需要知道service的名称才能完成这种问答形式的通信机制。

2. 要注意一点，service的响应要尽可能的快，因为client在没有得到反馈之前是处于停止状态的，直到它收到反馈信息才开始重新运作。所以如果一个service需要较长的时间去处理大量的数据才能得出结果，这时应该选择**action-server/action-client**这种通信机制。



# 创建Service messages
与publishers/subscribers通信一样的，需要定义两者通信的信息类型，创建的步骤如下，与publishers/subscribers通信机制中定义messages类型很相似，只需要改动一些关键词：
1. 创建一个新的package（里面存放节点代码及service messages文件等等）
``catkin_create_pkg example_service roscpp std_msgs``
2. 在包中创建一个叫service的文件夹

```
mkdir service
cd ./service
touch example.srv
```

3. 打开.srv文件，写入以下信息：

```
float64 aaa
---
bool on_the_list
bool good_guy
int32 age
string nickname
```

以上内容第一列为数据类型，第二类是定义的域名
注意 “- - -” ，在它的上方是“请求”信息的结构，在它的下方是“响应”信息的机构，如下图所示，注意不要弄错了。

4. 之后就要编辑package.xml，添加一下语句

<build_depend>message_generation</build_depend>
<run_depend>message_runtime</run_depend>

5. 编辑CMakeLists.txt，按顺序添加以下代码

find_package(catkin REQUIRED COMPONENTS roscpp rospy message_generation std_msgs std_srvs)

**注意**
这里的add_service_files与自定义messages类型有一点不同，在自定义messages类型中是用add_message_files指定.msg文件，而自定义service messages是用add_service_files指定.srv文件
```
 add_service_files(
   FILES
   Service/example.srv
 )

 generate_messages(
   DEPENDENCIES
   std_msgs
 )

catkin_package(

CATKIN_DEPENDS message_runtime

)
```

6. 先编译生成service messages的头文件
``catkin_make``或``catkin_build``
对应的头文件生成在

``～/catkin_ws/devel/include/Service``

7. 注意
头文件的名称与.srv 文件的名称相同，与**publishers/subscribers**通信中的定义**messages类**型相比，在include文件夹里除了生成serviceMsg.h这个头文件外，还生成了另外两个头文件，分别是serviceMsgRequest.h和serviceMsgResponse.h，这两个头文件其实都包含在serviceMsg.h中，所以之后调用的时候只需要添加serviceMsg.h这个头文件即可，如：


*至此自定义service messages类型的操作就结束了，下面开始写一个简单的service node和client node*

若是在其他包中调用该消息类型，还需要在package.xml中添加如下语句

```
<build_depend>example_service</build_depend>
```

# 编辑Service node
完成了消息类型的建立，之后就可以开始写services/clients的节点程序了，先从services节点开始，进入到“src”文件夹中创建文件

*怎么写节点见代码*

# 编辑Client node
最后是clients节点，发出“request”和接收“response”的对象，在src文件夹中创建文件

*怎么写节点见代码*

# CmakeLists.txt
然后在CMakeLists.txt中添加如下语句：

```
add_executable(example_client src/example_client.cpp)
target_link_libraries(example_client ${catkin_LIBRARIES})
```

最后编译catkin_make 完成后，分别在两个terminal中运行service和client

