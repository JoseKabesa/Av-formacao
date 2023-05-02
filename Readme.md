# Neste arquivo vou colocando as atividades que irei realizando da disciplina de avalição de formação

import numpy as np
import math as mt
import mpmath as mp
import matplotlib.pyplot as plt
from scipy import special as sp
# from pandas import dataframe

h = 10
phi = 0.25
ct = 0.0001
rw = 0.1
k = 50
mi = 1.5
pi = 100

Vw = pi*rw**2*10
cw = 0.0001

# C = Vw*cw
rd = 1
# Cd = C/(2*pi*phi*h*ct*rw**2)
Cd_v = [10, 10**2, 10**3, 10**4, 10**6]

n = 10000
t_v = np.linspace(10**2, 10**8, n)
s_v = [2, 8, 16, 22, 42]
pt = np.zeros(((len(t_v)), len(s_v)))
pcd = []

for c in range(len(Cd_v)):
    for s in range(len(s_v)):
        for t in range(len(t_v)):
            mp.dps = 1000
            mp.pretty = True


            def pd(u): return (sp.k0(rd * mt.sqrt(u)) + s*mt.sqrt(u)*sp.k1(mt.sqrt(u)))/(u*(mt.sqrt(u) * sp.k1(mt.sqrt(u)) +
                                                                                            Cd_v[c] * u * (sp.k0(
                        mt.sqrt(u)) + s_v[s]*mt.sqrt(u)*sp.k1(mt.sqrt(u)))))


            pt[t, s] = mp.invertlaplace(pd, t_v[t], method='stehfest', degree=20)
    pcd.append(pt.copy())

color = ['b', 'r', 'g', 'k', 'y', 'c']
marker = ['d', 'x', 'o', '+', '_']
for j in range(len(Cd_v)):
    for i in range(len(s_v)):
        plt.loglog(t_v, pcd[j][:, i], color=color[j], marker=marker[i])
plt.xlabel('td')
plt.ylabel('pd')
plt.show()