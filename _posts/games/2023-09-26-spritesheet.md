---
title: cat sprite
comments: true
hide: true
layout: default
description: sheet with sprites!
courses: { compsci: {week: 6} }
type: tangibles
---

<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="catSprite" src="{{site.baseurl}}/images/spriteCat.png">  // change sprite here
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="idle" checked>
            <label for="idle">Idle</label><br>
            <input type="radio" name="animation" id="falling">
            <label for="falling">Falling</label><br>
            <input type="radio" name="animation" id="Backwards">
            <label for="Backwards">Backwards</label><br>
            <input type="radio" name="animation" id="Forwards">
            <label for="Forwards">Forwards</label><br>
            <input type="radio" name="animation" id="Right">
            <label for="Right">Right</label><br>
            <input type="radio" name="animation" id="Left">
            <label for="Left">Left</label><br>
            <input type="radio" name="animation" id="Sit">
            <label for="Sit">Sit</label><br>
        </div>
    </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 32;  // matches sprite pixel width
        const SPRITE_HEIGHT = 32; // matches sprite pixel height
        const FRAME_LIMIT = 3; // matches number of frames per sprite row, this code assume each row is same
       // const FRAME_RATE = 4;  // Change this value to adjust the frame rate (frames per second)
        const SCALE_FACTOR = 3;  // control size of sprite on canvas
        const DESIRED_FRAME_RATE = 7; // 1 frames per second
        const FRAME_INTERVAL = 1000 / DESIRED_FRAME_RATE;

        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Cat {
            constructor() {
                this.image = document.getElementById("catSprite");
                this.x = 0;
                this.y = 0;
                this.minFrame = 0;
                this.frameY = 0;
                this.frameX = 0;
                this.maxFrame = FRAME_LIMIT;
                this.frameX = 0;
                this.frameY = 0;
            }

            // draw object
            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * SPRITE_WIDTH,
                    this.frameY * SPRITE_HEIGHT,
                    SPRITE_WIDTH,
                    SPRITE_HEIGHT,
                    this.x,
                    this.y,
                    canvas.width,
                    canvas.height
                );
            }

            // update frameX of object
            update() {
                if (this.frameX < this.maxFrame) {
                    this.frameX++;
                } else {
                    this.frameX = 0;
                }
            }
        }

        // object
        const cat = new Cat();

        // update frameY of object, action from idle, bark, walk radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'idle':
                        cat.frameY = 5;
                        break;
                    case 'falling':
                        cat.frameY = 6;
                        break;
                    case 'Backwards':
                        cat.frameY = 2;
                        break;
                    case 'Forwards':
                        cat.frameY = 0;
                        break;
                    case 'Right':
                        cat.frameY = 1;
                        break;
                    case 'Left':
                        cat.frameY = 3;
                        break;
                    case 'Sit':
                        cat.frameY = 4;
                        break;
                    default:
                        break;
                }
            }
        });

        let lastTimestamp = 0;

        // Animation recursive control function
        function animate(timestamp) {
            const deltaTime = timestamp - lastTimestamp;
            if (deltaTime >= FRAME_INTERVAL) {
                // Clears the canvas to remove the previous frame.
                ctx.clearRect(0, 0, canvas.width, canvas.height);

                // Draws the current frame of the sprite.
                cat.draw(ctx);

                // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
                cat.update();

                lastTimestamp = timestamp;
                }
        
                // Uses `requestAnimationFrame` to synchronize the animation loop with the display's refresh rate,
            // ensuring smooth visuals.
            requestAnimationFrame(animate);
        }

        // run 1st animate
        animate();
    });
</script>