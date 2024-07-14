---
layout: single
title:  "백준 문제집(수학)"
categories: algorithm
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 실버1

## 1629
```c++
//곱셈
#include <iostream>
using namespace std;
typedef unsigned long long ll;

ll a, b, c;

ll go(ll a, ll b) {
	if (b == 1) return a % c;

	ll ret = go(a, b / 2);
	ret = (ret * ret) % c;

	if (b % 2) ret = (ret * a) % c;
		 
	return ret;
}

int main() {
	ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	cin >> a >> b >> c;

	cout << go(a, b);

	return 0;
}
```
