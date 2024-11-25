### 위상정렬이란?

위상정렬(Topological Sort)는 그래프와 관련된 알고리즘 중 하나로 선후관계가 정의된 그래프 구조에서 정렬하기 위해 사용한다.
1. 선후관계과 정의되어야 한다.
2. 그래프가 순환하지 않아야 한다.(DAG, Directed Acyclic Graph)

![](https://velog.velcdn.com/images/rlagurwns112/post/388fffb1-6c94-474d-955b-e44cbd6cc5c6/image.png)

선후관계를 따진다면 이렇게 작성할 수 있다.

![](https://velog.velcdn.com/images/rlagurwns112/post/f330e88d-52b0-4b42-9982-5647876f6aec/image.png)

이때 노드를 방문하는 순서를 찾아야 한다면 위상 정렬을 사용할 수 있다. 참고로 결과 값은 여러 가지가 존재할 수 있다.

위상 정렬은 큐를 사용해서 구현한다.

1. 그래프의 각 노드들의 진입 차수 테이블 생성 및 진입 차수 계산
![](https://velog.velcdn.com/images/rlagurwns112/post/41127c26-ae4d-4f23-87d2-12ef1a7c07d3/image.png)

2. 진입 차수가 0인 노드 큐에 넣기 (이때 어떤 노드 먼저 시작하던지 관계없음)
![](https://velog.velcdn.com/images/rlagurwns112/post/751692d6-17b1-4be9-ab4e-e60352421088/image.png)

3. 큐에서 노드를 하나 꺼낸 후 꺼낸 노드와 간선으로 연결된 노드들의 진입 차수 감소 (진입 차수 테이블 갱신)
4. 진입 차수 테이블을 갱신 후 진입 차수의 값이 0인 노드가 있다면 큐에 넣기 (없으면 아무것도 안 함)
5. 3~4번의 순서를 큐에 더 이상 아무것도 없을 때까지 반복
![](https://velog.velcdn.com/images/rlagurwns112/post/215eb100-2a78-4c47-9328-bdfb508bf63a/image.png)
![](https://velog.velcdn.com/images/rlagurwns112/post/e1abd64d-6fbf-4dc8-86f9-fd4a4bcced04/image.png)
![](https://velog.velcdn.com/images/rlagurwns112/post/40de62b4-b789-4dc0-9e0b-0ef4503cef9c/image.png)
![](https://velog.velcdn.com/images/rlagurwns112/post/cf79507c-50e5-4666-9a06-6d7fa4365f6d/image.png)
![](https://velog.velcdn.com/images/rlagurwns112/post/4e7ad99d-1f80-472a-a89d-6f2421f4404d/image.png)
![](https://velog.velcdn.com/images/rlagurwns112/post/650f5ee7-1803-46d9-b66d-a1c2c96dfe89/image.png)
![](https://velog.velcdn.com/images/rlagurwns112/post/8cdc50fd-ec13-4490-87b6-1079bd5af1ff/image.png)
![](https://velog.velcdn.com/images/rlagurwns112/post/5ae49631-e415-4921-847f-4716ad29bf52/image.png)

만약 모든 순서가 끝났는데 진입 차수가 0이 아닌 노드가 있다면 그래프 내에서 순환이 존재한다고 볼 수 있다.
위의 그림을 따라서 본다면 결국 1, 7, 4, 3, 2, 8, 5, 6 순으로 정렬된다.

#### 자바 코드로 구현하기

```
import java.util.*;

public class Main {
    static int V, E; // V: 정점 개수, E: 간선 개수
    static ArrayList<ArrayList<Integer>> graph; // 그래프를 저장할 인접 리스트
    static int[] inDegree; // 각 정점의 진입차수를 저장할 배열
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        // 정점의 개수 V와 간선의 개수 E 입력
        V = sc.nextInt();
        E = sc.nextInt();
        
        // 그래프 초기화
        graph = new ArrayList<>();
        for (int i = 0; i <= V; i++) {
            graph.add(new ArrayList<>());
        }
        
        // 진입차수 배열 초기화
        inDegree = new int[V + 1];
        
        // 간선 정보 입력
        // A -> B 형태로 입력 (A를 선행으로 하는 B)
        for (int i = 0; i < E; i++) {
            int A = sc.nextInt();
            int B = sc.nextInt();
            graph.get(A).add(B);     // A에서 B로 가는 간선 추가
            inDegree[B]++;           // B의 진입차수 증가
        }
        
        // 위상정렬 수행
        topologicalSort();
    }
    
    // 위상정렬 함수
    static void topologicalSort() {
        Queue<Integer> queue = new LinkedList<>(); // 진입차수가 0인 정점을 저장할 큐
        ArrayList<Integer> result = new ArrayList<>(); // 위상정렬 결과를 저장할 리스트
        
        // 진입차수가 0인 정점을 큐에 삽입
        for (int i = 1; i <= V; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        // 큐가 빌 때까지 반복
        while (!queue.isEmpty()) {
            int current = queue.poll(); // 큐에서 정점 추출
            result.add(current);        // 결과 리스트에 추가
            
            // 현재 정점과 연결된 모든 정점들의 진입차수를 1 감소
            for (int next : graph.get(current)) {
                inDegree[next]--;
                
                // 진입차수가 0이 된 정점을 큐에 삽입
                if (inDegree[next] == 0) {
                    queue.offer(next);
                }
            }
        }
        
        // 위상정렬 결과 출력
        // 모든 정점을 방문하지 못했다면 사이클이 존재한다는 의미
        if (result.size() != V) {
            System.out.println("사이클이 존재합니다.");
            return;
        }
        
        // 위상정렬 결과 출력
        for (int vertex : result) {
            System.out.print(vertex + " ");
        }
    }
}
```


### 문제

[백준 2252](https://www.acmicpc.net/problem/2252)

![](https://velog.velcdn.com/images/rlagurwns112/post/8cfee144-9476-4899-937b-9b898bffb605/image.png)

---

### 알고리즘 [접근 방법]

학생들 간의 키 비교는 방향이 있는 그래프로 표현할 수 있다. A학생이 B학생보다 앞에 서야 한다는 것은 A->B로 표현된다. 선후관계가 있는 작업을 순서대로 나열하므로 위상정렬이 적합하다.

---

### 풀이방법

1. 각 학생을 노드, 키 비교 정보를 간선으로 하는 방향 그래프를 만든다.
2. 각 노드의 진입차수를 계싼한다.
3. 진입차수가 0인 학생부터 위상정렬을 수행한다.
4. 큐를 이용하여 순서대로 학생들을 나열한다.

---

### 풀이

```
import java.util.*;
import java.io.*;

public class Main {
    static int N, M;
    static ArrayList<ArrayList<Integer>> graph;
    static int[] inDegree;
    
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        
        // N: 학생 수, M: 키 비교 횟수
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        
        // 그래프 초기화
        graph = new ArrayList<>();
        for(int i = 0; i <= N; i++) {
            graph.add(new ArrayList<>());
        }
        
        // 진입차수 배열 초기화
        inDegree = new int[N + 1];
        
        // 키 비교 정보 입력
        for(int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int A = Integer.parseInt(st.nextToken());
            int B = Integer.parseInt(st.nextToken());
            graph.get(A).add(B);     // A가 B앞에 서야 함
            inDegree[B]++;           // B의 진입차수 증가
        }
        
        // 위상정렬 수행
        topologicalSort();
    }
    
    static void topologicalSort() {
        Queue<Integer> queue = new LinkedList<>();
        StringBuilder sb = new StringBuilder();
        
        // 진입차수가 0인 학생을 큐에 삽입
        for(int i = 1; i <= N; i++) {
            if(inDegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        // 큐가 빌 때까지 반복
        while(!queue.isEmpty()) {
            int current = queue.poll();
            sb.append(current).append(" ");
            
            // 현재 학생과 연결된 다음 학생들의 진입차수 감소
            for(int next : graph.get(current)) {
                inDegree[next]--;
                // 진입차수가 0이 되면 큐에 삽입
                if(inDegree[next] == 0) {
                    queue.offer(next);
                }
            }
        }
        
        System.out.println(sb);
    }
}
```
```
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int N, M; // N: 학생 수, M: 키 비교 횟수
vector<vector<int>> graph; // 인접 리스트로 구현한 그래프
vector<int> inDegree; // 진입차수 저장 배열

void topologicalSort() {
    queue<int> q;
    
    // 진입차수가 0인 노드를 큐에 삽입
    for(int i = 1; i <= N; i++) {
        if(inDegree[i] == 0) {
            q.push(i);
        }
    }
    
    // 큐가 빌 때까지 반복
    while(!q.empty()) {
        int cur = q.front();
        q.pop();
        
        cout << cur << " "; // 현재 노드 출력
        
        // 현재 노드와 연결된 모든 노드들의 진입차수를 1 감소
        for(int next : graph[cur]) {
            inDegree[next]--;
            // 진입차수가 0이 된 노드를 큐에 삽입
            if(inDegree[next] == 0) {
                q.push(next);
            }
        }
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
    
    // 입력 받기
    cin >> N >> M;
    
    // 그래프와 진입차수 배열 초기화
    graph.resize(N + 1);
    inDegree.resize(N + 1, 0);
    
    // 키 비교 정보 입력
    for(int i = 0; i < M; i++) {
        int A, B;
        cin >> A >> B;
        graph[A].push_back(B); // A -> B 방향 간선 추가
        inDegree[B]++; // B의 진입차수 증가
    }
    
    // 위상정렬 수행
    topologicalSort();
    
    return 0;
}
```
---

### 정리

순서가 있는 그래프(방향 그래프)이고 순환관계가 없으면 위상정렬을 고려해보자.
시간 복잡도는 O(V+E)이다.
참고로 여러 가지 답이 가능할 수 있으며 모든 학생의 순서를 정확히 알 수 없어도 된다는 사실을 인지하자.

# UnionFind

# UnionFind란?

- 서로소 집합 간의 합집합을 만드는 Union 연산과 노드의 루트 노드를 찾는 Find 연산
- ppt 참고 설명

## 문제

[유니온 파인드 실전 문제] 집합 표현하기 (백준 1717)

## 알고리즘

- UnionFind 알고리즘을 사용한다.

## SimpleUnion

```
function simpleUnion(x, y, parent):
    rootX = simpleFind(x, parent)
    rootY = simpleFind(y, parent)
    if rootX != rootY:
        parent[rootX] = rootY
```

정말 단순히 다른 트리의 루트를 갖다 붙이는 것이다.

## SimpleFind

```
function simpleFind(x, parent):
    if parent[x] == x:
        return x
    else:
        return simpleFind(parent[x], parent)
```

단순히 재귀호출로 부모 노드를 찾는 것이다. parent배열에는 각 노드의 부모 노드가 저장되어 있다.

만약 root node라면 자기 자신이 저장되어 있다.

## Union by Rank

```
function enhancedUnion(x, y, parent, rank):
    rootX = enhancedFind(x, parent)
    rootY = enhancedFind(y, parent)

    if rootX != rootY:
        if rank[rootX] < rank[rootY]:
            parent[rootX] = rootY
        else if rank[rootX] > rank[rootY]:
            parent[rootY] = rootX
        else:
            parent[rootY] = rootX
            rank[rootX] = rank[rootX] + 1
```

트리의 최대 높이를 저장하는 rank 배열을 통해, 높이가 더 낮은 트리가 높이가 더 높은 트리에 붙어서 합쳐진 트리의 깊이가 깊어지는 것을 최대한 막는다.

## Path Compression

```
function enhancedFind(x, parent):
    if parent[x] != x:
        parent[x] = enhancedFind(parent[x], parent)
    return parent[x]
```

find 연산 실행 시, 노드를 탐색할 때 루트를 찾아가며 결국 경유하는 노드가 직접 루트 노드를 가리키게 하여 트리의 깊이를 줄인다.

## 풀이방법

- UnionFind 클래스에서 Union과 Find 연산을 구현한다.
- 문제 조건에 맞게 출력한다.

## 풀이

```cpp
#include <iostream>
#include <vector>

using namespace std;

class UnionFind{
    public:
    // 각 노드의 부모를 저장하는 배열과 트리의 높이의 상한을 저장하는 배열
    vector<int> parent, rank;
    UnionFind(int n){
        parent.resize(n); // n개의 집합이 있다
        rank.resize(n, 1); // 각 트리의 높이는 처음에는 모두 1
        for(int i=0; i<n; i++){
            parent[i] = i; // 처음에는 각자의 부모가 자기 자신
        }
    }
    int Find(int s){
        if(parent[s] != s){
            parent[s] = Find(parent[s]); // 루트 노드를 찾기 위한 과정, 경로압축
        }
        return parent[s];
    }

    // 노드 수가 적은 트리가 더 많은 트리에 합쳐짐(그래야 높이를 줄일 수 있음 : weighting rule)
    bool Union(int a, int b){
        int root_a = Find(a);
        int root_b = Find(b);

        // 서로 같은 트리에 있지 않아야 합집합 가능
        if(root_a != root_b){
            if(rank[root_a] > rank[root_b]){
                parent[root_b] = root_a;
            }else if(rank[root_b] > rank[root_a]){
                parent[root_a] = root_b;
            }else{
                parent[root_a] = root_b; // 상한 높이가 같으면 임의로 집어넣음
                rank[root_b]++; // 트리의 높이가 같으면 합쳐진 트리의 높이가 1 증가할 수밖에 없음
            }
            return true;
        }
        return false;
    }

};

int main(void){
    ios_base::sync_with_stdio(0);
    cin.tie(NULL);
    cout.tie(NULL);

    // 입력
    int n,m;
    cin >> n >> m;
    // 초기화
    UnionFind U = UnionFind(n+1);
    int flag, a, b;

    for(int i=0; i<m; i++){
        cin >> flag >> a >> b;
        if(flag){
            // 1 a b : FIND
            (U.Find(a) == U.Find(b)) ? cout<<"YES\n" : cout <<"NO\n";
        }else{
            U.Union(a,b);
        }
    }

}
```

✏️ 다익스트라 알고리즘?
그래프에서 최단 거리를 구하는 알고리즘.
단일 시작점 알고리즘.
출발 지점 하나를 선택해 나머지 모든 지점까지의 최단 경로를 찾는 유형이다.
그리디 알고리즘 중 하나로, 각 단계에서 최단거리의 지점을 선택한다.


⚙️ 동작 과정
다익스트라 알고리즘의 동작 방식을 단계별로 나누어서 차례대로 살펴보자.

1. 인접 리스트로 그래프 구현하기
A -> [(H, 2), (E, 5)]
H -> [(A, 2), (F, 3), (E, 8)]
E -> [(A, 5), (H, 8), (F, 20)]
F -> [(H, 3), (C, 1)]
C -> [(F, 1)]
2. 최단 거리 배열 초기화하기
출발 노드는 거리가 없으므로 0을 초기화한다.
나머지 노드는 무한(INF)로 초기화한다.
3. 값이 가장 작은 노드 선택
최단 거리 배열에서 현재 값이 가장 작은 노드를 선택한다.
처음 시작할 때는 값인 0인 출발 노드를 선택하게 된다.
4. 최단 거리 배열 갱신
선택한 노드의 간선의 값을 바탕으로 다른 노드의 거리 값을 갱신한다.
⏰ 시간 복잡도
다익스트라 알고리즘의 시간 복잡도는 O(V) x {O(V) + O(V)} = O(V²) 이다.
(1) 모든 노드 V개에 대해 3번, 4번을 반복한다 : O(V)
(2) 값이 가장 작은 노드를 선택하는 시간 : O(V)
(3) 최단 거리 배열을 갱신하는 시간 : O(V)
💻 코드
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.StringTokenizer;

public class Main {

    static final int INF = Integer.MAX_VALUE;

    static int V, E, K; // 정점, 간선, 시작 지점
    static int[] distances; // 최단 거리 배열
    static ArrayList<Node>[] graph; // 인접 리스트 그래프
    static boolean[] visited; // 방문 여부 배열

    static class Node {
        int end;
        int distance;

        public Node(int end, int distance) {
            this.end = end;
            this.distance = distance;
        }
    }

    public void dijkstra(int K) {
        distances[K] = 0;

        for (int i = 1; i <= V; i++) {
            int minNode = -1;
            int minDistance = INF;

            // 아직 방문하기 않은 노드 중에 최단 거리의 노드 구하기
            for (int j = 1; j <= V; j++) {
                if (!visited[j] && distances[j] < minDistance) {
                    minNode = j;
                    minDistance = distances[j];
                }
            }

            if (minNode == -1) { // 방문 가능한 노드가 없는 경우
                break;
            }
            visited[minNode] = true;

            // 선택한 노드와 연결된 노드드르이 거리 갱신
            for (Node node : graph[minNode]) {
                int newNode = node.end;
                int newDistance = distances[minNode] + node.distance;

                if (newDistance < distances[newNode]) {
                    distances[newNode] = newDistance;
                }
            }
        }
    }
}

📕 문제


주어진 시작점에서 다른 모든 정점으로의 최단 경로를 구하는 프로그램을 작성하시오.
(모든 간선의 가중치는 10 이하의 자연수이다.)

Input

첫째 줄에 정점의 개수 V와 간선의 개수 E가 주어진다.
(1 ≤ V ≤ 20,000, 1 ≤ E ≤ 300,000)
모든 정점에는 1부터 V까지 번호가 매겨져 있다고 가정한다.
둘째 줄에는 시작 정점의 번호 K(1 ≤ K ≤ V)가 주어진다.
셋째 줄부터 E개의 줄에 걸쳐 각 간선을 나타내는 세 개의 정수 (u, v, w)가 주어진다.
이는 u에서 v로 가는 가중치 w인 간선이 존재한다는 뜻이다.
u와 v는 서로 다르며 w는 10 이하의 자연수이다.
서로 다른 두 정점 사이에 여러 개의 간선이 존재할 수도 있음에 유의한다.

Output

첫째 줄부터 V개의 줄에 걸쳐, i번째 줄에 i번 정점으로의 최단 경로의 경로값을 출력한다.
시작점 자신은 0으로 출력하고, 경로가 존재하지 않는 경우에는 INF를 출력하면 된다.

💡 접근 방법
주어진 시작점에서 시작해 모든 정점과의 경로를 구하는 문제이다.
따라서, 그리디 알고리즘 중 하나인 다익스트라 알고리즘을 활용하기에 적합한 문제로 보인다.

🌱 다익스트라 알고리즘
다익스트라 알고리즘이 처음이거나 익숙하지 않다면 아래의 링크를 통해 살펴보고 오자.

📎 다익스트라 알고리즘

💻 코드
방법(1)

package week06;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.StringTokenizer;

public class quiz03_1753_1 {

    /**
     * 6주차 개념 : 다익스트라 알고리즘
     * 백준 1753 - 방문 배열 활용
     */

    static final int INF = Integer.MAX_VALUE;

    static int V, E, K;
    static int[] distances;
    static ArrayList<Node>[] graph;
    static boolean[] visited;

    static class Node implements Comparable<Node> {
        int end;
        int distance;

        public Node(int end, int distance) {
            this.end = end;
            this.distance = distance;
        }

        @Override
        public int compareTo(Node other) {
            return Integer.compare(this.distance, other.distance);
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        // 변수 선언 및 초기화
        V = Integer.parseInt(st.nextToken()); // 정점 개수 V
        E = Integer.parseInt(st.nextToken()); // 간선 개수 E
        K = Integer.parseInt(br.readLine());  // 시작 노드 K

        visited = new boolean[V + 1];
        distances = new int[V + 1];
        graph = new ArrayList[V + 1];

        // 최소 거리 배열 및 그래프 생성
        for (int i = 1; i <= V; i++) {
            distances[i] = INF;
            graph[i] = new ArrayList<>();
        }

        // 정점 개수 E 만큼 반복해서 연결 정보 초기화
        for (int i = 1; i <= E; i++) {
            st = new StringTokenizer(br.readLine());
            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            int distance = Integer.parseInt(st.nextToken());

            graph[start].add(new Node(end, distance));
        }

        // 다익스트라 실행
        dijkstra();

        // 결과 출력
        for (int i = 1; i <= V; i++) {
            System.out.println(distances[i] == Integer.MAX_VALUE ? "INF" : distances[i]);
        }
    }

    private static void dijkstra() {
        distances[K] = 0;

        for (int i = 1; i <= V; i++) {
            int minNode = -1;
            int minDistance = INF;

            // 아직 방문하기 않은 노드 중에 최단 거리의 노드 구하기
            for (int j = 1; j <= V; j++) {
                if (!visited[j] && distances[j] < minDistance) {
                    minNode = j;
                    minDistance = distances[j];
                }
            }

            if (minNode == -1) { // 방문 가능한 노드가 없는 경우
                break;
            }
            visited[minNode] = true;

            // 선택한 노드와 연결된 노드드르이 거리 갱신
            for (Node node : graph[minNode]) {
                int newNode = node.end;
                int newDistance = distances[minNode] + node.distance;

                if (newDistance < distances[newNode]) {
                    distances[newNode] = newDistance;
                }
            }
        }
    }
}
방법(2)

package week06;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class quiz03_1753_2 {

    /**
     * 6주차 개념 : 다익스트라 알고리즘
     * 백준 1753 - 우선순위 큐 활용
     */

    static final int INF = Integer.MAX_VALUE;

    static int V, E, K;
    static int[] distances; // 시작 지점에서 각 노드의 최단 거리 저장
    static ArrayList<Node>[] graph; // 각 노드의 연결 정보

    static class Node implements Comparable<Node> {
        int end;
        int distance;

        public Node(int end, int distance) {
            this.end = end;
            this.distance = distance;
        }

        @Override
        public int compareTo(Node other) { // 우선순위 큐 정렬 기준 지정 : 거리를 기준으로 오름차순
            return Integer.compare(this.distance, other.distance);
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        // 변수 선언 및 초기화
        V = Integer.parseInt(st.nextToken()); // 정점 개수 V
        E = Integer.parseInt(st.nextToken()); // 간선 개수 E
        K = Integer.parseInt(br.readLine());  // 시작 노드 K

        distances = new int[V + 1];
        graph = new ArrayList[V + 1];

        // 최소 거리 배열 및 그래프 생성
        for (int i = 1; i <= V; i++) {
            distances[i] = INF;
            graph[i] = new ArrayList<>();
        }

        // 정점 개수 E 만큼 반복해서 연결 정보 초기화
        for (int i = 1; i <= E; i++) {
            st = new StringTokenizer(br.readLine());
            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            int distance = Integer.parseInt(st.nextToken());

            graph[start].add(new Node(end, distance));
        }

        // 다익스트라 실행
        dijkstra();

        // 결과 출력
        for (int i = 1; i <= V; i++) {
            System.out.println(distances[i] == Integer.MAX_VALUE ? "INF" : distances[i]);
        }
    }

    private static void dijkstra() {
        distances[K] = 0;
        PriorityQueue<Node> queue = new PriorityQueue<>();
        queue.add(new Node(K, 0));

        while (!queue.isEmpty()) {
            Node cur = queue.poll();
            int curNode = cur.end;
            int curDistance = cur.distance;

            if (curDistance > distances[curNode]) {
                continue;
            }

            for (Node node : graph[curNode]) {
                int newNode = node.end;
                int newDistance = curDistance + node.distance;

                if (newDistance < distances[newNode]) {
                    distances[newNode] = newDistance;
                    queue.add(new Node(newNode, newDistance));
                }
            }
        }
    }
}