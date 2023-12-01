# Assignment 4: Message Broker

---

## Installing Rabbitmq:

At first we need to update the system by running:

```bash
sudo apt update && upgrade
```

### Installing Erlang:

RabbitMQ requires Erlang. So, we need to install the erlang first.

To do that, we can run the following commands:

```bash
wget -O- https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc | sudo gpg --dearmor -o /usr/share/keyrings/erlang-solutions-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/erlang-solutions-archive-keyring.gpg] https://packages.erlang-solutions.com/ubuntu $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/erlang-solutions.list
sudo apt install erlang
```

![Screenshot from 2023-12-01 20-16-07.png](Assignment%204%20Message%20Broker%20ee7a472e75214232bcbd77eea2db4562/Screenshot_from_2023-12-01_20-16-07.png)

### Installing RabbitMQ:

In this step I will install rabbitmq-server in my machine. To do that, I have to run this command on terminal.

```bash
sudo apt update
wget -O- https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc | sudo gpg --dearmor -o /usr/share/keyrings/rabbitmq-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/rabbitmq-archive-keyring.gpg] https://packagecloud.io/rabbitmq/rabbitmq-server/ubuntu/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/rabbitmq.list
sudo apt install rabbitmq-server
```

![Screenshot from 2023-12-01 20-22-23.png](Assignment%204%20Message%20Broker%20ee7a472e75214232bcbd77eea2db4562/Screenshot_from_2023-12-01_20-22-23.png)

### Enabling rabbitmq:

once installed, by running this command, I can start and enable rabbitmq-server in my machine.

```bash
sudo systemctl start rabbitmq-server
sudo systemctl enable rabbitmq-server
```

### Checking the status:

We can check the status of the rabbitmq-server by running this command.

```bash
sudo systemctl status rabbitmq-server
```

![Screenshot from 2023-12-01 20-44-55.png](Assignment%204%20Message%20Broker%20ee7a472e75214232bcbd77eea2db4562/Screenshot_from_2023-12-01_20-44-55.png)

## Install Pika

Pika requires python and pip. So, If we do not have python and pip installed, we need to install it first. In this case, I am using python3.

So, let’s install python3 and pip3.

```bash
sudo apt install python3 python3-pip
```

Then, we can use the `pip` to install pika.

```bash
pip3 install pika
```

![Screenshot from 2023-12-01 20-49-11.png](Assignment%204%20Message%20Broker%20ee7a472e75214232bcbd77eea2db4562/Screenshot_from_2023-12-01_20-49-11.png)

Let’s verify that pika is installed

```bash
pip3 show pika
```

![Untitled](Assignment%204%20Message%20Broker%20ee7a472e75214232bcbd77eea2db4562/Untitled.png)

---

## **TASK1: Hello World**

### Creating a producer and a consumer script using pika:

Create a file `[producer.py](http://producer.py)` using a text editor. In this case I am using my default text editor neovim.

```bash
nvim producer.py
```

Then write this code in the file:

```python
import pika

# Connect to RabbitMQ server (assuming it's running on localhost)
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Declare a queue named 'hello' where the message will be sent
channel.queue_declare(queue='hello')

# Send a message
message = "Hello, RabbitMQ!"
channel.basic_publish(exchange='', routing_key='hello', body=message)

print(" [x] Sent 'Hello, RabbitMQ!'")

# Close the connection to RabbitMQ
connection.close()
```

save the the file and exit.

![Untitled](Assignment%204%20Message%20Broker%20ee7a472e75214232bcbd77eea2db4562/Untitled%201.png)

Now, let us create another file `consumer.py` using the same method:

```bash
nvim consumer.py
```

```python
import pika

# Connect to RabbitMQ server (assuming it's running on localhost)
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Declare the same queue as in the producer
channel.queue_declare(queue='hello')

# Define a callback function to handle received messages
def callback(ch, method, properties, body):
    print(" [x] Received %r" % body)

# Consume messages from the 'hello' queue with the callback function
channel.basic_consume(queue='hello', on_message_callback=callback, auto_ack=True)

print(' [*] Waiting for messages. To exit, press CTRL+C')
channel.start_consuming()
```

![Untitled](Assignment%204%20Message%20Broker%20ee7a472e75214232bcbd77eea2db4562/Untitled%202.png)

Now let us run the `consumer.py` file

```bash
python3 consumer.py
```

Now, Run the `producer.py` file

```bash
python3 producer.py
```

If everything runs perfectly, the output should seems like this:

![Untitled](Assignment%204%20Message%20Broker%20ee7a472e75214232bcbd77eea2db4562/Untitled%203.png)

---

## TAKS2: Work queues

To complete Task 2, "Work Queues," using RabbitMQ, we will create a task sender script and a worker script that processes tasks with varying complexities.

### **Create the Task Sender Script:**

Lets write a python script `new_task.py` that will send messages simulating tasks with various processing time.

```bash
 nvim new_task.py
```

Now write this code in the file.

```python
import pika
import sys

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='task_queue', durable=True)

message = ' '.join(sys.argv[1:]) or "Hello World!"
channel.basic_publish(
    exchange='',
    routing_key='task_queue',
    body=message,
    properties=pika.BasicProperties(
        delivery_mode=2,  # make message persistent
    ))

print(f" [x] Sent {message}")
connection.close()
```

Save and exit.

![Screenshot from 2023-12-01 21-53-32.png](Assignment%204%20Message%20Broker%20ee7a472e75214232bcbd77eea2db4562/Screenshot_from_2023-12-01_21-53-32.png)

### **Create the Worker Script**

Let us write another python script `worker.py` which will pick up tasks from the queue and simulate the processing of the tasks.

```bash
nvim worker.py
```

Now let us write this scrip in the file.

```python
import pika
import time

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='task_queue', durable=True)

def callback(ch, method, properties, body):
    print(f" [x] Received {body}")
    time.sleep(body.count(b'.'))
    print(" [x] Done")
    ch.basic_ack(delivery_tag=method.delivery_tag)

channel.basic_qos(prefetch_count=1)
channel.basic_consume(queue='task_queue', on_message_callback=callback)

print(' [*] Waiting for messages. To exit press CTRL+C')
channel.start_consuming()
```

Let us save and exit.

![Untitled](Assignment%204%20Message%20Broker%20ee7a472e75214232bcbd77eea2db4562/Untitled%204.png)

### Run the `worker.py` and Observe:

Now, let us run the `worker.py` and the `new_task.py` in a separate terminal.

![Screenshot from 2023-12-01 22-22-29.png](Assignment%204%20Message%20Broker%20ee7a472e75214232bcbd77eea2db4562/Screenshot_from_2023-12-01_22-22-29.png)

### My observation:

To illustrate RabbitMQ's work queue distribution, I set up three instances of `worker.py` in separate terminal windows, ordered from top to bottom based on their launch sequence. This arrangement aimed to showcase how RabbitMQ allocates tasks across different workers.

In a separate terminal acting as the task sender, I ran the `new_task.py` script using various commands to simulate tasks of differing lengths. For instance, commands like python3 `new_task.py` TaskA. and python3 `new_task.py` TaskB.. created tasks labeled from TaskA to TaskF, with increasing numbers of dots indicating longer processing times.

By monitoring the worker terminals, I observed the task distribution. The first worker (top terminal) handled '`TaskA.`' and '`TaskD..`' and “`TaskE..`”, the second worker (bottom terminal) processed '`TaskB..`' and '`TaskC..`' and `TaskF..`. This demonstrated RabbitMQ's ability to evenly distribute tasks among available workers, showcasing its load-balancing capabilities.

---