# Damping-vibration
# 阻尼振動可視化代碼介紹

以下代碼利用 `numpy` 和 `matplotlib` 庫生成 ​**2行3列** 的子圖，展示不同阻尼條件下的振動響應曲線。所有圖表共享相同的時間軸（0-5秒，步長0.002秒）。

## 代碼結構

### 1. 導入庫與通用參數

```python

```

import numpy as np
import matplotlib.pyplot as plt

# 通用參數

t = np.arange(0, 5, 0.002)  # 時間：0~5秒，步長0.002秒
omega_n_default = 5.0        # 默認無阻尼自然頻率

# 創建2行3列的子圖

fig, axes = plt.subplots(2, 3, figsize=(20, 10))
plt.subplots_adjust(wspace=0.3, hspace=0.4)  # 調整子圖間距

# --------------------------

# 圖1: 欠阻尼 (ζ=0.1)

# --------------------------

zeta_u = 0.1
omega_d = omega_n_default * np.sqrt(1 - zeta_u**2)
A = 1.0
B = (zeta_u * omega_n_default) / omega_d
x_under = np.exp(-zeta_u * omega_n_default * t) * (A * np.cos(omega_d * t) + B * np.sin(omega_d * t))

axes[0, 0].plot(t, x_under, color='red')
axes[0, 0].set_title('Underdamped (ζ=0.1)')
axes[0, 0].set_ylabel('Amplitude')
axes[0, 0].set_ylim(-1, 1)
axes[0, 0].grid(True)

# --------------------------

# 圖2: 臨界阻尼 (ζ=1.0)

# --------------------------

zeta_c = 1.0
x_critical = (1 + omega_n_default * t) * np.exp(-omega_n_default * t)

axes[0, 1].plot(t, x_critical, color='blue')
axes[0, 1].set_title('Critical Damping (ζ=1)')
axes[0, 1].set_ylim(0, 1)
axes[0, 1].grid(True)

# --------------------------

# 圖3: 過阻尼 (ζ=1.5)

# --------------------------

zeta_o = 1.5
r1 = (-zeta_o + np.sqrt(zeta_o**2 - 1)) * omega_n_default
r2 = (-zeta_o - np.sqrt(zeta_o**2 - 1)) * omega_n_default
C1 = r2 / (r2 - r1)
C2 = r1 / (r1 - r2)
x_over = C1 * np.exp(r1 * t) + C2 * np.exp(r2 * t)

axes[0, 2].plot(t, x_over, color='green')
axes[0, 2].set_title('Overdamped (ζ=1.5)')
axes[0, 2].set_ylim(0, 1)
axes[0, 2].grid(True)

# --------------------------

# 圖4: 欠阻尼比較 (ω_d=25)

# --------------------------

omega_d_target = 25.0  # 指定阻尼自然頻率
for zeta in [0.1, 0.05]:
omega_n = omega_d_target / np.sqrt(1 - zeta**2)
B = (zeta * omega_n) / omega_d_target
x = np.exp(-zeta * omega_n * t) * (np.cos(omega_d_target * t) + B * np.sin(omega_d_target * t))
axes[1, 0].plot(t, x, label=f'ζ={zeta}')

axes[1, 0].set_title('Underdamped Comparison (ω_d=25)')
axes[1, 0].set_xlabel('Time (s)')
axes[1, 0].set_ylabel('Amplitude')
axes[1, 0].set_ylim(-1, 1)
axes[1, 0].legend()
axes[1, 0].grid(True)

# --------------------------

# 圖5: 臨界阻尼 vs 過阻尼

# --------------------------

axes[1, 1].plot(t, x_critical, color='blue', label='Critical (ζ=1)')
axes[1, 1].plot(t, x_over, color='green', linestyle='--', label='Overdamped (ζ=1.5)')
axes[1, 1].set_title('Critical vs Overdamped')
axes[1, 1].set_xlabel('Time (s)')
axes[1, 1].set_ylabel('Amplitude')
axes[1, 1].set_ylim(0, 1)
axes[1, 1].legend()
axes[1, 1].grid(True)

# --------------------------

# 圖6: 過阻尼比較 (不同ζ值)

# --------------------------

for zeta in [1.5, 2.0, 3.0]:
r1 = (-zeta + np.sqrt(zeta**2 - 1)) * omega_n_default
r2 = (-zeta - np.sqrt(zeta**2 - 1)) * omega_n_default
C1 = r2 / (r2 - r1)
C2 = r1 / (r1 - r2)
x_over_comp = C1 * np.exp(r1 * t) + C2 * np.exp(r2 * t)
axes[1, 2].plot(t, x_over_comp, linestyle='--', label=f'ζ={zeta}')

axes[1, 2].set_title('Overdamped Comparison')
axes[1, 2].set_xlabel('Time (s)')
axes[1, 2].set_ylabel('Amplitude')
axes[1, 2].set_ylim(0, 1)
axes[1, 2].legend()
axes[1, 2].grid(True)

# 統一設置橫軸和刻度

for ax_row in axes:
for ax in ax_row:
ax.set_xlim(0, 5)
ax.set_xticks(np.arange(0, 5.1, 0.5))
ax.set_yticks(np.arange(-1, 1.1, 0.2) if ax.get_ylim()[0] < 0 else np.arange(0, 1.1, 0.2))

plt.tight_layout()
plt.show()

```

```

# 通用參數

t = np.arange(0, 5, 0.002)        # 時間軸：0~5秒
omega_n_default = 5.0             # 默認無阻尼自然頻率 (rad/s)
fig, axes = plt.subplots(2, 3, figsize=(20, 10))

```

```
