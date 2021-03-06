## Prison Cels After N Days

----
## 题目地址

[https://leetcode.com/problems/prison-cells-after-n-days/](https://leetcode.com/problems/prison-cells-after-n-days/)

## 题目描述

```text
There are 8 prison cells in a row, and each cell is either occupied or vacant.

Each day, whether the cell is occupied or vacant changes according to the following rules:

If a cell has two adjacent neighbors that are both occupied or both vacant, then the cell becomes occupied.
Otherwise, it becomes vacant.
(Note that because the prison is a row, the first and the last cells in the row can't have two adjacent neighbors.)

We describe the current state of the prison in the following way: cells[i] == 1 if the i-th cell is occupied, else cells[i] == 0.

Given the initial state of the prison, return the state of the prison after N days (and N such changes described above.)

Example 1:

Input: cells = [0,1,0,1,1,0,0,1], N = 7
Output: [0,0,1,1,0,0,0,0]
Explanation: 
The following table summarizes the state of the prison on each day:
Day 0: [0, 1, 0, 1, 1, 0, 0, 1]
Day 1: [0, 1, 1, 0, 0, 0, 0, 0]
Day 2: [0, 0, 0, 0, 1, 1, 1, 0]
Day 3: [0, 1, 1, 0, 0, 1, 0, 0]
Day 4: [0, 0, 0, 0, 0, 1, 0, 0]
Day 5: [0, 1, 1, 1, 0, 1, 0, 0]
Day 6: [0, 0, 1, 0, 1, 1, 0, 0]
Day 7: [0, 0, 1, 1, 0, 0, 0, 0]
```

## 代码

Approach \#2 find the loop

Well, the length of loop can be 1, 7, or 14.

```java
class Solution {
  public int[] prisonAfterNDays(int[] cells, int N) {
    for (N = (N - 1) % 14 + 1; N > 0; N--) {
      int[] temp = new int[8];
      for (int i = 1; i < 7; i++) {
        temp[i] = cells[i - 1] == cells[i + 1] ? 1 : 0;
      }
      cells = temp;
    }

    return cells;
  }
}
```

Approach \#2 Simulation

```java
class Solution {
    public int[] prisonAfterNDays(int[] cells, int N) {
        Map<Integer,int[]> map = new HashMap<Integer,int[]>();
        int days = N;
        int cycle = 0;
        while(days-- > 0){
            int[] nextDay = new int[8];
            for(int i = 1; i < 7; i++){
                nextDay[i] = cells[i - 1] == cells[i + 1] ? 1 : 0;
            }
            int[] first = map.getOrDefault(1, null);
            if(first != null){
                if(Arrays.equals(nextDay, first)){
                    if(N % (N - days - 1) == 0)
                        return cells;
                    break;
                }
            }
                        map.put(N - days, nextDay);
            cells = nextDay;
           }
        return days == -1 ? cells : map.get(N % (N - days - 1));
    }
}
```

Approach 3: Time Limit Exceeded

```java
class Solution {
    public int[] prisonAfterNDays(int[] cells, int N) {
        int n = cells.length;
        int temp[] = new int[n];

        while (N > 0) {
            for (int i = 0; i < n; i++) {
                temp[i] = cells[i];
            }

            temp[0] = 0;
            temp[n - 1] = 0;

            for (int i = 1; i <= n - 2; i++) {
                if (cells[i - 1] == cells[i + 1]) {
                    temp[i] = 1;
                } else {
                    temp[i] = 0;
                }
            }

            for (int i = 0; i < n; i++) {
                cells[i] = temp[i];
            }

            N--;
        }

        return cells;
    }
}
```

