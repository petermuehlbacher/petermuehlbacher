---
title: "Diffusion Maps"
about: "A brief motivation and implementation in Python of dimensionality reduction via the diffusion maps algorithm."

---
For an implementation with examples, see [this notebook](https://colab.research.google.com/drive/1GpRyvkh025iUB9BhPABLJvCs5_6BM1rT?usp=sharing). 

Diffusion maps refer to a non-linear dimensionality reduction algorithm. The key ideas are as follows:

* Assume data \\(x\\) is sampled from \\((X,\mathcal A,\mathbb P)\\) according to \\(\mathbb P\\), 
  * e.g. \\(X=\mathbb R^{10000}\\), but \\(\mathbb P\\) has full support on some low-dimensional submanifold.
* Given a symmetric kernel (e.g. Gaussian) \\(k(x,y)=k(y,x)\geq 0\\), we define
  * \\(d(x)=\int k(x,y)d\mathbb P(y)\\) (akin to the degree of a vertex in a graph, where \\(k\\) would be connectivity) and
  * \\(p(x,y)={k(x,y)\over d(x)}\\) (akin to transition probabilities of a random walk on the data manifold).
* Let \\(K_{ij}=k(x_i,x_j)\\) and \\(D_{ii}=\sum_j K_{ij}\\) (and 0 elsewhere; this is an empirical estimation of \\(d(x_i)\\)) and define 
  * \\(M=D^{-1}K\\) (akin to the "left (random-walk) normalized Laplacian matrix").
* Now \\[M_{ij}^t = \sum_l\lambda_l^t\psi_l(x_i)\phi_l(x_j)=\mathbb P(X_t=x_j\mid X_0=x_i)\\] is the probability of a random walk on our data to reach \\(x_j\\) from \\(x_i\\) in \\(t\\) steps; here \\(\psi,\phi\\) are the right- and left eigenvectors of \\(M\\), respectively).
* Define the diffusion distance (big if lots of short paths from \\(x_i\\) to \\(x_j\\) and vice versa) \\[D_t(x_i,x_j)^2:=\sum_k\frac{(M^t_{ik}-M^t_{jk})^2}{\phi_0(x_k)} = \sum_{l}\lambda_l^{2t}(\psi_l(x_i)-\psi_l(x_j))^2 = \|\Psi_t(x_i)-\Psi_t(x_j)\|^2,\\]
where \\(\Psi_t(x)=(\lambda_1^t\psi_1(x),\lambda_2^t\psi_2(x),\dots)\\).
  * Note that we do not include \\(\Psi_0\\), the eigenvector corresponding to eigenvalue 1!
* The last equality suggests using (the first few coordinates of) \\(\Psi_t(x)\\) as low-dimensional embeddings, since their Euclidean distance approximates the geodesic distance on the underlying manifold.
* \\(t\\) is a free parameter; intuitively bigger \\(t\\) corresponds to checking similarity on larger and larger scales, ignoring the microscopic structure.