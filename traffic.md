---
layout: default
title: Traffic Dashboard
permalink: /traffic/
---

# Site Traffic Dashboard

<p>This page shows daily views and unique visitors for the site.</p>

<!-- Summary Cards -->
<div style="display: flex; flex-wrap: wrap; gap: 1rem; margin-bottom: 2rem;">
  <div style="flex: 1; min-width: 150px; padding: 1rem; background: #1e90ff; color: white; border-radius: 8px; text-align:center;">
    <h3>Total Views</h3>
    <p style="font-weight: bold; font-size: 1.5rem;">{{ site.data.traffic_total.views }}</p>
  </div>
  <div style="flex: 1; min-width: 150px; padding: 1rem; background: #8a2be2; color: white; border-radius: 8px; text-align:center;">
    <h3>Unique Visitors</h3>
    <p style="font-weight: bold; font-size: 1.5rem;">{{ site.data.traffic_total.uniques }}</p>
  </div>
</div>

<hr />

## Daily Traffic

<canvas id="trafficChart" width="800" height="400"></canvas>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
  // Load traffic data from merged snapshot
  const trafficData = {{ site.data.traffic_history | jsonify }};

  if (trafficData.length === 0) {
    console.warn("No traffic data available.");
  } else {
    const labels = trafficData.map(d => d.date);
    const views = trafficData.map(d => d.views);
    const uniques = trafficData.map(d => d.uniques);

    const ctx = document.getElementById('trafficChart').getContext('2d');
    new Chart(ctx, {
      type: 'line',
      data: {
        labels: labels,
        datasets: [
          {
            label: 'Views',
            data: views,
            borderColor: 'rgba(75, 192, 192, 1)',
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
            tension: 0.3,
            fill: true,
            pointRadius: 3
          },
          {
            label: 'Unique Visitors',
            data: uniques,
            borderColor: 'rgba(153, 102, 255, 1)',
            backgroundColor: 'rgba(153, 102, 255, 0.2)',
            tension: 0.3,
            fill: true,
            pointRadius: 3
          }
        ]
      },
      options: {
        responsive: true,
        plugins: {
          title: {
            display: true,
            text: 'Daily Traffic Overview',
            font: { size: 18 }
          },
          tooltip: {
            mode: 'index',
            intersect: false
          },
          legend: {
            position: 'top'
          }
        },
        interaction: {
          mode: 'nearest',
          axis: 'x',
          intersect: false
        },
        scales: {
          y: {
            beginAtZero: true,
            ticks: { stepSize: 1 }
          },
          x: {
            ticks: { maxRotation: 45, minRotation: 0 }
          }
        }
      }
    });
  }
</script>

<hr />
