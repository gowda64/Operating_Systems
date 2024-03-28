>Have used proper variable names for easier understanding, feel free to user shorter variables for convenience.
# Program 1
FCFS
```java
    import java.util.*;

    public class FCFS {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        // First of all, take the number of processes
        System.out.println("Enter the number of processes:");
        int process_no = sc.nextInt();
        // Declare all the computational variables
        int process_id[] = new int[process_no];
        int arrival[] = new int[process_no];
        int burst[] = new int[process_no];
        int completion[] = new int[process_no];
        int turnaround[] = new int[process_no];
        int waiting[] = new int[process_no];
        float avg_turnaround = 0;
        float avg_waiting = 0;

        for (int i = 0; i < process_no; i++) {
            System.out.println("Enter the arrival and burst time for process number" + (i + 1));
            arrival[i] = sc.nextInt();
            burst[i] = sc.nextInt();
            process_id[i] = i + 1; // set the process ids in the array
        }
        int temp;
        // time to sort by arrival times since its FCFS
        for (int i = 0; i < process_no; i++) {
            for (int j = 0; j < process_no - i - 1; j++) {
                if (arrival[i] > arrival[i + 1]) {
                    temp = arrival[i];
                    arrival[i] = arrival[i + 1];
                    arrival[i + 1] = temp;
                    temp = burst[i];
                    burst[i] = burst[i + 1];
                    burst[i + 1] = temp;
                    temp = process_id[i];
                    process_id[i] = process_id[i + 1];
                    process_id[i + 1] = temp;
                }
            }
        }

        // find completion times
        for (int i = 0; i < process_no; i++) {
            if (i == 0)
                completion[i] = arrival[i] + burst[i];
            else {
                if (arrival[i] > completion[i - 1])
                    completion[i] = arrival[i] + burst[i];
                else
                    completion[i] = completion[i - 1] + burst[i];
            }
            turnaround[i] = completion[i] - waiting[i];
            waiting[i] = turnaround[i] - burst[i];
            avg_turnaround += turnaround[i];
            avg_waiting += waiting[i];
        }

        // print everything
        System.out.println("PROCESS\tARRIVAL\tBURST\tCOMPLETION\tWAITING\tTURNAROUND");
        for (int i = 0; i < process_no; i++) {
            System.out.println(process_id[i] + "\t" + arrival[i] + "\t" + burst[i] + "\t" + completion[i] + "\t"
                    + waiting[i] + "\t" + turnaround[i]);
        }
        System.out.println("Average Waiting time = " + (avg_waiting / process_no));
        System.out.println("Average Waiting time = " + (avg_turnaround / process_no));
    }
}
```
Output
```
Enter the number of processes:
3
Enter the arrival and burst time for process number1
0
24
Enter the arrival and burst time for process number2
0
3
Enter the arrival and burst time for process number3
0
4
PROCESS ARRIVAL BURST   COMPLETION      WAITING TURNAROUND
1       0       24      24      0       24
2       0       3       27      24      27
3       0       4       31      27      31
Average Waiting time = 17.0
Average Waiting time = 27.333334
```
# Program 1 
SJF
```java
import java.util.*;

public class SJFS {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        // First of all, take the number of processes
        System.out.println("Enter the number of processes:");
        int process_no = sc.nextInt();
        // Declare all the computational variables
        int process_id[] = new int[process_no + 1];
        int burst[] = new int[process_no + 1];
        int turnaround[] = new int[process_no + 1];
        int waiting[] = new int[process_no + 1];
        float avg_turnaround = 0;
        float avg_waiting = 0;

        for (int i = 0; i < process_no; i++) {
            System.out.println("Enter the burst time for process number " + (i + 1));
            burst[i] = sc.nextInt();
            process_id[i] = i + 1; // set the process ids in the array
        }
        int temp;
        // time to sort by burst times since its SJF
        for (int i = 0; i < process_no; i++) {
            for (int j = 0; j < process_no - i - 1; j++) {
                if (burst[j] > burst[j + 1]) {
                    temp = burst[j];
                    burst[j] = burst[j + 1];
                    burst[j + 1] = temp;
                    temp = process_id[j];
                    process_id[j] = process_id[j + 1];
                    process_id[j + 1] = temp;
                }
            }
        }

        // time for calculation
        for (int i = 0; i < process_no; i++) {
            waiting[i] = 0;
            turnaround[i] = 0;
        }
        for (int i = 0; i < process_no; i++) {
            turnaround[i] = burst[i] + waiting[i];
            waiting[i + 1] = waiting[i] + burst[i];
        }
        turnaround[process_no] = burst[process_no] + waiting[process_no];

        for (int j = 0; j < process_no; j++) {
            turnaround[j] = burst[j] + waiting[j];
            avg_turnaround += turnaround[j];
            avg_waiting += waiting[j];
        }

        // print everything
        System.out.println("PROCESS\tBURST\tWAITING\tTURNAROUND");
        for (int i = 0; i < process_no; i++) {
            System.out.println(process_id[i] + "\t" + burst[i] + "\t"
                    + waiting[i] + "\t" + turnaround[i]);
        }
        System.out.println("Average Waiting time = " + (avg_waiting / process_no));
        System.out.println("Average Turnaround time = " + (avg_turnaround / process_no));

    }
}
```
Output
```
Enter the number of processes:
3
Enter the burst time for process number 1
8
Enter the burst time for process number 2
7
Enter the burst time for process number 3
4
PROCESS BURST   WAITING TURNAROUND
3       4       0       4
2       7       4       11
1       8       11      19
Average Waiting time = 5.0
Average Turnaround time = 11.333333
```
# Program 2
Producer - Consumer Problem
```java
import java.util.concurrent.Semaphore;

class Queue {
    int item;
    static Semaphore semCon = new Semaphore(0);
    static Semaphore semProd = new Semaphore(1);

    void get() {
        try {
            semCon.acquire();
        } catch (InterruptedException e) {
            System.out.println("ERROR");
        }
        System.out.println("Consumer consumer item " + item);
        semProd.release();
    }

    void put(int item) {
        try {
            semProd.acquire();
        } catch (InterruptedException e) {
            System.out.println("Error");
        }
        System.out.println("Producer produced item " + item);
        semCon.release();
    }
}

class Producer implements Runnable {
    Queue q;

    Producer(Queue q) {
        this.q = q;
        new Thread(this, "Producer").start();
    }

    public void run() {
        for (int i = 0; i < 5; i++)
            q.put(i);
    }
}

class Consumer implements Runnable {
    Queue q;

    Consumer(Queue q) {
        this.q = q;
        new Thread(this, "Consumer").start();
    }

    public void run() {
        for (int i = 0; i < 5; i++)
            q.get();
    }
}

public class Producer_Consumer {
    public static void main(String args[]) {
        Queue q = new Queue();
        new Consumer(q);
        new Producer(q);
    }
}
```
Output
```
Producer produced item 0
Consumer consumer item 0
Producer produced item 1
Consumer consumer item 0
Producer produced item 2
Consumer consumer item 0
Producer produced item 3
Consumer consumer item 0
Producer produced item 4
Consumer consumer item 0
```
# Program 3
Dining Philosophers Problem
```java
import java.util.concurrent.Semaphore;

class Philosopher implements Runnable {
    private int phil;
    private Semaphore room;
    private Semaphore chopstick[];

    public Philosopher(int phil, Semaphore room, Semaphore[] chopstick) {
        this.phil = phil;
        this.room = room;
        this.chopstick = chopstick;
    }

    private void eat() {
        System.out.println("Philosopher " + phil + " is eating!");
    }

    @Override
    public void run() {
        try {
            room.acquire();
            System.out.println("Philosopher " + phil + " has entered");
            chopstick[phil].acquire();
            chopstick[(phil + 1) % 5].acquire();
            eat();
            Thread.sleep(2000);
            System.out.println("Philosopher " + phil + " has finished");
            chopstick[(phil + 1) % 5].release();
            chopstick[phil].release();
            room.release();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class Dining_Philosopher {
    public static void main(String[] args) {
        Semaphore room = new Semaphore(4);
        Semaphore[] chopstick = new Semaphore[5];
        for (int i = 0; i < 5; i++)
            chopstick[i] = new Semaphore(1);
        Thread[] threads = new Thread[5];
        for (int i = 0; i < 5; i++) {
            threads[i] = new Thread(new Philosopher(i, room, chopstick));
            threads[i].start();
        }
        for (int i = 0; i < 5; i++) {
            try {
                threads[i].join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```
Output
```
Philosopher 3 has entered
Philosopher 1 has entered
Philosopher 4 has entered
Philosopher 0 has entered
Philosopher 3 is eating!
Philosopher 1 is eating!
Philosopher 1 has finished
Philosopher 3 has finished
Philosopher 0 is eating!
Philosopher 2 has entered
Philosopher 2 is eating!
Philosopher 0 has finished
Philosopher 2 has finished
Philosopher 4 is eating!
Philosopher 4 has finished
```

