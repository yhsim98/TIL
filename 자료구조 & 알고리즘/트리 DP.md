# 트리 DP
특정한 i번째 노드를 루트로 하는 서브 트리에 대해 i 번째 루트 노드를 포함했을 때와 그렇지 않았을 때 조건에 맞는 답을 정의

기본적으로 Top-down 방식으로 해결하며, 보통 leaf node 부터 값을 알아낸다. DFS 로 leaf node 까지 순회한 뒤, 값을 알아내어 그 다음 노드들의 값들을 알아내는 방식으로 해결한다.

```
void dfs(tree, dp[][], node){
    if(node == leaf){
        dp[node][0] = ?
        dp[node][1] = ?
    }

    dp[node][0] = min(dp[child][1] + dp[child][0]);
    dp[node][1] = dp[child][0] + 1;
}
```

이런 느낌이다.

다음은 백준의 트리 dp 를 사용한 [사회망 서비스](https://www.acmicpc.net/problem/2533) 의 코드이다.

```
class Solve {

    static BufferedReader br = Main.br;
    static BufferedWriter bw = Main.bw;
    static StringTokenizer st;

    private int n;
    private int m;

    public void solve() throws IOException {
        st = new StringTokenizer(br.readLine(), " ");
        n = Integer.valueOf(st.nextToken());

        ArrayList<ArrayList<Integer>> relation = new ArrayList<>(n + 1);

        for(int i = 0; i <= n; i++){
            relation.add(new ArrayList<>());
        }

        for(int i = 1; i < n; i++){
            st = new StringTokenizer(br.readLine(), " ");
            int node1 = Integer.valueOf(st.nextToken());
            int node2 = Integer.valueOf(st.nextToken());

            relation.get(node1).add(node2);
            relation.get(node2).add(node1);
        }

        int[][] dp = new int[n + 1][2];

        dfs(relation, 1, -1, dp);

        System.out.println(Math.min(dp[1][0], dp[1][1]));
    }

    // 본인이 얼리어답터인 경우 0, 와 그렇지 않은 경우 1 dp[node][1] = dp[자식][0] + dp[자식][0], dp[node][0] = min(dp[자식들][0], dp[자식들][1]
    private void dfs(ArrayList<ArrayList<Integer>> relation, int node, int parent, int[][] dp){

        for(int i = 0; i < relation.get(node).size(); i++){
            int next = relation.get(node).get(i);
            if(next != parent) {
                dfs(relation, next, node, dp);
                dp[node][1] += dp[next][0];
                dp[node][0] += Math.min(dp[next][0], dp[next][1]);
            }
        }

        dp[node][0] += 1;
    }
}
```