---
layout: default
title: Process Scheduling
---

# Process Scheduling Gantt Chart

<div class="gantt-chart">
  {% for process in site.data.processes %}
    <div class="process" style="left: {{ process.start }}px; width: {{ process.duration }}px; background-color: {{ process.color }};">
      {{ process.name }}
    </div>
  {% endfor %}
</div>
