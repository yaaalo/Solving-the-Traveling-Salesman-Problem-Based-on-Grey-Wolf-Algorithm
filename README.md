import numpy as np

# 距离矩阵
distance_matrix = np.array([
    [0, 10, 15, 20, 25],
    [10, 0, 35, 25, 30],
    [15, 35, 0, 30, 20],
    [20, 25, 30, 0, 10],
    [25, 30, 20, 10, 0]
])

# 计算路径长度
def calculate_path_length(path, distance_matrix):
    length = 0
    for i in range(len(path) - 1):
        length += distance_matrix[path[i]][path[i + 1]]
    length += distance_matrix[path[-1]][path[0]]  # 返回起点
    return length

# 初始化灰狼群体
population_size = 4
num_cities = len(distance_matrix)
wolves = [np.random.permutation(num_cities) for _ in range(population_size)]

# 灰狼算法参数
max_iter = 100
a = 2

for iteration in range(max_iter):
    fitness = [calculate_path_length(wolf, distance_matrix) for wolf in wolves]
    alpha_index = np.argmin(fitness)
    alpha = wolves[alpha_index].copy()
    beta_index = np.argsort(fitness)[1]
    beta = wolves[beta_index].copy()
    delta_index = np.argsort(fitness)[2]
    delta = wolves[delta_index].copy()

    for i in range(population_size):
        A1 = 2 * a * np.random.rand(num_cities) - a
        C1 = 2 * np.random.rand(num_cities)
        D_alpha = np.abs(C1 * alpha - wolves[i])
        X1 = alpha - A1 * D_alpha

        A2 = 2 * a * np.random.rand(num_cities) - a
        C2 = 2 * np.random.rand(num_cities)
        D_beta = np.abs(C2 * beta - wolves[i])
        X2 = beta - A2 * D_beta

        A3 = 2 * a * np.random.rand(num_cities) - a
        C3 = 2 * np.random.rand(num_cities)
        D_delta = np.abs(C3 * delta - wolves[i])
        X3 = delta - A3 * D_delta

        wolves[i] = (X1 + X2 + X3) / 3

    a = a - 2 / max_iter

# 输出最优解
best_path = alpha
best_length = fitness[alpha_index]
print("最优路径：", best_path)
print("路径长度：", best_length)
