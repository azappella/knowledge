# HTMLCanvasElement

The HTMLCanvasElement interface provides properties and methods for manipulating the layout and presentation of <canvas> elements. 
The HTMLCanvasElement interface also inherits the properties and methods of the HTMLElement interface.

Source: https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement

## svg to canvas

### steps to draw svg to canvas


- Find the width and height of an SVG
- Clone the SVG node
- Create a blob object from the SVG
- Create a URL for the blob
- Load the URL into an image element
- Create a canvas with width and height of the SVG
- Draw the image to the canvas