# Significant Feature Enhancement

As part of the enemies group, we decided to make our significant feature enhancement a mini game within our groups main level 

This blog shows how we incorporated JavaScript Objects, Finite State Machines, and Single Responsibility Principle.

# JavaScript Objects

```javascript
{ name: 'star', id: 'star', class: Star, data: this.assets.obstacles.star, xPercentage: 0.4584, yPercentage: 0.75 },
```

# What Are JavaScript Objects?

At its core, JavaScript is an object-oriented language, which means that it treats almost everything as an object. This includes numbers, arrays, functions, and more. A JavaScript object is a collection of properties where each property is composed of a key-value pair. The keys are typically strings (or Symbols), while the values can be anything from strings, numbers, and functions, to other objects.

### Syntax and Structure

A JavaScript object can be defined using curly braces `{}` with zero or more properties within them. Each property has a key and a value, separated by a colon, and each property is separated by a comma. Here's the general structure:

```javascript

let objectName = {

  key1: value1,

  key2: value2,

  key3: value3

};

```

## An Example to Consider

To illustrate, let's consider a JavaScript object used in a gaming application:

```javascript

{ 

  name: 'star', 

  id: 'star', 

  class: Star, 

  data: this.assets.obstacles.star, 

  xPercentage: 0.4584, 

  yPercentage: 0.75 

}

```

This object represents a game element, presumably a star, with various properties:

- `name`: A string that represents the name of the object.

- `id`: A unique identifier for the object, often used for DOM manipulation or CSS styling.

- `class`: This could refer to a JavaScript class named `Star`. Classes are a template for creating objects.

- `data`: This property is fetching data from another object, indicating that properties can also retrieve their values dynamically.

- `xPercentage` and `yPercentage`: These could represent the position of the star on the screen, calculated as percentages to ensure responsiveness across different screen sizes.

### How Does It Work?

JavaScript objects are typically used for holding structured data and performing operations with that data. In our example, the object may be part of a larger system managing game state. Here's how this object fits into the bigger picture:

- **Data Organization**: Objects help in organizing data logically. Here, every aspect related to the 'star' is encapsulated within a single object.

- **Dynamic Access and Manipulation**: JavaScript allows you to access and manipulate these properties in real-time. For example, changing the `xPercentage` might move the star horizontally across the game's viewport.

- **Prototypal Inheritance**: JavaScript uses prototypal inheritance. If `Star` is a class, it might inherit methods and properties from its prototype, which can be used or overridden by the `star` object.

- **Interaction with Other Elements**: The `data` property might interact with other parts of the game's environment, demonstrating how objects can link and interact within the application.

## Practical Applications

JavaScript objects are used widely in web development. Here are a few practical applications:

1\. **Configuration Objects**: Objects can hold configuration settings for applications, making it easy to access and update settings dynamically.

2\. **Data Models**: Objects can represent real-world data in your application, such as users, products, and other resources.

3\. **Modules and Namespaces**: Objects help in encapsulating functionalities into modules or namespaces, reducing the global scope pollution and increasing code maintainability.

## Conclusion

JavaScript objects are foundational to almost all JavaScript programming tasks. They provide a flexible and powerful way to organize and manage data in web development. Whether you're managing game elements, like our star, or maintaining complex user data, understanding objects is essential for any JavaScript developer aiming to write clean, efficient, and scalable code.

For beginners and seasoned developers alike, mastering JavaScript objects is a step towards mastering JavaScript itself. Keep exploring and experimenting with different ways to utilize objects in your projects for more robust and innovative applications.

Single Responsibility Principle
===============================

This is the principle that every function, every method should have only one function.

Let's examine how refactoring a method to adhere to SRP can simplify code maintenance and improve flexibility, especially when extending a class. Here's the original code snippet with a complex `update()` method:

```
update() {
    super.update();
    this.setAnimation(this.state.animation);

    // Check for boundaries
    if (this.x <= this.minPosition || (this.x + this.canvasWidth >= this.maxPosition)) {
        if (this.state.direction === "left") {
            this.state.animation = "right";
            this.state.direction = "right";
        } else if (this.state.direction === "right") {
            this.state.animation = "left";
            this.state.direction = "left";
        }
    }

    // Update movement
    if (this.state.animation === "right") {
        this.speed = Math.abs(this.speed);
    } else if (this.state.animation === "left") {
        this.speed = -Math.abs(this.speed);
    } else if (this.state.animation === "idle") {
        this.speed = 0;
    } else if (this.state.animation === "death") {
        this.speed = 0;
    }

    // Move the enemy
    this.x += this.speed;

    this.playerBottomCollision = false;
}

```

This code does many things at once: setting animations, checking boundaries, and updating movement. To improve it, we can extract specific responsibilities into dedicated methods, as shown below:

```
checkBoundaries() {
    if (this.x <= this.minPosition || (this.x + this.canvasWidth >= this.maxPosition)) {
        if (this.state.direction === "left") {
            this.state.animation = "right";
            this.state.direction = "right";
        } else if (this.state.direction === "right") {
            this.state.animation = "left";
            this.state.direction = "left";
        }
    }
}

updateMovement() {
    if (this.state.animation === "right") {
        this.speed = Math.abs(this.speed);
    } else if (this.state.animation === "left") {
        this.speed = -Math.abs(this.speed);
    } else {
        this.speed = 0;
    }

    // Move the enemy
    this.x += this.speed;

    this.playerBottomCollision = false;
}

update() {
    super.update();
    this.setAnimation(this.state.animation);

    this.checkBoundaries();
    this.updateMovement();
}

```

Now, each method has a single responsibility, making it easier to read, understand, and maintain. This refactoring provides a few key benefits:

-   Clarity: The code is easier to understand since each method does one thing. This makes it simpler to follow the logic and identify specific responsibilities.

-   Modularity: By isolating functionality into separate methods, it's easier to change or extend specific behavior without affecting the rest of the class.

-   Extensibility: If you're creating a subclass that extends Enemy, you can easily override individual methods like updateMovement() to change behavior without needing to modify the original class. This flexibility is crucial in object-oriented programming, allowing for specialized behavior in subclasses.

For example, if we have a new Enemy that extends the Enemy class, and we want it to have a new movement instead of using the old one. We can just overwrite the method "updateMovement()" by creating a function that has the same name called "updateMovement()" and just put the code we want to overwrite into the new function.

We don't need to go back and change the Enemy class such as adding an if-statement to the code inside the update() function that manages the movement.

## Finite State Programming
```
 switch (this.state.collision) {
            case "tubeU":
                // 1. Caught in tube
                if (this.collisionData.touchPoints.this.top && this.collisionData.touchPoints.other.bottom) {
                    // Position player in the center of the tube
                    this.x = this.collisionData.newX;
                    // Using natural gravity wait for player to reach floor
                    if (Math.abs(this.y - this.bottom) <= GameEnv.gravity) {
                        // Force end of level condition
                        GameControl.transitionToLevel(GameEnv.levels[3])
                    }
                // 2. Collision between player right and tube
                } else if (this.collisionData.touchPoints.this.right) {
                    this.state.movement.right = false;
                    this.state.movement.left = true;
                // 3. Collision between player left and tube
                } else if (this.collisionData.touchPoints.this.left) {
                    this.state.movement.left = false;
                    this.state.movement.right = true;
                }
                break;
```
When the player collides with the top of the tubeU, the player drops downwards and then it transitions to the previous level, or level 3.
## Single State Responsibility
```
 // speed is used to background parallax behavior
    update() {
        this.speed = GameEnv.backgroundDirection * this.parallaxSpeed;
        super.update();
    }
    //Cause of limited bg cutout, keeping just incase it causes issues later
    draw() {
        this.ctx.clearRect(0, 0, this.canvasWidth, this.canvasHeight);
        super.draw();
    }
```
The update method and the draw method are called separately.