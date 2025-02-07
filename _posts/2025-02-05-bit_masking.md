---
layout: single
title:  "비트마스킹"
categories: algorithm
tag:
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "autoever"
#search: false
---

# 실버5
## 1094 막대기
```c++
#include <iostream>
using namespace std;

int x, sum, tmp, cnt;

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	cin >> x;
	while (sum != x) {
		tmp = 64; cnt++;
		while (tmp > x - sum) {
			tmp /= 2;
		}
		sum += tmp;
	}
	cout << cnt;
	return 0;
}
```

## 11723 집합
```c++
#include <iostream>
#include <string>
#include <memory.h>
using namespace std;

int a[24], n, m;
string s;

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	cin >> m;
	for (int i = 0; i < m;i++) {
		cin >> s;
		if (s == "add") {
			cin >> n;
			a[n] = 1;
		}
		else if (s == "remove") {
			cin >> n;
			a[n] = 0;
		}
		else if (s == "check") {
			cin >> n;
			if (a[n]) cout << 1 << "\n";
			else cout << 0 << "\n";
		}
		else if (s == "toggle") {
			cin >> n;
			if (a[n]) a[n] = 0;
			else a[n] = 1;
		}
		else if (s == "all") {
			fill(&a[0], &a[0] + 24, 1);
		}
		else if (s == "empty") {
			fill(&a[0], &a[0] + 24, 0);
		}
	}

	return 0;
}
```
# 골드4

## 1062 가르침
```c++
#include <iostream>
#include <vector>
#include <math.h>
using namespace std;

int n, k, num, ret = -987654321;
string s[54];
vector<int> v;

void go(int num, vector<int>& v) {
	int tmp = num;
	for (int i = 0; i < int(v.size()); i++)
		tmp += (1 << v[i]);
	int cnt = 0;
	for (int i = 0; i < n;i++) {
		int flag = 0;
		for (int j = 0; j < int(s[i].length()); j++) {
			if (tmp & (1 << (s[i][j] - 'a')))
				continue;
			else {
				flag = 1;
				break;
			}
		}
		if (!flag) cnt++;
	}

	ret = max(ret, cnt);
	return;
}

void combi(int start, vector<int>& v) {
	if (v.size() == k - 5) {
		go(int(pow(2, 'a' - 'a')) | int(pow(2, 'n' - 'a')) | int(pow(2, 't' - 'a')) | int(pow(2, 'i' - 'a')) | int(pow(2, 'c' - 'a')), v);
		return;
	}
	for (int i = start + 1; i < 26; i++) {
		if (i != int('a' - 'a') && i != int('n' - 'a') && i != int('t' - 'a') && i != int('i' - 'a') && i != int('c' - 'a')) {
			v.push_back(i);
			combi(i, v);
			v.pop_back();
		}
	}
	return;
}

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	cin >> n >> k;

	for (int i = 0; i < n;i++) {
		cin >> s[i];
	}

	if (k < 5) cout << 0;
	else {
		combi(-1, v);
		cout << ret;
	}
	return 0;
}
```

## 19942 다이어트
```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int n, mp, mf, ms, mv;
int smp, smf, sms, smv, smc;
int min_c = 987654321;
int a[20][6];
int flag;
vector<int> ret;
vector<int> v;
vector<vector<int>> v1;

void go(int idx, int check) {
	if (idx == -1) {
		go(idx + 1, 1); go(idx + 1, 0); return;
	}

	if (idx == n - 1 && check == 1) {
		v.push_back(idx);
		smp = 0; smf = 0; sms = 0; smv = 0; smc = 0;
		for (int i = 0; i < v.size(); i++) {
			smp += a[v[i]][0]; smf += a[v[i]][1]; sms += a[v[i]][2]; smv += a[v[i]][3]; smc += a[v[i]][4];
		}

		if (smp >= mp && smf >= mf && sms >= ms && smv >= mv) {
			flag = 1;
			if (min_c == smc) {
				min_c = smc;
				while (ret.size()) ret.pop_back();
				for (int i = 0; i < v.size(); i++) {
					ret.push_back(v[i]);
				}
				v1.push_back(ret);
			}
			if (min_c > smc) {
				while (v1.size()) v1.pop_back();
				min_c = smc;
				while (ret.size()) ret.pop_back();
				for (int i = 0; i < v.size(); i++) {
					ret.push_back(v[i]);
				}
				v1.push_back(ret);
			}
			v.pop_back();
			return;
		}
		else {
			v.pop_back();
			return;
		}
	}
	if (idx == n - 1 && check == 0) {
		smp = 0; smf = 0; sms = 0; smv = 0; smc = 0;
		for (int i = 0; i < v.size(); i++) {
			smp += a[v[i]][0]; smf += a[v[i]][1]; sms += a[v[i]][2]; smv += a[v[i]][3]; smc += a[v[i]][4];
		}

		if (smp >= mp && smf >= mf && sms >= ms && smv >= mv) {
			flag = 1;
			if (min_c == smc) {
				min_c = smc;
				while (ret.size()) ret.pop_back();
				for (int i = 0; i < v.size(); i++) {
					ret.push_back(v[i]);
				}
				v1.push_back(ret);
			}
			if (min_c > smc) {
				while (v1.size()) v1.pop_back();
				min_c = smc;
				while (ret.size()) ret.pop_back();
				for (int i = 0; i < v.size(); i++) {
					ret.push_back(v[i]);
				}
				v1.push_back(ret);
			}
			return;
		}
		else return;
	}

	if (check == 1) {
		v.push_back(idx); go(idx + 1, 1); go(idx + 1, 0); v.pop_back();
	}
	else {
		go(idx + 1, 1); go(idx + 1, 0);
	}
	return;
}

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	cin >> n;
	cin >> mp >> mf >> ms >> mv;
	for (int i = 0; i < n;i++) {
		for (int j = 0; j < 5;j++) {
			cin >> a[i][j];
		}
	}

	go(-1, 0);

	if (flag == 0) cout << -1 << "\n";
	else {
		cout << min_c << "\n";
		sort(v1.begin(), v1.end());
		for (auto it : v1[0])
			cout << it + 1 << " ";
	}
	return 0;
}
```
