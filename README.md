<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Click the Button!</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%);
            text-align: center;
            padding: 20px;
            overflow: hidden;
            transition: background 1s ease;
        }

        .container {
            background-color: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
            max-width: 600px;
            width: 90%;
            position: relative;
            z-index: 10;
            transition: transform 0.5s ease, opacity 0.5s ease;
        }

        .container.fade-out {
            transform: scale(0.9);
            opacity: 0;
            pointer-events: none;
        }

        h1 {
            color: #333;
            margin-bottom: 20px;
            font-size: 2.5em;
            transition: color 0.5s ease;
        }

        .celebrate h1 {
            color: #ff4081;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
        }

        p {
            color: #555;
            font-size: 1.2em;
            margin-bottom: 40px;
            line-height: 1.5;
        }

        .button-container {
            position: relative;
            min-height: 200px;
            margin: 30px 0;
        }

        #yesButton {
            background: linear-gradient(to right, #4CAF50, #45a049);
            color: white;
            border: none;
            padding: 20px 50px;
            font-size: 1.8em;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 8px 20px rgba(76, 175, 80, 0.4);
            transition: all 0.3s ease;
            z-index: 10;
            position: relative;
        }

        #yesButton:hover {
            transform: scale(1.05);
        }

        #noButton {
            background: linear-gradient(to right, #ff416c, #ff4b2b);
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 1.5em;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 6px 15px rgba(255, 65, 108, 0.4);
            position: absolute;
            transition: left 0.3s, top 0.3s, transform 0.3s;
        }

        /* Celebration Screen */
        .celebration-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 100%);
            display: none;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            z-index: 100;
            overflow: hidden;
        }

        .celebration-screen.active {
            display: flex;
            animation: fadeIn 0.8s ease;
        }

        .celebration-content {
            text-align: center;
            color: white;
            z-index: 110;
            padding: 40px;
            background: rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            max-width: 600px;
            animation: float 3s ease-in-out infinite;
        }

        .celebration-title {
            font-size: 4em;
            margin-bottom: 20px;
            text-shadow: 3px 3px 6px rgba(0, 0, 0, 0.3);
            animation: pulse 1.5s infinite;
        }

        .celebration-message {
            font-size: 1.8em;
            margin-bottom: 30px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
        }

        .celebration-submessage {
            font-size: 1.2em;
            opacity: 0.9;
            margin-top: 20px;
        }

        .restart-button {
            margin-top: 30px;
            padding: 15px 40px;
            font-size: 1.3em;
            background: white;
            color: #ff4081;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
            transition: all 0.3s ease;
        }

        .restart-button:hover {
            transform: scale(1.1);
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.3);
        }

        /* Confetti Elements */
        .confetti {
            position: absolute;
            width: 10px;
            height: 10px;
            background: #ff4081;
            top: -10px;
        }

        .confetti:nth-child(3n) {
            background: #4CAF50;
        }
        .confetti:nth-child(3n+1) {
            background: #FFD700;
        }
        .confetti:nth-child(3n+2) {
            background: #2196F3;
        }

        /* Balloons */
        .balloon {
            position: absolute;
            width: 60px;
            height: 80px;
            border-radius: 50%;
            bottom: -100px;
            z-index: 90;
        }

        .balloon:before {
            content: '';
            position: absolute;
            width: 2px;
            height: 50px;
            background: rgba(255, 255, 255, 0.7);
            top: 80px;
            left: 50%;
            transform: translateX(-50%);
        }

        .balloon.red {
            background: radial-gradient(circle at 30px 30px, #ff4081, #d81b60);
        }
        .balloon.blue {
            background: radial-gradient(circle at 30px 30px, #2196F3, #0d47a1);
        }
        .balloon.yellow {
            background: radial-gradient(circle at 30px 30px, #FFD700, #FF9800);
        }
        .balloon.green {
            background: radial-gradient(circle at 30px 30px, #4CAF50, #2E7D32);
        }
        .balloon.purple {
            background: radial-gradient(circle at 30px 30px, #9C27B0, #6A1B9A);
        }

        /* Hearts */
        .heart {
            position: absolute;
            width: 30px;
            height: 30px;
            background: #ff4081;
            transform: rotate(45deg);
            top: -40px;
            z-index: 90;
        }

        .heart:before,
        .heart:after {
            content: '';
            position: absolute;
            width: 30px;
            height: 30px;
            background: #ff4081;
            border-radius: 50%;
        }

        .heart:before {
            top: -15px;
            left: 0;
        }

        .heart:after {
            top: 0;
            left: -15px;
        }

        /* Fireworks */
        .firework {
            position: absolute;
            width: 4px;
            height: 4px;
            border-radius: 50%;
            z-index: 80;
        }

        /* Animations */
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }

        @keyframes fall {
            to {
                transform: translateY(100vh) rotate(360deg);
                opacity: 0;
            }
        }

        @keyframes balloonRise {
            0% {
                transform: translateY(0) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: translateY(-120vh) rotate(20deg);
                opacity: 0;
            }
        }

        @keyframes heartFloat {
            0% {
                transform: translateY(0) rotate(45deg) scale(1);
                opacity: 1;
            }
            100% {
                transform: translateY(-100vh) rotate(45deg) scale(1.5);
                opacity: 0;
            }
        }

        @keyframes fireworkExplode {
            0% {
                transform: scale(1);
                opacity: 1;
            }
            100% {
                transform: scale(0);
                opacity: 0;
            }
        }

        .fall {
            animation: fall 5s linear forwards;
        }

        .rise {
            animation: balloonRise 8s ease-in forwards;
        }

        .float {
            animation: heartFloat 6s ease-in forwards;
        }

        .explode {
            animation: fireworkExplode 1s ease-out forwards;
        }

        .counter {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.9);
            padding: 10px 20px;
            border-radius: 50px;
            font-weight: bold;
            color: #333;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            z-index: 120;
        }
    </style>
</head>
<body>
    <div class="counter">Attempts: <span id="attemptCount">0</span></div>
    
    <div class="container" id="mainContainer">
        <h1>Will you be my Valentine? 💝</h1>
        <p>A simple question... just click your answer!</p>

        <div class="button-container">
            <button id="yesButton">YES!</button>
            <button id="noButton">No</button>
        </div>

        <div id="message" class="message"></div>
    </div>

    <!-- Celebration Screen -->
    <div class="celebration-screen" id="celebrationScreen">
        <div class="celebration-content">
            <h1 class="celebration-title">🎉 CELEBRATION! 🎉</h1>
            <p class="celebration-message" id="celebrationMessage">Yay! You made the right choice! 🎊</p>
            <p class="celebration-submessage">It only took you <span id="finalAttempts">0</span> attempts to say YES! 💖</p>
            <button class="restart-button" id="restartButton">Play Again 🔄</button>
        </div>
    </div>

    <footer>
        Made with ❤️ | Click YES for a surprise!
    </footer>

    <script>
        // Get DOM elements
        const yesButton = document.getElementById('yesButton');
        const noButton = document.getElementById('noButton');
        const mainContainer = document.getElementById('mainContainer');
        const celebrationScreen = document.getElementById('celebrationScreen');
        const celebrationMessage = document.getElementById('celebrationMessage');
        const finalAttempts = document.getElementById('finalAttempts');
        const restartButton = document.getElementById('restartButton');
        const attemptCount = document.getElementById('attemptCount');
        const buttonContainer = document.querySelector('.button-container');
        
        // Variables
        let escapeAttempts = 0;
        const messages = [
            "Are you sure? 😢",
            "Think again!",
            "Please? 🥺",
            "Don't do this to me!",
            "You're breaking my heart 💔",
            "I'm not giving up!",
            "Just click YES!",
            "This is your last chance... 😏"
        ];
        
        const successMessages = [
            "YAY! You made the right choice! 🎉",
            "I knew you'd say yes! 😍",
            "Best decision ever! ❤️",
            "Let's celebrate! 🥂",
            "Hooray! My heart is smiling! 😊",
            "This calls for a celebration! 🎊"
        ];

        // Function to update attempt counter
        function updateCounter() {
            attemptCount.textContent = escapeAttempts;
        }

        // Function to get random position
        function getRandomPosition() {
            const containerRect = buttonContainer.getBoundingClientRect();
            const buttonRect = noButton.getBoundingClientRect();

            const maxX = containerRect.width - buttonRect.width;
            const maxY = containerRect.height - buttonRect.height;

            const randomX = Math.floor(Math.random() * maxX);
            const randomY = Math.floor(Math.random() * maxY);

            return { x: randomX, y: randomY };
        }

        // Function to move the "No" button
        function moveNoButton() {
            const newPos = getRandomPosition();
            noButton.style.left = newPos.x + 'px';
            noButton.style.top = newPos.y + 'px';

            escapeAttempts++;
            updateCounter();
            
            const scale = Math.max(0.5, 1 - (escapeAttempts * 0.05));
            noButton.style.transform = `scale(${scale})`;

            if (escapeAttempts <= messages.length) {
                noButton.textContent = messages[escapeAttempts - 1];
            }
        }

        // Function to create confetti
        function createConfetti() {
            const colors = ['#ff4081', '#4CAF50', '#FFD700', '#2196F3', '#9C27B0'];
            
            for (let i = 0; i < 150; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti fall';
                confetti.style.left = Math.random() * 100 + 'vw';
                confetti.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                confetti.style.width = Math.random() * 10 + 5 + 'px';
                confetti.style.height = confetti.style.width;
                confetti.style.animationDuration = Math.random() * 3 + 2 + 's';
                confetti.style.animationDelay = Math.random() * 1 + 's';
                document.body.appendChild(confetti);
                
                // Remove confetti after animation
                setTimeout(() => {
                    confetti.remove();
                }, 5000);
            }
        }

        // Function to create balloons
        function createBalloons() {
            const balloonColors = ['red', 'blue', 'yellow', 'green', 'purple'];
            
            for (let i = 0; i < 20; i++) {
                const balloon = document.createElement('div');
                balloon.className = `balloon ${balloonColors[Math.floor(Math.random() * balloonColors.length)]} rise`;
                balloon.style.left = Math.random() * 100 + 'vw';
                balloon.style.animationDelay = Math.random() * 3 + 's';
                balloon.style.animationDuration = Math.random() * 5 + 8 + 's';
                celebrationScreen.appendChild(balloon);
                
                setTimeout(() => {
                    balloon.remove();
                }, 11000);
            }
        }

        // Function to create hearts
        function createHearts() {
            for (let i = 0; i < 50; i++) {
                const heart = document.createElement('div');
                heart.className = 'heart float';
                heart.style.left = Math.random() * 100 + 'vw';
                heart.style.animationDelay = Math.random() * 2 + 's';
                heart.style.animationDuration = Math.random() * 4 + 4 + 's';
                heart.style.opacity = Math.random() * 0.5 + 0.5;
                celebrationScreen.appendChild(heart);
                
                setTimeout(() => {
                    heart.remove();
                }, 10000);
            }
        }

        // Function to create fireworks
        function createFireworks() {
            const fireworkColors = ['#ff4081', '#FFD700', '#2196F3', '#4CAF50', '#FFFFFF'];
            
            function createExplosion(x, y, color) {
                for (let i = 0; i < 30; i++) {
                    const particle = document.createElement('div');
                    particle.className = 'firework explode';
                    particle.style.left = x + 'px';
                    particle.style.top = y + 'px';
                    particle.style.backgroundColor = color;
                    
                    const angle = Math.random() * Math.PI * 2;
                    const velocity = Math.random() * 5 + 2;
                    const vx = Math.cos(angle) * velocity;
                    const vy = Math.sin(angle) * velocity;
                    
                    particle.style.setProperty('--vx', vx);
                    particle.style.setProperty('--vy', vy);
                    
                    celebrationScreen.appendChild(particle);
                    
                    setTimeout(() => {
                        particle.remove();
                    }, 1000);
                }
            }
            
            // Create 10 firework explosions at random positions
            for (let i = 0; i < 10; i++) {
                setTimeout(() => {
                    const x = Math.random() * window.innerWidth;
                    const y = Math.random() * window.innerHeight / 2;
                    const color = fireworkColors[Math.floor(Math.random() * fireworkColors.length)];
                    createExplosion(x, y, color);
                }, i * 300);
            }
        }

        // Function to start celebration
        function startCelebration() {
            // Fade out main container
            mainContainer.classList.add('fade-out');
            
            // Show celebration screen after delay
            setTimeout(() => {
                celebrationScreen.classList.add('active');
                
                // Set celebration message
                const randomMessage = successMessages[Math.floor(Math.random() * successMessages.length)];
                celebrationMessage.textContent = randomMessage;
                finalAttempts.textContent = escapeAttempts;
                
                // Create celebration effects
                createConfetti();
                createBalloons();
                createHearts();
                createFireworks();
                
                // Log to console
                console.log(`🎉 Celebration started! It took ${escapeAttempts} attempts to click YES!`);
            }, 500);
        }

        // Event Listeners
        noButton.addEventListener('mouseover', moveNoButton);

        yesButton.addEventListener('click', function() {
            // Disable buttons
            yesButton.disabled = true;
            noButton.disabled = true;
            
            // Change button appearance
            yesButton.textContent = "THANK YOU! 💖";
            yesButton.style.background = 'linear-gradient(to right, #FFD700, #FFA500)';
            yesButton.style.transform = 'scale(1.2)';
            
            // Start celebration
            startCelebration();
        });

        // Restart button functionality
        restartButton.addEventListener('click', function() {
            // Reset variables
            escapeAttempts = 0;
            updateCounter();
            
            // Reset button positions and styles
            noButton.style.left = '';
            noButton.style.top = '';
            noButton.style.transform = 'scale(1)';
            noButton.textContent = 'No';
            
            yesButton.textContent = 'YES!';
            yesButton.style.background = 'linear-gradient(to right, #4CAF50, #45a049)';
            yesButton.style.transform = '';
            yesButton.disabled = false;
            noButton.disabled = false;
            
            // Hide celebration screen
            celebrationScreen.classList.remove('active');
            
            // Remove all celebration elements
            const celebrationElements = document.querySelectorAll('.confetti, .balloon, .heart, .firework');
            celebrationElements.forEach(el => el.remove());
            
            // Show main container
            mainContainer.classList.remove('fade-out');
            
            // Move "No" button to random position
            setTimeout(() => {
                moveNoButton();
            }, 300);
        });

        // Initialize
        window.onload = function() {
            moveNoButton();
            updateCounter();
            
            // Add CSS for firework animation
            const style = document.createElement('style');
            style.textContent = `
                .firework {
                    animation: explode 1s ease-out forwards;
                }
                
                @keyframes explode {
                    0% {
                        transform: translate(0, 0) scale(1);
                        opacity: 1;
                    }
                    100% {
                        transform: translate(var(--vx, 0) * 50px, var(--vy, 0) * 50px) scale(0);
                        opacity: 0;
                    }
                }
            `;
            document.head.appendChild(style);
        };
    </script>
</body>
</html>
