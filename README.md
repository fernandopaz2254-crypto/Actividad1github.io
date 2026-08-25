<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tutorial Interactivo: Pruebas de Hipótesis (Prueba Z)</title>

    <!-- ========================================== -->
    <!-- 2. ESTILOS CSS                             -->
    <!-- ========================================== -->
    <style>
        /* Variables de color y diseño */
        :root {
            --primary-dark: #1e293b;
            --primary-blue: #2563eb;
            --primary-light: #eff6ff;
            --accent-blue: #3b82f6;
            --bg-main: #f8fafc;
            --card-bg: #ffffff;
            --text-main: #0f172a;
            --text-muted: #64748b;
            --border-color: #e2e8f0;
            --success-bg: #f0fdf4;
            --success-border: #86efac;
            --success-text: #166534;
            --fail-bg: #fef2f2;
            --fail-border: #fca5a5;
            --fail-text: #991b1b;
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            --radius: 12px;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background-color: var(--bg-main);
            color: var(--text-main);
            line-height: 1.6;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        /* Encabezado */
        header {
            background-color: var(--primary-dark);
            color: white;
            padding: 24px;
            border-radius: var(--radius);
            margin-bottom: 24px;
            box-shadow: var(--shadow);
        }

        header h1 {
            font-size: 1.8rem;
            margin-bottom: 8px;
            font-weight: 700;
        }

        header p {
            color: #cbd5e1;
            font-size: 0.95rem;
        }

        .hypothesis-box {
            display: flex;
            gap: 16px;
            background: rgba(255, 255, 255, 0.08);
            padding: 12px 16px;
            border-radius: 8px;
            margin-top: 16px;
            font-size: 0.9rem;
            flex-wrap: wrap;
        }

        .hypothesis-box div {
            flex: 1;
            min-width: 250px;
        }

        /* Layout Principal: Dos columnas */
        .grid-layout {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 24px;
            margin-bottom: 24px;
        }

        .card {
            background: var(--card-bg);
            border-radius: var(--radius);
            padding: 24px;
            box-shadow: var(--shadow);
            border: 1px solid var(--border-color);
        }

        .card-title {
            font-size: 1.25rem;
            color: var(--primary-dark);
            margin-bottom: 16px;
            padding-bottom: 8px;
            border-bottom: 2px solid var(--primary-light);
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        /* Formularios y Controles */
        .form-group {
            margin-bottom: 16px;
        }

        label {
            display: block;
            font-size: 0.875rem;
            font-weight: 600;
            margin-bottom: 6px;
            color: var(--primary-dark);
        }

        .help-text {
            font-size: 0.8rem;
            color: var(--text-muted);
            margin-bottom: 6px;
        }

        select, input, textarea {
            width: 100%;
            padding: 10px 12px;
            border: 1px solid var(--border-color);
            border-radius: 6px;
            font-size: 0.95rem;
            color: var(--text-main);
            background-color: #fff;
            transition: border-color 0.2s;
        }

        select:focus, input:focus, textarea:focus {
            outline: none;
            border-color: var(--primary-blue);
        }

        textarea {
            height: 100px;
            resize: vertical;
            font-family: monospace;
            font-size: 0.85rem;
        }

        .read-only-input {
            background-color: var(--primary-light);
            font-weight: bold;
            color: var(--primary-blue);
        }

        .btn {
            background-color: var(--primary-blue);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 6px;
            font-weight: 600;
            cursor: pointer;
            width: 100%;
            transition: background-color 0.2s;
            font-size: 1rem;
        }

        .btn:hover {
            background-color: #1d4ed8;
        }

        .btn-secondary {
            background-color: #e2e8f0;
            color: var(--primary-dark);
            margin-bottom: 12px;
        }

        .btn-secondary:hover {
            background-color: #cbd5e1;
        }

        .generator-controls {
            display: flex;
            gap: 12px;
            align-items: flex-end;
            margin-bottom: 12px;
        }

        .generator-controls .form-group {
            margin-bottom: 0;
            flex: 1;
        }

        /* Tarjetas de Resultados */
        .results-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 16px;
            margin-bottom: 20px;
        }

        .metric-card {
            background-color: var(--primary-light);
            border-radius: 8px;
            padding: 16px;
            text-align: center;
        }

        .metric-card .title {
            font-size: 0.75rem;
            text-transform: uppercase;
            letter-spacing: 0.05em;
            color: var(--text-muted);
            font-weight: 700;
        }

        .metric-card .value {
            font-size: 1.8rem;
            font-weight: 800;
            color: var(--primary-blue);
            margin-top: 4px;
        }

        /* Consola de Comparación */
        .console-box {
            background-color: #0f172a;
            color: #38bdf8;
            font-family: monospace;
            padding: 14px;
            border-radius: 8px;
            font-size: 1.1rem;
            text-align: center;
            margin-bottom: 20px;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.5);
        }

        /* Caja de Conclusión */
        .conclusion-box {
            padding: 16px;
            border-radius: 8px;
            font-size: 0.95rem;
            line-height: 1.5;
            border-left: 5px solid;
        }

        .conclusion-box.reject {
            background-color: var(--success-bg);
            border-color: var(--success-text);
            color: var(--success-text);
        }

        .conclusion-box.fail-reject {
            background-color: var(--fail-bg);
            border-color: var(--fail-text);
            color: var(--fail-text);
        }

        /* Sección Inferior: Procedimiento Matemático */
        .math-section {
            background: var(--card-bg);
            border-radius: var(--radius);
            padding: 24px;
            box-shadow: var(--shadow);
            border: 1px solid var(--border-color);
        }

        .steps-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 20px;
            margin-bottom: 24px;
        }

        .step-card {
            background: #f8fafc;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 16px;
        }

        .step-card h4 {
            font-size: 0.95rem;
            color: var(--primary-dark);
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .step-card .step-number {
            background-color: var(--primary-blue);
            color: white;
            border-radius: 50%;
            width: 22px;
            height: 22px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            font-size: 0.75rem;
        }

        .formula {
            font-family: "Courier New", Courier, monospace;
            background: #e2e8f0;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 0.9rem;
            margin: 6px 0;
            display: inline-block;
        }

        .math-result {
            font-weight: bold;
            color: var(--primary-dark);
            margin-top: 6px;
            font-size: 0.95rem;
        }

        /* Visualización de Canvás */
        .visualization-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #fff;
            padding: 16px;
            border-radius: 8px;
            border: 1px solid var(--border-color);
        }

        canvas {
            max-width: 100%;
            height: auto;
        }

        .canvas-caption {
            font-size: 0.9rem;
            font-weight: bold;
            color: var(--primary-dark);
            margin-top: 10px;
        }

        /* 11. DISEÑO RESPONSIVO */
        @media (max-width: 850px) {
            .grid-layout {
                grid-template-columns: 1fr;
            }
            
            .generator-controls {
                flex-direction: column;
                align-items: stretch;
            }
            
            .btn-secondary {
                margin-bottom: 0;
            }
        }
    </style>
</head>
<body>

<div class="container">
    <!-- ========================================== -->
    <!-- 1. ESTRUCTURA HTML - ENCABEZADO            -->
    <!-- ========================================== -->
    <header>
        <h1>Tutorial Interactivo de Pruebas de Hipótesis</h1>
        <p>Caso Práctico: ¿La nueva estrategia de anuncios pagados aumenta la tasa de conversión?</p>
        
        <div class="hypothesis-box">
            <div><strong>Hipótesis Nula (H₀):</strong> μ ≤ 8% (La nueva estrategia no aumenta la conversión; se mantiene en 8%).</div>
            <div><strong>Hipótesis Alternativa (H₁):</strong> μ > 8% (La nueva estrategia aumenta la tasa de conversión por encima de 8%).</div>
        </div>
    </header>

    <!-- LAYOUT PRINCIPAL (DOS COLUMNAS) -->
    <div class="grid-layout">
        
        <!-- COLUMNA IZQUIERDA: CONFIGURACIÓN Y SIMULADOR -->
        <section class="card">
            <h2 class="card-title">1. Configuración del Experimento</h2>
            
            <!-- Selector de Alfa -->
            <div class="form-group">
                <label for="alphaSelect">Nivel de Significancia (α):</label>
                <select id="alphaSelect">
                    <option value="0.01">0.01 (Confianza 99%)</option>
                    <option value="0.05" selected>0.05 (Estándar - Confianza 95%)</option>
                    <option value="0.10">0.10 (Confianza 90%)</option>
                </select>
                <div class="help-text">Establece el límite para el riesgo tolerable de cometer un error Tipo I (falso positivo).</div>
            </div>

            <!-- Tasa de Referencia -->
            <div class="form-group">
                <label for="mu0">Tasa de Referencia Histórica (μ₀):</label>
                <input type="text" id="mu0" class="read-only-input" value="8%" readonly>
                <div class="help-text">Representa el rendimiento histórico promedio (8%) de la estrategia tradicional de anuncios.</div>
            </div>

            <!-- Generador Automático -->
            <div class="generator-controls">
                <div class="form-group">
                    <label for="centerMean">Alrededor de (%):</label>
                    <input type="number" id="centerMean" value="11" step="0.5" min="0" max="100">
                </div>
                <button type="button" class="btn btn-secondary" onclick="generateRandomSample()">Generar 20 Campañas al Azar</button>
            </div>
            <div class="help-text">Simula datos de nuevas campañas centrados cerca de un valor estimado para probar el sistema.</div>

            <!-- Textarea de Datos -->
            <div class="form-group" style="margin-top: 12px;">
                <label for="dataInput">Tasas de Conversión de la Muestra (%):</label>
                <textarea id="dataInput" placeholder="Introduce valores separados por comas (ej. 9.5, 11.2, 8.8)"></textarea>
                <div class="help-text">Puedes editar manualmente los porcentajes de conversión de cada campaña separándolos por comas.</div>
            </div>

            <!-- Botón Principal -->
            <button type="button" class="btn" onclick="runAnalysis()">Analizar Datos y Tomar Decisión</button>
        </section>

        <!-- COLUMNA DERECHA: RESULTADOS DEL ANÁLISIS -->
        <section class="card">
            <h2 class="card-title">2. Resultados del Análisis</h2>
            
            <div class="results-grid">
                <div class="metric-card">
                    <div class="title">Valor P Calculado</div>
                    <div class="value" id="pValueDisplay">--</div>
                </div>
                <div class="metric-card">
                    <div class="title">Tu Nivel Alfa (α)</div>
                    <div class="value" id="alphaDisplay">0.05</div>
                </div>
            </div>

            <label>Comparación de Significancia:</label>
            <div class="console-box" id="consoleComparison">
                p = -- vs α = 0.05
            </div>

            <label>Conclusión Estadística:</label>
            <div class="conclusion-box fail-reject" id="conclusionBox">
                Haz clic en <strong>"Analizar Datos y Tomar Decisión"</strong> para evaluar la muestra.
            </div>
        </section>
    </div>

    <!-- SECCIÓN INFERIOR: PROCEDIMIENTO MATEMÁTICO -->
    <section class="math-section">
        <h2 class="card-title">3. El Procedimiento Matemático (Prueba Z de una Cola)</h2>
        
        <div class="steps-grid">
            <!-- Paso 1 -->
            <div class="step-card">
                <h4><span class="step-number">1</span> Estadísticas Muestrales</h4>
                <p>Calculadas directamente a partir de tus datos:</p>
                <ul style="margin-left: 20px; font-size: 0.9rem; margin-top: 6px;">
                    <li>Tamaño (n): <strong id="calcN">--</strong></li>
                    <li>Media (x̄): <strong id="calcMean">--%</strong></li>
                    <li>Desviación Estándar (s): <strong id="calcStd">--%</strong></li>
                </ul>
            </div>

            <!-- Paso 2 -->
            <div class="step-card">
                <h4><span class="step-number">2</span> Error Estándar (SE)</h4>
                <p>Estima la variabilidad esperada entre medias de muestras:</p>
                <div class="formula">SE = s / √n</div>
                <div class="math-result" id="calcSE">SE = --</div>
            </div>

            <!-- Paso 3 -->
            <div class="step-card">
                <h4><span class="step-number">3</span> Estadístico Z</h4>
                <p>Mide a cuántas desviaciones estándar está x̄ de μ₀:</p>
                <div class="formula">Z = (x̄ − μ₀) / SE</div>
                <div class="math-result" id="calcZ">Z = --</div>
            </div>
        </div>

        <!-- Paso 4: Explicación Gráfica -->
        <div class="step-card" style="width: 100%;">
            <h4><span class="step-number">4</span> ¿De dónde sale el Valor P?</h4>
            <p style="font-size: 0.9rem; margin-bottom: 12px;">
                El valor p representa la probabilidad de obtener un resultado tan o más extremo que el observado, asumiendo que la hipótesis nula (H₀) es cierta. Como queremos comprobar si la conversión <em>aumenta</em>, evaluamos el área en la <strong>cola derecha</strong> de la distribución normal estándar.
            </p>
            
            <div class="visualization-container">
                <canvas id="normalCanvas" width="600" height="220"></canvas>
                <div class="canvas-caption" id="canvasCaption">P(Z > --) = --</div>
            </div>
        </div>
    </section>
</div>

<!-- ========================================== -->
<!-- JAVASCRIPT PURO (LÓGICA Y CÁLCULOS)        -->
<!-- ========================================== -->
<script>
    // ==========================================
    // 3. VARIABLES Y CONFIGURACIÓN
    // ==========================================
    const MU_0 = 8.0; // Tasa de conversión histórica fija (8%)

    // Al cargar la página, generar datos iniciales por defecto
    window.onload = function() {
        generateRandomSample();
        runAnalysis();
    };

    // ==========================================
    // 4. GENERACIÓN DE DATOS
    // ==========================================
    /**
     * Genera 20 datos aleatorios simulando tasas de conversión (%) 
     * distribuidas normalmente alrededor de la media especificada.
     */
    function generateRandomSample() {
        const center = parseFloat(document.getElementById('centerMean').value) || 11;
        const n = 20;
        const stdDev = 2.2; // Desviación estándar controlada para la simulación
        const sample = [];

        for (let i = 0; i < n; i++) {
            // Algoritmo Box-Muller para generar números con distribución normal
            const u1 = Math.random();
            const u2 = Math.random();
            const z = Math.sqrt(-2.0 * Math.log(u1)) * Math.cos(2.0 * Math.PI * u2);
            
            let val = center + z * stdDev;
            if (val < 0) val = 0.5; // Evitar valores negativos imposibles
            sample.push(parseFloat(val.toFixed(2)));
        }

        document.getElementById('dataInput').value = sample.join(', ');
    }

    // ==========================================
    // 5. PROCESAMIENTO ESTADÍSTICO
    // ==========================================
    /**
     * Parsea el texto del textarea a un arreglo de números flotantes.
     */
    function parseInputData() {
        const text = document.getElementById('dataInput').value;
        return text
            .split(',')
            .map(item => parseFloat(item.trim()))
            .filter(num => !isNaN(num));
    }

    /**
     * Calcula la media aritmética de un arreglo.
     */
    function calculateMean(data) {
        const sum = data.reduce((acc, val) => acc + val, 0);
        return sum / data.length;
    }

    /**
     * Calcula la desviación estándar muestral (s) con corrección de Bessel (n-1).
     */
    function calculateSampleStdDev(data, mean) {
        if (data.length <= 1) return 0;
        const variance = data.reduce((acc, val) => acc + Math.pow(val - mean, 2), 0) / (data.length - 1);
        return Math.sqrt(variance);
    }

    // ==========================================
    // 6. CÁLCULO DE LA DISTRIBUCIÓN NORMAL
    // ==========================================
    /**
     * Aproximación de la Función de Distribución Acumulada (CDF) Normal Estándar.
     * Basado en la aproximación polinomial de Abramowitz & Stegun.
     */
    function stdNormalCDF(z) {
        if (z < -6.0) return 0.0;
        if (z > 6.0) return 1.0;

        const b1 = 0.319381530;
        const b2 = -0.356563782;
        const b3 = 1.781477937;
        const b4 = -1.821255978;
        const b5 = 1.330274429;
        const p  = 0.2316419;
        const c  = 0.3989422804014327; // 1 / sqrt(2*PI)

        const absZ = Math.abs(z);
        const t = 1.0 / (1.0 + p * absZ);
        const poly = t * (b1 + t * (b2 + t * (b3 + t * (b4 + t * b5))));
        const pdf = c * Math.exp(-0.5 * absZ * absZ);
        const cdf = 1.0 - pdf * poly;

        return z >= 0 ? cdf : 1.0 - cdf;
    }

    // ==========================================
    // 7. CÁLCULO DEL VALOR P
    // ==========================================
    /**
     * Para una prueba de cola derecha: P(Z > z) = 1 - CDF(z)
     */
    function calculateRightTailPValue(z) {
        return 1.0 - stdNormalCDF(z);
    }

    // ==========================================
    // 8 & 9. EJECUCIÓN DEL ANÁLISIS Y ACTUALIZACIÓN
    // ==========================================
    function runAnalysis() {
        const data = parseInputData();

        // Validación básica
        if (data.length < 2) {
            alert("Por favor, introduce al menos 2 valores numéricos válidos para realizar la prueba.");
            return;
        }

        const alpha = parseFloat(document.getElementById('alphaSelect').value);
        
        // 1. Estadísticas Descriptivas
        const n = data.length;
        const mean = calculateMean(data);
        const s = calculateSampleStdDev(data, mean);

        // 2. Error Estándar (SE)
        const se = s / Math.sqrt(n);

        // 3. Estadístico Z
        const z = se > 0 ? (mean - MU_0) / se : 0;

        // 4. Valor P (Cola Derecha)
        const pValue = calculateRightTailPValue(z);

        // --- ACTUALIZACIÓN DE LA INTERFAZ ---
        
        // Desglose del procedimiento matemático
        document.getElementById('calcN').innerText = n;
        document.getElementById('calcMean').innerText = mean.toFixed(2) + '%';
        document.getElementById('calcStd').innerText = s.toFixed(2) + '%';
        document.getElementById('calcSE').innerText = `SE = ${s.toFixed(2)} / √${n} = ${se.toFixed(4)}`;
        document.getElementById('calcZ').innerText = `Z = (${mean.toFixed(2)} − ${MU_0}) / ${se.toFixed(4)} = ${z.toFixed(2)}`;

        // Tarjetas y Consola
        document.getElementById('alphaDisplay').innerText = alpha.toFixed(2);
        
        // Formato para p-value muy pequeño
        const pValueFormatted = pValue < 0.0001 ? "< 0.0001" : pValue.toFixed(4);
        document.getElementById('pValueDisplay').innerText = pValueFormatted;

        // Comparación dinámica
        const symbol = pValue <= alpha ? "≤" : ">";
        const consoleText = `p = ${pValueFormatted} ${symbol} α = ${alpha.toFixed(2)}`;
        document.getElementById('consoleComparison').innerText = consoleText;

        // 8. Decisión de la Prueba de Hipótesis y Conclusión
        const conclusionBox = document.getElementById('conclusionBox');
        
        if (pValue <= alpha) {
            // Rechazar H0
            conclusionBox.className = "conclusion-box reject";
            conclusionBox.innerHTML = `
                <strong>¡Rechazo de Hipótesis Nula (H₀)!</strong><br>
                Los datos sugieren que la nueva estrategia de anuncios <strong>SÍ aumenta la tasa de conversión</strong> de manera estadísticamente significativa al nivel de significancia seleccionado (α = ${alpha}). Existe evidencia suficiente en esta muestra para apoyar el cambio de estrategia.
            `;
        } else {
            // No rechazar H0
            conclusionBox.className = "conclusion-box fail-reject";
            conclusionBox.innerHTML = `
                <strong>No se rechaza la Hipótesis Nula (H₀).</strong><br>
                Los datos <strong>no proporcionan evidencia estadística suficiente</strong> para concluir que la nueva estrategia aumenta la tasa de conversión por encima del 8% al nivel de significancia del ${alpha * 100}%. La diferencia observada podría deberse simplemente a la variabilidad aleatoria del muestreo.
            `;
        }

        // 10. Actualizar Gráfica
        drawNormalCurve(z, pValue, alpha);
    }

    // ==========================================
    // 10. GENERACIÓN DE LA GRÁFICA (CANVAS)
    // ==========================================
    function drawNormalCurve(zCalculated, pValue, alpha) {
        const canvas = document.getElementById('normalCanvas');
        const ctx = canvas.getContext('2d');
        const width = canvas.width;
        const height = canvas.height;

        // Limpiar lienzo
        ctx.clearRect(0, 0, width, height);

        // Parámetros de escalado visual
        const padding = 40;
        const plotWidth = width - 2 * padding;
        const plotHeight = height - 60;
        const centerY = height - 40;
        
        // Rango de Z visible en la gráfica (-4 a +4)
        const zMin = -4;
        const zMax = 4;

        function zToX(z) {
            return padding + ((z - zMin) / (zMax - zMin)) * plotWidth;
        }

        function pdf(z) {
            return (1 / Math.sqrt(2 * Math.PI)) * Math.exp(-0.5 * z * z);
        }

        const maxPdf = pdf(0);
        function yToCanvas(y) {
            return centerY - (y / maxPdf) * plotHeight;
        }

        // A. Dibujar Área Sombreada del Valor P (Cola Derecha)
        ctx.beginPath();
        const startZ = Math.max(zCalculated, zMin);
        const endZ = zMax;
        
        ctx.moveTo(zToX(startZ), centerY);
        for (let z = startZ; z <= endZ; z += 0.05) {
            ctx.lineTo(zToX(z), yToCanvas(pdf(z)));
        }
        ctx.lineTo(zToX(endZ), centerY);
        ctx.closePath();
        
        // Color verde de éxito o azul claro según rechazo
        ctx.fillStyle = pValue <= alpha ? 'rgba(34, 197, 94, 0.45)' : 'rgba(239, 68, 68, 0.35)';
        ctx.fill();

        // B. Dibujar la Curva Gaussiana
        ctx.beginPath();
        for (let z = zMin; z <= zMax; z += 0.05) {
            const x = zToX(z);
            const y = yToCanvas(pdf(z));
            if (z === zMin) ctx.moveTo(x, y);
            else ctx.lineTo(x, y);
        }
        ctx.strokeStyle = '#1e293b';
        ctx.lineWidth = 2.5;
        ctx.stroke();

        // C. Dibujar Eje Z
        ctx.beginPath();
        ctx.moveTo(padding, centerY);
        ctx.lineTo(width - padding, centerY);
        ctx.strokeStyle = '#94a3b8';
        ctx.lineWidth = 1.5;
        ctx.stroke();

        // D. Marcas de graduación en el eje Z (-3, -2, -1, 0, 1, 2, 3)
        ctx.fillStyle = '#64748b';
        ctx.font = '12px sans-serif';
        ctx.textAlign = 'center';
        for (let z = -3; z <= 3; z++) {
            const x = zToX(z);
            ctx.beginPath();
            ctx.moveTo(x, centerY);
            ctx.lineTo(x, centerY + 5);
            ctx.stroke();
            ctx.fillText(z.toString(), x, centerY + 20);
        }
        ctx.fillText("Eje Z", width - 20, centerY + 20);

        // E. Línea del Z Calculado
        const xCalc = zToX(Math.min(Math.max(zCalculated, zMin), zMax));
        ctx.beginPath();
        ctx.moveTo(xCalc, centerY);
        ctx.lineTo(xCalc, yToCanvas(pdf(Math.min(Math.max(zCalculated, zMin), zMax))) - 15);
        ctx.strokeStyle = '#2563eb';
        ctx.lineWidth = 2;
        ctx.setLineDash([4, 4]); // Línea punteada
        ctx.stroke();
        ctx.setLineDash([]); // Resetear punteado

        // Etiqueta sobre el valor Z
        ctx.fillStyle = '#2563eb';
        ctx.font = 'bold 12px sans-serif';
        ctx.fillText(`Z = ${zCalculated.toFixed(2)}`, xCalc, yToCanvas(pdf(Math.min(Math.max(zCalculated, zMin), zMax))) - 20);

        // Leyenda inferior de la gráfica
        const pFormatted = pValue < 0.0001 ? "< 0.0001" : pValue.toFixed(4);
        document.getElementById('canvasCaption').innerText = `Área sombreada: P(Z > ${zCalculated.toFixed(2)}) = ${pFormatted}`;
    }
</script>
</body>
</html>
