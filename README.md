## Welcome GeekVan GitHub Pages![3_lychallenger](img/3_lychallenger.jpg)



### Background

```markdown
2014.10-2018.07 东北大学
2018.09-2021.07 东北大学

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### **基于 Zookeeper 实现分布式锁**

```java
public class CreateGroup implements Watcher {
private static final int SESSION_TIMEOUT = 5000;
    private ZooKeeper zk;
    private CountDownLatch connectedSignal = new CountDownLatch(1);
    public void connect(String hosts) throws IOException, InterruptedException {
    zk = new ZooKeeper(hosts, SESSION_TIMEOUT, this);
    connectedSignal.await();
}
@Override
public void process(WatchedEvent event) { // Watcher interface
    if (event.getState() == KeeperState.SyncConnected) {
    connectedSignal.countDown();
    }
}
public void create(String groupName) throws KeeperException,
InterruptedException {
    String path = "/" + groupName;
    String createdPath = zk.create(path, null/*data*/, Ids.OPEN_ACL_UNSAFE,
    CreateMode.PERSISTENT);
    System.out.println("Created " + createdPath);
    }
    public void close() throws InterruptedException {
    zk.close();
}
public static void main(String[] args) throws Exception {
    CreateGroup createGroup = new CreateGroup();
    createGroup.connect(args[0]);
    createGroup.create(args[1]);
    createGroup.close();
	}
}
```

当 main() 执行时，首先创建了一个 CreateGroup 的对象，然后调用 connect() 方法，通过zookeeper的API与zookeeper服务器连接。创建连接我们需要3个参数：一是服务器端主机名称以及端口号，二是客户端连接服务器session的超时时间，三是Watcher接口的一个实例。 Watcher实例负责接收Zookeeper数据变化时产生的事件回调。

在连接函数中创建了zookeeper的实例，然后建立与服务器的连接。建立连接函数会立即返回，所以我们需要等待连接建立成功后再进行其他的操作。我们使用CountDownLatch来阻塞当前线程，直到zookeeper准备就绪。这时，我们就看到Watcher的作用了。我们实现了Watcher接口的一个方法：

public void process(WatchedEvent event);

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Lychallenger/Lychallenger.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
