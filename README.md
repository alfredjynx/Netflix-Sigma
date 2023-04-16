# Netflix-Sigma

### Auto Vetores e Auto Valores

Para uma matriz A não nula, temos autovetores e autovalores que são possíveis de serem calculados. Eles são características que nos permitem modificar e transladar pontos, modificando características da matriz original conforme essas matrizes oriundas são modificadas.

Com eles podemos decompor e recompor uma matriz da seguinte maneira: 

$$
Ax = x \lambda
$$

No exemplo, $A$ é nossa matriz original, $x$ é nossa matriz de auto vetores (colunas), e $\lambda$ é nossa matriz de autovalores. Nessa representação fica claro que uma matriz de autovetores multiplicando a matriz original é equivalente a mesma matriz de autovetores sendo multiplicada por uma matriz de autovalores.


Vamos supor que possuímos uma matriz de dois autovetores, um em cada coluna. Junto a eles, temos dois autovalores. Além disso, possuímos nossa matriz original ($A$). Esse sistema pode ser escrito na forma de uma multiplicação matricial, se assumirmos que nossos auto-vetores são vetores-coluna:

$$
A \begin{bmatrix} x_1 & x_2 \end{bmatrix} = \begin{bmatrix} x_1 & x_2 \end{bmatrix} \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} 
$$

Multiplicando os dois lados da equação por $\begin{bmatrix} x_1 & x_2 \end{bmatrix}^{-1}$, ficamos com:

$$
A \begin{bmatrix} x_1 & x_2 \end{bmatrix}\begin{bmatrix} x_1 & x_2 \end{bmatrix}^{-1} = \begin{bmatrix} x_1 & x_2 \end{bmatrix} \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} \begin{bmatrix} x_1 & x_2 \end{bmatrix}^{-1}
$$

Isso é necessário para isolarmos nossa matriz original, já que uma matriz vezes a sua inversa é igual a identidade, ou seja, descarta a necessedidade de continuarmos a levar em consideração ambas as matrizes na conta. Dessa maneira, ficamos com a seguinte equação:
$$
A = \begin{bmatrix} x_1 & x_2 \end{bmatrix} \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} \begin{bmatrix} x_1 & x_2 \end{bmatrix}^{-1}
$$

Para sistemas mais complexos, nos teríamos mais autovetores e mais autovalores, mas a ordem do processo continua a mesma. Em termos amplos:

$$
A = x \lambda x^{-1}
$$

Para chegarmos na utilidade do processo, vemos a matriz de covariância, que relaciona variáveis entre si. Cada variável é representada por uma coluna nessa matriz. Calculamos uma matriz de covariância da seguinte maneira: 
$$
C = A A^T
$$

Ao decompormos essa matriz de covariância e modificarmos os seus autovetores e autovalores, conseguimos reduzir as variáveis necessárias para a visualização de nossos dados. O processo é verdadeiro para matrizes quadradas, sendo chamado de PCA (Principal Components Analysis). 

Para englobar toda forma de matriz, temos o SVD (Singular Value Decomposition). 

Na decomposição SVD, usamos a formulação:

$
A = U \Sigma V^T,
$

onde:

* As colunas de $U$ são os auto-vetores de $A^T A$,
* As colunas de $V$ (e, portanto, as linhas de $V^T$) são auto-vetores de $A A^T$,
* $\Sigma$ é uma matriz onde $s_{i,i}$ é a raiz quadrada dos auto-valores de $A^T A$ ou de $A A^T$.

O SVD tem aplicações diferentes do PCA. O PCA busca reduzir o número de variáveis correlacionadas, facilitando a visualização de informações. O SVD é utilizado em diversas aplicações, mas a que nos interessa é a redução de ruído. 

Supondo que há uma imagem que possui pixeis esfumaçados ou esteja um pouco ganulada. Podemos representar esses pixeis em uma matriz, que chamaremos de $P$. Conseguimos calcular $U$, $S$ e $V^{T}$ de $P$.

$$
U, S, V^{T} = SVD(P) 
$$

No python, ficaria assim. 