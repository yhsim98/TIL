# KMP
문자열 매칭 알고리즘으로써 무려 O(n+m)에 작동하는 굉장히 호율적인 알고리즘이다. 

이해하는데 6시간은 걸린거 같다.... 다른 사람에게 설명하래도 힘들 것 같으니 나만이라도 알아볼 수 있도록 기록해보자.

KMP알고리즘의 기본 개념은 문자열을 비교해보다 틀리면 다음 문자부터 확인하는 것이 아니라 미리 기록한 실패함수라는 것을 가지고 더 효율적으로 비교해 보는 것이다. 

예를 들면 문자열 kkkkabcabdkkk가 있을 때, abcabc인 문자열 s가 있는 지 확인하려 할때 abcab까지 일치했고, s[0 ~ 1]과 s[3 ~ 4]가 같다는 것을 사전에 확인해 놨으니 s[3]부터 일치하는 지 확인하기 시작하면 되고, 사전에 확인해 놓은 배열을 실패함수라고 한다.

실패함수라는 이름에 어떤 것인지 굉장히 햇갈렸다. 결론부터 말하면 실패함수 pi가 있을 때 pi[j] = 3 이라면 끝에서 부터 길이 3의 문자열이 앞에서부터 길이 3의 문자열과 같다는 소리고, 현재 비교하고 있는 문자의 3칸 앞에서부터 다시 비교를 시작하면 된다. 

나도 내가 무슨말을 하는지 모르겠다... 구현코드 자체는 짧고 간단하기 때문에 한번 이해만 한다면 다음부터는 쉽게 떠올릴 수 있을 것 같다.

다음은 백준 1786번문제를 kmp를 사용해 푼 소스코드이다.

    #include <iostream> 
    #include <vector>
    #include <algorithm>
    #include <string>
    #include <queue>
    #include <deque>
    #include <tuple>
    #include <list>
    #include <set>
    #include <cmath>
    #include <stack>
    #include <map>
    //#include<bits/stdc++.h>
    #include<cstdio>

    using namespace std;

    #define For(i, n) for(int i = 0; i < n; i++)
    #define IOS  ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    #define endl  "\n";
    #define IF(ny, nx, n, m) if(0 <= ny && ny < n && 0 <= nx && nx < m)
    #define P pair<int, int> 
    const int INF = 987654321;
    typedef  long long ll;
    int dx[] = { 1, 0, -1, 0, 2, 1, -1, -2, -2, -1, 1, 2 };
    int dy[] = { 0, 1, 0, -1, -1, -2, -2, -1, 1, 2, 2, 1 };
    int hx[] = { 2, 1, -1, -2, -2, -1, 1, 2 };
    int hy[] = { -1, -2, -2, -1, 1, 2, 2, 1 };

    vector<int> getPi(string s) {
        vector<int> fi(s.size(), 0);

        int j = 0;

        for (int i = 1; i < s.size(); i++) {
            while (j > 0 && s[i] != s[j]) j = fi[j - 1];
            if (s[i] == s[j]) fi[i] = ++j;
        }

        return fi;
    }

    void solve() {
        IOS;
        string s1, s2;
        
        getline(cin, s1);
        getline(cin, s2);

        vector<int> fi = getPi(s2);
        
        vector<int> ans;

        int j = 0;
        for (int i = 0; i < s1.size(); i++) {
            while(j > 0 && s1[i] != s2[j]) j = fi[j - 1];
            if (s1[i] == s2[j]) {
                if (j == s2.size() - 1) {
                    ans.push_back(i - j);
                    j = fi[j];
                }
                else
                    j++;
            }
        }

        cout << ans.size() << endl;
        for (auto i : ans) cout << i + 1 << " ";

    }


    int main() {
        int t = 1;
        //cin >> t;
        while (t--) {
            solve();
        
        return 0;
    }

