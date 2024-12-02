## 최소 신장 트리

최소 신장 트리(minimum spanning tree)란 그래프에서 모든 노드를 연결할 때 사용된 엣지들의 가중치의 합을 최소로 하는 트리 -> 크루스칼, 프림 알고리즘이 있음

### 예시

다음과 같은 도시와 길이 있다고 생각해보자

도시: A, B, C, D
길(가중치)
A-B: 1
A-C: 3
B-C: 2
B-D: 4
C-D: 5

모든 도시를 연결하는 여러 방법이 있다. 예를 들어,
A-B-C-D (가중치: 1 + 2 + 4 = 7)
A-B-D-C (가중치: 1 + 4 + 5 = 10)
이 중에서 가장 적은 가중치를 가진 연결 방법이 최소 신장 트리이다.

### 특징

- 모든 노드가 연결되어 있어야 한다.
- 끊어진 도시가 하나라도 있으면 안된다.
- 사이클이 없어야 한다.
  - 예를 들어, A-B-C-A처럼 같은 길을 돌아오는 루프는 없어야 한다.
- 가중치의 합이 최소여야 한다.
  - - 가장 적은 비용으로 연결할 수 있는 방법을 찾는 게 핵심\*\*
- 간선의 개수는 항상 N-1.
  - 도시(노드)의 개수가 N개라면, MST에 포함된 길(간선)은 항상 N-1개이다.

## 최소 신장 트리 과정

1. 엣지 리스트로 그래프를 구현, 유니온 파인드 리스트 초기화하기
   왜 유니온 파인드가 쓰이는가? -> 싸이클 판별을 위해서
2. 엣지 리스트에 담긴 그래프 데이터를 가중치 기준으로 오름차순 정렬을 한다.
3. 가중치가 낮은 엣지부터 연결을 시도한다.
   두 노드의 대표 노드가 다를 경우에만 연결 -> 대표 노드가 같다면 두 노드를 연결했을 때 싸이클이 만들어진다.
4. 과정3 반복
5. 엣지의 개수가 N-1개가 되면 종료하고 총 엣지 비용 출력

!https://velog.velcdn.com/images/kohuijae/post/1539814c-46df-4db7-b5b6-ea11021d9d01/image.png

!https://velog.velcdn.com/images/kohuijae/post/e49a7566-47d7-463d-8894-1bb85c1e2f9d/image.png

!https://velog.velcdn.com/images/kohuijae/post/03a20a8c-cf32-4e13-ac16-4fbc04445ffb/image.png

!https://velog.velcdn.com/images/kohuijae/post/6d23003d-1d0b-4213-9a44-2fed09b7fa5c/image.png

!https://velog.velcdn.com/images/kohuijae/post/8e0f5d58-74b2-405e-94a6-1e7581b050d2/image.png

!https://velog.velcdn.com/images/kohuijae/post/86768dbb-666b-440e-bb55-785e3edcd5a5/image.png

!https://velog.velcdn.com/images/kohuijae/post/bb90603a-ff7c-43eb-81ec-5c24326433b5/image.png

!https://velog.velcdn.com/images/kohuijae/post/3b312d9d-1180-47df-b5e2-f36f4df6a58c/image.png

## 백준 1197

!https://velog.velcdn.com/images/kohuijae/post/4987e97a-293b-4929-9536-83669cbae81f/image.png

```java
import java.io.*;
import java.util.*;

public class Main {
	//엣지리스트 생성
    static int[][] graph;
    //부모 노드
    static int[] parent;
    //최종값
    static int total;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int V = Integer.parseInt(st.nextToken());
        int E = Integer.parseInt(st.nextToken());

        graph = new int[E][3];
        for(int i=0; i<E; i++){
            st = new StringTokenizer(br.readLine());
            graph[i][0] = Integer.parseInt(st.nextToken());
            graph[i][1] = Integer.parseInt(st.nextToken());
            graph[i][2] = Integer.parseInt(st.nextToken());
        }
        Arrays.sort(graph, Comparator.comparingInt(o -> o[2]));

        parent = new int[V+1];
        for(int i=1; i<V+1; i++){
            parent[i] = i;
        }

        kruskal(graph, parent);
    }

    static void kruskal(int[][] graph, int[] parent){
        total = 0;
        for(int i=0; i<graph.length; i++){
            if(find(parent, graph[i][0]) != find(parent, graph[i][1])){
                total += graph[i][2];
                union(parent, graph[i][0], graph[i][1]);
            }
        }
        System.out.println(total);
    }

    static void union(int[] parent, int i, int j){
        i = find(parent, i);
        j = find(parent, j);
        if(i<j){
            parent[j] = i;
        }else{
            parent[i] = j;
        }
    }

    static int find(int[] parent, int i){
        if(parent[i] != i){
            parent[i] = find(parent, parent[i]);
        }
        return parent[i];
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

## `플로이드 워셜`

### 플로이드-워셜(Floyd-Warshall) ?

- **각각의 모든 노드에 대하여 다른 모든 노드로의 최단 거리**를 구할 수 있는 알고리즘 !
  ↔ 특정 시작점에서 다른 모든 노드로의 최단 거리를 구하는 `다익스트라`, `벨만-포드` 알고리즘

- **음수 가중치를 갖는 간선**이 있어도 수행 가능하나, `음수 사이클`이 있는 경우엔 **사용할 수 없다.**

---

### 💡 플로이드 워셜 핵심 개념 - `동적 계획법(DP)`

- `DP`는 하나의 큰 문제를 여러 개의 작은 문제로 나눈 후에, **그 결과를 저장**하여 다시 **큰 문제를 해결할 때 사용**하기 위한 알고리즘이다 !
  → **`기억하며 풀기**(Memoization)`    1.`메모이제이션`
  : 변수 값에 따른 결과를 저장할 배열 등을 미리 만들고, 그 결과를 나올 때마다 배열 내에 저장하고 그 **저장된 값을 재사용**하는 방식으로 문제를 해결한다.
      2. `점화식 만들기`
      : 변수 간의 관계식을 만드는 것이라고 보면 된다.
      ( ex: 피보나치 수열 `f(n) = f(n-1) + f(n-2)` )

<aside>
💡

`DP를 적용시킬 수 있는 조건` ?

1. `중복되는 부분 문제 (Overlapping Sub-problems)`

- `DP`는 기본적으로 문제를 나누고 그 문제의 결과 값을 재활용해서 전체 답을 구하기 때문에, **동일한 작은 문제들이 반복하여 나타나는 경우**에 사용이 가능한 것 !

1. `최적 부분 구조 (Optimal Substructure)`

- 부분 문제의 최적 결과 값을 사용해 **전체 문제의 `최적 결과`를 낼 수 있는 경우** 사용이 가능하다 !
</aside>

- DP 문제 해결 방식
  - `Bottom-Up` → 반복문 사용(`최적 부분 구조` 보장)
  - `Top-Down` → 재귀 사용(`Memoization` 활용)

---

### 💡 플로이드 와샬 - `구현`

- `시간 복잡도`: `O(V^3)`
  → 노드 수(V)가 작을 경우 효율적이다.
  ( 일반적으로 V ≤ 500 정도가 적합하다고 한다. )

- 2차원 배열에 모든 노드에 대하여 다른 모든 노드로의 최단 거리를 저장한다.
  → 중간 노드를 고려하여 경로를 **점진적으로 최적화**해나간다.
  ( `dist[i][j]` : 노드 **i**에서 노드 **j**로 가는 최단 경로의 비용 )

      > `점화식`
      >
      >
      > $$
      > dist[i][j] = \min(dist[i][j], dist[i][k] + dist[k][j])
      > $$
      >

      1. 배열(`dist[i][j]`)을 선언하고 초기화 한다.
      → `i == j`인 경우엔 **최단 경로를 0**으로, 이외의 경우엔 **INF**로 초기화한다.

      2. 출발 노드는 u, 도착 노드는 v, 이 간선의 가중치는 w라고 했을 때, `dist[u][v] = Math.min(dist[u][v], w)`로 주어진 그래프 정보를 업데이트한다.
      ( `min`으로 중복 간선 처리 )

      3. `DP`를 이용해 **작은 문제(경로에 중간 노드 포함)를 점진적으로 해결**해나간다.

- `다익스트라`, `벨만-포드` 알고리즘에서는 `INFINITY`값을 `Integer.MAX_VALUE`로 초기화했지만, `플로이드-워셜` 알고리즘에선 `INF` 값에 어떤 수를 더한 값을 비교하게 될 수 있기 때문에 **10억 정도의 적당히 큰 수**로 초기화 한다.

```java
import java.util.*;
import java.io.*;

public class FloydWarshall {
    static final int INF = 100000000; // 무한대 값
    static int[][] dist; // 최단 거리 저장 배열
    static int n, m; // 노드 수, 간선 수

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;

        // 노드 수와 간선 수 입력
        st = new StringTokenizer(br.readLine());
        n = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(st.nextToken());

        // 최단 거리 배열 초기화
        dist = new int[n + 1][n + 1];
        for (int i = 1; i <= n; i++) {
            Arrays.fill(dist[i], INF);
            dist[i][i] = 0; // 자기 자신으로 가는 비용은 0
        }

        // 간선 정보 입력 및 초기화
        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(br.readLine());
            int u = Integer.parseInt(st.nextToken());
            int v = Integer.parseInt(st.nextToken());
            int w = Integer.parseInt(st.nextToken()); // 가중치
            dist[u][v] = Math.min(dist[u][v], w); // 중복 간선 처리
        }

        // 플로이드-워셜 알고리즘 수행
        floydWarshall();
    }

    public static void floydWarshall() {
        for (int k = 1; k <= n; k++) { // 중간 노드
            for (int i = 1; i <= n; i++) { // 시작 노드
                for (int j = 1; j <= n; j++) { // 도착 노드
                    if (dist[i][k] != INF && dist[k][j] != INF) { // 경로가 존재하는 경우
                        dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
                    }
                }
            }
        }
    }
}
```

---

## **문제: `운동` (백준 1956)**

### 문제 링크

[](https://www.acmicpc.net/problem/1956)

---

### 알고리즘 [접근 방법]

- 주어진 간선 정보들로부터 **가중치 값의 합이 가장 작은 사이클**을 찾는 문제이기 때문에, 모든 노드 쌍의 최단 거리를 계산해야 할 필요가 있기에 `플로이드-워셜` 알고리즘을 사용하면 된다.

---

### 해결 코드

- 이 문제는 주어진 그래프에 대해 플로이드-워셜 알고리즘을 수행하고, `dist[i][i]`의 값들 중에 가장 작은 값을 출력하면 된다.
  → 일반적인 경우(자기 자신으로 가는 간선 X)에 **자기 자신으로 가는 가중치 값이 0으로 초기화**되지만, 이 문제에선 **자기 자신으로 가는 가중치 값이 0으로 초기화해선 안된다 !**

```java
import java.util.*;
import java.io.*;

public class Main {

    static final int INF = 100000000;
    static int[][] dist;
    static int V, E;

    public static void main(String[] args) throws Exception {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(st.nextToken());

        // 최단 거리 배열 초기화
        dist = new int[V + 1][V + 1];
        for (int i = 1; i <= V; ++i) {
            Arrays.fill(dist[i], INF);
            // dist[i][i] = 0; // 이 문제에선 자기 자신으로 가는 비용이 0이 아닐 수 있다.
        }

        // 간선 정보 입력 및 초기화
        for (int i = 0; i < E; i++) {
            st = new StringTokenizer(br.readLine());
            int u = Integer.parseInt(st.nextToken());
            int v = Integer.parseInt(st.nextToken());
            int w = Integer.parseInt(st.nextToken());
            dist[u][v] = Math.min(dist[u][v], w); // 중복 간선 처리
        }

        floydWarshall();

        // 최소 사이클 찾기
        int minCycle = INF;
        for (int i = 1; i <= V; i++) {
            if (dist[i][i] < minCycle) {
                minCycle = dist[i][i];
            }
        }

        // 결과 출력
        if (minCycle == INF) {
            System.out.print("-1");
        } else {
            System.out.print(minCycle);
        }
    }

    public static void floydWarshall() {
        for (int k = 1; k <= V; ++k) {
            for (int i = 1; i <= V; ++i) {
                for (int j = 1; j <= V; ++j) {
                    if (dist[i][k] != INF && dist[k][j] != INF) { // 중간 경로가 존재하는 경우
                        dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
                    }
                }
            }
        }
    }
}
```

---

### 정리

- **각각의 모든 노드에 대하여 다른 모든 노드로의 최단 거리**를 구해야 하는 경우엔 `플로이드-워셜` 알고리즘을 사용하자 !
  ( ex: 최소 가중치값의 합을 갖는 사이클을 찾아야 하는 경우 )
- 이 알고리즘을 사용할 땐 `음수 가중치`가 있어도 괜찮지만, `음수 사이클`이 존재해선 안된다 !