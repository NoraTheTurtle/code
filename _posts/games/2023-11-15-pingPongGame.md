---
toc: true
comments: True
layout: post
title: Ping Pong Game
description: A simple ping pong game built with HTML, CSS, and JavaScript
type: tangibles
courses: { compsci: {week: 8} }
---

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ping Pong Game</title>
    <style>
        body {
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color:green; 
            color: white;
            font-family: Arial, sans-serif;
            position: relative; /* Added for positioning instructions */
        }8
        #gameCanvas {
            border: 2px solid white;
        }
        /* Updated styles for the instructions */
        .instructions {
            position: absolute;
            left: 10px;
            top: 10px;
            font-size: 18px; /* Changed font size to 18 */
        }
        .instructions span {
            display: block;
        }
    </style>
</head>
<body>

<canvas id="gameCanvas" width="800" height="400"></canvas>

<!-- Updated instructions for controlling paddles with font size 18 -->
<div class="instructions">
    <span>Press <strong>W</strong> to go up and <strong>E</strong> to go down</span>
    <span>Press <strong>I</strong> to go up and <strong>O</strong> to go down</span>
</div>

<script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');

    // ball starting point + velocity
    let ballX = canvas.width / 2;
    let ballY = canvas.height / 2;
    let ballSpeedX = 5;
    let ballSpeedY = 5;

    // paddle
    const paddleWidth = 10;
    const paddleHeight = 100;
    let paddle1Y = canvas.height / 2 - paddleHeight / 2;
    let paddle2Y = canvas.height / 2 - paddleHeight / 2;
    const paddleSpeed = 10;

    // Control variables for paddle movement
    let leftPaddleUp = false;
    let leftPaddleDown = false;
    let rightPaddleUp = false;
    let rightPaddleDown = false;

    // Update paddles' positions based on key events
    function keyDownHandler(event) {
        if (event.key === 'w') {
            leftPaddleUp = true;
        } else if (event.key === 'e') {
            leftPaddleDown = true;
        } else if (event.key === 'i') {
            rightPaddleUp = true;
        } else if (event.key === 'o') {
            rightPaddleDown = true;
        }
    }

    function keyUpHandler(event) {
        if (event.key === 'w') {
            leftPaddleUp = false;
        } else if (event.key === 'e') {
            leftPaddleDown = false;
        } else if (event.key === 'i') {
            rightPaddleUp = false;
        } else if (event.key === 'o') {
            rightPaddleDown = false;
        }
    }

    document.addEventListener('keydown', keyDownHandler);
    document.addEventListener('keyup', keyUpHandler);

    // Update paddles' positions based on keys pressed
    function movePaddles() {
        if (leftPaddleUp && paddle1Y > 0) {
            paddle1Y -= paddleSpeed;
        } else if (leftPaddleDown && paddle1Y < canvas.height - paddleHeight) {
            paddle1Y += paddleSpeed;
        }

        if (rightPaddleUp && paddle2Y > 0) {
            paddle2Y -= paddleSpeed;
        } else if (rightPaddleDown && paddle2Y < canvas.height - paddleHeight) {
            paddle2Y += paddleSpeed;
        }
    }

    // Animating
    function draw() {
        // Clear canvas
        ctx.fillStyle = 'midnightblue';
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        // Draw paddles as white
        ctx.fillStyle = 'white';
        ctx.fillRect(0, paddle1Y, paddleWidth, paddleHeight);
        ctx.fillRect(canvas.width - paddleWidth, paddle2Y, paddleWidth, paddleHeight);

        // Draw ball
        ctx.fillStyle = 'white';
        ctx.beginPath();
        ctx.arc(ballX, ballY, 10, 0, Math.PI * 2);
        ctx.fill();

        // move ball
        ballX += ballSpeedX;
        ballY += ballSpeedY;

        // ball bounce off walls
        if (ballY < 0 || ballY > canvas.height) {
            ballSpeedY = -ballSpeedY;
        }

        // ball hits paddles
        if (
            (ballX <= paddleWidth && ballY > paddle1Y && ballY < paddle1Y + paddleHeight) ||
            (ballX >= canvas.width - paddleWidth && ballY > paddle2Y && ballY < paddle2Y + paddleHeight)
        ) {
            ballSpeedX = -ballSpeedX;
        }

        // Ball reset if missed
        if (ballX < 0 || ballX > canvas.width) {
            ballX = canvas.width / 2;
            ballY = canvas.height / 2;
            ballSpeedX = -ballSpeedX;
        }
    }

    function gameLoop() {
        draw();
        movePaddles(); // Call the function to move paddles
        requestAnimationFrame(gameLoop);
    }

    gameLoop();
</script>

</body>