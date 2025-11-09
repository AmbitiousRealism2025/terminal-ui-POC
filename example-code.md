# Base Terminal Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Terminal</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Courier New', monospace;
            background-color: #000;
            color: #0f0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .terminal {
            width: 800px;
            height: 600px;
            background-color: #000;
            border: 2px solid #0f0;
            border-radius: 5px;
            padding: 20px;
            overflow-y: auto;
        }
        .output {
            white-space: pre-wrap;
            margin-bottom: 10px;
        }
        .input-line {
            display: flex;
        }
        .prompt {
            color: #0f0;
            margin-right: 5px;
        }
        #input {
            background: transparent;
            border: none;
            color: #0f0;
            font-family: 'Courier New', monospace;
            font-size: 16px;
            outline: none;
            flex-grow: 1;
        }
        .cursor {
            display: inline-block;
            width: 10px;
            height: 20px;
            background-color: #0f0;
            animation: blink 1s infinite;
        }
        @keyframes blink {
            0%, 49% { opacity: 1; }
            50%, 100% { opacity: 0; }
        }
    </style>
</head>
<body>
    <div class="terminal">
        <div id="output"></div>
        <div class="input-line">
            <span class="prompt">$</span>
            <input type="text" id="input" autofocus>
        </div>
    </div>
    <script>
        const output = document.getElementById('output');
        const input = document.getElementById('input');

        const commands = {
            help: 'Available commands: help, clear, echo, date, about',
            about: 'Terminal UI - A simple web-based terminal',
            date: () => new Date().toString(),
            clear: () => { output.innerHTML = ''; return ''; },
            echo: (args) => args.join(' ')
        };

        input.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                const cmd = input.value.trim();
                const parts = cmd.split(' ');
                const command = parts[0];
                const args = parts.slice(1);

                addOutput(`$ ${cmd}`);

                if (commands[command]) {
                    const result = typeof commands[command] === 'function'
                        ? commands[command](args)
                        : commands[command];
                    if (result) addOutput(result);
                } else if (cmd) {
                    addOutput(`Command not found: ${command}`);
                }

                input.value = '';
            }
        });

        function addOutput(text) {
            const div = document.createElement('div');
            div.className = 'output';
            div.textContent = text;
            output.appendChild(div);
            output.parentElement.scrollTop = output.parentElement.scrollHeight;
        }

        addOutput('Welcome to Terminal UI. Type "help" for available commands.');
    </script>
</body>
</html>
```
