<div style="font-family: Arial, sans-serif; line-height: 1.6; background:#f9f9f9; padding:20px; border-radius:10px;">
  <h2>🟤 Cinnamon Price Forecasting Project</h2>

  <h3>📊 About the Data</h3>
  <p>
    The project uses real cinnamon price data from <strong>2012–2025</strong>, including weekly/monthly prices, rainfall (mm), and export quantity (kg).  
    To fill missing months and enhance training, synthetic data is generated based on rainfall patterns, season, and price ranges per year.
  </p>

  <h3>🤖 Why LightGBM</h3>
  <ul>
    <li>Fast training and high performance on tabular data.</li>
    <li>Handles large datasets and missing values efficiently.</li>
    <li>Supports feature importance analysis for model interpretability.</li>
    <li>Produces stable and accurate predictions for time-series forecasting.</li>
  </ul>

  <h3>📈 Model Evaluation & Accuracy</h3>
  <p>
    The model is evaluated using:
  </p>
  <ul>
    <li><strong>RMSE:</strong> ~150–170 Rs (average prediction error)</li>
    <li><strong>R² Score:</strong> 0.96–0.97 (variance explained)</li>
    <li><strong>MAPE:</strong> 3–5% (average percentage error)</li>
  </ul>
  <p>
    Visualizations include actual vs predicted prices, feature importance, and correlation plots to understand model performance and key factors affecting cinnamon prices.
  </p>
</div>
