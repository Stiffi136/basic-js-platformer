# Basic JavaScript Platformer made with ChatGPT

Live version available here:
https://stiffi136.github.io/basic-js-platformer/

## Description

I experimented with ChatGPT to see how fast someone with a bit of programming knowledge could get a working prototype for a game from ChatGPT. This is the result after **10 minutes** of prompting, testing and putting everything together.

## Prompts and Responses

### Prompt

Show me example code for a basic platformer game

### Response

Here is an example of basic platformer game code in JavaScript using the Canvas API:

```html
<canvas id="game"></canvas>
<script>
  // Get canvas and context
  const canvas = document.getElementById('game');
  const ctx = canvas.getContext('2d');

  // Create player object
  let player = {
    x: 50,
    y: 50,
    width: 20,
    height: 20,
    speed: 5,
  };

  // Create platforms array
  let platforms = [
    { x: 0, y: 300, width: 800, height: 10 },
    { x: 200, y: 200, width: 100, height: 10 },
    { x: 600, y: 100, width: 100, height: 10 },
  ];

  // Handle player input
  document.addEventListener('keydown', function (event) {
    if (event.keyCode === 37) {
      // Left arrow key
      player.x -= player.speed;
    } else if (event.keyCode === 38) {
      // Up arrow key
      player.y -= player.speed;
    } else if (event.keyCode === 39) {
      // Right arrow key
      player.x += player.speed;
    } else if (event.keyCode === 40) {
      // Down arrow key
      player.y += player.speed;
    }
  });

  // Game loop
  function update() {
    // Clear canvas
    ctx.clearRect(0, 0, 800, 400);

    // Draw player
    ctx.fillRect(player.x, player.y, player.width, player.height);

    // Draw platforms
    ctx.fillStyle = 'gray';
    for (let i = 0; i < platforms.length; i++) {
      ctx.fillRect(
        platforms[i].x,
        platforms[i].y,
        platforms[i].width,
        platforms[i].height
      );
    }

    // Check for collision with platforms
    for (let i = 0; i < platforms.length; i++) {
      if (
        player.x + player.width > platforms[i].x &&
        player.x < platforms[i].x + platforms[i].width &&
        player.y + player.height > platforms[i].y &&
        player.y < platforms[i].y + platforms[i].height
      ) {
        player.y = platforms[i].y - player.height; // Move player to top of platform
      }
    }

    // Repeat update function after a set amount of time
    setTimeout(update, 1000 / 30);
  }
  update();
</script>
```

This code creates a player object with a position, size, and speed, and an array of platform objects with positions and sizes. It then uses event listeners to handle player input, moving the player left, right, up, or down when the corresponding arrow keys are pressed. The game loop uses the Canvas API to draw the player and platforms on the screen, and checks for collisions between the player and platforms, moving the player to the top of the platform when a collision is detected.

Please note that this is a very basic example and you will need to add more features such as jumping, scrolling, enemies, items, and scoring to create a complete game.

Also, you should use a game engine or library to make it more efficient and lessen the complexity of

### Prompt

I can't see the platforms

### Response

I apologize for the oversight. The platforms may not be visible because the code is drawing them with the same color as the background.

You can fix this by adding a fillStyle property to the context before drawing the platforms:

```js
ctx.fillStyle = 'gray';
```

This will set the fill color of the platforms to gray, making them visible on the canvas.

You could also set the fill style to another color or use an image as a sprite.

Also, it's worth mentioning that this is a very basic example, to make a complete game you will need to add more features such as jumping, scrolling, enemies, items, and scoring among others. Also, there are game engines and libraries that could make the development process more efficient and less complex.

### Prompt

I think the canvas is to small so the platforms are out of bounds

### Response

Yes, that's possible. In this example, the canvas is set to a size of 800x400 pixels. If the platforms are positioned outside of this area, they will not be visible.

You can easily fix this by adjusting the width and height of the canvas element in the HTML, or by setting the width and height properties of the canvas object in JavaScript.

```html
<canvas id="game" width="1200" height="600"></canvas>
```

or

```js
const canvas = document.getElementById('game');
canvas.width = 1200;
canvas.height = 600;
```

You should also consider that the player's movement is limited to the canvas area and you may need to add some boundaries to the player's movement so it can't move out of the canvas, also you may need to add scrolling feature to make the level bigger than the canvas.

Please let me know if you need more information.

### Prompt

add gravity to the player object

### Response

Here's an example of how you could add gravity to the player object:

```js
let player = {
  x: 50,
  y: 50,
  width: 20,
  height: 20,
  speed: 5,
  gravity: 0.5,
  velocity: 0,
};
```

You can then update the player's y-position in the game loop to simulate gravity:

```js
// Update player y-position with gravity
player.velocity += player.gravity;
player.y += player.velocity;
```

Additionally, you will need to check for collision with the platforms, and if a collision is detected, set the velocity to 0, so the player stops falling when it lands on a platform.

```js
// Check for collision with platforms
for (let i = 0; i < platforms.length; i++) {
  if (
    player.x + player.width > platforms[i].x &&
    player.x < platforms[i].x + platforms[i].width &&
    player.y + player.height > platforms[i].y &&
    player.y < platforms[i].y + platforms[i].height
  ) {
    player.velocity = 0; // Stop falling
    player.y = platforms[i].y - player.height; // Move player to top of platform
  }
}
```

Also, you could add jumping feature to the player, when the user press the up arrow key you could add a negative velocity on the y-axis, this will make the player jump.

```js
// Handle player input
document.addEventListener('keydown', function (event) {
  if (event.keyCode === 37) {
    // Left arrow key
    player.x -= player.speed;
  } else if (event.keyCode === 38) {
    // Up arrow key
    player.velocity = -10;
  } else if (event.keyCode === 39) {
    // Right arrow key
    player.x += player.speed;
  }
});
```

It's important to notice that this is a basic example and you will need to add more features like collision detection, scrolling, enemies, items, and scoring to create a complete game.

Please let me know if you have any questions or need more information.

### Prompt

the inputs are blocking each other. make them non blocking

### Response

The inputs are blocking each other because the code is updating the player's position based on a single key press, rather than on the key being held down.

One way to fix this is by using the keyup and keydown events, instead of keypress, to track whether a key is being held down.

Here's an example of how you could change the input handling code to make the inputs non-blocking:

```js
// Keep track of which keys are currently pressed
let keysPressed = {};

// Handle keydown events
document.addEventListener('keydown', function (event) {
  keysPressed[event.keyCode] = true;
});

// Handle keyup events
document.addEventListener('keyup', function (event) {
  keysPressed[event.keyCode] = false;
});
```

Then, in the game loop, you can check the keysPressed object to see which keys are currently being held down, and move the player accordingly.

```js
// Game loop
function update() {
  // Clear canvas
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Update player position based on input
  if (keysPressed[37]) {
    // Left arrow key
    player.x -= player.speed;
  }
  if (keysPressed[38]) {
    // Up arrow key
    player.velocity = -10;
  }
  if (keysPressed[39]) {
    // Right arrow key
    player.x += player.speed;
  }

  // Update player y-position with gravity
  player.velocity += player.gravity;
  player.y += player.velocity;

  // Draw player
  ctx.fillRect(player.x, player.y, player.width, player.height);

  // Draw platforms
  ctx.fillStyle = 'gray';
  for (let i = 0; i < platforms.length; i++) {
    ctx.fillRect(
      platforms[i].x,
      platforms[i].y,
      platforms[i].width,
      platforms[i].height
    );
  }

  // Check for collision with platforms
  for (let i = 0; i < platforms.length; i++) {
    if (
      player.x + player.width > platforms[i].x &&
      player.x < platforms[i].x + platforms[i].width &&
      player.y + player.height > platforms[i].y &&
      player.y < platforms[i].y + platforms[i].height
    ) {
      player.velocity = 0; // Stop falling
      player.y = platforms[i].y - player.height; // Move player to top of platform
    }
  }

  // Repeat update function after a set amount of time
  setTimeout(update, 1000 / 30);
}
update();
```

This way, the player's position will be updated continuously while a key is being held down, rather than just when it is first pressed.

Keep in mind that this is a basic example and you will need to add more features like collision detection, scrolling, enemies, items, and scoring to create a complete game. Also, there are game engines and libraries that could make the development process more efficient and less complex.

Please let me know if you have any questions or need more information.
