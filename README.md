# Burridge-Knopoff 1D Model Simulator

---

## English

### Overview
This is an educational earthquake simulator based on the **Burridge-Knopoff model (1D)**, implemented in pure HTML5 and JavaScript. It visually reproduces the physical mechanisms of earthquake generation, specifically how elastic strain energy accumulates along a fault line due to tectonic plate motion, and how it is rapidly released through sudden slip events (earthquakes).

> **Disclaimer:** This project is created for **educational and entertainment purposes only**. The physics have been heavily simplified to prioritize real-time performance in a web browser. It is **not suitable for professional research or actual seismic hazard prediction**. Real fault zones contain complex distributions of stable sliding areas and slow-slip regions, unlike this simplified uniformly locked model. The author assumes no responsibility for any damages or issues arising from the use of this software.

### Features & Controls
* **Real-time Asperity Editing:** Click (or tap) directly on the blocks in the 1D model canvas to increase their friction strength $F_s$. Shift+Click decreases it.
* **Preset Distributions:** Choose from various fault profiles (Giant Central Asperity, Scattered Strong Asperities, Uniform Fault, Generally Weak Fault).
* **Adjustable Parameters:** Real-time control over Plate Subduction Velocity ($V$), Max/Min Fault Strength, Fluctuation/Noise level, and Event Clustering Timeout.
* **Data Export/Import:** * Export the full earthquake event history and slip distributions as CSV files for further analysis.
    * Save and Load custom Asperity Distributions (block strengths) via CSV.

### Visualizations & Insights
1.  **Spring-Block Model 1D Cross Section:** Displays the current state of the fault. Block colors indicate asperity strength (red = stronger locking). Strain gauges show accumulated energy. You can visually understand where strain is building up and how ruptures propagate across the fault.
2.  **Spatiotemporal Rupture Distribution:** A time-space map plotting earthquakes. Red dots indicate epicenters. Green areas represent ruptures of $M_w \le 5.5$, and red areas represent mega-quakes of $M_w > 5.5$. Useful for identifying seismic gaps and recurrence intervals.
3.  **G-R Law (Magnitude-Frequency Distribution):** A log-log graph automatically calculating the Gutenberg-Richter law. It determines the magnitude of completeness ($M_c$) and estimates the $b$-value using Aki's Maximum Likelihood Estimation.
4.  **Total Strain Energy:** Tracks elastic strain energy over time, perfectly visualizing the "Elastic Rebound Theory."
5.  **Cumulative Slip (Avg):** Shows whether the fault slip is smooth (creep) or sudden and step-like (large earthquakes).
6.  **M-T Diagram:** A time-series plot of earthquake magnitudes, instantly showing when and how large seismic events occurred.

### Algorithm & Mathematical Model
The simulation solves the equations of motion for $N$ interconnected blocks using the **4th-order Runge-Kutta method (RK4)** with a fixed timestep ($dt=0.005$) and internal substeps for stability.

* **Equation of Motion:**
    $$m_i \frac{d^2u_i}{dt^2} = K(Vt - u_i) + k(u_{i+1} - 2u_i + u_{i-1}) - F(\text{slip state}) - \eta \frac{du_i}{dt}$$
* **Friction Model:** A simplified "Static-Dynamic Friction" model. A block starts sliding when the combined spring force exceeds the static friction $F_s$. During sliding, it is resisted by dynamic friction $F_d$ (set to $0.4 \times F_s$).
* **Event Clustering:** If a block slips and an adjacent block is also slipping simultaneously, they are clustered into a single "earthquake event" based on a specified timeout threshold.
* **Magnitude Calculation:** Based on the total seismic moment $M_0 = \mu A \times (\text{Average Slip})$, which is then converted to Moment Magnitude:
    $$M_w = \frac{2}{3}(\log_{10}M_0 - 9.1)$$

### Key Parameters
| Constant / Variable | Meaning |
| :--- | :--- |
| `V` | Plate subduction velocity (Tectonic loading rate) |
| `k` | Inter-block spring constant (Interaction/Rigidity between segments) |
| `K` | Pulling spring constant (Coupling to the driving plate) |
| `Fs` / `Fd` | Static / Dynamic friction force (Fault strength / Asperity) |
| `Radiation Damping` ($\eta$) | Energy dissipation factor representing seismic wave radiation |

---

## 日本語

### 概要
本プロジェクトは、地震発生の物理的なメカニズムを視覚的に理解するための**Burridge-Knopoff（バネ・ブロック）モデル**を用いた1次元シミュレータです。HTML5とJavaScriptのみで実装されており、断層における弾性歪みエネルギーの蓄積と、限界に達した際の高速なすべり（地震）によるエネルギー解放のプロセスをブラウザ上でリアルタイムに再現・可視化します。

> **免責事項:** 本ツールは**教育・エンターテインメント目的**で作成されており、ブラウザでのリアルタイム動作を優先して物理モデルを極端に簡略化しています。**研究用途や実際の地震予知・予測には適しません。** 実際の地震発生場では、定常すべり領域やスロー地震領域などが複雑に分布しており、一様に固着しているわけではありません。本シミュレータを利用したことによるいかなる損害についても、製作者は一切の責任を負いません。

> **【アスペリティという用語について】** > 本シミュレーターでは、断層面上において周囲よりも固着が強く、プレート運動に対して「滑り」が遅れている領域（滑り欠損領域）を便宜上「アスペリティ」と呼称しています。

### 操作方法と機能
* **直感的な断層強度の操作:** 実行中、1次元断面図上のブロックを直接クリック（スマホではタップ）することで、その地点の摩擦強度 $F_s$ を増加させることができます。Shiftキーを押しながらクリックすると強度を減少できます。
* **プリセット:** 「中央巨大アスペリティ」「極大アスペリティ点在」「均質な断層」「全体的に弱い断層」など、多様な初期状態からシミュレーションを開始できます。
* **パラメータ変更:** ブロック数（最大50個）、プレート沈み込み速度（$V$）、断層強度の最大値・最小値・ノイズ強度などをスライダーで動的に変更可能です。
* **データ出力・読み込み (CSV):**
  * 発生したすべての地震イベント履歴（開始時間、継続時間、震央、関与ブロック、最大すべり量、総すべり量、Mw）や、各イベントの詳細な滑り分布をCSVでエクスポート可能。
  * 作成したアスペリティ分布（各ブロックの強度設定）をCSVで保存・読み込み可能。

### 各グラフ・図の表示内容と得られる知見
1. **バネ・ブロックモデル 1次元断面図**
   * **内容:** バネで繋がれたブロック群の現在の状態。ブロックの色は赤に近いほど固着が強い（アスペリティ）ことを示し、上のゲージは歪みの蓄積度合いを示します。
   * **得られること:** 歪みがどこに集中しているか、破壊がどのように伝播・停止するかが視覚的に把握できます。
2. **地震の時空間分布（震源域の分布）**
   * **内容:** 横軸にブロック位置、縦軸に時間をとった時空間図です。赤い点は「震央」を示し、緑の矩形は $M_w5.5$ 以下の震源域、赤い矩形は $M_w5.5$ 以上の巨大地震の震源域を表します。
   * **得られること:** 単独ブロックのすべりか、連動した巨大地震かといった破壊の成長過程や、しばらく地震が起きていない「地震空白域」、再来周期の規則性などを読み取れます。
3. **G-R則 (規模別頻度分布)**
   * **内容:** 地震規模と発生回数の関係を示す両対数グラフ（グーテンベルク・リヒター則）。
   * **得られること:** データから完全性マグニチュード($M_c$)を自動判定し、Akiの最尤法で直線の傾き「b値」を算出して赤破線で描画します。固着状態の強弱によって大地震の割合（b値）がどう変化するかを観察できます。
4. **全歪みエネルギー**
   * **内容:** 断層全体に蓄えられている弾性ひずみエネルギーの総量（バネの伸びの二乗和）の時間変化。
   * **得られること:** プレート運動によってゆっくりとエネルギーが蓄積され、地震の発生と共に一気に解放される過程が確認できます。
5. **累積すべり量(平均)**
   * **内容:** 断層全体の平均的な絶対変位（累積すべり量）のグラフ。
   * **得られること:** 長期的には沈み込み速度と同じ傾きで増加しますが、大地震時にステップ状に急上昇するか、滑らかに増加するかの断層の振る舞いの違いが分かります。
6. **M-T図 (Magnitude-Time)**
   * **内容:** 時間経過に対する発生地震のマグニチュードをプロットした時系列図。

### シミュレーション手法とアルゴリズム
$N$個のブロックの運動方程式を、計算安定性の高い**4次ルンゲ＝クッタ法 (RK4)**を用いて数値積分（時間刻み $dt=0.005$、サブステップ10回）しています。

* **運動方程式:**
    $$m_i \frac{d^2u_i}{dt^2} = K(Vt - u_i) + k(u_{i+1} - 2u_i + u_{i-1}) - F(\text{摩擦}) - \eta \frac{du_i}{dt}$$
* **摩擦モデル:** 単純な「静止・動摩擦モデル」を採用。バネから受ける力 $F_{\text{spring}}$ が静止摩擦力 $F_s$ を超えるとブロックが滑り出し、滑り中は一定の動摩擦力 $F_d$（本モデルでは $F_s \times 0.4$ に設定）と、地震波によるエネルギー散逸を模した放射減衰項 $\eta \frac{du_i}{dt}$ による抵抗を受けます。
* **イベントのクラスタリング:** 速度が0を超えて滑り始めたブロックを検知し、隣接するブロックが同時に滑っていれば物理的な因果関係があるとみなして「同一の地震イベント」として結合（クラスタリング）します。判定猶予（タイムアウト）パラメータによってイベントの区切りを調整しています。
* **マグニチュードの算出:** 1イベントで滑った各ブロックのすべり量と面積から地震モーメントを算出し、モーメントマグニチュードへ換算しています。
    $$M_0 = \mu A \sum (\text{すべり量})$$
    $$M_w = \frac{2}{3}(\log_{10}M_0 - 9.1)$$

### 主な定数・係数の意味
| 定数・係数 | 意味 |
| :--- | :--- |
| `V` | プレート沈み込み速度（外部からの載荷速度） |
| `k` | ブロック間のバネ定数（隣接セグメントとの相互作用・剛性） |
| `K` | プレート引張バネ定数（下部地殻やスラブからの引っ張り強度） |
| `Fs` / `Fd` | 静止 / 動摩擦力（アスペリティの強度・断層の固着強度） |
| `Radiation Damping` | 放射減衰係数（地震波として空間へ散逸するエネルギーの割合） |
