+++
title = 'Matrix C++ Container'
date = 2023-10-04T10:00:00-04:00
draft = false
+++

Following last week's ICPC contest I spent all week trying to make a C++ matrix
class template we could use for the commpetitions.

The problem was a very basic fibonacci-like recurrence relation of the form:
`f(n) = f(n-1) + f(n-2) + 2 * n * n + 5`

The main issue with this was the fact that the largest input size was 10^18.
Computers on average (mildly lower than average nowadays) can do around 10^8
cycles per second.
Which left us with an estimate ~10^10 seconds left to compute the largest
fibonacci number...
Though that may be impossible there is an algorithm to compute any recurrence
relation in big-O log time.
I will not go into detail on how to do that (though I might just make another
blog post about it), [here is a link to an amazing article](https://comeoncodeon.wordpress.com/2011/05/08/recurrence-relation-and-matrix-exponentiation/)
on how to create a fibonacci matrix based on any recurrence relation.

Code:
```c++
struct Matrix {
	Matrix(sz r, sz c) : data(vi(r * c, 0)), cols(c), rows(r)
	{
	}

	int& operator[](sz i)
	{
		return data[i];
	}

	int& operator()(sz r, sz c)
	{
		return data[r * cols + c];
	}

	int operator()(sz r, sz c) const
	{
		return data[r * cols + c];
	}

	vi get_row(sz r) const
	{
		auto ret = vi(cols, 0);
		sz i = 0;
		for (auto& e : ret) {
			e = data[rows * r + i];
			i++;
		}
		return ret;
	}

	vi get_col(sz c) const
	{
		auto ret = vi(rows, 0);
		sz i = 0;
		for (auto& e : ret) {
			e = data[cols * i + c];
			i++;
		}
		return ret;
	}

	void print() const
	{
		for (sz i = 0; i < rows; i++) {
			for (sz j = 0; j < cols; j++)
				cout << (*this)(i, j) << " ";
			cout << endl;
		}
	}

	Matrix operator*(const Matrix& rhs) const
	{
		auto out = Matrix(rhs.rows, cols);
		for (sz i = 0; i < cols; i++) {
			auto sel_row = get_row(i);
			for (sz j = 0; j < rhs.rows; j++) {
				auto sel_col = rhs.get_col(j);
				int sum = inner_product(sel_row.begin(),
							sel_row.end(),
							sel_col.begin(), 0);
				out(i, j) = sum;
			}
		}
		return out;
	}

	void operator*=(const Matrix& rhs)
	{
		for (sz i = 0; i < cols; i++) {
			auto sel_row = get_row(i);
			for (sz j = 0; j < rhs.rows; j++) {
				auto sel_col = rhs.get_col(j);
				int sum = inner_product(sel_row.begin(),
							sel_row.end(),
							sel_col.begin(), 0);
				(*this)(i, j) = sum;
			}
		}
	}

	vi data;
	sz cols;
	sz rows;
};
```

# Links
- Here is a link to the matrix github repo: 
