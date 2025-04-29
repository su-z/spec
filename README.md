# SPEC

A library to compute the spectrum of an infinite-dimensional operator, optimized for accuracy and speed, written in rust.

Some source codes are to be released later. See dependencies like `ztensor` and `type-generic-umfpack` in my profile page.

## The problem

It is always tricky to find numerically the spectrum of an infinite matrix (which usually is the representation of an operator in $\ell^2(\mathbb Z), \ell^2(\mathbb N)$). The naive approach of finite-dimensional truncation simply fails to work. For example, the right shift operator (the matrix with 1 on its superdiagonal and 0 everywhere else) has unit circle on $\mathbb C$ as its spectrum for the case of $\ell^2(\mathbb N),$ and the unit disk in the case of $\ell^2(\mathbb N).$ Unfortunately, if we just truncate the first $n\times n$ components, we end up with a Jordan block, which has eigenvalue $\{0\}$ only.

This has cause endless problems of mathematicians, physicists and other people who are working with operators whose spectrum is very difficult to compute. In general, we cannot recover the spectrum accurately from finitely many entries of the matrix, but under suitable assumptions and an improved algorithm, we can easily avoid lots of errors, like what's in the example above, to reduce bad surprises significantly.

Most of fundamental ideas of the algorithms are given in the following paper, which can be found [here](https://arxiv.org/abs/1508.03280).

*Ben-Artzi, J., Colbrook, M. J., Hansen, A. C., Nevanlinna, O., & Seidel, M. (2015). Computing Spectra--On the Solvability Complexity Index Hierarchy and Towers of Algorithms. arXiv preprint arXiv:1508.03280.*

The algorithm proposed in the original paper is sufficient to achieve nice theoretical results, but for the simplicity of presentation, they do not include implementation details which could make them much faster and very practical. This is the purpose of this repository.

## The Solution

By improving accuracy (compare to naive matrix truncations), do we have to lose some speed? **The answer is NO.** The naive truncation tries to find all eivenvalues of a huge matrix, which is cubersome (typically $O(n^3)$ via QR factorization). It is one of the non-computable problems people have to compute in practice. With algorithm in this repo, the complexity is (in some sense) roughly $O(n^2)$ only, and fully takes advantage of sparsity. In fact it is many times faster.

## What this package includes

The codebase contains:

1. Implementation of the main spec algorithm.
2. And impelementation of a fast solver for smallest singular value of a large, sparse matrix.
3. Generic data structure `ZTensor` for infinite matrices and generic computation interfaces which are very flexible.
4. Auxillary tools.
