#### <div id="queue"> 队列</div> 

在使用queue队列的时候，发现会传入一个delay参数，表示延时执行。那么就要思考一个问题：a先入队列，delay比较长，b后入队列，delay较短。那么会发生什么情况呢？

按照队列先进先出的原则，即使b的延时执行时间较长，也应该等到它之前的先出队列，这样的话，会造成队列阻塞。这显然不是我们想要的。

为了验证以上情况，特意测试了一下。
下面是一个邮件发送的请求。

    CQueue::push(EmailSendJob::class, [
                'sender'  => ['发送方@email.com' => '数据报表'],
                'email'   => '接收方@email.com',
                'subject' => ' 邮件主题',
                'content' => "发送时间" . $rand,
            ], ['delay' => $rand]);

第一次执行时，$rand = 5 , 第二次执行 $rand = 3; 看一下谁会先发送。

结果 ： 先收到第二次执行的邮件。
结论 ：delay时间短的先执行。我们可能会疑惑，这明显不符合队列的原则。

接下来，我们去探索一下为什么会发生这种情况，底层到底是怎么去实现的。
分析：

首先看一下配置文件中的

```
  $baseConfig  = [
    'class'         => '\yii\queue\redis\Queue',
    // 'on afterPush'  => ['\common\queue\ExecEventHandler', 'afterPush'],
    // 'on beforeExec' => ['\common\queue\ExecEventHandler', 'beforeExec'],
    // 'on afterExec'  => ['\common\queue\ExecEventHandler', 'afterExec'],
    'on afterError' => ['\common\queue\ExecEventHandler', 'afterError'],
    'redis'         => [
        'class'    => 'yii\redis\Connection',   //驱动程序使用Redis存储队列数据，也可能会使用MemCache或其他的来存储。
        'hostname' => 'redis_Ip',       //redis的ip地址
        //有密码的话也要加进去
        'port'     => 6379,   //端口号
        'database' => 0,   //使用哪个数据库
    ],
];
```

我们现在知道了，他是使用Redis来存储队列数据，那么我们在执行时看一下redis会产生什么数据。
首先，我们选择一个新库，这里我选择db1。里面刚开始是没有数据的。
然后上面代码的database 改为1 ，delay设置为120（秒），运行。。。。。。
> console 下脚本运行的命令  php yii 控制器名/方法名

![屏幕快照 2019-07-06 上午10.36.01.png](https://upload-images.jianshu.io/upload_images/7893160-224b0a200bd3fa1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以看到，redis生成了三个数据， 分别是log_queue.delayed，log_queue.message_id，log_queue.messages。
我们看一下他们的数据类型和内容。

![delayed.png](https://upload-images.jianshu.io/upload_images/7893160-21f042ff08f33708.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![message_id_.png](https://upload-images.jianshu.io/upload_images/7893160-1f8ba37a3e505038.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![messages.png](https://upload-images.jianshu.io/upload_images/7893160-45269b4552420a6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先，他们是zset（集合的形式存储的）。
第一张表：存储的是delay的数据，score 表示其过期时间，那value 列是什么数据呢？
因为redis集合具有唯一性，但是有可能delay的值是相同的，所以加了一个字段。并且数据还可以排序。

第二张表：存储的是最后一次成功后返回的value值。

第三张表：存储数据，所有内容数据都存储在value字段中。


分析完毕。