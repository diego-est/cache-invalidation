---
title: ICPC Formulas and Shortcuts
date: 2023-10-08T21:25:27-04:00
description: Assortment of ICPC formulas and shortcuts that I find useful.
slug: icpc-formulas
categories:
    - Math
    - Competitive Programming
tags:
    - C++
weight: 1
---

# Code templates
Here are several code templates that will probably have to be typed out for
every exercise.
## C++
```cpp
#include <bits/stdc++.h>
using namespace std;
#define fastIO ios_base::sync_with_stdio(false), cin.tie(NULL), cout.tie(NULL);
#define all(x) x.begin(), x.end()
#define int int64_t
typedef size_t sz;
typedef pair<int, int> ii;
typedef vector<int> vi;
typedef vector<pair<int, int>> vii;
typedef vector<vector<int>> vvi;

int32_t main()
{
    fastIO;

	/* solution */

	return 0;
}
```

# Formulas
These code templates are based off the previous C++ code header.
## Matrices
[Matrices](matrix.md) are very useful for recurrence relations.

# Longest common subsequence
```cpp
template<typename T>
int lcs(T& seq1, T& seq2)
{
	int n = seq1.size();
	int m = seq2.size();

	vi prev(m + 1, 0), cur(m + 1, 0);

	for (int i = 1; i < n + 1; i++) {
		for (int j = 1; j < m + 1; j++) {
			if (seq1[i - 1] == seq2[j - 1])
				cur[j] = 1 + prev[j - 1];
			else
				cur[j] = max(cur[j - 1], prev[j]);
		}
		prev = cur;
	}

	return cur[m];
}
```
## Longest Increasing Subsequence
```cpp
template<typename T>
int lis(vi arr)
{
	int n = vi.size();
	vi ans;

	ans.push_back(nums[0]);

	for (int i = 1; i < n; i++) {
		if (nims[i] > ans.back())
			ans.push_back(nums[i]);
		else {
			int low = lower_bound(all(ans), nums[i]) - ans.begin();
			ans[low] = nums[i];
		}
	}
	return ans.size();
}
```
