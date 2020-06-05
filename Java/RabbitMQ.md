## RabbitMQ
- **消息中间件**  
`MQ`全称`Message Queue`，消息队列是进程/线程之间的通信方法。  
- **应用场景**  
任务异步处理；应用程序解耦合；削峰填谷  
![](./Pics/MQ应用场景_1.png)  
![](./Pics/MQ应用场景_2.png)  
![](./Pics/MQ应用场景_3.png)  
- **工作模式**  
1. `Simple`简单模式：一个生产者将消息发送到队列，一个消费者监听队列并消费消息。如：聊天  
![](./Pics/MQ_Simple.png)  
```
public class ConnectionUtil {

    public static Connection getConnection() throws Exception {
        // 定义连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 设置服务地址
        factory.setHost("localhost");
        // 设置端口
        factory.setPort(5672);
        // 设置账号信息
        factory.setVirtualHost("mqhost");
        factory.setUsername("pkz33");
        factory.setPassword("pkz33");
        // 通过工厂获取连接
        Connection connection = factory.newConnection();
        return connection;
    }
}

public class Sender {

    private final static String QUEUE_NAME = "mq_simple";

    public static void main(String[] args) throws Exception {
        // 获取连接
        Connection connection = ConnectionUtil.getConnection();
        // 创建通道
        Channel channel = connection.createChannel();

        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);

        // 消息内容
        String message = "MQ simple";
        channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
        System.out.println("Producer sent: " + message);
        //关闭通道和连接
        channel.close();
        connection.close();
    }
}

public class Receiver {

    private final static String QUEUE_NAME = "mq_simple";

    public static void main(String[] args) throws Exception {

        // 获取连接
        Connection connection = ConnectionUtil.getConnection();
        // 创建通道
        Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);

        // 定义消费者
        QueueingConsumer consumer = new QueueingConsumer(channel);

        // 监听队列
        channel.basicConsume(QUEUE_NAME, true, consumer);

        // 获取消息
        while (true) {
            QueueingConsumer.Delivery delivery = consumer.nextDelivery();
            String message = new String(delivery.getBody());
            System.out.println("Consumer received: " + message);
        }
    }
}
```
2. `Work`工作模式：一个生产者将消息发送到队列，多个消费者同时监听队列并消费消息，一条消息只能被一个消费者消费。消息从队列异步推送给消费者，消费者的`ack`确认也异步发送给队列，`Qos`的`prefetchCount`参数用来限制已推送但尚未获得`ack`确认的消息的数量。默认为`0`，为轮询分发模式，队列会将所有消息尽快发送给消费者；设置为`1`时，为公平分发模式，队列只有在收到消费者发回的上一条消息的`ack`确认后，才会向该消费者发送下一条消息。如：红包  
![](./Pics/MQ_Work.png)  
```
public class Sender {

    private final static String QUEUE_NAME = "mq_work";

    public static void main(String[] args) throws Exception {
        // 获取连接
        Connection connection = ConnectionUtil.getConnection();
        Channel channel = connection.createChannel();

        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);

        for (int i = 0; i < 100; i++) {
            // 消息内容
            String message = "MQ work" + i;
            channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
            System.out.println("Producer sent: " + message);

            Thread.sleep(i * 10);
        }

        channel.close();
        connection.close();
    }
}

public class Receiver1 {

    private final static String QUEUE_NAME = "mq_work";

    public static void main(String[] args) throws Exception {

        // 获取连接
        Connection connection = ConnectionUtil.getConnection();
        Channel channel = connection.createChannel();

        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);

        // 同一时刻只会发一条消息给消费者
        //channel.basicQos(1);

        // 定义队列的消费者
        QueueingConsumer consumer = new QueueingConsumer(channel);
        // 监听队列，false表示手动返回完成状态，true表示自动
        channel.basicConsume(QUEUE_NAME, true, consumer);
        // channel.basicConsume(QUEUE_NAME, false, consumer);

        // 获取消息
        while (true) {
            QueueingConsumer.Delivery delivery = consumer.nextDelivery();
            String message = new String(delivery.getBody());
            System.out.println("Consumer1 Received: " + message);

            Thread.sleep(10);
            // 手动返回确认状态
            // channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
        }
    }
}

public class Receiver2 {

    private final static String QUEUE_NAME = "mq_work";

    public static void main(String[] args) throws Exception {

        // 获取连接
        Connection connection = ConnectionUtil.getConnection();
        Channel channel = connection.createChannel();

        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);

        // 同一时刻只会发一条消息给消费者
        //channel.basicQos(1);

        // 定义队列的消费者
        QueueingConsumer consumer = new QueueingConsumer(channel);
        // 监听队列，false表示手动返回完成状态，true表示自动
        channel.basicConsume(QUEUE_NAME, true, consumer);
        // channel.basicConsume(QUEUE_NAME, false, consumer);
        // 获取消息
        while (true) {
            QueueingConsumer.Delivery delivery = consumer.nextDelivery();
            String message = new String(delivery.getBody());
            System.out.println("Consumer2 Received: " + message);

            Thread.sleep(10);
            // 手动返回确认状态
            // channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
        }
    }
}
```
3. `Publish/Subscribe`发布订阅模式：生产者将消息发送给交换机，交换机将消息复制同步到所有绑定的队列，消费者监听各自的队列并消费消息。如：邮件群发、群聊、广播  
![](./Pics/MQ_Publish_Subscribe.png)  
```
public class Sender {

    private final static String EXCHANGE_NAME = "mq_exchange";

    public static void main(String[] args) throws Exception {
        // 获取连接
        Connection connection = ConnectionUtil.getConnection();
        // 创建通道
        Channel channel = connection.createChannel();

        // 声明exchange
        channel.exchangeDeclare(EXCHANGE_NAME, "fanout");

        // 消息内容
        String message = "MQ publish subscribe";
        channel.basicPublish(EXCHANGE_NAME, "", null, message.getBytes());
        System.out.println("Producer sent: " + message);

        channel.close();
        connection.close();
    }
}

public class Receiver1 {

    private final static String QUEUE_NAME = "mq_publish_subscribe1";

    private final static String EXCHANGE_NAME = "mq_exchange";

    public static void main(String[] argv) throws Exception {

        // 获取连接
        Connection connection = ConnectionUtil.getConnection();
        // 创建通道
        Channel channel = connection.createChannel();

        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);

        // 绑定队列到交换机
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "");

        // 同一时刻只会发一条消息给消费者
        channel.basicQos(1);

        // 定义队列的消费者
        QueueingConsumer consumer = new QueueingConsumer(channel);
        // 监听队列，手动返回完成
        channel.basicConsume(QUEUE_NAME, false, consumer);

        // 获取消息
        while (true) {
            QueueingConsumer.Delivery delivery = consumer.nextDelivery();
            String message = new String(delivery.getBody());
            System.out.println("Consumer1 received: " + message);
            Thread.sleep(10);

            channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
        }
    }
}

public class Receiver2 {

    private final static String QUEUE_NAME = "mq_publish_subscribe2";

    private final static String EXCHANGE_NAME = "mq_exchange";

    public static void main(String[] argv) throws Exception {

        // 获取连接
        Connection connection = ConnectionUtil.getConnection();
        // 创建通道
        Channel channel = connection.createChannel();

        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);

        // 绑定队列到交换机
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "");

        // 同一时刻只会发一条消息给消费者
        channel.basicQos(1);

        // 定义队列的消费者
        QueueingConsumer consumer = new QueueingConsumer(channel);
        // 监听队列，手动返回完成
        channel.basicConsume(QUEUE_NAME, false, consumer);

        // 获取消息
        while (true) {
            QueueingConsumer.Delivery delivery = consumer.nextDelivery();
            String message = new String(delivery.getBody());
            System.out.println("Consumer2 received: " + message);
            Thread.sleep(10);

            channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
        }
    }
}
```
4. `Routing`路由模式  
![](./Pics/MQ_Routing.png)  
5. `Topics`主题模式  
![](./Pics/MQ_Topics.png)  
