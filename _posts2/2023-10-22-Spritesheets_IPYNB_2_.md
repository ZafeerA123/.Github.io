---
toc: False
comments: True
layout: post
title: Spritesheets
description: A documentation of our spritesheets
type: tangibles
courses: {'compsci': {'week': 5}}
permalink: /tangibles/spritesheets
---

```python

<style>
    .container{
        display:block;
        background-color:white;
    }
    .container2{
        width:25%;
        height:25%;
        display:inline-block;
        background-color:white;
    }
</style>
<canvas id="mainDisplay" class="container" height="500px" width="500px"></canvas>
<br>
<canvas id="subDisplay" class="container2" height="500px" width="500px"></canvas>
<canvas id="subDisplay1" class="container2" height="500px" width="500px"></canvas>
<canvas id="subDisplay2" class="container2" height="500px" width="500px"></canvas>

<script type="module">
//import needed modules
import Controller from "/Group/myScripts/GameScripts/CharacterMovement.js";
import Object from "/Group/myScripts/GameScripts/CreateObject.js";
import light from "/Group/myScripts/GameScripts/Lights.js";
import {Display,subDisplay} from "/Group/myScripts/GameScripts/Displays.js"

//define canvas
var canvas = document.getElementById("mainDisplay");
var subCanvas = document.getElementById("subDisplay");
var subCanvas1 = document.getElementById("subDisplay1");
var subCanvas2 = document.getElementById("subDisplay2")

//bind inputs to a controller
var myCharacter = new Controller();
document.addEventListener("keydown",myCharacter.handleKeydown.bind(myCharacter));
document.addEventListener("keyup",myCharacter.handleKeyup.bind(myCharacter));

//create objects
    //main character
    var characterSpriteSheet = new Image();
    characterSpriteSheet.src = "/Group/images/Game/squidambient-sprite.png";
    var myCharacterObject = new Object("character", characterSpriteSheet,[190,175],[190,175],[250,500],4,1);

    //backgrounds
        //apartment background
        var redPixelSprite = new Image();
        redPixelSprite.src = "/Group/images/Game/redPixel.png"
        var redObject = new Object ("background1",redPixelSprite,[1,1],[100,500],[0,500],1,1);
        var redObject2 = new Object ("background3", redPixelSprite,[1,1],[100,500],[200,500],1,1);
        var redObject3 = new Object ("background5", redPixelSprite,[1,1],[100,500],[400,500],1,1);
        var whitePixelSprite = new Image();
        whitePixelSprite.src = "/Group/images/Game/whitePixel.png"
        var whiteObject = new Object ("background 2",whitePixelSprite,[1,1],[100,500],[100,500],1,1);
        var whiteObject2 = new Object ("background 4",whitePixelSprite,[1,1],[100,500],[300,500],1,1);
        //hallway

        //

    //lighting
    var lightingSprite = new Image();
    lightingSprite.src = "/Group/images/Game/ShadingV3.png";
    var lightObject = new Object("light",lightingSprite,[500,500],[500,500],[0,0],1,1);
    
    //neighbor

    //boxes

    //text


//red and white display
var subDisplay1 = new subDisplay(subCanvas,[redObject,whiteObject,redObject2,whiteObject2,redObject3]);
subDisplay1.OverrideScroll([0,0]);

//character display
var subDisplay2 = new subDisplay(subCanvas1,[myCharacterObject]);
subDisplay2.OverrideScroll([0,0]);

//shadow display
var subDisplay3 = new subDisplay(subCanvas2);

//main display
var MainDisplay = new Display(canvas,subDisplay1);


var bool = false
var currentFrame = 0;
var sec = 0;
var active = true; //set to false to stop all animation
var fps = 24;
function frame(){
    currentFrame = (currentFrame+1)%fps;
    if (currentFrame == 0){sec+=1};


    if (bool == false){ //if display with person is active
    var pos = myCharacter.onFrame(fps); //update frame, and get position
    pos = [pos.x,500-pos.y]; //fix position
    myCharacterObject.OverridePosition(pos); //update character Position
    }

    if(currentFrame % Math.round(fps/4)==0){ //update lighting
        light([[400,500,.5],[100,250,1],[400,100,1]],lightObject,subCanvas2,false);
    }

    if (sec % 5 ==0 && currentFrame == 0){ //set active display
        if(bool==false){
            MainDisplay.setActiveDisplay(subDisplay1);
            bool = true;
        }
        else{
           MainDisplay.setActiveDisplay([subDisplay2,subDisplay3]);
            bool = false; 
        }
    }

    subDisplay2.draw(1); //update SubCanvas (without offset)

    MainDisplay.draw(1); //update Main Canvas

setTimeout(function() {if(active == true){requestAnimationFrame(frame)}}, 1000 / fps);
}

window.addEventListener("load",function(){subDisplay1.draw(0)}) //wait for window to load then draw static canvas

frame(); //run frame


</script>
```


      File "/tmp/ipykernel_2279/2288158355.py", line 1
        <style>
        ^
    SyntaxError: invalid syntax



## Code Explanation
HTML Structure: The code starts with a <style> section defining CSS styles for two classes, "container" and "container2." These styles define the appearance of containers on the web page.

Canvas Elements: Three <canvas> elements are created with IDs "mainDisplay," "subDisplay," "subDisplay1," and "subDisplay2." These canvases are used for rendering graphics and creating a multi-layered scene.

JavaScript Module Import: JavaScript modules are imported using the import statements. These modules include functions and classes for handling character movement, object creation, lighting, and more.

Canvas Selection: The code uses document.getElementById to select the canvas elements, creating variables for each of them. These variables will be used to draw graphics on the canvases.

Controller and Event Listeners: A controller object (myCharacter) is created to handle user input, and event listeners are added to capture keyboard events (keydown and keyup). These events likely control character movement.

Object Creation: Various objects are created, including the main character, background objects, lighting objects, and more. Each object is initialized with specific properties and images.

Display Initialization: Sub-display objects are created, initialized with canvas elements and objects to be displayed. These objects likely manage the rendering of different layers of the scene.

Animation Loop: An animation loop (frame) is defined to update the game's logic and graphics. It includes logic for character movement, lighting, and changing active displays.

Window Load Event: An event listener is used to ensure that a static canvas (subDisplay1) is drawn after the window has loaded.

Start Animation: The animation loop is initiated with the frame() function.

This code sets up a complex scene with multiple canvases and objects, likely for a 2D game or interactive web application. It handles user input, object rendering, and dynamic changes in the scene. The code leverages the HTML5 canvas element and JavaScript to create an interactive and visually appealing experience for users.


## What is a hidden canvas?
A hidden canvas is an HTML canvas element that's not shown on a webpage when it loads. You can make it visible when you want to use it. It's commonly used in web development for various tasks like drawing graphics, creating charts, animations, or interactive elements. The "hidden" part means that it's initially invisible due to its CSS property, 'display,' set to "none."

Here's why hidden canvases are useful:

Pre-rendering and Efficiency: They let you prepare graphics or complex content in the background. This improves performance by avoiding the need to recreate content each time it's displayed.

Dynamic Content: Hidden canvases are handy for creating interactive elements that need to be drawn or updated based on user actions, but you don't want to display them all the time. For example, in a game, you can set up the game board on a hidden canvas and only show it when the game starts.

Canvas Manipulation: You can use hidden canvases to draw and manipulate images, graphics, or charts using JavaScript's Canvas API. It's like having a backstage area for creating complex visuals without users seeing the behind-the-scenes work.

Responsive Design: In responsive web design, you can render elements off-screen and then display them in the right size or orientation for different devices.

In the code example earlier, a hidden canvas is used to draw a main menu. It's hidden at first, and when you click the "Show Menu" button, it becomes visible, showing the menu. Clicking "Hide Menu" makes it invisible again. This keeps the webpage clean and shows the canvas only when needed, making it great for interactive and dynamic web applications.
