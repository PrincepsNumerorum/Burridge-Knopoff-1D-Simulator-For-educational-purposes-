# Burridge-Knopoff 1D Model Simulator

## English

### Overview
This is an educational earthquake simulator based on the **Burridge-Knopoff model (1D)**, implemented in pure HTML5 and JavaScript. It visualizes how strain energy accumulates along a fault and is released through sudden slip events (earthquakes).

> **Disclaimer:** This project is for **educational and entertainment purposes only**. The physics have been significantly simplified for browser-based real-time simulation. It is **not suitable for professional research or seismic hazard prediction**. The author assumes no responsibility for any damages or issues arising from the use of this software.

### Algorithm & Mathematical Model
The simulation solves the equations of motion for $N$ interconnected blocks using the **4th-order Runge-Kutta method (RK4)**.

* **Equation of Motion:**
    $$m_i \frac{d^2u_i}{dt^2} = K(Vt - u_i) + k(u_{i+1} - 2u_i + u_{i-1}) - F(\text{slip state}) - \eta \frac{du_i}{dt}$$
* **Friction Model:** A simplified "Static-Dynamic Friction" model. A block starts sliding when spring force exceeds static friction $F_s$, and is resisted by dynamic friction $F_d$ during sliding.
* **Magnitude Calculation:** Based on the total moment $M_0 = \mu A \cdot \sum \text{slip}$, then converted to Moment Magnitude ($M_w$).

### Key Parameters
| Constant | Meaning |
| :--- | :--- |
| `V` | Plate convergence velocity (Loading rate) |
| `k` | Leaf spring constant (Interaction between blocks) |
| `K` | Main spring constant (Tectonic loading) |
| `Fs` / `Fd` | Static / Dynamic friction force (Asperity strength) |
| `Radiation Damping` | Energy loss through seismic wave radiation |

---

## 日本語

### 概要
このプロジェクトは、地震発生の物理的なメカニズムを理解するための**Burridge-Knopoff（バネ・ブロック）モデル**を用いた1次元シミュレータです。断層における歪みエネルギーの蓄積と、高速なすべり（地震）による解放のプロセスをブラウザ上で可視化します。

> **免責事項:** 本ツールは**教育・エンターテインメント目的**で作成されており、ブラウザでの動作を優先して物理モデルを極端に簡略化しています。**研究用途や実際の地震予知・予測には適しません。** 本シミュレータを利用したことによるいかなる損害についても、製作者は一切の責任を負いません。

### アルゴリズムと使用数式
$N$個のブロックの運動方程式を**4次ルンゲ＝クッタ法 (RK4)**を用いて数値的に解いています。

* **運動方程式:**
    $$m_i \frac{d^2u_i}{dt^2} = K(Vt - u_i) + k(u_{i+1} - 2u_i + u_{i-1}) - F(\text{摩擦}) - \eta \frac{du_i}{dt}$$
* **摩擦モデル:** 静止摩擦係数 $F_s$ を超えると滑り出し、運動中は動摩擦係数 $F_d$ が作用する簡略化された摩擦モデルを採用しています。
* **マグニチュード計算:** 合計すべり量から地震モーメント $M_0 = \mu A \cdot \sum \text{slip}$ を算出し、モーメントマグニチュード ($M_w$) に変換しています。

### 主な定数・係数の意味
| 定数・係数 | 意味 |
| :--- | :--- |
| `V` | プレート沈み込み速度（外部からの載荷速度） |
| `k` | ブロック間のバネ定数（隣接セグメントとの相互作用） |
| `K` | プレートバネ定数（地殻の剛性・歪みの溜まりやすさ） |
| `Fs` / `Fd` | 静止 / 動摩擦力（アスペリティの強度・断層の固着強度） |
| `Radiation Damping` | 放射減衰（地震波として散逸するエネルギー） |

### 機能
* **アスペリティ編集:** 実行中にブロックを直接クリック（またはShift+クリック）することで、その地点の摩擦強度（アスペリティ）をリアルタイムに変更可能。
* **時空間分布図:** 地震の連動範囲を可視化。
* **データ出力:** 発生した地震の履歴（開始時間、継続時間、 震央、マグニチュード等）をCSV形式でエクスポート可能。

---
