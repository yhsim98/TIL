# LCS(Longest Common Subsequence)
최장 공통 부분 수열이다. 

## 구현 방법
[여기에 잘 설명되 있다](https://www.crocus.co.kr/787)  
dp를 이용한 방식으로 풀면 된다. 시간복잡도는 두 문자열의 길이의 곱 O(nm)이다.

## 소스코드

    str1 = '0' + s1;
    str2 = '0' + s2;
    
    vector<vector<int>> table(len1, vector<int> (len2));

    for(int i = 1; i < len1; i++){
        for(int k = 1; k < len2; k++){

            if(str1[i] == str2[k])
                table[i][k] = table[i - 1][k - 1] + 1;
            else {
                table[i][k] = (table[i - 1][k] > table[i][k - 1]) ? table[i - 1][k] : table[i][k - 1];
            }

        }
    }


