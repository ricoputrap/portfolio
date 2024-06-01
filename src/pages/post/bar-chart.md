---
layout: "../../layouts/post.astro"
title: Creating a Bar Chart using ChartJS
description: Representing the frequency or distribution of a set of discrete data points in different categories.
dateFormatted: June 1st, 2024
---

## 1. Set up a new Vite project
Open your terminal and run the following command to create a new Vite project:

```bash
npm init vite@latest chart-js-bar-chart --template react-ts
```

This command initializes a new Vite project with React and TypeScript template.

## 2. Install Chart JS and its types
Navigate to the project directory:

```bash
cd chart-js-bar-chart
```

Install **Chart.js** and its corresponding TypeScript types by running the following command:

```bash
npm install chart.js @types/chart.js
```

## 3. Create a BarChart component
Create a new file called `BarChart.tsx` in the src directory and add the following code:

```tsx
import React, { useEffect, useRef } from 'react';
import Chart from 'chart.js/auto';

interface BarChartProps {
  data: number[];
  labels: string[];
}

const BarChart: React.FC<BarChartProps> = ({ data, labels }) => {
  const chartRef = useRef<HTMLCanvasElement>(null);

  useEffect(() => {
    if (chartRef.current) {
      const ctx = chartRef.current.getContext('2d');
      if (ctx) {
        new Chart(ctx, {
          type: 'bar',
          data: {
            labels: labels,
            datasets: [
              {
                label: 'Data',
                data: data,
                backgroundColor: 'rgba(75, 192, 192, 0.2)',
                borderColor: 'rgba(75, 192, 192, 1)',
                borderWidth: 1,
              },
            ],
          },
          options: {
            responsive: true,
            scales: {
              y: {
                beginAtZero: true,
              },
            },
          },
        });
      }
    }
  }, [data, labels]);

  return <canvas ref={chartRef} />;
};

export default BarChart;
```

## 4. Use the BarChart component
Open the `src/App.tsx` file and replace its contents with the following code:

```tsx
import React from 'react';
import BarChart from './BarChart';

const App: React.FC = () => {
  const data = [10, 20, 30, 40, 50];
  const labels = ['Label 1', 'Label 2', 'Label 3', 'Label 4', 'Label 5'];

  return (
    <div className="App">
      <h1>Bar Chart Example</h1>
      <BarChart data={data} labels={labels} />
    </div>
  );
};

export default App;
```

## 5. Start the development server
```bash
Save the changes and run the following command in the terminal:
```

This starts the development server using Vite and opens the app in your browser. You should see a bar chart with the provided data.

That's it! You have successfully created a bar chart using Chart.js in React with TypeScript using Vite. You can customize the chart further by referring to the Chart.js documentation and exploring additional options and configurations.