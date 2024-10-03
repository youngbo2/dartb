24/09/27 fri

https://school.programmers.co.kr/learn/courses/30/lessons/340213
![image](https://github.com/user-attachments/assets/0fde5432-3055-448b-a839-104d5c2fd758)


24/09/28 sat

https://school.programmers.co.kr/learn/courses/30/lessons/138477
![image](https://github.com/user-attachments/assets/2190f905-adec-4f1d-947f-5cd6313b289b)

24/10/01 tue

https://www.acmicpc.net/problem/25214
```
N = input()
A = list(map(int,input().split()))
answer = 0
mincount = A[0]
maxcount = A[0]
for a in A:
    if a > maxcount:
        maxcount = a
    answer = max(maxcount - mincount, answer)
    if mincount > a:
        mincount = a
        maxcount = a
    print(answer, end=" ")
```
