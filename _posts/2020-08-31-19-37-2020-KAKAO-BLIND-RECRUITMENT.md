---
layout: post
title: "2020 Kakao Blind Recruitment"
categories:
  - Kakao Blind Recuitment
tags:
  - C++
  - Kakao Blind Recuitment
  - Programmers
last_modified_at: 2020-09-03-21-14
---

<strong> 문제 1:</strong> 문자열 압축

<strong>문제 설명</strong>
{% highlight txt %}
데이터 처리 전문가가 되고 싶은 어피치는 문자열을 압축하는 방법에 대해 공부를 하고
있습니다. 최근에 대량의 데이터 처리를 위한 간단한 비손실 압축 방법에 대해 공부를
하고 있는데, 문자열에서 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는
값으로 표현하여 더 짧은 문자열로 줄여서 표현하는 알고리즘을 공부하고 있습니다.
간단한 예로 aabbaccc의 경우 2a2ba3c(문자가 반복되지 않아 한번만 나타난 경우
1은 생략함)와 같이 표현할 수 있는데, 이러한 방식은 반복되는 문자가 적은 경우
압축률이 낮다는 단점이 있습니다. 예를 들면, abcabcdede와 같은 문자열은 전혀
압축되지 않습니다. 어피치는 이러한 단점을 해결하기 위해 문자열을 1개 이상의 단위로
잘라서 압축하여 더 짧은 문자열로 표현할 수 있는지 방법을 찾아보려고 합니다.
예를 들어, ababcdcdababcdcd의 경우 문자를 1개 단위로 자르면 전혀 압축되지
않지만, 2개 단위로 잘라서 압축한다면 2ab2cd2ab2cd로 표현할 수 있습니다. 다른
방법으로 8개 단위로 잘라서 압축한다면 2ababcdcd로 표현할 수 있으며, 이때가 가장
짧게 압축하여 표현할 수 있는 방법입니다. 다른 예로, abcabcdede와 같은 경우,
문자를 2개 단위로 잘라서 압축하면 abcabc2de가 되지만, 3개 단위로 자른다면
2abcdede가 되어 3개 단위가 가장 짧은 압축 방법이 됩니다. 이때 3개 단위로 자르고
마지막에 남는 문자열은 그대로 붙여주면 됩니다. 압축할 문자열 s가 매개변수로 주어질
때, 위에 설명한 방법으로 1개 이상 단위로 문자열을 잘라 압축하여 표현한 문자열 중
가장 짧은 것의 길이를 return 하도록 solution 함수를 완성해주세요.
{% endhighlight %}

<strong>제한사항</strong>
{% highlight txt %}
s의 길이는 1 이상 1,000 이하입니다.
s는 알파벳 소문자로만 이루어져 있습니다.
{% endhighlight %}


<strong>입출력 예</strong>

| s | result |
| --- | --- |
| "aabbaccc" | 7 |
| "ababcdcdababcdcd" | 9 |
| "abcabcdede" | 8 |
| "abcabcabcabcdededededede" | 14 |
| "xababcdcdababcdcd" | 17 |


<strong>입출력 예에 대한 설명</strong>
{% highlight txt %}
입출력 예 #1
문자열을 1개 단위로 잘라 압축했을 때 가장 짧습니다.
{% endhighlight %}

{% highlight txt %}
입출력 예 #2
문자열을 8개 단위로 잘라 압축했을 때 가장 짧습니다.
{% endhighlight %}

{% highlight txt %}
입출력 예 #3
문자열을 3개 단위로 잘라 압축했을 때 가장 짧습니다.
{% endhighlight %}

{% highlight txt %}
입출력 예 #4
문자열을 2개 단위로 자르면 abcabcabcabc6de 가 됩니다.
문자열을 3개 단위로 자르면 4abcdededededede 가 됩니다.
문자열을 4개 단위로 자르면 abcabcabcabc3dede 가 됩니다.
문자열을 6개 단위로 자를 경우 2abcabc2dedede가 되며, 이때의 길이가 14로
가장 짧습니다.
{% endhighlight %}

{% highlight txt %}
입출력 예 #5
문자열은 제일 앞부터 정해진 길이만큼 잘라야 합니다.
따라서 주어진 문자열을 x / ababcdcd / ababcdcd 로 자르는 것은 불가능
합니다. 이 경우 어떻게 문자열을 잘라도 압축되지 않으므로 가장 짧은 길이는
17이 됩니다.
{% endhighlight %}

{% highlight cpp %}
#include <string>
#include <vector>

using namespace std;

int solution(string s) {
    int answer = 0;
    unsigned cnt = 0;
    unsigned min = s.size();
    string word;
    string num;
    vector<char> vc;
    vector<string> vs;
    vector<char> emptch;
   
    for (unsigned i = 0;
         i != s.size() / 2; i++) {
        for (auto c : s) {
            word += c;
            
            if (cnt == i) {
                vs.push_back(word);
                cnt = 0;
                word.clear();
            } else {
                cnt++;
            }
        } cnt = 1;
        
        if (!word.empty()) {
            vs.push_back(word);
            word.clear();
        }
        
        for (decltype(vs.size()) j = 0;
             j != vs.size() - 1; j++) {
            if (vs[j] == vs[j + 1]) {
                cnt++;
            } else {
                if (cnt != 1) {
                    if (cnt >= 10) {
                        num = to_string(cnt);
                        
                        for (auto c : num)
                            vc.push_back(c);
                    } else 
                        vc.push_back('0' + cnt);
                    
            }
                for (auto c : vs[j])
                    vc.push_back(c);
                cnt = 1;
            }
        }
        if (cnt != 1) {
            if (cnt >= 10) {
                num = to_string(cnt);
                        
                for (auto c : num)
                    vc.push_back(c);
            } else 
                vc.push_back('0' + cnt);
        }
        
        for (auto c : vs[vs.size() - 1])
            vc.push_back(c); 
        
        if (min > vc.size())
            min = vc.size();
        
        vs.clear();
        vc.clear();
        cnt = 0;
    }
    
    return answer = min;
}
{% endhighlight %}


<strong> 문제 2:</strong> 괄호 변환

<strong>문제 설명</strong>

{% highlight txt %}
카카오에 신입 개발자로 입사한 콘은 선배 개발자로부터 개발역량 강화를 위해 다른
개발자가 작성한 소스 코드를 분석하여 문제점을 발견하고 수정하라는 업무 과제를
받았습니다. 소스를 컴파일하여 로그를 보니 대부분 소스 코드 내 작성된 괄호가 개수는
맞지만 짝이 맞지 않은 형태로 작성되어 오류가 나는 것을 알게 되었습니다.
수정해야 할 소스 파일이 너무 많아서 고민하던 콘은 소스 코드에 작성된 모든 괄호를
뽑아서 올바른 순서대로 배치된 괄호 문자열을 알려주는 프로그램을 다음과 같이 개발하려고
합니다.
{% endhighlight %}

<strong>용어의 정의</strong>
{% highlight txt %}
'(' 와 ')' 로만 이루어진 문자열이 있을 경우, '(' 의 개수와 ')' 의 개수가 같다면
이를 균형잡힌 괄호 문자열이라고 부릅니다. 그리고 여기에 '('와 ')'의 괄호의 짝도
모두 맞을 경우에는 이를 올바른 괄호 문자열이라고 부릅니다. 예를 들어, "(()))("와
같은 문자열은 균형잡힌 괄호 문자열 이지만 올바른 괄호 문자열은 아닙니다. 반면에
"(())()"와 같은 문자열은 균형잡힌 괄호 문자열 이면서 동시에 올바른 괄호 문자열 입니다.
'(' 와 ')' 로만 이루어진 문자열 w가 균형잡힌 괄호 문자열 이라면 다음과 같은 과정을
통해 올바른 괄호 문자열로 변환할 수 있습니다.
{% endhighlight %}

{% highlight txt %}
1. 입력이 빈 문자열인 경우, 빈 문자열을 반환합니다. 
2. 문자열 w를 두 "균형잡힌 괄호 문자열" u, v로 분리합니다. 단, u는 "균형잡힌 괄호
   문자열"로 더 이상 분리할 수 없어야 하며, v는 빈 문자열이 될 수 있습니다. 
3. 문자열 u가 "올바른 괄호 문자열" 이라면 문자열 v에 대해 1단계부터 다시 수행합니다. 
  3-1. 수행한 결과 문자열을 u에 이어 붙인 후 반환합니다. 
4. 문자열 u가 "올바른 괄호 문자열"이 아니라면 아래 과정을 수행합니다. 
  4-1. 빈 문자열에 첫 번째 문자로 '('를 붙입니다. 
  4-2. 문자열 v에 대해 1단계부터 재귀적으로 수행한 결과 문자열을 이어 붙입니다. 
  4-3. ')'를 다시 붙입니다. 
  4-4. u의 첫 번째와 마지막 문자를 제거하고, 나머지 문자열의 괄호 방향을 뒤집어서 뒤에
       붙입니다. 
  4-5. 생성된 문자열을 반환합니다.
{% endhighlight %}
  
{% highlight txt %}
균형잡힌 괄호 문자열 p가 매개변수로 주어질 때, 주어진 알고리즘을 수행해 올바른 괄호
문자열로 변환한 결과를 return 하도록 solution 함수를 완성해 주세요.
{% endhighlight %}

<strong>매개변수 설명</strong>
{% highlight txt %}
p는 '(' 와 ')' 로만 이루어진 문자열이며 길이는 2 이상 1,000 이하인 짝수입니다.
문자열 p를 이루는 '(' 와 ')' 의 개수는 항상 같습니다.
만약 p가 이미 올바른 괄호 문자열이라면 그대로 return 하면 됩니다.
{% endhighlight %}

<strong>입출력 예</strong>
| p | result |
| --- | --- |
| "(()())()" | "(()())()" |
| ")(" | "()" |
| "()))((()" | "()(())()" |

<strong>입출력 예에 대한 설명</strong>
{% highlight txt %}
입출력 예 #1
이미 올바른 괄호 문자열 입니다.
{% endhighlight %}

{% highlight txt %}
입출력 예 #2
두 문자열 u, v로 분리합니다.
u = ")("
v = ""
u가 올바른 괄호 문자열이 아니므로 다음과 같이 새로운 문자열을 만듭니다.
v에 대해 1단계부터 재귀적으로 수행하면 빈 문자열이 반환됩니다.
u의 앞뒤 문자를 제거하고, 나머지 문자의 괄호 방향을 뒤집으면 ""이 됩니다.
따라서 생성되는 문자열은 "(" + "" + ")" + ""이며, 최종적으로 "()"로 변환됩니다.
{% endhighlight %}

{% highlight txt %}
입출력 예 #3
두 문자열 u, v로 분리합니다.
u = "()"
v = "))((()"
문자열 u가 올바른 괄호 문자열이므로 그대로 두고, v에 대해 재귀적으로 수행합니다.
다시 두 문자열 u, v로 분리합니다.
u = "))(("
v = "()"
u가 올바른 괄호 문자열이 아니므로 다음과 같이 새로운 문자열을 만듭니다.
v에 대해 1단계부터 재귀적으로 수행하면 "()"이 반환됩니다.
u의 앞뒤 문자를 제거하고, 나머지 문자의 괄호 방향을 뒤집으면 "()"이 됩니다.
따라서 생성되는 문자열은 "(" + "()" + ")" + "()"이며, 최종적으로 "(())()"를 반환합니다.
처음에 그대로 둔 문자열에 반환된 문자열을 이어 붙이면 "()" + "(())()" = "()(())()"가
됩니다.
{% endhighlight %}

{% highlight cpp %}
#include <string>
#include <vector>

using namespace std;

string function(string p) {
    vector<unsigned> vi(2, 0);
    vector<char> vc;
    auto &left = vi[0];
    auto &right = vi[1];
    string u, v, temp, temp2;
    bool flag = false;
    
    if (p.empty())
        return p;
        
    for (auto c : p) {
        if (vc.empty()) {
            if (c == '(')
                vc.push_back(c);
            else {
                vc.push_back(c);
                break;
            }
        } else {
            if (c == ')')
                vc.pop_back();
            else 
                vc.push_back(c);
        }
    }
    
    if (vc.empty())
        return p;
    vc.clear();   
    
    for (auto c : p) {
        if (flag) {
            v += c;
        } else {
            if (c == '(')
                left++;
            else
                right++;
            u += c;
            
            if (left == right)
                flag = true;
        }
    }
    
    for (auto c : u) {
        if (vc.empty()) {
            if (c == '(')
                vc.push_back(c);
            else {
                vc.push_back(c);
                break;
            }
        } else {
            if (c == ')')
                vc.pop_back();
            else 
                vc.push_back(c);
        }
    }
    
    if (vc.empty())
        return u + function(v);
    vc.clear();    
    
    temp = '(' + function(v) + ')';
    
    for (decltype(u.size()) i = 1; i != u.size() - 1; i++)
        temp2 += u[i];
    
    for (decltype(temp2.size()) i = 0; i != temp2.size(); i++) {
        if (temp2[i] == '(')
            temp2[i] = ')';
        else 
            temp2[i] = '(';
    }
    
    return temp + temp2;
}

string solution(string p) {
    string answer = "";
    vector<char> vc;
    
    if (p.empty())
        return answer;
    
    for (auto c : p) {
        if (vc.empty()) {
            if (c == '(')
                vc.push_back(c);
            else {
                vc.push_back(c);
                break;
            }
        } else {
            if (c == ')')
                vc.pop_back();
            else 
                vc.push_back(c);
        }
    }
    
    if (vc.empty())
        return p;
    vc.clear();
    
    answer = function(p);
    
    return answer;
}
{% endhighlight %}

<strong> 문제 3:</strong> 자물쇠와 열쇠

<strong>문제 설명</strong>

{% highlight txt %}
고고학자인 "튜브"는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을
발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 자물쇠로 잠겨 있었고 문
앞에는 특이한 형태의 열쇠와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는
종이가 발견되었습니다. 잠겨있는 자물쇠는 격자 한 칸의 크기가 1 x 1인 N x N
크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 M x M 크기인 정사각 격자 형태로
되어 있습니다. 자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다.
열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면
자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는
자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과
자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는
안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.
열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로
주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return
하도록 solution 함수를 완성해주세요.
{% endhighlight %}

<strong>제한사항</strong>

{% highlight txt %}
key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
M은 항상 N 이하입니다.
key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
- 0은 홈 부분, 1은 돌기 부분을 나타냅니다.
{% endhighlight %}

<strong>입출력 예</strong>

| key | lock | result |
| --- | --- | --- |
| [[0, 0, 0], [1, 0, 0], [0, 1, 1]] | [[1, 1, 1], [1, 1, 0], [1, 0, 1]] | true |

<strong>입출력 예에 대한 설명</strong>

![자물쇠.jpg](/assets/images/2020-08-31-19-37-2020-KAKAO-BLIND-RECRUITMENT/자물쇠.jpg)

{% highlight txt %}
key를 시계 방향으로 90도 회전하고, 오른쪽으로 한 칸, 아래로 한 칸 이동하면 lock의
홈 부분을 정확히 모두 채울 수 있습니다.
{% endhighlight %}


{% highlight cpp %}
#include <string>
#include <vector>

using namespace std;

vector<vector<int>> rotate(vector<vector<int>> &key) {
    vector<vector<int>> temp(key);
    
    for (decltype(key.size()) i = 0; i != key.size(); i++)
        for (decltype(key.size()) j = 0; j != key.size(); j++)
            key[i][key.size() - j - 1] = temp[key.size() - j - 1][key.size() - i - 1];
    return key;
}

bool insertKey(vector<vector<int>> &key, vector<vector<int>> &lock, vector<vector<int>> board, decltype(board.size()) a, decltype(board.size()) b) {
    for (decltype(key.size()) i = 0, n = a; i != key.size(); i++, n++) {
        for (decltype(key.size()) j = 0, m = b; j != key.size(); j++, m++) {
            board[n][m] ^= key[i][j];
            if (!board[n][m] && key[i][j]) {
                return false;
            }
        }
    }

    for (decltype(lock.size()) i = 0; i != lock.size(); i++) {
        for (decltype(lock.size()) j = 0; j != lock.size(); j++) {
            if (!board[key.size() + i - 1][key.size() + j - 1]) {
                return false;
            }
        }
    } return true;
}

bool solution(vector<vector<int>> key, vector<vector<int>> lock) {
    vector<vector<int>> board(key.size() * 2 + lock.size() - 2, vector<int>(key.size() * 2 + lock.size() - 2));
    const unsigned size = 4;
    unsigned flag = 0;
    
    for (decltype(lock.size()) i = 0; i != lock.size(); i++) {
        for (decltype(lock.size()) j = 0; j != lock.size(); j++) {
            if (!lock[i][j]) {
                flag++;
            } board[key.size() + i - 1][key.size() + j - 1] = lock[i][j];
        }
    } if (!flag)
        return true;

    for (decltype(board.size()) i = 0; i != key.size() + lock.size() - 1; i++) {
        for (decltype(board.size()) j = 0; j != key.size() + lock.size() - 1; j++) {
            for (unsigned n = 0; n != size; n++) {
                if (insertKey(key, lock, board, i, j)) {
                    return true;
                } rotate(key);
            }
        }
    } return false;
}
{% endhighlight %}
