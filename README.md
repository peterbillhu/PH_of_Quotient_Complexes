# Pesistent homology of quotient complexes (QCs)

![Quotient_Complex_Filtration_2](https://github.com/peterbillhu/PH_of_Quotient_Complexes/assets/28446650/14d968b0-bef8-4cb9-ab79-cdd03342d9e9)

## Introduction

In the field of algebraic topology, the cell complex offers a broader and more versatile description compared to the simplicial complex. Its structure allows for greater flexibility in theoretical arguments, avoiding the rigid face constraints inherent in the simplicial complex [4].

From a data science aspect, persistent homology (PH), based on a continuous deformation of topological spaces, is a novel way to estimate the global and local topology of the based manifold of a given point cloud data [5,6,7] and has been widely applied in many areas of image processing, biomolecule science, machine learning, and deep learning frameworks.

This project proposes a Gudhi-based [1] Python code for constructing and computing the PH of quotient complexes. In the current version, we mainly focus on cell complexes (i.e., quotient complexes (QCs)) built by abstract simplicial complexes and the quotient relation on their vertices (i.e., 0-simplexes). Especially, we also demonstrate the construction of Vietoris--Rips complexes as a filtration and their quotient version [8]. Based on the Gudhi package, the PH of the QC filtration can be calculated easily. The code for more general quotient complexes, such as the quotient relation of higher-dimensional simplices, will be consistently updated in this project.

The primary mathematical framework and released code were contributed by Dr. Kelin Xia[^1] and Dr. Chuan-Shen Hu in the School of Physical and Mathematical Sciences at Nanyang Technological University, Singapore. On the other hand, our in-preparation application of QC's PH on 2D perovskite materials[^2] also relies on the foundational code of this project, contributed by Chuan-Shen Hu[^3], Rishikanta Mayengbam[^4], Kelin Xia[^3], and Tze Chien Sum[^4].

[^1]: https://personal.ntu.edu.sg/xiakelin/index.html
[^2]: http://www.pdb.nmse-lab.ru/
[^3]: Division of Mathematical Sciences, School of Physical and Mathematical Sciences, Nanyang Technological University, Singapore 637371
[^4]: Division of Physics & Applied Physics, School of Physical and Mathematical Sciences, Nanyang Technological University, Singapore 637371

## Mathematical formulation

For a given topological filtration $\emptyset \subseteq X_1 \subseteq X_2 \subseteq \cdots \subseteq X_n$, $A \subseteq X$, and $\sim_A$ an equivalence relation on $A$, one can regard $\sim_A$ as an equivalence relation on each $X_i$. Actually, for each space $X_i$ one can define it as the least equivalence relation generated by the $\sim_A$, which is again denoted as $\sim_A$ by abusing the notations. 

Furthermore, based on the equivalence relation $\sim_A$ each $X_i$ admits the quotient space $X_i/\sim_A$. By the universal property of quotient spaces, the original filtration $\emptyset \subseteq X_1 \subseteq X_2 \subseteq \cdots \subseteq X_n$ induces the following filtration of quotient spaces:

$$\emptyset \subseteq X_1/\sim_A \subseteq X_2/\sim_A \subseteq \cdots \subseteq X_n/\sim_A,$$

where the inclusion relations are based on the canonical one-to-one and continuous maps $X_i/\sim_A \hookrightarrow X_{i+1}/\sim_A$ with $[x] \mapsto [x]$. Furthermore, the functoriality of homology induces the following persistent homology

$$0 \xrightarrow{} H_q(X_1/\sim_A) \xrightarrow{\psi_1} H_q(X_2/\sim_A) \xrightarrow{\psi_2} \cdots \xrightarrow{\psi_{n-1}} H_q(X_n/\sim_A)$$

of vector spaces over the binary field $\mathbb{Z}/2\mathbb{Z}$. We call it the _q-th persistent homology of the quotient space filtration_ with non-negative integer $q$.

From a computational perspective, this project focuses on the construction of _quotient complex (QC) filtration_ based on a filtration of simplicial complexes 

$$\emptyset \subseteq K_1 \subseteq K_2 \subseteq \cdots \subseteq K_n.$$

In particular, we consider $A$ as the vertex set of $K_1$, i.e., the set of $0$-simplices of $K_1$, and define an equivalence relation $\sim_A$ that ``glues'' some points of $A$ together. Based on the equivalence relation, we may define $\overline{K_i} = K_i/ \sim_A$ and obtain the following filtration of cell complexes:

$$\emptyset \subseteq \overline{K_1} \subseteq \overline{K_2} \subseteq \cdots \subseteq \overline{K_n}.$$

In this project, we develop a tool based on the Gudhi package [1] to construct and build the QC filtration and its persistent homology.

## Required packages

The core is programmed in Python language with the following TDA packages:

1.  Gudhi [1] (available on https://gudhi.inria.fr/index.html)
2.  Ripser [2] (available on https://ripser.scikit-tda.org/en/latest/)
3.  Persim [3] (available on https://persim.scikit-tda.org/en/latest/reference/index.html)

```python
!pip install ripser persim
!pip install gudhi
```
**Remark** It is important to highlight that the fundamental algorithms and functions developed for this project are exclusively built upon the Gudhi package. Nevertheless, certain tools for plotting and persistence diagram analysis may incorporate Ripser and Persim. Users who are specifically focused on the persistent homology of quotient complexes have the option to remove the code associated with Ripser and Persim.

## Released versions

PH_QC_v2023_07_24.py

## Important functions

Using the Gudhi package, there are two primary types of ways to produce a simplicial complex: using a simplicial tree structure or distance-based filtrations (such as Vietoris--Rips complexes and Alpha complexes). We first introduce the simplicial tree method for the construction. The following Python code depicts a toy example of the simplicial tree construction. 

```python
## Generate a simplicial tree:
import gudhi as gd
## Declaration of the simplicial tree 
st = gd.SimplexTree()
## Moment 0
st.insert([0], 0) ## 0-simplex
st.insert([1], 0) ## 0-simplex
st.insert([2], 0) ## 0-simplex
st.insert([3], 0) ## 0-simplex
st.insert([4], 0) ## 0-simplex
st.insert([5], 0) ## 0-simplex
## Moment 1
st.insert([0, 1], 1) ## 1-simplex
st.insert([0, 2], 1) ## 1-simplex
st.insert([1, 2], 1) ## 1-simplex
st.insert([3, 4], 1) ## 1-simplex
st.insert([3, 5], 1) ## 1-simplex
st.insert([4, 5], 1) ## 1-simplex
## Moment 2
st.insert([2, 3], 2) ## 1-simplex
## Moment 3
st.insert([0, 1, 2], 3) ## 2-simplex
## Moment 4
st.insert([3, 4, 5], 3) ## 2-simplex
```

The simplicial tree built above can be printed as follows:
>The simplex tree=  
>The simplicial complex is of dimension 2 - 15 simplices - 6 vertices.  
>[0] -> 0.00  
>[1] -> 0.00  
>[2] -> 0.00  
>[3] -> 0.00  
>[4] -> 0.00  
>[5] -> 0.00  
>[0, 1] -> 1.00  
>[0, 2] -> 1.00  
>[1, 2] -> 1.00  
>[3, 4] -> 1.00  
>[3, 5] -> 1.00  
>[4, 5] -> 1.00  
>[2, 3] -> 2.00  
>[0, 1, 2] -> 3.00  
>[3, 4, 5] -> 3.00  

This filtration of simplicial complexes can be represented by the following figure, where the right-hand side dashed region represents the labeled 6 vertices $v_0, v_1, v_2, v_3, v_4, v_5$ in the above example.

![Simplicial_complex_filtration_v2](https://github.com/peterbillhu/PH_of_Quotient_Complexes/assets/28446650/9cdfb830-b060-4b51-a52b-28b151008cf1)

Based on the filtration of simplicial complexes $V = K_0 \subseteq K_1 \subseteq K_2 \subseteq K_3 \subseteq K_4$, we can define an equivalence relation $\sim_V$ on $V$ and build the QC filtration $V/\sim_V \subseteq K_1/\sim_V \subseteq K_2/\sim_V \subseteq K_3/\sim_V \subseteq K_4/\sim_V$ as introduced in the ``Mathematical formulation'' section. For example, we can consider the equivalence relation $\sim = \sim_V$ on $V$ that is generated by the following relations:

$$v_0 \sim v_3, v_1 \sim v_4, \text{ and } v_2 \sim v_5.$$

Analog to the graphic representation as above, we can annotate the points $v_0, v_1, v_2, v_3, v_4, v_5$ by red, green, and blue colors. Points that share the same color if they are equivalent. Specifically, the groups of vertices $\{ v_0, v_3 \}$, $\{ v_1, v_4 \}$, and $\{ v_2, v_5 \}$ are colored by red, blue, and green.

![Simplicial_complex_filtration_colored_v2](https://github.com/peterbillhu/PH_of_Quotient_Complexes/assets/28446650/089d9dd6-c8c6-4ef4-ae9c-5add73e47294)

In the latest version of this project (PH_QC_v2023_12_12.py), we developed the function **get_point_quotient_complex** gluing points to produce this cell complex filtration. By gluing points with the same color, the filtration $\overline{K_0} \subseteq \overline{K_1} \subseteq \overline{K_2} \subseteq \overline{K_3} \subseteq \overline{K_4}$ (with $\overline{K_i} = K_i/\sim_V$) of cell complexes can be illustrated as the following figure.

![image](https://github.com/peterbillhu/PH_of_Quotient_Complexes/assets/28446650/7beb32d6-1a79-4346-b58b-fb56afb939a4)

In summary, with the simplicial tree defined above, we use the following code to compute the persistence barcode of the produced QC complex filtration. Here we use the code in version PH_QC_v2023_07_24.py as a demonstration.

```python
import PQSC_v2023_07_24 as QC_tool
QC_st = QC_tool.get_point_quotient_complex(st, quotient_dictionary)
QC_tool.compute_persistence_diagrams(new_st, max_Betti_degree=1, reduce_negative_barcodes=True)
```
The output would be as follows:

>[array([[ 0.,  1.],  
        [ 0.,  1.],  
        [ 0., inf]]),  
 array([[ 1.,  3.],  
        [ 1.,  3.],  
        [ 1., inf],  
        [ 1., inf],  
        [ 2., inf]])]  




## Google colab tutorial


## References

[1] Maria, Clément, et al. "The gudhi library: Simplicial complexes and persistent homology." Mathematical Software–ICMS 2014: 4th International Congress, Seoul, South Korea, August 5-9, 2014. Proceedings 4. Springer Berlin Heidelberg, 2014.

[2] Tralie, Christopher, Nathaniel Saul, and Rann Bar-On. "Ripser.py: A lean persistent homology library for python." Journal of Open Source Software 3.29 (2018): 925.

[3] Saul, Nathaniel, and Chris Tralie. "Scikit-tda: Topological data analysis for python." URL https://doi.org/10.5281/zenodo 2533369 (2019).

[4] Hatcher, Allen. "Algebraic Topology." (2000). Available on the website https://pi.math.cornell.edu/~hatcher/AT/AT.pdf.

[5] Carlsson, Gunnar. "Topology and data." Bulletin of the American Mathematical Society 46.2 (2009): 255-308.

[6] Carlsson, Gunnar, et al. "Persistence barcodes for shapes." Proceedings of the 2004 Eurographics/ACM SIGGRAPH symposium on Geometry processing. 2004.

[7] Ghrist, Robert. "Barcodes: the persistent topology of data." Bulletin of the American Mathematical Society 45.1 (2008): 61-75.

[8] Dey, Tamal K., Dayu Shi, and Yusu Wang. "Simba: An efficient tool for approximating rips-filtration persistence via sim plicial ba tch collapse." Journal of Experimental Algorithmics (JEA) 24 (2019): 1-16.
