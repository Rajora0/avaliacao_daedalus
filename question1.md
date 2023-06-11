# Relatório: Trajetória de Veículo Utilizando ICP

[Confira o projeto no Google Colab](https://colab.research.google.com/drive/1irgcT1dC6QW5TC9ews4bC0O00r47pQoV?usp=sharing)

## Introdução

O objetivo deste projeto é estimar a trajetória de um veículo utilizando nuvens de pontos obtidas a partir de um sensor LiDAR. Foi utilizado o conjunto de dados KITTI DATASET e o algoritmo Iterative Closest-Points (ICP) para estimar a trajetória percorrida pelo carro. 

## Carregamento dos Dados

Os arquivos .obj contendo as nuvens de pontos foram carregados utilizando a biblioteca Trimesh, enquanto as matrizes de transformação ground-truth foram obtidas a partir do arquivo .npy, usando a biblioteca NumPy.

``` python
import trimesh
import numpy as np

member):ground_truth_transformation = np.load("ground_truth.npy")
```

## Visualização da Trajetória Ground-Truth

A trajetória ground-truth foi plotada utilizando a biblioteca Matplotlib e o pyplot:

``` python
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

x_ground_truth = ground_truth_transformation[:, 0, 3]
y_ground_truth = ground_truth_transformation[:, 1, 3]
z_ground_truth = ground_truth_transformation[:, 2, 3]

ax.plot(x_ground_truth, y_ground_truth, z_ground_truth, label="Trajetória Ground-Truth")
ax.set_xlabel("X")
ax.set_ylabel("Y")
ax.set_zlabel("Z")
ax.legend()

plt.show()
```

## Implementação do Algoritmo ICP

O algoritmo Iterative Closest Points (ICP) foi implementado para estimar a matriz de transformação, utilizando o método de estimação "point_to_point". Para fazer isso, o traço utilizado foi o Open3D, porém, a implementação do ICP foi feita do zero.

``` python
def registration_icp(source, target, max_correspondence_distance=1e-6,
                     init=np.eye(4), estimation_method="point_to_point",
                     criteria=ICPConvergenceCriteria()):
    transformation = init
    prev_error = np.inf

    for _ in range(criteria.max_iteration):
        source_transformed = np.dot(source, transformation[:3, :3].T) + transformation[:3, 3]
        source_correspondence, target_correspondence, valid_distances = find_correspondences(source_transformed, target, max_correspondence_distance)

        if len(source_correspondence) == 0 or len(target_correspondence) == 0:
            print("No corresponding points found. Exiting ICP loop.")
            break

        if estimation_method == "point_to_point":
            delta_transformation = compute_point_to_point_transformation(source_correspondence, target_correspondence)
            transformation = np.dot(delta_transformation, transformation)
        else:
            raise NotImplementedError("Only 'point_to_point' estimation method is implemented")

        error = np.mean(valid_distances)
        delta_error = prev_error - error
        prev_error = error

        if delta_error < criteria.relative_fitness:
            break

    return transformation
```

## Estimando a Trajetória

A trajetória foi estimada utilizando a função que implementa o algoritmo ICP. Cada nuvem de pontos foi transformada e registrada em relação à primeira nuvem de pontos do conjunto de dados.

``` python
max_iter = 50
tol = 1e-6
criteria = ICPConvergenceCriteria(max_iteration=max_iter, relative_fitness=tol)

transformations = [np.eye(4)]  # A transformação da primeira nuvem de pontos é a matriz identidade
for i in tqdm(range(1, len(point_clouds))):
    source = point_clouds[i]
    target = point_clouds[0]
    transform = registration_icp(source, target, criteria=criteria)
    transformations.append(transform)

transformations_array = np.array(transformations)
```

## Comparação dos Resultados

Cada transformação estimada foi comparada com sua correspondente na matriz ground-truth. A visualização dos resultados foi feita através de um gráfico 3D, utilizando a biblioteca Matplotlib e o pyplot:

``` python
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

x = transformations_array[:, 0, 3]
y = transformations_array[:, 1, 3]
z = transformations_array[:, 2, 3]

ax.view_init(elev=-150, azim=120)
ax.plot(x, y, z, color='red', label=f"Trajetória Estimada com ICP Original: {error:.6f})")

ax.set_xlabel("X")
ax.set_ylabel("Y")
ax.set_zlabel("Z")
ax.legend()

plt.show()
```

## Conclusão

Foi possível estimar a trajetória do veículo utilizando o algoritmo ICP com uma implementação própria, sem depender diretamente de bibliotecas de terceiros. A trajetória estimada foi visualizada em um gráfico 3D e comparada com a trajetória ground-truth, mostrando resultados consistentes e próximos da realidade. Um dos desafios desse projeto foi a implementação correta do algoritmo ICP e sua adaptação ao conjunto de dados fornecido. No entanto, o resultado final mostrou-se satisfatório, cumprindo os objetivos propostos.
