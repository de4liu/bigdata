## How to add an image to your jupyter notebook and submit it with image

### Simplest workflow
1. upload the image (most commonly jpg or png) to your directory
2. In a markdown cell, add `![alternative text](path-to-image-file)` to display the image.

3. Export the Jupyter Notebook to PDF and submit the PDF

### In the event that PDF converter encounters an error, you may either

#### Embed image in html

1. convert your image using base64 encoder using online services [https://www.base64-image.de/](https://www.base64-image.de/) 

2. In a markdown cell, enter an image tag `<img src="data:image/png;base64"/>`, and replace the `data:image/...` portion with your encoded image from your encoder. The image should display in the rendered markdown cell. 
    
3. Export the HTML and submit it.

> The reason that we have to embed the image is because HTML files link to images rather than embed them by default. Even though you have uploaded your image to your workspace, I cannot access this file in your space. So when you convert it to HTML and view it in a place other than your server, the image cannot be displayed. Embedding forces the HTML to store the image data within the HTML file.

#### Submit HTML and image(s) separately.

1. Alternatively, you can submit your HTML **as well as your images** to Canvas.







