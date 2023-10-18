## How does a browser render HTML, CSS, JS to DOM? What is the mechanism behind it?  

There are multiple steps that a browser takes to render a website. These steps are called Critical Rendering Path. These steps include fetching resources(source code) from the server, parsing received HTML to create DOM, parsing CSS to create CSSOM, creating Render Tree which is the combination of DOM and CSSOM, calculating positioning and dimensions (Layout) of all elements, and finally Painting actual pixels on the screen.

<picture>
  <img alt="Shows the Critical Rendering Path." src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*ejR8TwuRBqTRDKmAJeltig.png" width="400" height="300">
</picture>


We will use the example shown below to understand the whole process
HTMl:
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="main.css" />
  </head>
  <body>
    <div>
      Critical Rendering Path -
      <span>Medium</span>
    </div>
    <a href="https://jagjeets.medium.com">Profile link</a>
  </body>
</html>
```
css:
```
div {
  font-size: 24px;
}

div span {
  font-weight: bold;
}

a, span {
  color: green;
}
```
<picture>
  <img alt="Shows the Output." src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*Nv13EEqauZsCBNLOm3v8iA.jpeg" width="400" height="200">
</picture>

###  Construction of DOM(Document Object Model)  

- Browser receives an HTML document from the server in the `binary stream format`, which is basically a text file with a response header `Content-Type = text/html; charset=UTF-8`.
- Now the browser starts reading the HTML document. It converts all the HTML elements to a JS object called a **Node**.
- Now the browser has to create a "tree-like" structure of these node objects.

<picture>
  <img alt="Shows the DOM." src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*6ha-wC-ugBBJzY-ErTLCCQ.png" width="400" height="300">
</picture>

###  Construction of CSSOM(CSS Object Model)  

- Now the browser reads CSS from all the sources & constructs a CSSOM, which looks similar to the DOM.
- Each node in this CSSOM contains information about CSS and will be copied to the DOM element it targets.
- In the absence of any applied CSS, browsers apply default stylesheets.

<picture>
  <img alt="Shows the CSSOM." src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*6ha-wC-ugBBJzY-ErTLCCQ.png" width="400" height="300">
</picture>

###  Construction of Render Tree  

- DOM & CSSOM are combined together to form a Render tree that contains the nodes which have to be displayed on the page.
- From the root of the DOM tree, each visible node is traversed and a respective CSSOM rule is applied. Finally, it gives the render tree containing visible nodes with content and styling.
- Render Tree contains only nodes that will be visible on the screen, for example, nodes of the tags like `meta`, `script`, `link`, etc, or nodes that are hidden using CSS styles (like `display: none`), are not included.

<picture>
  <img alt="Shows the Render Tree." src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*H_RwwV5GmXiTYnBfgd7c2Q.png" width="400" height="300">
</picture>

###  Layout  

- This phase can also be said as **geometry phase**.
- Render Tree is used to find the dimensions and position of each element on the viewport.  In this way, a box model is generated which knows the exact positions and size.
- In the example above, if we add a width of 50% to div, then the final calculated width of div will be 50% of the width of the body tag. The percentage value is converted to pixels based on the viewport size.

<picture>
  <img alt="Shows the Layout." src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*qH1NprBo4q29jYBPtHGLEQ.png" width="400" height="200">
</picture>

- Box model generated in Layout phase :
<picture>
  <img alt="Shows the Layout." src="https://res.cloudinary.com/practicaldev/image/fetch/s--OizgsJP6--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1640074982590/znytiyVvy.png" width="300" height="200">
</picture>

###  Paint  
- Now we have all the content, styles, dimensions, and position of each element, it's time for the final Paint step which renders each node in the Render Tree onto the screen.

<picture>
<img alt="Shows Paint" src="https://res.cloudinary.com/practicaldev/image/fetch/s---alGqEMv--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1639944825643/nqjp1rPMo.png" width="600" height="200">
</picture>

###  References  

- "HTML, CSS Parsing and Web Page Rendering Process in Browser" by Jagjeet Singh - [Link](https://javascript.plainenglish.io/web-performance-understanding-critical-rendering-path-72283caefc1f)
- "How does a Browser render a Webpage?" by Anuradha Aggarwal - [Link](https://dev.to/anuradha9712/how-does-a-browser-render-a-webpage-2en8)
- "Critical Rendering Path" by Ilya Grigorik - [Link](https://web.dev/articles/critical-rendering-path)
- "How browser rendering works — behind the scenes" by Ohans Emmanuel - [Link](https://blog.logrocket.com/how-browser-rendering-works-behind-scenes/)
