<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PR Number Generator</title>
    <style>
        @keyframes loading {
            0% { width: 0%; left: 0%; }
            50% { width: 100%; left: 0%; }
            100% { width: 0%; left: 100%; }
        }

        @keyframes buttonPress {
            0% { transform: scale(1); }
            50% { transform: scale(0.95); }
            100% { transform: scale(1); }
        }

        @keyframes numberAppear {
            0% { opacity: 0; transform: scale(0.5); }
            60% { transform: scale(1.2); }
            100% { opacity: 1; transform: scale(1); }
        }

        body {
            font-family: 'Poppins', sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: #121212;
            color: white;
        }

        .container {
            text-align: center;
            padding: 30px;
            border: 3px solid #fa0000;
            border-radius: 15px;
            background: #1e1e1e;
            max-width: 450px;
            width: 90%;
            box-shadow: 0 10px 25px rgba(250, 0, 0, 0.2);
            position: relative;
            overflow: hidden;
            transition: all 0.3s ease;
        }

        .loading-bar {
            position: absolute;
            top: 0;
            left: 0;
            height: 3px;
            background: #fa0000;
            width: 0%;
            display: none;
        }

        h1 {
            color: white;
            margin-bottom: 25px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
        }

        #prNumber {
            font-size: 2em;
            margin: 25px 0;
            padding: 15px;
            border: 2px solid #fa0000;
            background: #fa0000;
            color: white;
            border-radius: 10px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(250, 0, 0, 0.3);
            animation: numberAppear 0.3s ease-out;
        }

        .copied-notification {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            z-index: 100;
            display: none;
            animation: numberAppear 0.3s ease-out;
        }

        .main-button {
            padding: 15px 40px;
            font-size: 1.2em;
            cursor: pointer;
            background: #fa0000;
            color: white;
            border: none;
            border-radius: 15px;
            margin: 20px;
            font-weight: bold;
            transition: all 0.2s ease;
            display: inline-block;
            box-shadow: 0 6px 12px rgba(250, 0, 0, 0.4);
        }

        .main-button:hover {
            background: #ff3333;
            transform: translateY(-3px);
        }

        .main-button:active {
            animation: buttonPress 0.3s ease-out;
        }

        .switch-button {
            padding: 8px;
            font-size: 1em;
            cursor: pointer;
            background: #444;
            color: white;
            border: none;
            border-radius: 50%;
            font-weight: bold;
            transition: all 0.2s ease;
            margin-top: 20px;
        }

        .switch-button:hover {
            background: #666;
        }

        .switch-button i {
            font-size: 1.2em;
        }

        .footer {
            margin-top: 30px;
            font-size: 0.9em;
            color: #aaa;
        }

        /* Mobile Styles */
        @media (max-width: 480px) {
            .container {
                width: 40%;
                padding: 15px;
                height: auto;
                min-width: 200px;
            }

            .main-button {
                font-size: 1.1em;
                padding: 12px 30px;
            }

            #prNumber {
                font-size: 1.5em;
            }

            .switch-button {
                font-size: 1em;
                padding: 8px;
            }

            h1 {
                font-size: 1.5em;
            }

            .footer {
                font-size: 0.8em;
            }
        }
    </style>
</head>
<body>
    <div class="container" id="container">
        <div class="loading-bar" id="loadingBar"></div>
        <h1>PR Number Generator</h1>
        <button class="main-button" onclick="generatePRNumber()">Generate PR Number</button>
        <div id="prNumber" onclick="copyToClipboard()"></div>
        <div class="generate-text">Click the button to generate a new number</div>
    </div>

    <!-- Mobile Switch Button placed outside the main box -->
    <button class="switch-button" onclick="switchToMobile()">
        <i>&#x1F4F1;</i> <!-- Phone icon -->
    </button>

    <div class="footer">
        All rights reserved - Sethil Yaddehige
    </div>
    
    <div class="copied-notification" id="copiedNotification">Copied to clipboard!</div>

    <script>
        let counter = 1;
        let isMobile = false;

        function generatePRNumber() {
            const loadingBar = document.getElementById('loadingBar');
            loadingBar.style.display = 'block';
            loadingBar.style.animation = 'loading 1s ease-in-out';

            setTimeout(() => {
                const now = new Date();
                const year = String(now.getFullYear()).slice(2);  // Get the last 2 digits of the year
                const month = String(now.getMonth() + 1).padStart(2, '0');  // Ensure month is two digits
                const day = String(now.getDate()).padStart(2, '0');  // Ensure day is two digits
                const hours = String(now.getHours()).padStart(2, '0');  // Ensure hours are two digits
                const minutes = String(now.getMinutes()).padStart(2, '0');  // Ensure minutes are two digits
                const uniqueNumber = String(counter % 100).padStart(2, '0');  // Ensure the counter is two digits

                // Now the format will be PR{year}{month}{day}-{hours}{minutes}-{uniqueNumber}
                const prNumber = `PR${year}${month}${day}-${hours}${minutes}-${uniqueNumber}`;
                
                document.getElementById('prNumber').innerText = prNumber;
                loadingBar.style.display = 'none';
                counter++; // Increment counter for next number
            }, 1000);
        }

        function copyToClipboard() {
            const prNumber = document.getElementById('prNumber').textContent;
            if (!prNumber) return;

            navigator.clipboard.writeText(prNumber).then(() => {
                const notification = document.getElementById('copiedNotification');
                notification.style.display = 'block';
                setTimeout(() => {
                    notification.style.display = 'none';
                }, 2000);
            });
        }

        function switchToMobile() {
            const container = document.getElementById('container');
            const button = document.querySelector('.switch-button');

            if (isMobile) {
                container.style.width = '450px';  // Normal desktop size
                container.style.padding = '30px';
                document.querySelector('.main-button').style.fontSize = '1.2em';
                document.querySelector('.main-button').style.padding = '15px 40px';
                document.querySelector('#prNumber').style.fontSize = '2em';
                button.style.fontSize = '1em';
                isMobile = false;
            } else {
                container.style.width = '40%';  // Mobile size
                container.style.padding = '15px';
                document.querySelector('.main-button').style.fontSize = '1.1em';
                document.querySelector('.main-button').style.padding = '12px 30px';
                document.querySelector('#prNumber').style.fontSize = '1.5em';
                button.style.fontSize = '1em';
                isMobile = true;
            }
        }
    </script>
</body>
</html>
