---
layout: single
title:  "백준 문제집(완전탐색, 백트래킹)"
categories: algorithm
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 골드3

## 4179 불

```c++
#include <iostream>
#include <queue>
#include <map>
using namespace std;

const int dy[4] = { 0,-1,0,1 };
const int dx[4] = { 1,0,-1,0 };

queue<pair<int, int>> q;
string s;
int a[1004][1004], visited_f[1004][1004], visited_j[1004][1004];
int r, c, y, x, ny, nx, sy, sx, flag, flag1;
int ret = 987654321;

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	cin >> r >> c;

	for (int i = 0; i < r; i++) {
		cin >> s;
		for (int j = 0; j < c; j++) {
			if (s[j] == '#') {
				a[i][j] = 0;
			}
			else if (s[j] == 'J') {
				visited_j[i][j] = 1;
				sy = i; sx = j;
				a[i][j] = 1;
			}
			else if (s[j] == 'F') {
				visited_f[i][j] = 1;
				q.push({ i,j });
				a[i][j] = 1;
				flag1 = 1;
			}
			else {
				a[i][j] = 1;
			}
		}
	}
	
	while (q.size()) {
		tie(y, x) = q.front(); q.pop();
		for (int i = 0; i < 4; i++) {
			ny = y + dy[i];
			nx = x + dx[i];
			if (ny < 0 || ny >= r || nx < 0 || nx >= c) continue;
			if (a[ny][nx] == 0 || visited_f[ny][nx] != 0) continue;
			visited_f[ny][nx] = visited_f[y][x] + 1;
			q.push({ ny,nx });
		}
	}

	q.push({ sy,sx });

	while (q.size()) {
		tie(y, x) = q.front(); q.pop();
		for (int i = 0; i < 4; i++) {
			ny = y + dy[i];
			nx = x + dx[i];
			if (ny < 0 || ny >= r || nx < 0 || nx >= c) continue;
			if (a[ny][nx] == 0 || visited_j[ny][nx] != 0) continue;
			visited_j[ny][nx] = visited_j[y][x] + 1;
			q.push({ ny,nx });
		}
	}

	if (flag1 == 1) {
		for (int i = 0; i < r; i++) {
			if (visited_j[i][0] < visited_f[i][0] && visited_j[i][0] != 0) {
				flag = 1; ret = min(ret, visited_j[i][0]);
			}
			if (visited_j[i][c - 1] < visited_f[i][c - 1] && visited_j[i][c - 1] != 0) {
				flag = 1; ret = min(ret, visited_j[i][c - 1]);
			}
		}

		for (int i = 0; i < c; i++) {
			if (visited_j[0][i] < visited_f[0][i] && visited_j[0][i] != 0) {
				flag = 1; ret = min(ret, visited_j[0][i]);
			}
			if (visited_j[r - 1][i] < visited_f[r - 1][i] && visited_j[r - 1][i] != 0) {
				flag = 1; ret = min(ret, visited_j[r - 1][i]);
			}
		}
	}

	else {
		for (int i = 0; i < r; i++) {
			if (visited_j[i][0] != 0) {
				flag = 1; ret = min(ret, visited_j[i][0]);
			}
			if (visited_j[i][c - 1] != 0) {
				flag = 1; ret = min(ret, visited_j[i][c - 1]);
			}
		}

		for (int i = 0; i < c; i++) {
			if (visited_j[0][i] != 0) {
				flag = 1; ret = min(ret, visited_j[0][i]);
			}
			if (visited_j[r - 1][i] != 0) {
				flag = 1; ret = min(ret, visited_j[r - 1][i]);
			}
		}
	}
	if (flag == 1) {
		cout << ret;
	}
	else {
		cout << "IMPOSSIBLE ";
	}
	return 0;
}
```

# 골드4
## 1987 알파벳

```c++
#include <iostream>
using namespace std;

const int dy[4] = { 0,-1,0,1 };
const int dx[4] = { 1,0,-1,0 };

int r, c, ret = -987654321;
char a[24][24];
int visited[24][24];
int check[26];
string s;

void dfs(int y, int x, int n) {
	for (int i = 0; i < 4; i++) {
		int ny = y + dy[i];
		int nx = x + dx[i];

		if (ny < 0 || ny >= r || nx < 0 || nx >= c) continue;
		if (check[a[ny][nx] - 'A']) continue;
		check[a[ny][nx] - 'A'] = 1;
		dfs(ny, nx, n + 1);
		check[a[ny][nx] - 'A'] = 0;
	}
	ret = max(ret, n);
	return;
}

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	cin >> r >> c;
	for (int i = 0; i < r; i++) {
		cin >> s;
		for (int j = 0; j < c; j++) {
			a[i][j] = s[j];
		}
	}
	check[a[0][0] - 'A'] = 1;
	dfs(0, 0, 1);

	cout << ret;
	return 0;
}
```

## 14497 주난의 난
```c++
#include <iostream>
#include <queue>
#include <map>
#include <memory.h>
using namespace std;

const int dy[4] = { 0,-1,0,1 };
const int dx[4] = { 1,0,-1,0 };

int a[304][304], visited[304][304];
int n, m, y1, x1, y2, x2, ret, y, x, ny, nx, flag;
string s;
queue<pair<int, int>> q;

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);

	cin >> n >> m;
	cin >> y1 >> x1 >> y2 >> x2;

	y1--; x1--; y2--; x2--;

	for (int i = 0; i < n; i++) {
		cin >> s;
		for (int j = 0; j < m; j++) {
			if (s[j] == '1') {
				a[i][j] = 1;
			}
		}
	}

	while (1) {
		ret++;
		fill(&visited[0][0], &visited[0][0] + 304 * 304, 0);
		q.push({ y1, x1 }); visited[y1][x1] = 1;
		while (q.size()) {
			tie(y, x) = q.front(); q.pop();
			for (int i = 0; i < 4; i++) {
				ny = y + dy[i];
				nx = x + dx[i];
				if (ny < 0 || ny >= n || nx < 0 || nx >= m) continue;
				if (visited[ny][nx] == 1) continue;
				if (a[ny][nx] == 1) {
					a[ny][nx] = 0;
					visited[ny][nx] = 1;
					continue;
				}
				if (ny == y2 && nx == x2) {
					flag = 1; break;
				}
				q.push({ ny,nx }); visited[ny][nx] = 1;
			}
			if (flag) break;
		}
		if (flag) break;
	}

	cout << ret;
	return 0;
}
```

# 골드5

## 2589
```c++
#include <iostream>
#include <queue>
#include <map>
#include <memory.h>
using namespace std;

queue<pair<int, int>> q;
string s;
const int dy[4] = { 0,-1,0,1 };
const int dx[4] = { 1,0,-1,0 };
int n, m, ret = -987654321;
int y, x, ny, nx;

int a[54][54], visited[54][54];

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	
	cin >> n >> m;
	for (int i = 0; i < n;i++) {
		cin >> s;
		for (int j = 0; j < m;j++) {
			if (s[j] == 'W') a[i][j] = 0;
			else a[i][j] = 1;
		}
	}

	for (int i = 0; i < n;i++) {
		for (int j = 0; j < m;j++) {
			fill(&visited[0][0], &visited[0][0] + 54 * 54, 0);
			if (a[i][j] == 1 && visited[i][j] == 0) {
				q.push({ i,j }); visited[i][j] = 1;
			}
			while (q.size()) {
				tie(y, x) = q.front(); q.pop();
				for (int h = 0; h < 4;h++) {
					ny = y + dy[h];
					nx = x + dx[h];
					if (ny < 0 || ny >= n || nx < 0 || nx >= m) continue;
					if (a[ny][nx] == 0 || visited[ny][nx]) continue;
					visited[ny][nx] = visited[y][x] + 1;
					q.push({ ny,nx });
				}
			}
			for (int q = 0; q < n;q++) {
				for (int w = 0; w < m;w++) {
					ret = max(ret, visited[q][w]);
				}
			}
		}
	}


	cout << ret - 1<< "\n";
	return 0;
}
```

