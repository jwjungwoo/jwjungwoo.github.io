---
layout: single
title:  "백준 문제집(문자열)"
categories: algorithm
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 실버3 한국이 그리울 땐~

## 9996
```c++
//한국이 그리울 땐 서버에 접속하지
#include <iostream>
using namespace std;
string s, input, f, d;
int n;

int main() {
	cin >> n;
	cin >> s;
	f = s.substr(0, s.find('*'));
	d = s.substr(s.find('*') + 1, s.length());

	for (int i = 0; i < n;i++) {
		cin >> input;

		if (input.length() >= s.length() - 1) {
			if (input.substr(0, f.length()) == f && input.substr(input.length() - d.length(), input.length()) == d)
				cout << "DA\n";
			else
				cout << "NE\n";
		}
		else {
			cout << "NE\n";
		}
	}


	return 0;
}
```

# 실버5
## 14405 피카츄
```c++
#include <iostream>
using namespace std;

int flag;
string s;

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	cin >> s;
	for (int i = 0; i < s.length(); i++) {
		if (i < s.length() - 1 && (s.substr(i, 2) == "pi" || s.substr(i, 2) == "ka")) i += 1;
		else if (i < s.length() - 2 && (s.substr(i, 3) == "chu")) i += 2;
		else { flag = 1; break;}
	}
	if (flag) cout << "NO\n";
	else cout << "YES\n";
	return 0;
}
```
