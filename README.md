[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/aQEb8Lmc)
# COMP4651 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Saturday

---

### Name:  CHAN Ka Tik
### Student Id:  20955811
### Email:  ktchanay@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > Your answer goes here.
    
    Measurement Tool: sysbench 1.0.20
     
    configuration:

    CPU Test: ```sysbench cpu --cpu-max-prime=20000 --threads=N --time=30 run```

    Memory Test: ```sysbench memory --memory-block-size=1K --memory-total-size=NG --memory-access-mode=seq run```

    parameter explaination: --cpu-max-prime=20000: Calculates prime numbers up to 20,000, large enough to stress CPU; --threads=1 for t2.micro, 2 for t2.medium/c5d.large: Matches instance vCPU count; --time=30 seconds: Provides statistically reliable results; --memory-block-size=1K: Tests small memory allocations;--memory-total-size=2G (t2.micro), 4G (t2.medium/c5d.large): Scales with instance RAM to avoid swapping while maximizing memory pressure;--memory-access-mode=seq:Provides consistent baseline measurement of memory bandwidth.
   
    Result explaination: CPU metric: "events per second": Number of prime number calculations completed per second; Memory metric: "MiB/sec": Amount of data transferred per second during memory operations.
   
3. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` |    879.41 events/sec        |     521.86 MiB/sec       |
    | `t2.medium`  |       1652.36 events/sec         |       527.04 MiB/sec     |
    | `c5d.large` |       697.78 events/sec          |      5663.41 MiB/sec              |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |       4,600         |     0.246     |
    | `m5.large` - `m5.large`   |       4,960         |     0.273     |
    | `c5n.large` - `c5n.large` |      4,960          |    0.296      |
    | `t3.medium` - `c5n.large` |   4,880        |   0.471  |
    | `m5.large` - `c5n.large`  |     4,960      |   0.541  |
    | `m5.large` - `t3.medium`  |    4,900       |   0.980  |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |      494          |   55.014       |
    | N. Virginia - N. Virginia |     43,500           |   0.020       |
    | Oregon - Oregon           |      44,300          |     0.020     |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
