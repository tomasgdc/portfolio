---
layout: default
title: Traffic Dashboard
permalink: /traffic/
---

# Site Traffic Dashboard

<p>This page shows daily traffic, clones, and referrers for the site.</p>

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
  <div style="flex: 1; min-width: 150px; padding: 1rem; background: #ff8c00; color: white; border-radius: 8px; text-align:center;">
    <h3>Total Clones</h3>
    <p style="font-weight: bold; font-size: 1.5rem;">{{ site.data.traffic_full.clones | map: "count" | sum }}</p>
  </div>
  <div style="flex: 1; min-width: 150px; padding: 1rem; background: #32cd32; color: white; border-radius: 8px; text-align:center;">
    <h3>Unique Cloners</h3>
    <p style="font-weight: bold; font-size: 1.5rem;">{{ site.data.traffic_full.clones | map: "uniques" | sum }}</p>
  </div>
</div>

<hr />

## Daily Traffic

<canvas id="trafficChart" width="800" height="400"></canvas>

## Daily Clones

<canvas id="clonesChart" width="800" height="400" style="margin-top: 2rem;"></canvas>

## Top Referrers

<canvas id="referrersChart" width="800" height="400" style="margin-top: 2rem;"></canvas>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
  // -----------------------------
  // Traffic Data
  // -----------------------------
  const trafficData = {{ site.data.traffic_history | jsonify }};
  const trafficFull = {{ site.data.traffic_full | jsonify }};

  // -----------------------------
  // Views & Unique Visitors
  // -----------------------------
  if (trafficData.length > 0) {
    const labels = trafficData.map(d => d.date);
    const views = trafficData.map(d => d.views);
    const uniques = trafficData.map(d => d.uniques);

    const ctx = document.getElementById('trafficChart').getContext('2d');
    new Chart(ctx, {
      type: 'line',
      data: {
        labels,
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
            text: 'Daily Views & Unique Visitors',
            font: { size: 18 }
          },
          tooltip: { mode: 'index', intersect: false },
          legend: { position: 'top' }
        },
        interaction: { mode: 'nearest', axis: 'x', intersect: false },
        scales: {
          y: { beginAtZero: true, ticks: { stepSize: 1 } },
          x: { ticks: { maxRotation: 45, minRotation: 0 } }
        }
      }
    });
  }

  // -----------------------------
  // Clones & Unique Cloners
  // -----------------------------
  if (trafficFull.clones && trafficFull.clones.length > 0) {
    const cloneLabels = trafficFull.clones.map(d => d.timestamp.slice(0,10));
    const cloneCounts = trafficFull.clones.map(d => d.count);
    const cloneUniques = trafficFull.clones.map(d => d.uniques);

    const ctxClones = document.getElementById('clonesChart').getContext('2d');
    new Chart(ctxClones, {
      type: 'line',
      data: {
        labels: cloneLabels,
        datasets: [
          {
            label: 'Clones',
            data: cloneCounts,
            borderColor: 'rgba(33, 150, 243, 1)',
            backgroundColor: 'rgba(33, 150, 243, 0.2)',
            tension: 0.3,
            fill: true,
            pointRadius: 3
          },
          {
            label: 'Unique Cloners',
            data: cloneUniques,
            borderColor: 'rgba(156, 39, 176, 1)',
            backgroundColor: 'rgba(156, 39, 176, 0.2)',
            tension: 0.3,
            fill: true,
            pointRadius: 3
          }
        ]
      },
      options: {
        responsive: true,
        plugins: {
          title: { display: true, text: 'Daily Clones & Unique Cloners', font: { size: 18 } },
          tooltip: { mode: 'index', intersect: false },
          legend: { position: 'top' }
        },
        interaction: { mode: 'nearest', axis: 'x', intersect: false },
        scales: { y: { beginAtZero: true, ticks: { stepSize: 1 } }, x: { ticks: { maxRotation: 45, minRotation: 0 } } }
      }
    });
  }

  // -----------------------------
  // Referrers Chart
  // -----------------------------
  if (trafficFull.referrers && trafficFull.referrers.length > 0) {
    const refLabels = trafficFull.referrers.map(d => d.referrer);
    const refCounts = trafficFull.referrers.map(d => d.count);

    const ctxRef = document.getElementById('referrersChart').getContext('2d');
    new Chart(ctxRef, {
      type: 'bar',
      data: {
        labels: refLabels,
        datasets: [{
          label: 'Visits from Referrer',
          data: refCounts,
          backgroundColor: 'rgba(255, 193, 7, 0.7)',
          borderColor: 'rgba(255, 193, 7, 1)',
          borderWidth: 1
        }]
      },
      options: {
        responsive: true,
        plugins: {
          title: { display: true, text: 'Top Referrers', font: { size: 18 } },
          legend: { display: false }
        },
        scales: { y: { beginAtZero: true } }
      }
    });
  }
</script>