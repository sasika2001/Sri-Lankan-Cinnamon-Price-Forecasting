<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Cinnamon Price Forecasting — README</title>
  <style>
    body { font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; line-height:1.6; margin: 0; padding: 24px; background: #f7f8fb; color:#111;}
    .container { max-width: 900px; margin: 0 auto; background: #fff; padding: 28px; border-radius: 12px; box-shadow: 0 6px 24px rgba(30,40,60,0.06); }
    h1{ margin-top:0; font-size: 28px; }
    h2{ color:#0f172a; font-size:20px; margin-bottom:6px; }
    p.lead { color:#334155; }
    code, pre { background:#0f172a10; padding:6px; border-radius:6px; font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, "Roboto Mono", monospace; }
    pre { overflow:auto; }
    ul { margin-top:0; }
    .badge { display:inline-block; padding:6px 10px; margin-right:8px; background:#eef2ff; color:#3730a3; border-radius:999px; font-size:13px; }
    .meta { font-size:13px; color:#374151; }
    .example { background:#fafafa; padding:12px; border-radius:8px; border:1px solid #eef2f7; }
    footer { margin-top:26px; font-size:13px; color:#475569; }
    a { color:#0f62fe; text-decoration:none; }
    .commands { display:grid; gap:8px; grid-template-columns:1fr; }
  </style>
</head>
<body>
  <div class="container">
    <h1>🟤 Cinnamon Price Forecasting — Project README</h1>
    <p class="lead">Forecast cinnamon prices using a LightGBM model trained on a mixture of real historical data (2012–2025) and generated synthetic data. Includes preprocessing, feature engineering (rainfall, export volume, season), model training, hyperparameter tuning, and visualizations.</p>

    <div class="meta">
      <span class="badge">Python</span>
      <span class="badge">LightGBM</span>
      <span class="badge">Pandas</span>
      <span class="badge">Matplotlib</span>
    </div>

    <h2>Table of contents</h2>
    <ul>
      <li><a href="#overview">Overview</a></li>
      <li><a href="#dataset">Dataset</a></li>
      <li><a href="#features">Features</a></li>
      <li><a href="#preprocessing">Preprocessing</a></li>
      <li><a href="#modeling">Modeling</a></li>
      <li><a href="#metrics">Key metrics & interpretation</a></li>
      <li><a href="#run">How to run</a></li>
      <li><a href="#files">Repo files</a></li>
      <li><a href="#results">Results & visuals</a></li>
      <li><a href="#license">License</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>

    <h2 id="overview">Overview</h2>
    <p>Goal: predict cinnamon price (in Rs) per time unit (week/month) using environmental & trade-related features. The project uses real recorded prices for years 2012–2025 (where available) and synthesizes additional samples to create a richer dataset for model training and scenario experiments.</p>

    <h2 id="dataset">Dataset</h2>
    <ul>
      <li><strong>Real data:</strong> historical cinnamon prices (2012–2025), rainfall (mm), export_kg, year, month/week.</li>
      <li><strong>Synthetic data:</strong> generated to fill gaps and simulate scenarios; carefully blended so the distribution matches real data ranges.</li>
      <li><strong>File formats:</strong> CSV files (e.g. <code>cinnamon_real.csv</code>, <code>cinnamon_synthetic.csv</code>, merged into <code>cinnamon_merged.csv</code>).</li>
    </ul>

    <h2 id="features">Main features</h2>
    <ul>
      <li><code>Week / Date</code> — time index</li>
      <li><code>Rainfall_mm</code> — rainfall for the period</li>
      <li><code>Export_kg</code> — export volume</li>
      <li><code>Season</code> — derived (e.g., monsoon / dry)</li>
      <li><code>Lag features</code> — previous price values (lag1, lag4) for time-series signal</li>
    </ul>

    <h2 id="preprocessing">Preprocessing</h2>
    <ol>
      <li>Parse date column (<code>pd.to_datetime</code>), sort by time.</li>
      <li>Fill missing price values with interpolation or forward-fill (documented in notebook).</li>
      <li>Create seasonal indicators and lag features.</li>
      <li>Train–test split preserving temporal order (no random shuffle for time-series).</li>
      <li>Scale features if needed (LightGBM often handles raw numeric features fine).</li>
    </ol>

    <h2 id="modeling">Modeling</h2>
    <p>Model: <strong>LightGBM</strong> (gradient boosting). Typical pipeline used:</p>
    <div class="example">
      <pre><code># pseudocode overview
1. Prepare X_train, y_train, X_valid, y_valid
2. Create lgb.Dataset objects
3. Train with early_stopping on validation
4. Perform grid/random search for hyperparameters (learning_rate, max_depth, num_leaves, min_data_in_leaf)
5. Save best model and produce predictions
</code></pre>
    </div>

    <h2 id="metrics">Key metrics & interpretation</h2>
    <p>Metrics shown in results:</p>
    <ul>
      <li><strong>RMSE</strong> (root-mean-square error) — average prediction error in Rs (lower is better).</li>
      <li><strong>R²</strong> (coefficient of determination) — proportion of variance explained (closer to 1 is better).</li>
      <li><strong>MAPE</strong> (mean absolute percentage error) — average percent error (useful for relative accuracy).</li>
    </ul>
    <p class="meta"><em>Example (from recent runs): RMSE ≈ 150–170 Rs, R² ≈ 0.96–0.97, MAPE ≈ 3–5%.</em></p>

    <h2 id="run">How to run (quick)</h2>
    <p>Clone repo, set up virtual env, install requirements, then run training notebook or script.</p>
    <div class="commands">
      <pre><code># clone
git clone https://github.com/yourusername/cinnamon-forecast.git
cd cinnamon-forecast

# create env & install
python -m venv .venv
# Windows
.venv\Scripts\activate
# mac/linux
source .venv/bin/activate

pip install -r requirements.txt

# run training script or notebook
python train_model.py
# or
jupyter lab
</code></pre>
    </div>

    <h2 id="files">Repository structure</h2>
    <pre><code>
README.html
requirements.txt
data/
  cinnamon_real.csv
  cinnamon_synthetic.csv
  cinnamon_merged.csv
notebooks/
  01_data_prep.ipynb
  02_train_and_tune.ipynb
scripts/
  train_model.py
  generate_synthetic.py
  evaluate.py
models/
  best_model.txt
figures/
  actual_vs_predicted.png
</code></pre>

    <h2 id="results">Results & visualizations</h2>
    <p>Include these plots in <code>figures/</code> (notebooks produce them):</p>
    <ul>
      <li>Actual vs Predicted (line chart)</li>
      <li>Feature importance (LightGBM)</li>
      <li>Yearly average price trend</li>
      <li>Scatter: <code>Export_kg</code> vs <code>Price_Rs</code>, and <code>Rainfall_mm</code> vs <code>Price_Rs</code></li>
      <li>Correlation heatmap</li>
    </ul>

    <h2 id="notes">Short script for printing predicted values (example)</h2>
    <pre><code># evaluate.py snippet
import pandas as pd
import joblib

model = joblib.load('models/best_model.pkl')
X_test = pd.read_csv('data/X_test.csv')
preds = model.predict(X_test)
print('Predicted prices (first 10):')
print(preds[:10])
</code></pre>

    <h2 id="tips">Tips & best practices</h2>
    <ul>
      <li>Always keep a time-ordered validation set — do not randomly shuffle for time-series.</li>
      <li>Document how synthetic data was created (seed, ranges, method) so results are reproducible.</li>
      <li>Use SHAP or LightGBM feature importance to explain model decisions.</li>
      <li>Log experiments (e.g., MLflow or simple CSV logs) with hyperparameters and metrics.</li>
    </ul>

    <h2 id="license">License</h2>
    <p>Choose a license (e.g., MIT). Add <code>LICENSE</code> file. Example: <code>MIT</code>.</p>

    <h2 id="contact">Contact</h2>
    <p>If you have questions or want to improve the repo structure, open an issue or contact: <strong>your-email@example.com</strong></p>

    <footer>
      <p>Created for the cinnamon price forecasting project — include your name, affiliation, and any acknowledgements here.</p>
    </footer>
  </div>
</body>
</html>
