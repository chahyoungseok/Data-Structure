# Algorithm

<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#greedy">Greedy</a></li>
    <li><a href="#avatar">Avatar</a></li>
    <li><a href="#dfs-bfs">DFS BFS</a></li>
    <li><a href="#sort">Sort</a></li>
    <li><a href="#search">Search</a></li>
    <li><a href="#dynamic-programming">Dynamic Programming</a></li>
    <li><a href="#shortest-path">Shortest Path</a></li>
    <li><a href="#graph">Graph</a></li>
    <li><a href="#practice-solution">Practice Solution</a></li>
  </ol>
</details>
<br>

## Greedy
 

### 거스름돈

``` changeMoney
changeMoney = int(input("거스름돈을 입력하세요: "))
money_unit = [500, 100, 50, 10]

for i in range(0,4) :
    print(str(money_unit[i]) + "원의 개수는 : " + str(int(changeMoney / money_unit[i])) + "개 입니다.")
    changeMoney %= money_unit[i]
```

<br>

### 큰수의 법칙

``` law of large numbers 
// N개 주어진 자연수들을 연속으로 더하되 K번 초과하지않고 총 M번 더했을 때의 최대값 (중복가능)

N, M, K = map(int,input().split())

intArr = list(map(int,input().split()))
intArr.sort()

max = intArr[N - 1]
submax = intArr[N - 2]

count = int(M/(K + 1))

sum = submax * count + max * (M - count)
print(sum)
``` 

<br>

### 숫자 카드 게임

``` number card game
// 각각의 행에 해당하는 M개의 자연수들 중 최솟값을 구하고, 그 N개의 열중 최댓값을 구하여라

N, M = map(int, input().split()) // N은 행의 개수, M은 열의 개수

card = []

for inList in range(0,N) :
    inList = list(map(int,input().split()))
    card.append(min(inList))

print(max(card))
```

<br>

### 1이 될 때까지

``` until it becomes 1
// 1. N에서 1을 뺀다.
// 2. N을 K로 나눈다. (나누어 떨어질때만)
// 위의 1 ,2를 1이 될 때까지 수행하는 최소연산의 수는?

N, K = map(int, input().split())

count = 0

while True :
    remainder = (N // K) * K
    count += N - remainder
    N = remainder

    if N < K :
        break

    count += 1
    N //= K

count += N - 1

print(count)
```

<br>

## Avatar


### 상하좌우

``` Up Down Left Right
// 여행가가 L, R, U, D으로 움직이고 정해진 N x N 지도에서 벗어나게되면 무시하는 방법으로 여행가의 마지막위치를 추적하는 프로그램
N = int(input())
moveList = list(map(str, input().split()))

x, y = 1, 1
preX, preY = 0, 0

move_types = ['L','R','U','D']

mx = [0,0,-1,1]
my = [-1,1,0,0]

for move in moveList :
    for i in range(0,len(move_types)) :
        if move == move_types[i] :
            preX = x + mx[i]
            preY = y + my[i]
            break

    if (preX > 0 and preX < 6) and (preY > 0 and preY < 6) :
        x = preX
        y = preY

print(str(x) + " " + str(y))
```
 
<br>

### 시각

``` clock
N = int(input())

count = 0
for i in range(0, N + 1) :
    for j in range(0, 60) :
        for h in range(0, 60) :
            if '3' in str(i) + str(j) + str(h) :
                count += 1

print(count)
```

<br>

### 왕실의 나이트

``` royal night
def numberOfCase(canMove, RC) :
    if RC == 1 or RC == 8:
        canMove /= 2
    elif RC == 2 or RC == 7:
        if canMove % 4 == 0:
            canMove *= 3 / 4
        else:
            canMove *= 2 / 3

    return canMove

loc = input()

row = int(loc[1])
col = ord(loc[0]) - ord('a') + 1

canMove = 8
canMove = numberOfCase(canMove,row)
canMove = numberOfCase(canMove,col)

print(int(canMove))
```

<br>

### 게임 개발

``` game development
N, M = map(int, input().split())
a, b, d = map(int, input().split())

mapVis = [[0] * M for i in range(N)]
mapVis[a][b] = 1
visCount = 1

mapInf = []
for i in range(N) :
    mapInf.append(list(map(int, input().split())))

dirCount = 0
dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

while True :
    d = (d + 3) % 4
    CooA = a + dx[d]
    CooB = b + dy[d]

    if dirCount == 4 :
        invD = (d + 2) % 4
        a = a + dx[invD]
        b = b + dy[invD]

        if mapVis[a][b] == 1 or mapInf[a][b] == 1 :
            break
        else :
            mapVis[a][b] = 1
            visCount += 1
            continue

    if CooA < 0 or CooA > N - 1 or mapVis[CooA][CooB] == 1 or mapInf[CooA][CooB] == 1 :
        dirCount += 1
        continue
    else :
        a = CooA
        b = CooB
        mapVis[a][b] = 1
        visCount += 1
        dirCount = 0

print(visCount)
```

<br>

## DFS BFS


### 이론

탐색
 - 많은 양의 데이터 중에서 원하는 데이터를 찾는 과정

자료구조
 - 데이터를 표현하고 관리하고 처리하기 위한 구조

Stack 사용법 (in Python)
 - 별도의 라이브러리가 필요없고, 기본 리스트에서 append, pop등을 사용

Queue 사용법 (in Python)
 - from collections import deque
 - deque : stack과 queue의 장점을 모두 채택한 것

재귀함수
 - 자기 자신을 다시 호출하는 함수

<br><br>

DFS
 - 깊이우선탐색 이라고 부르며, 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘

``` dfs
def dfs(graph,  v, visited) :
    visited[v] = True
    print(v, end=' ')

    for i in graph[v] :
        if not visited[i] :
            dfs(graph, i, visited)

graph =[
    [],
    [2,3,8],
    [1,7],
    [1,4,5],
    [3,5],
    [3,4],
    [7],
    [2,6,8],
    [1,7]
]

visited = [False] * 9

dfs(graph, 1, visited)
```

<br><br>

BFS
 - 너비우선탐색 이라고 부르며, 가까운 노드부터 탐색하는 알고리즘
 - 인접한 노드가 여러개 있을 때, 숫자가 작은 노드부터 큐에 삽입하였다

``` bfs
from collections import deque

def bfs (graph, start, visited) :
    queue = deque([start])

    visited[start] = True

    while queue :
        v = queue.popleft()
        print(v, end=' ')

        for i in graph[v] :
            if not visited[i]:
                queue.append(i)
                visited[i] = True

graph =[
    [],
    [2,3,8],
    [1,7],
    [1,4,5],
    [3,5],
    [3,4],
    [7],
    [2,6,8],
    [1,7]
]

visited = [False] * 9

bfs(graph, 1, visited)
```

<br>

### 음료수 얼리기

``` freeze drinks
def confirmAround(i, j) :
    global iceMap

    if i < 0 or i > len(iceMap) - 1 or j < 0 or j > len(iceMap[0]) - 1  :
        return False

    if iceMap[i][j] == 0 :
        iceMap[i][j] = 1

        confirmAround(i - 1, j)
        confirmAround(i, j + 1)
        confirmAround(i + 1, j)
        confirmAround(i, j - 1)

        return True
    return False


N, M = map(int, input().split())

iceMap = []
iceCount = 0

for i in range(N) :
    iceMap.append(list(map(int,input())))


for i in range(N) :
    for j in range(M) :
        if confirmAround(i, j) :
            iceCount += 1

print(iceCount)
```

<br>

### 미로 탈출

``` Escape maze
from collections import deque

N, M = map(int, input().split())

mazeMap = []
for i in range(N) :
    mazeMap.append(list(map(int, input())))

x, y = 0, 0

dx = [-1, 0, 1, 0]
dy = [0, -1, 0, 1]

mazeQueue = deque()
mazeQueue.append((x, y))

while mazeQueue :
    x, y = mazeQueue.popleft()

    for i in range(4) :
        preX = dx[i] + x
        preY = dy[i] + y

        if preX < 0 or preY < 0 or preX > N - 1 or preY > M - 1 :
            continue

        if mazeMap[preX][preY] == 1 :
            mazeQueue.append((preX, preY))
            mazeMap[preX][preY] = mazeMap[x][y] + 1

print(mazeMap[N - 1][M - 1])
```

<br>


## Sort


### 선택정렬

``` select sort
array = [7,5,9,0,3,1,6,2,4,8]

for i in range(len(array)) :
    min_index = i
    for j in range(i+1, len(array)) :
        if array[min_index] > array[j] :
            min_index = j
    array[i], array[min_index] = array[min_index], array[i]

print(array)
```

<br>

### 삽입정렬

``` insert sort
array = [7,5,9,0,3,1,6,2,4,8]

for i in range(1, len(array)) :
    for j in range(i, 0, -1) :
        if array[j] < array[j - 1] :
            array[j], array[j - 1] = array[j - 1], array[j]
        else :
            break

print(array)
```

<br>

### 퀵 정렬

``` quick sort
array = [5,7,9,0,3,1,6,2,4,8]

def quick_sort(array, start, end) :
    if start >= end :
        return

    pivot = start
    left = start + 1
    right = end

    while left <= right :
        while left <= end and array[left] <= array[pivot] :
            left += 1

        while right > start and array[right] >= array[pivot] :
            right -= 1

        if left > right :
            array[right], array[pivot] = array[pivot], array[right]
        else :
            array[left], array[right] = array[right], array[left]

    quick_sort(array, start, right - 1)
    quick_sort(array, right + 1, end)

quick_sort(array, 0, len(array) - 1)
print(array)
```

<br>

### 계수 정렬

<br>

특징
 - 데이터의 개수가 1000개정도 이하일 때 사용하면 너무좋은 알고리즘

``` count sort
array = [7,5,9,0,3,1,6,2,9,1,4,8,0,5,2]

count = [0] * (max(array) + 1)

for i in range(len(array)) :
    count[array[i]] += 1

for i in range(len(count)) :
    for j in range(count[i]) :
        print(i, end=' ')
```

<br>

### 큰수의 법칙

``` law of large numbers 
// N개 주어진 자연수들을 연속으로 더하되 K번 초과하지않고 총 M번 더했을 때의 최대값 (중복가능)

N, M, K = map(int,input().split())

intArr = list(map(int,input().split()))
intArr.sort()

max = intArr[N - 1]
submax = intArr[N - 2]

count = int(M/(K + 1))

sum = submax * count + max * (M - count)
print(sum)
```

<br>

### 위에서 아래로

``` top down
N = int(input())

array = []
for i in range(N) :
    array.append(int(input()))

array = sorted(array, reverse=True)

for i in array :
    print(i, end=' ')
```

<br>

### 성적이 낮은 순서로 학생 출력하기

``` print students in descending order of grades
N = int(input())

array = []

for i in range(N) :
    input_data = input().split()
    array.append((input_data[0], int(input_data[1])))

array = sorted(array, key= lambda score: score[1])

for student in array :
    print(student[0], end=' ')
```

<br>

### 두 배열의 원소 교체

``` swap elements in two arrays
N, K = map(int, input().split())

A = list(map(int, input().split()))
B = list(map(int, input().split()))

A.sort()
B.sort(reverse=True)

for i in range(K) :
    if B[i] > A[i] :
        A[i], B[i] = B[i], A[i]
    else :
        break

print(sum(A))
```

<br>

## Search


### 순차탐색

``` sequential search
def sequential_search(n, target, array) :
    for i in range(n) :
        if array[i] == target :
            return i + 1

print("생성할 원소 개수를 입력한 다음 한 칸 띄고 찾을 문자열을 입력하세요.")
input_data = input().split()
n = int(input_data[0])
target = input_data[1]

print("앞서 적은 원소 개수만큼 문자열을 입력하세요. 구분은 띄어쓰기 한 칸으로 합니다.")
array = input().split()

print(sequential_search(n, target, array))
```

<br>

### 이진탐색

``` binary search
# 데이터가 정렬되어 있을 때, 사용할 수 있다.
# 시간 복잡도가 O(logN) 이기 떄문에 데이터가 1000만 이상으로 가게되면 가급적 사용해야한다.

def binary_search(array, target, start, end) :
    if start > end:
        return None
    mid = (start + end) // 2

    if array[mid] == target :
        return mid
    elif array[mid] > target :
        return binary_search(array, target, start, mid - 1)
    else :
        return binary_search(array, target, mid + 1, end)

n, target = list(map(int, input().split()))

array = list(map(int, input().split()))

result = binary_search(array, target, 0, n - 1)
if result == None :
    print("원소가 존재하지 않습니다.")
else :
    print(result + 1)
```

<br>

### sys 라이브러리

``` sys
# 데이터의 개수가 1000만을 넘어가거나 탐색범위의 크기가 1000억 이상인 문제들이 나올 때, input()은 동작속도가 느리므로 아래의 라이브러리를 응용하자.

import sys

input_data = sys.stdin.readline().rstrip()

print(input_data)
```

<br>

### 부품 찾기

``` find equipments
def binary_search(array, target, start, end) :
    if start > end:
        return None
    mid = (start + end) // 2

    if array[mid] == target :
        return mid
    elif array[mid] > target :
        return binary_search(array, target, start, mid - 1)
    else :
        return binary_search(array, target, mid + 1, end)


N = int(input())
equipments_shop = list(map(int, input().split()))
equipments_shop.sort()

M = int(input())
equipments_customer = list(map(int, input().split()))

for i in equipments_customer :
    result = binary_search(equipments_shop, i, 0, N - 1)

    if result != None :
        print("yes", end =' ')
    else :
        print("no", end = ' ')
```

<br>

### 떡볶이 떡 만들기

``` make rice cake
N, M = map(int, input().split())
riceCake = list(map(int, input().split()))

result = 0
start = 0
end = max(riceCake)

while start <= end :
    remainCake = 0
    mid = (start + end) // 2

    for i in riceCake :
        if i > mid :
            remainCake += i - mid

    if remainCake > M :
        result = mid
        start = mid + 1
    elif remainCake == M :
        result = mid
        break
    else :
        end = mid - 1

print(result)
```

<br>

## Dynamic Programming


### 이론

다이나믹 프로그래밍
 - 큰 문제를 작게 나누고, 같은 문제라면 한 번씩만 풀어 문제를 효율적으로 해결하는 알고리즘 기법.

메모제이션 or 캐싱
 - 한 번 구한 결과를 메모리 공간에 메모해두고 같은 식을 다시 호출하면 메모한 결과를 그대로 가져오는 기법.


<br>

### Top Down 방식(하향식)
 - 메모제이션 방식을 사용하지 않음.
 - 구현가능하다면 Bottom Up 방식을 사용하는게 좋음.

``` top down
d = [0] * 100

def pibo(x) :
    print("f(" + str(x) + ")", end=" ")
    if x == 1 or x == 2 :
        return 1
    if d[x] != 0 :
        return d[x]
    d[x] = pibo(x - 1) + pibo(x - 2)
    return d[x]

pibo(6)
```

### Bottom Up 방식(상향식)
 - 메모제이션 방식을 사용함.

``` bottom up
d = [0] * 100

d[1] = 1
d[2] = 1
n = 99

for i in range(3, n + 1) :
    d[i] = d[i - 1] + d[i - 2]

print(d[n])
```

<br>

### 1로 만들기

``` make 1
X = int(input())

d = [0] * 30001

for i in range(2, X + 1):
    d[i] = d[i - 1] + 1

    if i % 2 == 0 :
        d[i] = min(d[i], d[i // 2] + 1)

    if i % 3 == 0:
        d[i] = min(d[i], d[i // 3] + 1)

    if i % 5 == 0:
        d[i] = min(d[i], d[i // 5] + 1)

print(d[X])
```

<br>

### 개미 전사

``` warrier ant
N = int(input())
storage = list(map(int, input().split()))

d = [0] * 100

d[0] = storage[0]
d[1] = max(storage[0], storage[1])

for i in range(2, N) :
    d[i] = max(d[i - 1], storage[i] + d[i - 2])

print(d[N-1])
```

<br>

### 효율적인 화폐 구성

``` efficient money structure
N, M = map(int, input().split())

moneyAry = []
for i in range(N) :
    moneyAry.append(int(input()))

d = [10001] * (M+1)
d[0] = 0

for i in range(N) :
    for j in range(moneyAry[i],M + 1) :
        if d[j - moneyAry[i]] != 10001 :
            d[j] = min(d[j],d[j - moneyAry[i]] + 1)

if d[M] == 10001 :
    print(-1)
else :
    print(d[M])
```

<br>

## Shortest Path


### 이론


Dijkstra
 - 음의 간선이 없을때, 정상동작
 - 그리디 알고리즘으로 분류됨
 - 각 노드들에 대한 최단거리 정보를 항상 1차원리스트에 저장


<br>

### Dijkstra

``` dijkstra
import heapq

INF = int(1e9)

n, m = map(int, input().split())
start = int(input())

graph = [[] for i in range(n + 1)]
distance = [INF] * (n + 1)

for _ in range(m) :
    a, b, c = map(int, input().split())
    graph[a].append((b,c))

# graph는 (노드, 거리) / Heap은 (거리, 노드) 를 주의!
def dijkstra(start) :
    q = []
    heapq.heappush(q,(0,start))
    distance[start] = 0

    while q:
        dist, now = heapq.heappop(q)
        if distance[now] < dist :
            continue

        for i in graph[now] :
            cost = dist + i[1]
            if cost < distance[i[0]] :
                distance[i[0]] = cost
                heapq.heappush(q,(cost, i[0]))

dijkstra(start)

for i in range(1, n + 1) :
    if distance[i] == INF :
        print("INFINITY")
    else :
        print(distance[i])
```

Floyd
 - 모든 지점에서 다른 모든 지점까지의 최단경로
 - 다이나믹 프로그래밍으로 분류된다.


<br>

### Floyd

``` floyd
n, m = map(int, input().split())

graph = [[int(1e9)] * (n + 1) for _ in range(n + 1)]

for i in range(1, n+1) :
    graph[i][i] = 0

for _ in range(m) :
    a, b, c = map(int, input().split())
    graph[a][b] = c

for k in range(1, n+1) :
    for a in range(1, n+1) :
        for b in range(1, n+1) :
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])

for a in range(1, n+1) :
    for b in range(1, n+1) :
        if graph[a][b] == int(1e9) :
            print("INFINITY", end=" ")
        else :
            print(graph[a][b], end=" ")
    print()
```

<br>

### 미래 도시

``` future city
N, M = map(int, input().split())

INF = int(1e9)
graph = [[INF] * (N + 1) for _ in range(N + 1)]

for i in range(1, N + 1) :
    graph[i][i] = 0

for i in range(M) :
    a,b = map(int, input().split())
    graph[a][b] = 1
    graph[b][a] = 1

X, K = map(int, input().split())

for k in range(1,N + 1) :
    for i in range(1, N + 1) :
        for j in range(1, N + 1) :
            graph[i][j] = min(graph[i][k] + graph[k][j], graph[i][j])

distances = graph[1][K] + graph[K][X]

if distances >= INF :
    print("-1")
else :
    print(distances)
```

<br>

### 전보

``` telegram
import heapq

INF = int(1e9)
visitCount = 0
maxDistance = 0

N, M, C = map(int, input().split())

graph = [[] for _ in range(N + 1)]

for i in range(M) :
    X, Y, Z = map(int, input().split())
    graph[X].append((Y,Z))

distances = [INF for _ in range(N + 1)]

q = []
heapq.heappush(q, (0,C))
distances[C] = 0

while q :
    dist, now = heapq.heappop(q)

    if dist > distances[now] :
        continue

    for i in graph[now] :
        cost = dist + i[1]
        if distances[i[0]] > cost :
            distances[i[0]] = cost
            heapq.heappush(q, (cost, i[0]))

for i in range(1, M + 1) :
    if distances[i] != INF :
        visitCount += 1
        maxDistance = max(maxDistance, distances[i])

print("가장 멀리있는 노드의 거리는 : " + str(maxDistance) + "\n방문한 노드의 개수는 : " + str(visitCount))
```

<br>

## Graph


### 이론

인접행렬 
 - 2차원 배열을 사용하는 방식
 - ex) Floyd

인접리스트
 - 리스트를 사용하는 방식
 - Dijkstra

서로소 집합
 - 공통원소가 없는 두 집합을 의미한다.
 - 서로소 부분 집합들로 나누어진 원소들의 데이터를 처리하기 위한 자료구조로 서로소 집합 자료구조가 있다.
 - cf) union - find 자료구조

<br>

### union - find 
 - 1. union 연산을 확인하여, 서로 연결된 두 노드 A,B를 확인한다. 
 - 2. A와 B의 루트노드를 각각 찾고, 작은 루트노드를 큰 루트노드가 가르키게한다.
 - 3. 모든 union 연산을 처리할 때까지 1,2번과정을 반복한다.

```union - find
def find_parent(parent, x) :
    if parent[x] != x:
        return find_parent(parent, parent[x])
    return parent[x]

def union_parent(parent, a, b) :
    a = find_parent(parent, a)
    b = find_parent(parent, b)

    if a < b :
        parent[b] = a
    else :
        parent[a] = b

v, e = map(int, input().split())
parent = [0] * (v + 1)

for i in range(1, v + 1) :
    parent[i] = i

for i in range(e) :
    a, b = map(int, input().split())
    union_parent(parent, a, b)

print("각 원소가 속한 집합: ", end='')
for i in range(1, v + 1) :
    print(find_parent(parent, i), end=' ')

print()

print("부모 테이블: ", end='')
for i in range(1, v + 1) :
    print(parent[i], end=' ')
```

<br>

### 사이클 판별 알고리즘

``` discriminate cycle
def find_parent(parent, x) :
    if parent[x] != x:
        return find_parent(parent, parent[x])
    return parent[x]

def union_parent(parent, a, b) :
    a = find_parent(parent, a)
    b = find_parent(parent, b)

    if a < b :
        parent[b] = a
    else :
        parent[a] = b


v, e = map(int, input().split()) 
parent = [0] * (v + 1)

for i in range(1, v + 1) :
    parent[i] = i

cycle = False

for i in range(e) :
    a, b = map(int, input().split())
    if find_parent(parent, a) == find_parent(parent, b) :
        cycle = True
        break
    else :
        union_parent(parent, a, b)

if cycle :
    print("사이클이 발생했습니다.")
else :
    print("사이클이 발생하지 않았습니다.")
```

<br>

### Kruskal 알고리즘

``` kruskal
def find_parent(parent, x) :
    if parent[x] != x:
        return find_parent(parent, parent[x])
    return parent[x]

def union_parent(parent, a, b) :
    a = find_parent(parent, a)
    b = find_parent(parent, b)

    if a < b :
        parent[b] = a
    else :
        parent[a] = b


v, e = map(int, input().split())
parent = [0] * (v + 1)

edges = []
result = 0

for i in range(1, v + 1) :
    parent[i] = i

cycle = False

for i in range(e) :
    a, b, cost = map(int, input().split())
    edges.append((cost, a, b))

edges.sort()

for edge in edges :
    cost, a, b = edge
    if find_parent(parent, a) != find_parent(parent, b) :
        union_parent(parent, a, b)
        result += cost

print(result)
```

<br>

### topological sort
 - 1. 진입차수가 0인 노드를 큐에 넣는다.
 - 2. 큐에서 원소를 꺼내 해당 노드에서 출발하는 간선을 그래프에서 제거한다.
 - 3. 새롭게 진입차수가 0이 된 노드를 큐에 넣는다.
 - 4. 2번과 3번의 과정을 큐가 빌 때까지 반복한다.

``` topological sort
from collections import deque

v, e = map(int, input().split())

indegree = [0] * (v + 1)

graph = [[] for i in range(v + 1)]

for _ in range(e):
    a, b = map(int, input().split())
    graph[a].append(b)

    indegree[b] += 1


def topology_sort():
    result = []
    q = deque()

    for i in range(1, v + 1):
        if indegree[i] == 0:
            q.append(i)

    while q:
        now = q.popleft()
        result.append(now)

        for i in graph[now]:
            indegree[i] -= 1
            if indegree[i] == 0:
                q.append(i)

    for i in result:
        print(i, end=' ')


topology_sort()
```

<br>

## Practice Solution

<br>

### Greedy Solution


#### Adventure's Guild

``` Adventure's Guild
N = int(input())

fear_rate = list(map(int, input().split()))
rate_array = [0 for _ in range(0, N)]
group_count = 0

for i in range(0, N) :
    rate_array[fear_rate[i]] += 1

for i in range(1, N) :
    group_count += rate_array[i] // i

print(group_count)
```

<br>

#### Multiple or Add

``` multiple or add
S = input()

result = int(S[0])

for i in range(1, len(S)) :
    num = int(S[i])
    if num <= 1 or result <= 1 :
        result += num
    else :
        result *= num

print(result)
```

<br>

#### Reverse String

``` reverse string
S = input()

origin_state = True
reverse_count = 0
current_number = S[0]

for i in range(1,len(S)) :
    if current_number != S[i] and origin_state:
        reverse_count += 1
        origin_state = False
    if current_number == S[i] :
        origin_state = True

print(reverse_count)
```

<br>

#### Don't make Money

``` don't make Money
N = int(input())
money_unit = list(map(int, input().split()))
money_unit.sort()

target = 1

for x in money_unit :
    if target < x :
        break
    target += x

print(target)
```

<br>

#### Choose the bowling ball

``` choose the bowling ball
N, M = map(int, input().split())
ball_weights = list(map(int, input().split()))

case_count = 0
weights = [0] * (M + 1)

for x in ball_weights :
    weights[x] += 1

for i in range(1, M + 1) :
    N = N - weights[i]
    case_count += N * weights[i]

print(case_count)
```

<br>

#### mukBang live

``` mukBang live
import heapq

def solution(food_times, k):
    food_count = len(food_times)
    time_now = 0
    q = []
    for i in range(0,food_count) :
        heapq.heappush(q, (food_times[i], i + 1))

    while True :
        time, num = heapq.heappop(q)

        if k > time_now + (time * (len(q) + 1)) :
            time_now += time * (len(q) + 1)
        else :
            time, num = q[(k - time_now - 1) % len(q)]
            return num


print("정전이후 먹어야 될 음식의 번호 : " + str(solution([3, 1, 2], 5)) + "번")
```

<br>

### Avatar Solution

<br>

#### lucky straight

``` lucky straight
N = input()
half_index = int(len(N) / 2)

left_sum = 0
right_sum = 0

for i in range(0, half_index) :
    left_sum += int(N[i])
    right_sum += int(N[i + half_index])

if left_sum == right_sum :
    print("LUCKY")
else :
    print("READY")
```

<br>

#### String Resort

``` string reSort
S = input()
n_sum = 0
q = []

for i in S :
    if i.isalpha() :
        q.append(i)
    else :
        n_sum += int(i)

q.sort()

if n_sum != 0 :
    q.append(str(n_sum))

print(''.join(q))
```

<br>

#### Compress String

``` compress string
def solution(s) :
    minLen = int(1e9)

    for length in range(1, int(len(s) / 2) + 1):
        compressStr = ""
        sameCount = 1
        preStr = s[0:length]

        for i in range(length,len(s) + 1,length) :
            if len(s) >= i + length :
                if preStr == s[i: i + length]:
                    sameCount += 1
                else:
                    if sameCount != 1 :
                        compressStr += str(sameCount) + preStr
                    else :
                        compressStr += preStr

                    preStr = s[i: i + length]
                    sameCount = 1
            else :
                if sameCount != 1:
                    compressStr += str(sameCount) + preStr
                else :
                    compressStr += preStr

                compressStr += s[i : len(s)]

        minLen = min(minLen, len(compressStr))

    return minLen

s = input()
print(solution(s))
```

<br>

#### Lock and Key

``` lock and key
def rotaition(key) :
    newKey = [[0, 0, 0], [0, 0, 0], [0, 0, 0]]
    for i in range(len(key)):
        for j in range(len(key)):
            if key[i][j] == 1:
                newKey[len(key) - j - 1][i] = 1
    return newKey

def check(lock) :
    lock_length = len(lock) // 3
    for i in range(lock_length, 2 * lock_length) :
        for j in range(lock_length, 2 * lock_length) :
            if lock[i][j] != 1 :
                return False

    return True

def solution(key, lock):
    key_size = len(key)
    lock_size = len(lock)
    new_lock = [[0] * (lock_size * 3) for _ in range(lock_size * 3)]

    for i in range(lock_size) :
        for j in range(lock_size):
            new_lock[lock_size + i][lock_size + j] = lock[i][j]

    for ro in range(4) :
        key = rotaition(key)
        for x in range(lock_size * 2) :
            for y in range(lock_size * 2) :
                for i in range(key_size) :
                    for j in range(key_size) :
                        new_lock[x + i][y + j] += key[i][j]

                if check(new_lock) :
                    return True

                for i in range(lock_size) :
                    for j in range(lock_size) :
                        new_lock[x + i][y + j] -= key[i][j]

    return False

key = [[0,0,0], [1,0,0], [0,1,1]]
lock = [[1,1,1], [1,1,0], [1,0,1]]

print(solution(key, lock))
```
