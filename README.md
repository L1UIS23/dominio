<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Solucionador de Desigualdades</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f4f4f4;
            color: #333;
        }
        .container {
            background-color: #fff;
            padding: 25px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            max-width: 700px; /* Un poco más ancho */
            margin: 30px auto;
        }
        h1, h2 {
            color: #0056b3;
            text-align: center;
            margin-bottom: 20px;
        }
        .section {
            margin-bottom: 30px;
            padding: 20px;
            border: 1px solid #ddd;
            border-radius: 6px;
            background-color: #f9f9f9;
        }
        .input-group {
            display: flex;
            align-items: center;
            margin-bottom: 15px;
            flex-wrap: wrap;
            gap: 10px;
        }
        .input-group label {
            min-width: 40px;
            font-weight: bold;
        }
        .input-group input[type="number"],
        .input-group select {
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 4px;
            flex-grow: 1;
            min-width: 60px;
        }
        button {
            background-color: #007bff;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            margin-top: 10px;
            display: block;
            width: 100%;
        }
        button:hover {
            background-color: #0056b3;
        }
        .result {
            margin-top: 20px;
            padding: 15px;
            border: 1px solid #cce5ff;
            background-color: #e0f2ff;
            border-radius: 6px;
            font-size: 1.1em;
            color: #004085;
            min-height: 50px;
        }
        .error {
            color: #dc3545;
            font-weight: bold;
        }
        .graph-tip {
            margin-top: 10px;
            font-size: 0.9em;
            color: #666;
            border-top: 1px dashed #ccc;
            padding-top: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Solucionador de Desigualdades</h1>
        <p style="text-align: center; margin-bottom: 25px;">Introduce los coeficientes y la desigualdad. Para visualizar la gráfica, utiliza un graficador externo como Desmos o GeoGebra.</p>

        <div class="section">
            <h2>Desigualdad Lineal: $ax + b \text{ OP } 0$</h2>
            <div class="input-group">
                <label for="linear_a">a:</label>
                <input type="number" id="linear_a" value="1">
                <span style="font-weight: bold;">x +</span>
                <label for="linear_b">b:</label>
                <input type="number" id="linear_b" value="0">
                <select id="linear_op">
                    <option value="gt">&gt;</option>
                    <option value="lt">&lt;</option>
                    <option value="ge">&ge;</option>
                    <option value="le">&le;</option>
                </select>
                <span style="font-weight: bold;">0</span>
            </div>
            <button onclick="solveLinear()">Resolver Lineal</button>
            <div id="linear_result" class="result"></div>
        </div>

        <div class="section">
            <h2>Desigualdad Cuadrática: $ax^2 + bx + c \text{ OP } 0$</h2>
            <div class="input-group">
                <label for="quadratic_a">a:</label>
                <input type="number" id="quadratic_a" value="1">
                <span style="font-weight: bold;">x² +</span>
                <label for="quadratic_b">b:</label>
                <input type="number" id="quadratic_b" value="0">
                <span style="font-weight: bold;">x +</span>
                <label for="quadratic_c">c:</label>
                <input type="number" id="quadratic_c" value="0">
                <select id="quadratic_op">
                    <option value="gt">&gt;</option>
                    <option value="lt">&lt;</option>
                    <option value="ge">&ge;</option>
                    <option value="le">&le;</option>
                </select>
                <span style="font-weight: bold;">0</span>
            </div>
            <button onclick="solveQuadratic()">Resolver Cuadrática</button>
            <div id="quadratic_result" class="result"></div>
        </div>
    </div>

    <script>
        // Función para resolver y mostrar resultado lineal
        function solveLinear() {
            const a = parseFloat(document.getElementById('linear_a').value);
            const b = parseFloat(document.getElementById('linear_b').value);
            const op = document.getElementById('linear_op').value;
            const resultDiv = document.getElementById('linear_result');
            resultDiv.innerHTML = ''; 

            if (isNaN(a) || isNaN(b)) {
                resultDiv.innerHTML = '<span class="error">Por favor, introduce valores numéricos válidos.</span>';
                return;
            }

            let solution = '';
            let graphTip = '';
            const functionText = `y = ${a}x + ${b}`;

            if (a === 0) {
                if (op === 'gt') solution = (b > 0) ? 'Todos los números reales (x ∈ ℝ)' : 'No hay solución (∅)';
                else if (op === 'lt') solution = (b < 0) ? 'Todos los números reales (x ∈ ℝ)' : 'No hay solución (∅)';
                else if (op === 'ge') solution = (b >= 0) ? 'Todos los números reales (x ∈ ℝ)' : 'No hay solución (∅)';
                else if (op === 'le') solution = (b <= 0) ? 'Todos los números reales (x ∈ ℝ)' : 'No hay solución (∅)';
                
                graphTip = `Para visualizar, grafica la línea "${functionText}" en Desmos.`;
                if (solution.includes('Todos')) {
                    graphTip += ` Si ${b} ${op === 'gt' ? '>' : op === 'lt' ? '<' : op === 'ge' ? '≥' : '≤'} 0, toda la línea satisface la desigualdad.`;
                } else {
                    graphTip += ` Si ${b} ${op === 'gt' ? '>' : op === 'lt' ? '<' : op === 'ge' ? '≥' : '≤'} 0, ninguna parte de la línea satisface la desigualdad.`;
                }

            } else {
                let x_intercept = -b / a;
                let inequalitySymbol = '';
                let graphCondition = '';

                if (a > 0) { // Pendiente positiva
                    if (op === 'gt') { solution = `x > ${x_intercept.toFixed(2)}`; inequalitySymbol = '>'; graphCondition = 'la parte de la línea que está POR ENCIMA del eje X'; }
                    else if (op === 'lt') { solution = `x < ${x_intercept.toFixed(2)}`; inequalitySymbol = '<'; graphCondition = 'la parte de la línea que está POR DEBAJO del eje X'; }
                    else if (op === 'ge') { solution = `x ≥ ${x_intercept.toFixed(2)}`; inequalitySymbol = '≥'; graphCondition = 'la parte de la línea que está POR ENCIMA o SOBRE el eje X'; }
                    else if (op === 'le') { solution = `x ≤ ${x_intercept.toFixed(2)}`; inequalitySymbol = '≤'; graphCondition = 'la parte de la línea que está POR DEBAJO o SOBRE el eje X'; }
                } else { // Pendiente negativa (a < 0)
                    if (op === 'gt') { solution = `x < ${x_intercept.toFixed(2)}`; inequalitySymbol = '<'; graphCondition = 'la parte de la línea que está POR ENCIMA del eje X'; } // Invertido
                    else if (op === 'lt') { solution = `x > ${x_intercept.toFixed(2)}`; inequalitySymbol = '>'; graphCondition = 'la parte de la línea que está POR DEBAJO del eje X'; } // Invertido
                    else if (op === 'ge') { solution = `x ≤ ${x_intercept.toFixed(2)}`; inequalitySymbol = '≤'; graphCondition = 'la parte de la línea que está POR ENCIMA o SOBRE el eje X'; } // Invertido
                    else if (op === 'le') { solution = `x ≥ ${x_intercept.toFixed(2)}`; inequalitySymbol = '≥'; graphCondition = 'la parte de la línea que está POR DEBAJO o SOBRE el eje X'; } // Invertido
                }
                
                graphTip = `Para visualizar, grafica la línea "${functionText}" en Desmos o GeoGebra. El intervalo que satisface la desigualdad (${solution}) corresponde a ${graphCondition}.`;
            }
            resultDiv.innerHTML = `<strong>Solución:</strong> ${solution}<div class="graph-tip">${graphTip}</div>`;
        }

        // Función para resolver y mostrar resultado cuadrático
        function solveQuadratic() {
            const a = parseFloat(document.getElementById('quadratic_a').value);
            const b = parseFloat(document.getElementById('quadratic_b').value);
            const c = parseFloat(document.getElementById('quadratic_c').value);
            const op = document.getElementById('quadratic_op').value;
            const resultDiv = document.getElementById('quadratic_result');
            resultDiv.innerHTML = ''; 

            if (isNaN(a) || isNaN(b) || isNaN(c)) {
                resultDiv.innerHTML = '<span class="error">Por favor, introduce valores numéricos válidos.</span>';
                return;
            }

            if (a === 0) {
                resultDiv.innerHTML = '<span class="error">Si a=0, esto es una desigualdad lineal. Usa el solucionador de arriba.</span>';
                return;
            }

            const delta = b * b - 4 * a * c;
            let solution = '';
            let graphTip = '';
            const functionText = `y = ${a}x² + ${b}x + ${c}`;
            let rootsDescription = '';

            if (delta >= 0) {
                const root1_val = (-b - Math.sqrt(delta)) / (2 * a);
                const root2_val = (-b + Math.sqrt(delta)) / (2 * a);

                let root1 = Math.min(root1_val, root2_val);
                let root2 = Math.max(root1_val, root2_val);

                if (delta === 0) {
                    // Una raíz real
                    rootsDescription = `La parábola toca el eje X en x = ${root1.toFixed(2)}.`;
                    if (a > 0) { // Parábola abre hacia arriba
                        if (op === 'gt') solution = `x ∈ ℝ, excepto x = ${root1.toFixed(2)}`;
                        else if (op === 'ge') solution = `x ∈ ℝ`;
                        else if (op === 'lt' || op === 'le') solution = `No hay solución (∅)${op === 'le' ? ` o solo x = ${root1.toFixed(2)}` : ''}`;
                    } else { // Parábola abre hacia abajo
                        if (op === 'lt') solution = `x ∈ ℝ, excepto x = ${root1.toFixed(2)}`;
                        else if (op === 'le') solution = `x ∈ ℝ`;
                        else if (op === 'gt' || op === 'ge') solution = `No hay solución (∅)${op === 'ge' ? ` o solo x = ${root1.toFixed(2)}` : ''}`;
                    }
                } else {
                    // Dos raíces reales distintas
                    rootsDescription = `La parábola cruza el eje X en x = ${root1.toFixed(2)} y x = ${root2.toFixed(2)}.`;
                    if (a > 0) { // Parábola abre hacia arriba
                        if (op === 'gt') solution = `x < ${root1.toFixed(2)} o x > ${root2.toFixed(2)}`;
                        else if (op === 'lt') solution = `${root1.toFixed(2)} < x < ${root2.toFixed(2)}`;
                        else if (op === 'ge') solution = `x ≤ ${root1.toFixed(2)} o x ≥ ${root2.toFixed(2)}`;
                        else if (op === 'le') solution = `${root1.toFixed(2)} ≤ x ≤ ${root2.toFixed(2)}`;
                    } else { // Parábola abre hacia abajo
                        if (op === 'gt') solution = `${root1.toFixed(2)} < x < ${root2.toFixed(2)}`;
                        else if (op === 'lt') solution = `x < ${root1.toFixed(2)} o x > ${root2.toFixed(2)}`;
                        else if (op === 'ge') solution = `${root1.toFixed(2)} ≤ x ≤ ${root2.toFixed(2)}`;
                        else if (op === 'le') solution = `x ≤ ${root1.toFixed(2)} o x ≥ ${root2.toFixed(2)}`;
                    }
                }
                
                let conditionText = '';
                if (op === 'gt') conditionText = 'la parte de la parábola que está ESTRICTAMENTE POR ENCIMA del eje X';
                else if (op === 'lt') conditionText = 'la parte de la parábola que está ESTRICTAMENTE POR DEBAJO del eje X';
                else if (op === 'ge') conditionText = 'la parte de la parábola que está POR ENCIMA O SOBRE el eje X';
                else if (op === 'le') conditionText = 'la parte de la parábola que está POR DEBAJO O SOBRE el eje X';

                graphTip = `Para visualizar, grafica la parábola "${functionText}" en Desmos o GeoGebra. ${rootsDescription} El intervalo que satisface la desigualdad (${solution}) corresponde a ${conditionText}.`;

            } else {
                // Raíces complejas (la parábola nunca cruza el eje x)
                rootsDescription = `La parábola NO cruza el eje X.`;
                if (a > 0) { // Parábola abre hacia arriba, siempre por encima del eje x
                    if (op === 'gt' || op === 'ge') solution = 'Todos los números reales (x ∈ ℝ)';
                    else if (op === 'lt' || op === 'le') solution = 'No hay solución (∅)';
                } else { // Parábola abre hacia abajo, siempre por debajo del eje x
                    if (op === 'lt' || op === 'le') solution = 'Todos los números reales (x ∈ ℝ)';
                    else if (op === 'gt' || op === 'ge') solution = 'No hay solución (∅)';
                }

                let conditionText = '';
                if (op === 'gt' || op === 'ge') conditionText = `la parábola siempre está ${a > 0 ? 'por encima' : 'por debajo'} del eje X`;
                else if (op === 'lt' || op === 'le') conditionText = `la parábola siempre está ${a > 0 ? 'por encima' : 'por debajo'} del eje X`;

                graphTip = `Para visualizar, grafica la parábola "${functionText}" en Desmos o GeoGebra. ${rootsDescription} La solución (${solution}) se determina porque ${conditionText}.`;
            }
            resultDiv.innerHTML = `<strong>Solución:</strong> ${solution}<div class="graph-tip">${graphTip}</div>`;
        }
    </script>
</body>
</html>
