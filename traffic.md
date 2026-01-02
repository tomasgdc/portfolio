---
layout: default
title: Traffic
permalink: /traffic/
---

# Site Traffic Dashboard

<p>This page shows daily views and unique visitors for the site.</p>

## Visitors Summary

{% if site.data.traffic_total %}
<p style="font-weight: 500; font-size: 0.9rem; color: #555;">
👀 Total Views: {{ site.data.traffic_total.views }} ·
👤 Unique Visitors: {{ site.data.traffic_total.uniques }}
</p>
{% else %}
<p style="font-weight: 500; font-size: 0.9rem; color: #555;">
Visitor statistics not available yet.
</p>
{% endif %}

---

## Traffic Graph

<canvas id="trafficChart" width="600" height="300"></canvas>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
    // Build trafficData from _data/traffic_history.json
    const trafficData = [
    {% for day in site.data.traffic_history %}
    { date: "{{ day.date }}", views: {{ day.views }}, uniques: {{ day.uniques }} }{% unless forloop.last %},{% endunless %}
    {% endfor %}
    ];
    
    // Only draw chart if there is data
    if (trafficData.length > 0) {
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
                tension: 0.3
            },
            {
                label: 'Unique Visitors',
                data: uniques,
                borderColor: 'rgba(153, 102, 255, 1)',
                backgroundColor: 'rgba(153, 102, 255, 0.2)',
                tension: 0.3
            }
            ]
        },
        options: {
            responsive: true,
            plugins: {
            legend: { position: 'top' },
            title: { display: true, text: 'Daily Traffic' }
            },
            scales: {
            y: { beginAtZero: true, ticks: { stepSize: 1 } }
            }
        }
        });
    }
</script>