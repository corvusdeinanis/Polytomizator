# Polytomizator
A web applet that helps generate poly art using an uploaded background image.

This was made using JS and HTML. Primary library used was p5.js.

## Using this program
A working link to this program is provided here https://stonet2000.github.io/Polytomizator

Or...

You can clone and run this repository through npm. You will need Git and Node.js installed however. Using your command line (e.g Terminal on Mac OSX)

``` bash
# To clone this repository
git clone 
# Then to enter the repostory
cd Polytomizator
# Install dependencies and start up the app
npm install && npm start
```

To compile this into an app run

``` bash
# Download all app distributions
npm run build
# Mac OSX
npm run build:osx
# Windows
npm run build:win
# Linux
npm run build:linux
```

## Features
### Making Poly Art
- Upload local image files to the page to make poly art with
- Different brushes, brush sizes, and densities to play around with to add or remove points onto the canvas
- Triangulates the points using Delaunay triangulation. Algorithm used provided by https://github.com/mapbox/delaunator
- Poly art can be created instantly or with a "flowering effect" with the colors being added to the triangles starting from the center and radially expanding outwards
- Poly art can be auto-generated by computer. A combination of randomness and edge detection algorithms help make the poly art look better.
- Can auto-generate two kinds of poly art, normal, and cubic like
- Can snap vertices or draw vertices on to a set grid
- Can undo or redo placement of vertices
- Has other display modes of the triangulation (e.g. displaying the respective circumcircles instead of the triangles)
- Can download at varying sizes depending on the desired number of megapixels. (On a MacBook Air 2017, I was able to create a 120MP sized poly art image)
- Can adjust canvas size to work on for bigger or smaller screens
### Downloading and Loading Work
- Creates poly art from triangulation and can be downloaded at high resolutions
- Save created points, triangles, and colors in between browsing sessions (Just don't clear cache)
- Save poly art to a .svg file for further editing or manual scaling with other applications

## Planned Features
- Other cool/interesting forms of poly art that can be generated by computer
- More options to work with
- Improved auto-generated poly art (basically make it look even nicer)

## Some poly art made by this program
<p align="center">
  <img src ="https://github.com/StoneT2000/StoneT2000.github.io/blob/master/images/NasaShuttle.jpg" width="399" height="auto"></img>
  <img src ="https://github.com/StoneT2000/StoneT2000.github.io/blob/master/images/NasaShuttlePolyArt.png" width="399" height="auto"></img>
  <img src ="https://github.com/StoneT2000/StoneT2000.github.io/blob/master/images/MountainHimilayas.jpg" width="800" height="auto"></img>
  <img src ="https://github.com/StoneT2000/StoneT2000.github.io/blob/master/images/MountainHimilayasPoly.jpg" width="800" height="auto"></img>
  <img src ="https://github.com/StoneT2000/StoneT2000.github.io/blob/master/images/PolyFlowerEffect2.gif" width="800" height="auto"></img>
</p>

## Further Technical Details

### How it works:
Vertices stored in a hash map for quick searches (allows for far faster erasing of vertices)

Delaunay Triangulation algorithm generates triangles from vertices.
The program finds the average color of all pixels within each triangle and stores into an array for colors.
The program then runs a loop through all the triangles and displays the triangles and their corresponding colors.

#### Downloading the poly art
First, an off-screen canvas is made with p5. That canvas is then enlarged, and the coordinates of all the triangles are all scaled upwards, creating larger triangles. Then the enlarged canvas is downloaded.

The SVG file is created by creating a file with the proper SVG formatting. The program goes through all the triangle vertices and their colors to create an SVG file (primarily using the <polygon ... /> tag. Allows users to import these shapes into a program that parses SVG files.

#### Poly Art Auto Generation
Algorithms run are done on a separate thread using web-workers, allowing for a nice loading screen to be displayed.

Edge detection algorithms created through the convolution of a 3x3 smoothing kernel and 3x3 edge detection kernel. The kernels create a photo where edges are bright, which are detected by scanning through the entire array of pixels and for those that are bright, an 'edge' vertex is placed at its position. Then a for loop is run through the detected edge vertices in order of decreasing brightness, and vertices around the detected edge vertex are erased

Then some filler vertices are placed randomly around the canvas.

Once scanned and filtered with edges detected, it doesn't need to be run again unless a new image is put up or if the canvas is resized.

In general, a combination of edge detection methods and some vertex filtering functions help create neat looking poly art.

## Acknowledgements
Many thanks to Vincent and many others as well for helping make this better.
