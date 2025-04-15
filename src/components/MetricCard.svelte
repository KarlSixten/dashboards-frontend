<script>
    export let title;
    export let value;
  </script>
  
  <div class="metric-card">
    <h2>{title}</h2>
    <p>{value}</p>
  </div>

  <style>
    .metric-card {
      background: #f9f9f9;
      border-radius: 1rem;
      padding: 1.5rem;
      text-align: center;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h2 {
      margin: 0;
      font-size: 1.2rem;
      color: #444;
    }
    p {
      margin: 0.5rem 0 0;
      font-size: 2rem;
      font-weight: bold;
      color: #ff5c28;
    }
  </style>