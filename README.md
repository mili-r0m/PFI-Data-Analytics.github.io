<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard PFI - Milagros Romeo</title>
    <script src="https://cdn.plot.ly/plotly-2.27.0.min.js"></script>
    <style>
        /* Estilos generales - Tema Oscuro */
        body {
            background-color: #121212;
            color: #ffffff;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
        }
        
        h1 {
            text-align: center;
            color: #ffffff;
            margin-bottom: 30px;
            font-weight: 300;
            letter-spacing: 1px;
        }

        /* Contenedor de KPIs */
        .kpi-container {
            display: flex;
            justify-content: space-between;
            gap: 20px;
            margin-bottom: 30px;
        }

        .kpi-card {
            background-color: #1e1e1e;
            border-radius: 10px;
            padding: 20px;
            flex: 1;
            text-align: center;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
            border: 1px solid #333;
        }

        .kpi-card h3 {
            margin: 0;
            font-size: 14px;
            color: #a0a0a0;
            text-transform: uppercase;
        }

        .kpi-card p {
            margin: 10px 0 0 0;
            font-size: 28px;
            font-weight: bold;
            color: #4C78A8; /* Azul principal */
        }

        .kpi-card .roi-value {
            color: #F58518; /* Naranja secundario */
        }

        /* Filtros */
        .filter-section {
            background-color: #1e1e1e;
            padding: 15px 20px;
            border-radius: 10px;
            margin-bottom: 30px;
            display: flex;
            align-items: center;
            gap: 15px;
            border: 1px solid #333;
        }

        select {
            background-color: #2d2d2d;
            color: white;
            border: 1px solid #444;
            padding: 8px 15px;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            outline: none;
        }

        /* Contenedor de Gráficos */
        .charts-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .chart-box {
            background-color: #1e1e1e;
            padding: 20px;
            border-radius: 10px;
            border: 1px solid #333;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
        }

        /* Responsive para pantallas chicas */
        @media (max-width: 900px) {
            .charts-container {
                grid-template-columns: 1fr;
            }
            .kpi-container {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>

    <h1>Dashboard de Rendimiento - PFI Milagros Romeo</h1>

    <!-- Filtros -->
    <div class="filter-section">
        <label for="canalFilter"><strong>Filtrar por Canal:</strong></label>
        <select id="canalFilter" onchange="actualizarDashboard()">
            <option value="Todos">Todos los Canales</option>
            <option value="RRSS">Redes Sociales (RRSS)</option>
            <option value="TV">Televisión (TV)</option>
            <option value="Email">Email Marketing</option>
        </select>
    </div>

    <!-- KPIs -->
    <div class="kpi-container">
        <div class="kpi-card">
            <h3>Ingresos Totales</h3>
            <p id="kpi-ingresos">$ 4,450,210</p>
        </div>
        <div class="kpi-card">
            <h3>Costo Total Marketing</h3>
            <p id="kpi-costos">$ 124,500</p>
        </div>
        <div class="kpi-card">
            <h3>ROI Promedio</h3>
            <p id="kpi-roi" class="roi-value">34.7%</p>
        </div>
    </div>

    <!-- Gráficos -->
    <div class="charts-container">
        <div class="chart-box">
            <div id="lineChart"></div>
        </div>
        <div class="chart-box">
            <div id="barChart"></div>
        </div>
    </div>

    <script>
        const layoutOscuro = {
            plot_bgcolor: '#1e1e1e',
            paper_bgcolor: '#1e1e1e',
            font: { color: '#ffffff' },
            xaxis: { gridcolor: '#333333', zerolinecolor: '#444444' },
            yaxis: { gridcolor: '#333333', zerolinecolor: '#444444' },
            margin: { t: 40, l: 50, r: 20, b: 50 }
        };

        function renderizarGraficos() {
            const meses = ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago', 'Sep', 'Oct', 'Nov', 'Dic'];
            const ventasEnCampana = [400, 450, 420, 580, 600, 750, 800, 810, 790, 850, 900, 950];
            const ventasFueraCampana = [350, 380, 360, 400, 420, 450, 460, 450, 480, 500, 520, 550];

            const trace1 = {
                x: meses, y: ventasEnCampana,
                type: 'scatter', mode: 'lines+markers',
                name: 'En Campaña',
                line: { color: '#4C78A8', width: 3 }
            };

            const trace2 = {
                x: meses, y: ventasFueraCampana,
                type: 'scatter', mode: 'lines+markers',
                name: 'Fuera de Campaña',
                line: { color: '#F58518', width: 3 }
            };

            const layoutLine = {
                ...layoutOscuro,
                title: 'Evolución Mensual de la Venta Promedio',
                legend: { orientation: 'h', y: -0.2 }
            };

            Plotly.newPlot('lineChart', [trace1, trace2], layoutLine, {responsive: true});

            const productos = ['Decoración', 'Heladeras', 'Secadoras', 'Lámparas', 'TVs'];
            const prodCampana = [850, 1200, 950, 450, 1500];
            const prodFuera = [500, 800, 600, 300, 900];

            const traceBar1 = {
                x: productos, y: prodCampana,
                type: 'bar',
                name: 'En Campaña',
                marker: { color: '#4C78A8' }
            };

            const traceBar2 = {
                x: productos, y: prodFuera,
                type: 'bar',
                name: 'Fuera de Campaña',
                marker: { color: '#F58518' }
            };

            const layoutBar = {
                ...layoutOscuro,
                title: 'Venta Promedio por Producto',
                barmode: 'group',
                legend: { orientation: 'h', y: -0.2 }
            };

            Plotly.newPlot('barChart', [traceBar1, traceBar2], layoutBar, {responsive: true});
        }

        function actualizarDashboard() {
            const filtro = document.getElementById("canalFilter").value;
            
            
            if (filtro === "RRSS") {
                document.getElementById("kpi-ingresos").innerText = "$ 1,250,000";
                document.getElementById("kpi-costos").innerText = "$ 45,000";
                document.getElementById("kpi-roi").innerText = "26.7%";
            } else if (filtro === "TV") {
                document.getElementById("kpi-ingresos").innerText = "$ 2,100,500";
                document.getElementById("kpi-costos").innerText = "$ 65,000";
                document.getElementById("kpi-roi").innerText = "31.3%";
            } else if (filtro === "Email") {
                document.getElementById("kpi-ingresos").innerText = "$ 1,099,710";
                document.getElementById("kpi-costos").innerText = "$ 14,500";
                document.getElementById("kpi-roi").innerText = "74.8%";
            } else {
                document.getElementById("kpi-ingresos").innerText = "$ 4,450,210";
                document.getElementById("kpi-costos").innerText = "$ 124,500";
                document.getElementById("kpi-roi").innerText = "34.7%";
            }

        
        }

        
        renderizarGraficos();
    </script>
</body>
</html>
