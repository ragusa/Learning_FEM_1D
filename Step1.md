# Step-1

In [Step-1](./Learning_FEM_1D_step1.ipynb),  we do two upgrades to Step-0:
- the integrals for the reference-element matrices are computed using a numerical quadrature.
- material properties will now be piece-wise constant.

## Introduce a numerical quadrature

We introduce a numerical quadrature to perform the spatial integration **in the reference element**.
The numerical quadrature is a set of quadrature weights $w_q$ and quadrature abscissae $s_q$ on the reference element. The number of quadrature points $Q$ depends on the accuracy of the chosen quadrature,
$$
(w_q,s_q)_{1 \le q \le Q}
$$

That is, we compute the following integrals numerically:
- For the diffusion term:
$$ \int_{-1}^{+1}ds \, \frac{db_{\hat i}(s)}{ds}\frac{db_{\hat j}(s)}{ds} \approx \sum_{q=1}^Q w_q \left.\frac{db_{\hat i}}{ds}\right|_{s_q} \left.\frac{db_{\hat j}}{ds}\right|_{s_q}$$
- For the reaction term,
$$ \int_{-1}^{+1}ds\, b_{\hat i}(s) b_{\hat j}(s) \approx \sum_{q=1}^Q w_q  b_{\hat i}(s_q) b_{\hat j}(s_q)$$
- For the source term,
$$ \int_{-1}^{+1}ds\, b_{\hat i}(s) \approx \sum_{q=1}^Q w_q   b_{\hat i}(s_q)$$


Recall that the  **reference element** is, in 1D, just the interval $[-1,\,+1]$. The reference element length is 2. Other choices are possible but this one is the most common one.

The two **reference basis functions** restricted to the element: are:
$$ b_{\text{Left}}(s) = \frac{1-s}{2} $$
and
$$ b_{\text{Right}}(s) = \frac{1+s}{2} $$


## Mesh-dependent (zone-wise) properties

Up to now, the data $D$, $\sigma$ and $q$ was uniform.

We will introduce the concept of geometry zones or regions. For now, we assume that all regions will have the same width.

Let us say we have 2 materials, index by 0 and 1. An alternating layout in the geometry could be:
| name |  |  |  |  |
| :- | -: | -: | -: | -: |
| region ID | 0 | 1 | 2 | 3 |
|material ID | 0 | 1 | 0 | 1|

However, this is just the material zones. A mesh may need to be much finer. For example, each zone will be cut into 10 elements, so for the example above, we would have 4 regions and 40 elements (and 41 vertices).

For now, each region has the same width, so cutting up the geometry and making sure that the element edges align with the region edges is easy. We will use non uniform meshes in another Notebook.