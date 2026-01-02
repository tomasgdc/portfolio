---
layout: default
title: Traffic Dashboard
permalink: /traffic/
---

# Site Traffic Dashboard

<p>This page shows daily traffic, clones, and referrers for the site.</p>

<!-- Summary Cards -->
<div style="display: flex; flex-wrap: wrap; gap: 1rem; margin-bottom: 2rem;">

  <!-- Total Views -->
  <div style="flex: 1; min-width: 150px; padding: 1rem; background: #1e90ff; color: white; border-radius: 8px; text-align:center;">
    <h3>Total Views</h3>
    <p style="font-weight: bold; font-size: 1.5rem;">
      {{ site.data.traffic_total.views | default: 0 }}
    </p>
  </div>

  <!-- Unique Visitors -->
  <div style="flex: 1; min-width: 150px; padding: 1rem; background: #8a2be2; color: white; border-radius: 8px; text-align:center;">
    <h3>Unique Visitors</h3>
    <p style="font-weight: bold; font-size: 1.5rem;">
      {{ site.data.traffic_total.uniques | default: 0 }}
    </p>
  </div>

  <!-- Total Clones -->
  <div style="flex: 1; min-width: 150px; padding: 1rem; background: #ff8c00; color: white; border-radius: 8px; text-align:center;">
    <h3>Total Clones</h3>
    <p style="font-weight: bold; font-size: 1.5rem;">
      {% assign total_clones = 0 %}
      {% for c in site.data.traffic_full.clones %}
        {% assign total_clones = total_clones | plus: c.count %}
      {% endfor %}
      {{ total_clones | default: 0 }}
    </p>
  </div>

  <!-- Unique Cloners -->
  <div style="flex: 1; min-width: 150px; padding: 1rem; background: #32cd32; color: white; border-radius: 8px; text-align:center;">
    <h3>Unique Cloners</h3>
    <p style="font-weight: bold; font-size: 1.5rem;">
      {% assign total_unique_clones = 0 %}
      {% for c in site.data.traffic_full.clones %}
        {% assign total_unique_clones = total_unique_clones | plus: c.uniques %}
      {% endfor %}
      {{ total_unique_clones | default: 0 }}
    </p>
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
  // Safe data defaults
  // -----------------------------
  {% if site.data.traffic_history %}
    const trafficData = {{ site.data.traffic_history | jsonify }};
  {% else %}
    const trafficData = [];
  {% endif %}

  {% if site.data.traffic_full %}
    const trafficFull = {{ site.data.traffic_full | jsonify }};
  {% else %}
    const trafficFull = {};
  {% endif %}

  const clonesData = trafficFull.clones || [];
  const referrersData = trafficFull.referrers || [];

  // -----------------------------
  // Views & Unique Visitors
  // -----------------------------
  const trafficLabels = trafficData.length ? trafficData.map(d => d.date) : ['No Data'];
  const trafficViews = trafficData.length ? trafficData.map(d => d.views) : [0];
  const trafficUniques = trafficData.length ? trafficData.map(d => d.uniques) : [0];

  const ctxTraffic = document.getElementById('trafficChart').getContext('2d');
  new Chart(ctxTraffic, {
    type: 'line',
    data: {
      labels: trafficLabels,
      datasets: [
        {
          label: 'Views',
          data: trafficViews,
          borderColor: 'rgba(75, 192, 192, 1)',
          backgroundColor: 'rgba(75, 192, 192, 0.2)',
          tension: 0.3,
          fill: true,
          pointRadius: 3
        },
        {
          label: 'Unique Visitors',
          data: trafficUniques,
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
        title: { display: true, text: 'Daily Views & Unique Visitors', font: { size: 18 } },
        tooltip: { mode: 'index', intersect: false },
        legend: { position: 'top' }
      },
      interaction: { mode: 'nearest', axis: 'x', intersect: false },
      scales: { y: { beginAtZero: true, ticks: { stepSize: 1 } }, x: { ticks: { maxRotation: 45, minRotation: 0 } } }
    }
  });

  // -----------------------------
  // Clones & Unique Cloners
  // -----------------------------
  const cloneLabels = clonesData.length ? clonesData.map(d => d.timestamp ? d.timestamp.slice(0,10) : '') : ['No Data'];
  const cloneCounts = clonesData.length ? clonesData.map(d => d.count) : [0];
  const cloneUniques = clonesData.length ? clonesData.map(d => d.uniques) : [0];

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

  // -----------------------------
  // Referrers Chart
  // -----------------------------
  const refLabels = referrersData.length ? referrersData.map(d => d.referrer) : ['No Referrers'];
  const refCounts = referrersData.length ? referrersData.map(d => d.count) : [0];

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
</script>