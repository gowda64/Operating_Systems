# Program 6
Bankers Algorithm
```java
import java.util.*;

public class Bankers_Algorithm {
    private int need[][], allocate[][], max[][], avail[][], np, nr;

    private void input() {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter no. of processes and resources : ");
        np = sc.nextInt(); // no. of process
        nr = sc.nextInt(); // no. of resources
        need = new int[np][nr]; // initializing arrays
        max = new int[np][nr];
        allocate = new int[np][nr];
        avail = new int[1][nr];

        System.out.println("Enter allocation matrix -->");
        for (int i = 0; i < np; i++)
            for (int j = 0; j < nr; j++)
                allocate[i][j] = sc.nextInt(); // allocation matrix

        System.out.println("Enter max matrix -->");
        for (int i = 0; i < np; i++)
            for (int j = 0; j < nr; j++)
                max[i][j] = sc.nextInt(); // max matrix

        System.out.println("Enter available matrix -->");
        for (int j = 0; j < nr; j++)
            avail[0][j] = sc.nextInt(); // available matrix

        sc.close();
    }

    private int[][] calc_need() {
        for (int i = 0; i < np; i++)
            for (int j = 0; j < nr; j++) // calculating need matrix
                need[i][j] = max[i][j] - allocate[i][j];

        return need;
    }

    private boolean check(int i) {
        // checking if all resources for ith process can be allocated
        for (int j = 0; j < nr; j++)
            if (avail[0][j] < need[i][j])
                return false;
        return true;
    }

    public void isSafe() {
        input();
        calc_need();
        boolean done[] = new boolean[np];
        int j = 0;

        while (j < np) { // until all process allocated
            boolean allocated = false;
            for (int i = 0; i < np; i++)
                if (!done[i] && check(i)) { // trying to allocate
                    for (int k = 0; k < nr; k++)
                        avail[0][k] = avail[0][k] - need[i][k] + max[i][k];
                    System.out.println("Allocated process : " + i);
                    allocated = done[i] = true;
                    j++;
                }
            if (!allocated)
                break; // if no allocation
        }
        if (j == np) // if all processes are allocated
            System.out.println("\nSafely allocated");
        else
            System.out.println("All proceess cant be allocated safely");
    }

    public static void main(String[] args) {
        new Bankers_Algorithm().isSafe();
    }
}
```
Output
```
Enter no. of processes and resources : 5
3
Enter allocation matrix -->
0 1 0
2 0 0
3 0 2
2 1 1
0 0 2
Enter max matrix -->
7 5 3
3 2 2 
9 0 2
2 2 2
4 3 3
Enter available matrix -->
3 3 2
Allocated process : 1
Allocated process : 3
Allocated process : 4
Allocated process : 0
Allocated process : 2

Safely allocated
```
# Program 7
Safety Algorithm
```java
public class Safety_Algorithm {
    public static void main(String[] args) {
        int n, m, i, j, k;
        n = 5; // Number of processes
        m = 3; // Number of resources
        int[][] alloc = { { 0, 1, 0 }, // P0 // Allocation Matrix
                { 2, 0, 0 }, // P1
                { 3, 0, 2 }, // P2
                { 2, 1, 1 }, // P3
                { 0, 0, 2 } };

        int[][] max = { { 7, 5, 3 }, // P0 // MAX Matrix
                { 3, 2, 2 }, // P1
                { 9, 0, 2 }, // P2
                { 2, 2, 2 }, // P3
                { 4, 3, 3 } };

        int[] avail = { 3, 3, 2 }; // Available Resources

        int[] f = new int[n];
        int[] ans = new int[n];
        int ind = 0;
        for (k = 0; k < n; k++) {
            f[k] = 0;
        }
        int[][] need = new int[n][m];
        for (i = 0; i < n; i++) {
            for (j = 0; j < m; j++)
                need[i][j] = max[i][j] - alloc[i][j];
        }
        int y = 0;
        for (k = 0; k < 5; k++) {
            for (i = 0; i < n; i++) {
                if (f[i] == 0) {

                    int flag = 0;
                    for (j = 0; j < m; j++) {
                        if (need[i][j] > avail[j]) {
                            flag = 1;
                            break;
                        }
                    }

                    if (flag == 0) {
                        ans[ind++] = i;
                        for (y = 0; y < m; y++)
                            avail[y] += alloc[i][y];
                        f[i] = 1;
                    }
                }
            }
        }

        int flag = 1;

        // To check if sequence is safe or not
        for (i = 0; i < n; i++) {
            if (f[i] == 0) {
                flag = 0;
                System.out.println("The given sequence is not safe");
                break;
            }
        }

        if (flag == 1) {
            System.out.println("Following is the SAFE Sequence");
            for (i = 0; i < n - 1; i++)
                System.out.print(" P" + ans[i] + " ->");
            System.out.print(" P" + ans[n - 1]);
        }
    }
}
```
Output
```
Following is the SAFE Sequence
 P1 -> P3 -> P4 -> P0 -> P2
```
# Program 8
Page Replacement Algorithms
```java
import java.util.*;

class PageReplacement {
    static final int FRAME_SIZE = 3;

    static void FIFO(int[] pages) {
        int[] frames = new int[FRAME_SIZE];
        for (int i = 0; i < FRAME_SIZE; i++)
            frames[i] = 999;
        int nextFrameIndex = 0;
        int pageFaults = 0;

        for (int page : pages) {
            boolean pageExists = false;
            for (int frame : frames) {
                if (frame == page) {
                    pageExists = true;
                    break;
                }
            }

            if (!pageExists) {
                if (nextFrameIndex == FRAME_SIZE) {
                    nextFrameIndex = 0;
                }
                frames[nextFrameIndex++] = page;
                pageFaults++;
            }
            for (int frame : frames)
                System.out.print(frame + "\t");
            System.out.println();
        }
        System.out.println("FIFO Page Faults = " + pageFaults);
    }

    static void LRU(int[] pages) {
        int[] frames = new int[FRAME_SIZE];
        for (int i = 0; i < FRAME_SIZE; i++)
            frames[i] = 999;
        int[] lastUsed = new int[FRAME_SIZE];
        int time = 0;
        int pageFaults = 0;

        for (int page : pages) {
            boolean pageExists = false;
            for (int i = 0; i < FRAME_SIZE; i++) {
                if (frames[i] == page) {
                    lastUsed[i] = time++;
                    pageExists = true;
                    break;
                }
            }

            if (!pageExists) {
                int minIndex = 0;
                for (int i = 1; i < FRAME_SIZE; i++) {
                    if (lastUsed[i] < lastUsed[minIndex]) {
                        minIndex = i;
                    }
                }
                frames[minIndex] = page;
                lastUsed[minIndex] = time++;
                pageFaults++;
            }
            for (int frame : frames)
                System.out.print(frame + "\t");
            System.out.println();
        }
        System.out.println("LRU Page Replacement Algorithm:");
        System.out.println("Page Faults:" + pageFaults);
    }

    static void LFU(int[] pages) {
        int[] frames = new int[FRAME_SIZE];
        for (int i = 0; i < FRAME_SIZE; i++)
            frames[i] = 999;
        int[] freq = new int[FRAME_SIZE];
        int pageFaults = 0;

        for (int page : pages) {
            boolean pageExists = false;
            for (int i = 0; i < FRAME_SIZE; i++) {
                if (frames[i] == page) {
                    freq[i]++;
                    pageExists = true;
                    break;
                }
            }
            if (!pageExists) {
                int minIndex = 0;
                for (int i = 1; i < FRAME_SIZE; i++)
                    if (freq[i] < freq[minIndex])
                        minIndex = i;
                frames[minIndex] = page;
                freq[minIndex] = 1;
                pageFaults++;
            }
            for (int frame : frames)
                System.out.print(frame + "\t");
            System.out.println();
        }
        System.out.println("LFU Page Replacement Algorithm:");
        System.out.println("Page Faults:" + pageFaults);
    }

    public static void main(String args[]) {
        int[] pages = { 7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0, 1 };
        Scanner sc = new Scanner(System.in);
        int choice;
        do {
            System.out.println("\nMenu:\n1.FIFO\n2.LRU\n3.LFU\n4.Exit\nEnter your choice:");
            choice = sc.nextInt();
            switch (choice) {
                case 1:
                    FIFO(pages);
                    break;
                case 2:
                    LRU(pages);
                    break;
                case 3:
                    LFU(pages);
                    break;
                case 4:
                    System.out.println("Exiting...");
                    break;
                default:
                    System.out.println("Invalid Choice! Please enter a valid option!");
            }
        } while (choice != 4);
        sc.close();
    }
}
```
Output
```
Menu:
1.FIFO
2.LRU
3.LFU
4.Exit
Enter your choice:
1
7       999     999
7       0       999
7       0       1
2       0       1
2       0       1
2       3       1
2       3       0
4       3       0
4       2       0
4       2       3
0       2       3
0       2       3
0       2       3
0       1       3
0       1       2
0       1       2
0       1       2
7       1       2
7       0       2
7       0       1
FIFO Page Faults = 15

Menu:
1.FIFO
2.LRU
3.LFU
4.Exit
Enter your choice:
2
7       999     999
0       999     999
0       1       999
0       1       2
0       1       2
0       3       2
0       3       2
0       3       4
0       2       4
3       2       4
3       2       0
3       2       0
3       2       0
3       2       1
3       2       1
0       2       1
0       2       1
0       7       1
0       7       1
0       7       1
LRU Page Replacement Algorithm:
Page Faults:12

Menu:
1.FIFO
2.LRU
3.LFU
4.Exit
Enter your choice:
3
7       999     999
7       0       999
7       0       1
2       0       1
2       0       1
3       0       1
3       0       1
4       0       1
2       0       1
3       0       1
3       0       1
3       0       1
3       0       2
3       0       1
3       0       2
3       0       2
3       0       1
3       0       7
3       0       7
3       0       1
LFU Page Replacement Algorithm:
Page Faults:14

Menu:
1.FIFO
2.LRU
3.LFU
4.Exit
Enter your choice:
4
Exiting...
```
# Program 9
Paging Technique
```java
import java.util.Scanner;

public class Paging_Tech {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int ms, ps, nop, np, rempages, i, j, x, y, pa, offset;
        int[] s = new int[10];
        int[][] fno = new int[10][20];
        System.out.print("\nEnter the memory size -- ");
        ms = scanner.nextInt();
        System.out.print("\nEnter the page size -- ");
        ps = scanner.nextInt();
        nop = ms / ps;
        System.out.println("\nThe no. of pages available in memory are -- " + nop);
        System.out.print("\nEnter number of processes -- ");
        np = scanner.nextInt();
        rempages = nop;
        for (i = 1; i <= np; i++) {
            System.out.printf("\nEnter no. of pages required for p[%d]-- ", i);
            s[i] = scanner.nextInt();
            if (s[i] > rempages) {
                System.out.println("\nMemory is Full");
                break;
            }
            rempages = rempages - s[i];
            System.out.printf("\nEnter pagetable for p[%d] -- ", i);
            for (j = 0; j < s[i]; j++)
                fno[i][j] = scanner.nextInt();
        }
        System.out.println("\nEnter Logical Address to find Physical Address ");
        System.out.println("\nEnter process no. and pagenumber and offset ");
        x = scanner.nextInt();
        y = scanner.nextInt();
        offset = scanner.nextInt();
        if (x > np || y >= s[x] || offset >= ps)
            System.out.println("\nInvalid Process or Page Number or offset");
        else {

            pa = fno[x][y] * ps + offset;
            System.out.println("\nThe Physical Address is -- " + pa);
        }
        scanner.close();
    }
}
```
Output
```
Enter the memory size -- 1000

Enter the page size -- 100

The no. of pages available in memory are -- 10

Enter number of processes -- 3

Enter no. of pages required for p[1]-- 4

Enter pagetable for p[1] -- 8 6 9 5

Enter no. of pages required for p[2]-- 5

Enter pagetable for p[2] -- 1 4 5 7 3

Enter no. of pages required for p[3]-- 5

Memory is Full

Enter Logical Address to find Physical Address

Enter process no. and pagenumber and offset
2
3
60

The Physical Address is -- 760
```
# Program 10
Disk Scheduling
```java
import java.util.*;

public class DiskScheduling {
    static final int MAX_CYLINDER = 199; // Maximum cylinder number
    static final int MIN_CYLINDER = 0; // Minimum cylinder number

    // FIFO (First In, First Out) Disk Scheduling Algorithm
    static void fifo(int[] requests, int head) {
        System.out.println("FIFO Disk Scheduling Algorithm:");
        int totalSeekTime = 0;
        int currentHead = head;

        for (int request : requests) {
            totalSeekTime += Math.abs(currentHead - request);
            currentHead = request;
            System.out.println(currentHead);
        }

        System.out.println("Total Seek Time: " + totalSeekTime);
    }

    // SCAN (Elevator) Disk Scheduling Algorithm
    static void scan(int[] requests, int head) {
        System.out.println("SCAN Disk Scheduling Algorithm:");
        int totalSeekTime = 0;
        int currentHead = head;
        boolean directionUp = true;

        Arrays.sort(requests);

        // Find the index where the head is located
        int index = Arrays.binarySearch(requests, head);
        if (index < 0) {
            // If head is not in the requests array, find the index where it should be
            // inserted
            index = -index - 1;
        }

        // Scan in one direction until reaching the end
        for (int i = index; i < requests.length; i++) {
            totalSeekTime += Math.abs(currentHead - requests[i]);
            currentHead = requests[i];
            System.out.println(currentHead);
        }

        // Change direction and continue scanning until the beginning
        totalSeekTime += Math.abs(currentHead - MAX_CYLINDER);
        currentHead = MAX_CYLINDER;
        System.out.println(currentHead);
        directionUp = false;

        for (int i = index - 1; i >= 0; i--) {
            totalSeekTime += Math.abs(currentHead - requests[i]);
            currentHead = requests[i];
            System.out.println(currentHead);
        }

        System.out.println("Total Seek Time: " + totalSeekTime);
    }

    // C-SCAN (Circular SCAN) Disk Scheduling Algorithm
    static void cScan(int[] requests, int head) {
        System.out.println("C-SCAN Disk Scheduling Algorithm:");
        int totalSeekTime = 0;
        int currentHead = head;

        Arrays.sort(requests);

        // Find the index where the head is located
        int index = Arrays.binarySearch(requests, head);
        if (index < 0) {
            // If head is not in the requests array, find the index where it should be
            // inserted
            index = -index - 1;
        }

        // Scan in one direction until reaching the end
        for (int i = index; i < requests.length; i++) {
            totalSeekTime += Math.abs(currentHead - requests[i]);
            currentHead = requests[i];
            System.out.println(currentHead);
        }

        // Move to the beginning of the disk
        totalSeekTime += Math.abs(currentHead - MIN_CYLINDER);
        currentHead = MIN_CYLINDER;
        System.out.println(currentHead);

        // Continue scanning until reaching the head position again
        for (int i = 0; i < index; i++) {
            totalSeekTime += Math.abs(currentHead - requests[i]);
            currentHead = requests[i];
            System.out.println(currentHead);
        }

        System.out.println("Total Seek Time: " + totalSeekTime);
    }

    public static void main(String[] args) {
        int[] requests = { 98, 183, 37, 122, 14, 124, 65, 67 }; // Example disk requests
        int head = 53; // Example initial head position

        Scanner scanner = new Scanner(System.in);
        int choice;
        do {
            System.out.println("\nMenu:");
            System.out.println("1. FIFO Disk Scheduling Algorithm");
            System.out.println("2. SCAN Disk Scheduling Algorithm");
            System.out.println("3. C-SCAN Disk Scheduling Algorithm");
            System.out.println("4. Exit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    fifo(requests, head);
                    break;
                case 2:
                    scan(requests, head);
                    break;
                case 3:
                    cScan(requests, head);
                    break;
                case 4:
                    System.out.println("Exiting...");
                    break;
                default:
                    System.out.println("Invalid choice! Please enter a valid option.");
            }
        } while (choice != 4);
        scanner.close();
    }
}

```
Output
```
Menu:
1. FIFO Disk Scheduling Algorithm
2. SCAN Disk Scheduling Algorithm
3. C-SCAN Disk Scheduling Algorithm
4. Exit
Enter your choice: 1
FIFO Disk Scheduling Algorithm:
98
183
37
122
14
124
65
67
Total Seek Time: 640

Menu:
1. FIFO Disk Scheduling Algorithm
2. SCAN Disk Scheduling Algorithm
3. C-SCAN Disk Scheduling Algorithm
4. Exit
Enter your choice: 2
SCAN Disk Scheduling Algorithm:
65
67
98
122
124
183
199
37
14
Total Seek Time: 331

Menu:
1. FIFO Disk Scheduling Algorithm
2. SCAN Disk Scheduling Algorithm
3. C-SCAN Disk Scheduling Algorithm
4. Exit
Enter your choice: 3
C-SCAN Disk Scheduling Algorithm:
65
67
98
122
124
183
0
14
37
Total Seek Time: 350

Menu:
1. FIFO Disk Scheduling Algorithm
2. SCAN Disk Scheduling Algorithm
3. C-SCAN Disk Scheduling Algorithm
4. Exit
Enter your choice: 4
Exiting...
```
