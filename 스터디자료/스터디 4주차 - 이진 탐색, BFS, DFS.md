### 이진 탐색

탐색 범위를 두 부분으로 분할하면서 찾는 방식이다.
시간 복잡도는 평균 O(logN)이다.

![](https://velog.velcdn.com/images/rlagurwns112/post/49487338-bfc3-4f60-9512-833394d09648/image.gif)

#### 참고 : 순차 탐색(Sequential Search)

순차 탐색은 리스트 안에 있는 특정 데이터를 찾기 위해 앞에서부터 데이터를 차례로 확인하는 방법. 시간 복잡도 O(N)이다.
인덱스 0부터 차례로 탐색하며 원소를 건너뛰는 일이 없이 순차적으로 탐색한다.

#### 진행 과정

**우선 정렬을 해야 한다.**
start end mid 값을 설정한다.
mid와 내가 구하고자 하는 값과 비교한다.
구할 값이 mid보다 높으면 start = mid +1, 반대면 end = mid -1
start > end 될 때까지 계속 반복한다.

#### JAVA 코드로 표현

```
public static int binarySearch(int[] arr, int M) { // arr 배열에서 M을 찾는 이진 탐색 알고리즘

        // 오름차순 정렬 먼저 실행
        Arrays.sort(arr);

        int start = 0;
        int end = arr.length - 1;
        int mid = 0;

        while (start <= end) {

            // 중간값을 설정
            mid = (start + end) / 2;

            // 중간값이 M과 같다면 mid를 반환
            if (M == arr[mid]) {
                return mid;
                // 중간값이 M보다 작다면 start를 mid + 1로 설정
            } else if (arr[mid] < M) {
                start = mid + 1;
                // 중간값이 M보다 크다면 end를 mid - 1로 설정
            } else if (M < arr[mid]) {
                end = mid - 1;
            }
        }

        throw new NoSuchElementException("타겟 존재하지 않음");
    }
```

#### C++ 코드로 표현

```
int binsearch(int data[], int n, int key) {
    int start, end;
    int mid;

    low = 0;
    end = n - 1;
    while (start <= end) {
        mid = (start + end) / 2;
        if (key == data[mid]) {            //탐색 성공
            return mid;
        }
        else if (key < data[mid]) {        //탐색 범위를 아래쪽으로
            end = mid - 1;
        }
        else if (key > data[mid]) {        //탐색 범위를 위쪽으로
            start = mid + 1;
        }
    }
    return -1;                            //탐색 실패
}

```

#### 장점

처음부터 끝까지 돌면서 탐색하는 것보다 훨씬 빠른 장점을 가진다.
이분 탐색의 경우 시간 복잡도가 O(logN)이다.

### 문제

백준 1920번 수 찾기

https://www.acmicpc.net/problem/1920

![](https://velog.velcdn.com/images/rlagurwns112/post/b2af4aee-9c78-4c28-95fa-7a9b8a397169/image.png)

---

### 알고리즘 [접근 방법]

순차 탐색 또는 이진 탐색을 이용하자.
시간 제한은 2초이므로 약 2억번의 연산 이내로 하면 된다.
N의 최대 값은 1000000이므로 O(n^2)인 알고리즘만 피하자
이진 탐색을 이용하면 약 O(log1000000) ~= 20이다.

---

### 풀이방법

이진 탐색을 이용하기 위해 오름차순으로 정렬한다.
그리고 while문을 활용하여 이진 탐색을 실행한다.
start, mid, end를 시작, 중간, 끝 인덱스로 초기화한다.

---

### 풀이

```
public class Main {

    public static int[] arr;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        arr = new int[N];

        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        for (int i = 0; i < N; i++) {
            arr[i] = Integer.parseInt(st.nextToken());
        }

        Arrays.sort(arr);

        int M = Integer.parseInt(br.readLine());

        st = new StringTokenizer(br.readLine(), " ");

        for (int i = 0; i < M; i++) {
            System.out.println(binarySearch(arr,Integer.parseInt(st.nextToken())));
        }
    }

    static int binarySearch(int[] arr, int target) {
        int start = 0;
        int end = arr.length -1;
        int mid = 0;

        while (start <= end) {
            mid = (start + end) / 2;

            if (target == arr[mid]) {
                return 1;
            } else if (arr[mid] < target) {
                start = mid +1;

            } else if (target < arr[mid]) {
                end = mid -1 ;
            }
        }
        return 0;
    }
}
```

---

### 정리

업다운 게임이라고 생각하면 이해하기 쉽다

## DFS란?

- 깊이 우선 탐색은 그래프의 시작 노드에서 출발하여 탐색할 한 쪽 분기를 정하여 최대 깊이까지 탐색을 마친 후 다른 쪽 분기로 이동하여 다시 탐색을 수행하는 알고리즘이다.

- 미로를 생각하면, 한 방향으로만 갈 수 있을 때까지 계속 가다가 더 이상 갈 수 없게 되면 뒤로 돌아 가장 가까운 갈림길로 가서 그곳에서부터 다른 방향으로 다시 탐색을 하는데, 이것이 DFS라고 생각하면 편하다.

- 즉, 넓게(wide) 탐색하기 전에 깊게(deep) 탐색하는 것이다.

## DFS 특징

- 그래프 완전 탐색 기법 중 하나이다 -> 모든 노드를 탐색하고자 할때 사용한다.

- Stack과 재귀함수를 이용하여 구현할 수 있다.

- 재귀함수를 이용하므로 스택 오버 플로에 유의해야한다.

- Stack을 사용하기 때문에 후입선출의 방식을 따른다.

- 깊이 우선 탐색을 응용하여 풀 수 있는 문제는 단절점 찾기, 단절선 찾기, 사이클 찾기, 위상 정렬 등이 있다.

## DFS 예시

![](https://velog.velcdn.com/images/kohuijae/post/e0fb9a74-1e6f-4633-b79d-842dd59ca9e2/image.png)
![](https://velog.velcdn.com/images/kohuijae/post/3c8bb8d6-74b8-4567-a1f6-0c757d7af9eb/image.png)

## DFS의 시간 복잡도

DFS의 시간 복잡도를 계산할 때는 그래프가 인접 리스트나 인접 행렬로 구현되었는지에 따라 다르다.

**노드의 수 = V, 간선의 수 = E**

1. 인접 리스트를 이용한 DFS
   인접 리스트는 각 노드마다 해당 노드에 연결된 간선들을 리스트로 저장하는 방식이다.

```
노드 1: [2, 5]
노드 2: [1, 5]
노드 3: [4]
노드 4: [3, 6]
노드 5: [1, 2]
노드 6: [4]
```

- 노드 방문: DFS는 모든 노드를 한 번씩 방문하므로, 노드 방문에 걸리는 시간은 O(V)이다.
- 간선 탐색: 각 노드에서 해당 노드와 연결된 모든 간선을 하나씩 탐색하므로, 간선을 모두 탐색하는 데 걸리는 시간은 O(E)이다. 한 번 간선을 따라가면 그 간선은 다시 탐색되지 않는다.

- 따라서 인접 리스트를 사용한 DFS의 전체 시간 복잡도는 **O(V + E)**이다.

2. 인접 행렬를 이용한 DFS
   인접 행렬은 노드 간의 연결 관계를 2차원 배열로 저장하는 방식이다. 노드 u와 노드 v가 연결되어 있으면 matrix[u][v] = 1로 저장한다.

```
    1  2  3  4  5  6
1 [ 0, 1, 0, 0, 1, 0 ]
2 [ 1, 0, 0, 0, 1, 0 ]
3 [ 0, 0, 0, 1, 0, 0 ]
4 [ 0, 0, 1, 0, 0, 1 ]
5 [ 1, 1, 0, 0, 0, 0 ]
6 [ 0, 0, 0, 1, 0, 0 ]
```

- 노드 방문: 노드를 한 번 방문하는데 걸리는 시간은 여전히 O(V)이다.
- 간선 탐색: 인접 행렬에서는 노드 간 연결 여부를 확인하려면 모든 노드 쌍을 확인해야 하므로, 각 노드마다 O(V) 시간이 소요된다.
- 따라서, 인접 행렬을 사용한 DFS의 시간 복잡도는 **O(V²)**다.

## 백준11723

![](https://velog.velcdn.com/images/kohuijae/post/59eb1fa3-ed8b-4789-bf4a-10e8fb5a87c1/image.png)

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
import java.util.Stack;

public class Main {

    static int N, M;
    static List<List<Integer>> list;
    static boolean[] visited;

    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        N = input.nextInt();
        M = input.nextInt();

        visited = new boolean[N + 1];
        list = new ArrayList<>();


        for (int i = 0; i <= N; i++) {
            list.add(new ArrayList<>());
        }

        for (int i = 1; i <= M; i++) {
            int u = input.nextInt();
            int v = input.nextInt();
            list.get(u).add(v);
            list.get(v).add(u);
        }
        /*
        [
         노드 1: [2, 5]
		 노드 2: [1, 5]
         노드 3: [4]
         노드 4: [3, 6]
         노드 5: [1, 2]
         노드 6: [4]
        ]
        */

        int count = 0;
        for (int i = 1; i <= N; i++) {
            if (!visited[i]) {
                dfsStack(i);
                count++;
            }
        }

        System.out.println(count);
    }

    //재귀를 이용한 DFS
    static void dfs(int n) {
        visited[n] = true;
        for (int i = 0; i < list.get(n).size(); i++) {
            int neighbor = list.get(n).get(i);
            if (!visited[neighbor]) {
                dfs(neighbor);
            }
        }
    }

    // 스택을 사용한 DFS
    static void dfsStack(int n) {
        Stack<Integer> stack = new Stack<>();
        stack.push(n);
        visited[n] = true;

        while (!stack.isEmpty()) {
            int current = stack.pop();

            for (int i = 0; i < list.get(current).size(); i++) {
                int neighbor = list.get(current).get(i);
                if (!visited[neighbor]) {
                    stack.push(neighbor);
                    visited[neighbor] = true;
                }
            }
        }
    }
    // 1 -> 5 -> 2
    // 3 -> 4 -> 6
}
```

재귀사용![](https://velog.velcdn.com/images/kohuijae/post/ca81147d-bc9a-4439-a118-17338f78b39a/image.png)

스택사용
![](https://velog.velcdn.com/images/kohuijae/post/e395edd3-e55e-419c-9cd9-9372562cbfdc/image.png)

# BFS : 너비 우선 탐색

## BFS (Breadth First Search) : 너비 우선 탐색

그래프 이론에서, 시작 노드에서 가까운 거리부터 먼 거리 순서로 탐색하는 방법

- 깊이=1 인 노드 탐색 → 깊이=2인 노드 탐색 …
- DFS와 동일하게 특정 노드를 탐색헀는지 확인하는 배열이 필요하다.
- DFS와 다르게 Queue 자료구조를 이용한다.
- 가중치가 없는 그래프에서, 출발 노드에서 목표 노드까지의 최단 길이 경로를 보장한다.
- 재귀적으로 시행하지 않는다.

![스크린샷 2024-10-14 18.50.42.png](BFS%20%E1%84%82%E1%85%A5%E1%84%87%E1%85%B5%20%E1%84%8B%E1%85%AE%E1%84%89%E1%85%A5%E1%86%AB%20%E1%84%90%E1%85%A1%E1%86%B7%E1%84%89%E1%85%A2%E1%86%A8%2011f0f86e463d80b3aa1de18cf13b6e07/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-10-14_18.50.42.png)

## 탐색 방법

1. 시작 노드를 탐색한다. → 큐에 시작 노드를 넣고 탐색했다고 체크한다.
2. 시작 노드에서 인접한 노드들을 모두 탐색한다. → 현재 위치에서 갈 수 있는 모든 노드를 큐에 넣는 동시에, 탐색했다고 체크한다.
3. 탐색하는 노드들마다 이에 인접한 노드들을 모두 큐에 넣고 탐색한다.
4. 모든 노드를 탐색할 때까지 반복한다.

## 시간 복잡도

![스크린샷 2024-10-14 19.17.30.png](BFS%20%E1%84%82%E1%85%A5%E1%84%87%E1%85%B5%20%E1%84%8B%E1%85%AE%E1%84%89%E1%85%A5%E1%86%AB%20%E1%84%90%E1%85%A1%E1%86%B7%E1%84%89%E1%85%A2%E1%86%A8%2011f0f86e463d80b3aa1de18cf13b6e07/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-10-14_19.17.30.png)

![스크린샷 2024-10-14 19.17.44.png](BFS%20%E1%84%82%E1%85%A5%E1%84%87%E1%85%B5%20%E1%84%8B%E1%85%AE%E1%84%89%E1%85%A5%E1%86%AB%20%E1%84%90%E1%85%A1%E1%86%B7%E1%84%89%E1%85%A2%E1%86%A8%2011f0f86e463d80b3aa1de18cf13b6e07/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-10-14_19.17.44.png)

![스크린샷 2024-10-14 19.18.12.png](BFS%20%E1%84%82%E1%85%A5%E1%84%87%E1%85%B5%20%E1%84%8B%E1%85%AE%E1%84%89%E1%85%A5%E1%86%AB%20%E1%84%90%E1%85%A1%E1%86%B7%E1%84%89%E1%85%A2%E1%86%A8%2011f0f86e463d80b3aa1de18cf13b6e07/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-10-14_19.18.12.png)

- 인접 행렬로 표현 시 $O(V^2)$ (V\*V 배열이므로 정점 V에 방문할 때마다 최대 V개의 간선을 탐색해야 함)
- 인접 리스트로 표현 시 $O(V+E)$(모든 정점 탐색 + 모든 연결된 간선 탐색)
- 2차원 미로(아래 문제)에서 $O(n*m)$ (V=n\*m, E=4)

## 문제

![스크린샷 2024-10-14 18.58.36.png](BFS%20%E1%84%82%E1%85%A5%E1%84%87%E1%85%B5%20%E1%84%8B%E1%85%AE%E1%84%89%E1%85%A5%E1%86%AB%20%E1%84%90%E1%85%A1%E1%86%B7%E1%84%89%E1%85%A2%E1%86%A8%2011f0f86e463d80b3aa1de18cf13b6e07/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-10-14_18.58.36.png)

## 알고리즘

- Queue를 이용한 bfs를 시행한다.

## 풀이방법

- 이동 가능한 방향의 경우를 쌍으로 만들어 인접 노드를 쉽게 확인할 수 있게 한다.
- 미로 주변을 1로 감싸면 더 쉽게 경계조건을 체크할 수 있고, 1부터 시작하는 인덱스 사용에도 좋다.
- Queue를 이용한 bfs를 시행한다.

## 풀이

```cpp
#include <iostream>
#include <queue>
#include <string>
using namespace std;

// 가능한 이동방향의 쌍
int mvRow[4] = {-1, 1, 0, 0};
int mvCol[4] = {0, 0, -1, 1};

void bfs(int** a, int visited[102][102], int n, int m){
    queue<pair<int, int> > q;
    // 첫 노드을 큐에 넣고 탐색했다고 체크한다.
    q.push(make_pair(n,m));
    visited[n][m] = 1;
    // 탐색할 노드가 없을 때까지 탐색한다.
    while(!q.empty()){
        int row = q.front().first;
        int col = q.front().second;
        q.pop();
        // 인접한 모든 가능한 노드들을 탐색
        for(int i=0; i<4; i++){
            int nextRow = row + mvRow[i];
            int nextCol = col + mvCol[i];
            // 이동 가능하고 아직 탐색하지 않았다면 큐에 넣고 방문했다고 체크
            if(a[nextRow][nextCol]==1 && visited[nextRow][nextCol]==0){
                q.push(make_pair(nextRow, nextCol));
                visited[nextRow][nextCol] = visited[row][col] + 1;
            }
        }
    }
}

int main(void){
    ios_base::sync_with_stdio(0);
    cin.tie(NULL);
    cout.tie(NULL);

    int n,m;
    cin >> n >> m;
    // 배열 선언
    int** maze = new int*[n+2];
    for(int i=0; i<n+2; i++){
        maze[i] = new int[m+2];
    }
    // visited 배열은 탐색했는지 체크하는 동시에, 해당 인덱스까지 오는 거리를 저장한다.
    int visited[102][102] = {0,};

    // 테두리 부분을 모두 1로 초기화(더 쉬운 조건으로 + 1-based index 이용)
    for(int i=0; i<n+2; i++){
        maze[i][0] = 0;        // 각 행의 첫 번째 열을 1로
        maze[i][m+1] = 0;      // 각 행의 마지막 열을 1로
    }
    for(int j=0; j<m+2; j++){
        maze[0][j] = 0;        // 첫 번째 행을 1로
        maze[n+1][j] = 0;      // 마지막 행을 1로
    }

    // 입력 전처리 및 배열에 넣기
    string line;
    for(int i=1; i<n+1; i++){
        cin >> line;
        for(int j=1;j<m+1;j++){
            maze[i][j] = line[j-1]-'0';
        }
    }

    // 출력
    // for(int i=0;i<n+2;i++){
    //     for(int j=0;j<m+2;j++){
    //         cout << maze[i][j];
    //     }
    //     cout << '\n';
    // }

    bfs(maze, visited, 1,1);
    cout << visited[n][m] <<'\n';

    // 메모리 해제
    for(int i=0; i<n+2; i++){
        delete[] maze[i];
    }
    delete[] maze;

}
```
