<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web-based Compiler</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #040404;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            max-width: 600px;
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            text-align: center;
        }

        h1 {
            margin-top: 0;
            color: #333;
        }

        label {
            font-weight: bold;
        }

        select, textarea, button {
            margin-top: 10px;
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }

        textarea {
            height: 150px;
        }

        button {
            background-color: #007bff;
            color: #fff;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #0056b3;
        }

        #output {
            margin-top: 20px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            background-color: #f9f9f9;
            text-align: left;
            white-space: pre-wrap;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Web-based Compiler</h1>
        <label for="language">Select Language:</label>
        <select id="language">
            <option value="solidity">Solidity</option>
            <option value="Rust">Rust</option>
            <option value="Motoko">Motoko</option>

            <!-- Add options for Rust and Motoko if desired -->
        </select>
        <br>
        <label for="difficulty">Select Difficulty:</label>
        <select id="difficulty">
            <option value="easy">Easy</option>
            <option value="medium">Medium</option>
            <option value="hard">Hard</option>
        </select>
        <br>
        <textarea id="code" placeholder="Enter your code here..."></textarea>
        <br>
        <button onclick="compile()">Compile</button>
        <div id="output"></div>
    </div>

    <script>
        async function compile() {
            const language = document.getElementById('language').value;
            const difficulty = document.getElementById('difficulty').value;
            const code = document.getElementById('code').value;

            let compiledOutput;
            switch (language) {
                case 'solidity':
                    compiledOutput = await compileSolidity(code);
                    break;
                // Add cases for Rust and Motoko if desired
                default:
                    return displayOutput('Unsupported language');
            }

            // Compare compiled output with expected hash if needed

            displayOutput(compiledOutput);
        }

        async function compileSolidity(code) {
            try {
                const response = await fetch('https://remix.ethereum.org/api/compile', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({ language: 'Solidity', sources: { 'contract.sol': { content: code }}})
                });
                const data = await response.json();
                return data.result['contract.sol']['evm']['bytecode']['object'];
            } catch (error) {
                return 'Compilation failed';
            }
        }
       
        

function displayOutput(output) {
    document.getElementById('output').innerText = output;
}

        // Add similar functions for Rust and Motoko compilation using other APIs

        function displayOutput(output) {
            document.getElementById('output').innerText = output;
        }
    </script>
</body>
</html>
