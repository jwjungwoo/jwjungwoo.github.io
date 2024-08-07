---
layout: single
title:  "백준 문제집(구현)"
categories: algorithm
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 실버1

## 1992
```c++
#include <iostream>
#include <string>
using namespace std;

string ret, s;
int n, flag, dis;
int arr[68][68];

string go(int y, int x, int n) {
	dis = arr[y][x];
	for (int i = y; i < y + n; i++) {
		for (int j = x; j < x + n; j++) {
			if (arr[i][j] != dis) {
				return "(" + go(y, x, n / 2) + go(y, x + n / 2, n / 2) + go(y + n / 2, x, n / 2) + go(y + n / 2, x + n / 2, n / 2) + ")";
			}
		}
	}
	return to_string(dis);
}

int main() {
	ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> s;
		for (int j = 0; j < n; j++) {
			arr[i][j] = s[j] - '0';
		}
	}

	cout << go(0,0,n) << "\n";

	return 0;
}
```

# 실버3

## 2910

```c++
//빈도 정렬
#include <iostream>
#include <vector>
#include <map>
#include <algorithm>
using namespace std;

int n, c, a;
vector<pair<int, int>> v1; //빈도

map<int, int> mp1; // 빈도
map<int, int> mp2; // 순서

bool oper(pair<int, int> p1, pair<int, int> p2) {
	if(mp1[p1.first] != mp1[p2.first])
		return p1.second > p2.second;
	else {
		return mp2[p1.first] < mp2[p2.first];
	}
}

int main() {
	ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	cin >> n >> c;
	for (int i = 0; i < n; i++) {
		cin >> a;
		mp1[a]++;
		if (!mp2[a])
			mp2[a] = i + 1;
	}

	for (auto it : mp1)
		v1.push_back({ it.first,it.second });

	sort(v1.begin(), v1.end(), oper);

	for (auto it : v1) {
		for (int i = 0; i < mp1[it.first]; i++)
			cout << it.first << " ";
	}

	return 0;
}
```

# 실버4

## 1620

```c++
//나는야 포켓몬 마스터 이다솜
//http://boj.kr/0f8a07e622564e12bd5f262fc8f0d61b
#include <iostream>
#include <map>
using namespace std;

map<string, int> mp1;
map<int, string> mp2;
int n, m;
string s;

int main() {
	ios_base::sync_with_stdio(false); cin.tie(0); cout.tie(0);
	cin >> n >> m;
	for (int i = 0; i < n;i++) {
		cin >> s;
		mp1[s] = i + 1;
		mp2[i + 1] = s;
	}

	for (int i = 0;i < m;i++) {
		cin >> s;
		if (atoi(s.c_str()))
			cout << mp2[atoi(s.c_str())] << "\n";
		else
			cout << mp1[s] << "\n";
	}
	return 0;
}
```

## 3986

```c++
//좋은 단어
#include <iostream>
#include <stack>
using namespace std;

int n, ret;
stack<char> stk;
string s;

int main() {
	cin >> n;
	for (int i = 0; i < n;i++) {
		cin >> s;
		while (stk.size()) 
			stk.pop();
		for (int j = 0; j < s.length();j++) {
			if (!stk.size())
				stk.push(s[j]);
			else {
				if (stk.top() == s[j]) {
					stk.pop();
				}
				else {
					stk.push(s[j]);
				}
			}
		}
		if (!stk.size())
			ret++;
	}
	cout << ret;
	return 0;
}
```

# 실버3

## 1213
```c++
//팰린드롬 만들기
#include <iostream>
#include <algorithm>
using namespace std;
int arr[26];
int flag, ff;
string ret, temp;
string s;

int main() {
	ios_base::sync_with_stdio(false); cin.tie(0); cout.tie(0);
	cin >> s;
	for (int i = 0; i < s.length();i++)
		arr[s[i] - 'A']++;

	for (int i = 0; i < 26;i++) {
		if (arr[i] % 2 == 1) {
			flag++;ff = i;
		}
	}

	if (flag >= 2) {
		cout << "I'm Sorry Hansoo\n";
	}

	else {
		if (flag == 0) {
			for (int i = 0; i < 26;i++) {
				for (int j = 0; j < arr[i] / 2;j++)
					ret += char('A' + i);
			}
			temp = ret;
			reverse(temp.begin(), temp.end());
			ret += temp;
			cout << ret << "\n";
		}
		else {
			for (int i = 0; i < 26;i++) {
				for (int j = 0; j < arr[i] / 2;j++)
					ret += char('A' + i);
			}
			temp = ret;
			reverse(temp.begin(), temp.end());
			ret += char('A' + ff);
			ret += temp;
			cout << ret << "\n";
		}
	}

	return 0;
}
```

## 2559
```c++
//수열
#include <iostream>
#include <numeric>
using namespace std;
int n, m, num, psum[100004];
int ret = -987654321;
int main() {
	cin >> n >> m;
	for (int i = 1; i <= n;i++) {
		cin >> num;
		psum[i] = psum[i - 1] + num;
	}
	for (int i = m;i <= n;i++) {
		ret = max(ret, psum[i] - psum[i - m]);
	}

	cout << ret << "\n";
	return 0;
}
```

## 9375
```c++
//패션왕 신해빈
#include <iostream>
#include <map>
#include <vector>
#include <algorithm>
using namespace std;
int n, m;
string a, b;
map<string, int> mp;
vector<string> v;
int ret = 1;

int main() {
	ios_base::sync_with_stdio(false); cin.tie(0); cout.tie(0);
	cin >> n;
	for (int i = 0; i < n;i++) {
		v.clear(); mp.clear(); ret = 1;
		cin >> m;
		for (int i = 0; i < m;i++) {
			cin >> a >> b;
			if (find(v.begin(),v.end(),b) == v.end())
				v.push_back(b);

			if (mp[b] == 0)
				mp[b] = 1;
			else
				mp[b]++;
		}
		for (auto it : v) {
			ret *= (mp[it] + 1);
		}
		ret -= 1;
		cout << ret << "\n";
	}
	return 0;
}
```

# 실버5

## 2828
```c++
//사과 담기 게임
#include <iostream>
using namespace std;

int n, m, j, a, ret, pos, temp;

int main() {
	ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	cin >> n >> m; cin >> j;
	pos = 1;
	for (int i = 0; i < j; i++) {
		cin >> a;
		if ((pos <= a) && (a <= (pos + m - 1)))
			continue;
		else {
			if (a > pos) {
				temp = (a - (pos + m - 1));
				pos += temp;
				ret += temp;
			}
			else {
				temp = (pos - a);
				pos -= temp;
				ret += temp;
			}
		}
	}

	cout << ret << "\n";

	return 0;
}
```

## 4659
```c++
#include <iostream>
using namespace std;

int f1, f2, f3, f4;
char c, temp;

int main() {
	ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	string s;

	while (1) {
		cin >> s;
		f1 = f2 = f3 = f4 = 0;
		if (s == "end") break;
		for (int i = 0; i < s.length();i++) {
			if (i != 0) {
				temp = s[i - 1]; c = s[i];
				if (temp == c && temp != 'e' && temp != 'o')
					f4 = 1;
			}
			if (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' || s[i] == 'o' || s[i] == 'u') {
				f3 = 1; f1++; f2 = 0;
			}
			else {
				f2++; f1 = 0;
			}
			if (f1 == 3 || f2 == 3)
				f4 = 1;
		}
		if (f3 == 1 && f4 == 0)
			cout << "<" << s << "> is acceptable.\n";
		else
			cout << "<" << s << "> is not acceptable.\n";
	}
	return 0;
}
```

## 10709
```c++
#include <iostream>
using namespace std;

string s;
int v[104][104];
int h, w, cnt;
bool flag;

int main() {
	ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	cin >> h >> w;

	for (int i = 0; i < h;i++) {
		cin >> s;
		cnt = 0;
		flag = 0;
		for (int j = 0; j < w;j++) {
			if (s[j] == '.' && flag == 0) {
				v[i][j] = -1;
			}
			else if (s[j] == 'c') {
				flag = 1;
				v[i][j] = 0; 
				cnt = 0;
				cnt++;
			}
			else {
				v[i][j] = cnt;
				cnt++;
			}
		}
	}

	for (int i = 0; i < h;i++) {
		for (int j = 0; j < w;j++) {
			cout << v[i][j] << " ";
		}
		cout << "\n";
	}

	return 0;
}
```
