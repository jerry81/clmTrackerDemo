# clmTrackerDemo

clm - constrained local models 

## how to display web camera stream in video element

create video element 

navigator.getUserMedia polyfill 

<code>navigator.getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia || navigator.msGetUserMedia;</code>

get the camera

<code>            // check for camerasupport
            if (navigator.mediaDevices) {
                navigator.mediaDevices.getUserMedia({ video: true }).then(gumSuccess).catch(gumFail);
            } else if (navigator.getUserMedia) {
                navigator.getUserMedia({ video: true }, gumSuccess, gumFail); # this returns a stream and passes it to success callback
            } else {
                insertAltVideo(vid);
                alert("Your browser does not seem to support getUserMedia, using a fallback video instead.");
            }
</code>

in get user media success caallback 

set video element's srcObject to stream 
(or src)
play video vid.play()

## how to perform an animation 

use window.requestAnimFrame(callback)

note this is different from requestAnimationFrame

create a function that performs a render, fx

requestAnimFrame(fx) as first line of function

example: 

<code>
				function positionLoop() {
					requestAnimFrame(positionLoop);
					var positions = ctracker.getCurrentPosition();
					// do something with the positions ...
					// print the positions
					var positionString = "";
					if (positions) {
						for (var p = 0;p < 10;p++) {
							positionString += "featurepoint "+p+" : ["+positions[p][0].toFixed(2)+","+positions[p][1].toFixed(2)+"]<br/>";
						}
						document.getElementById('positions').innerHTML = positionString;
					}
				}
				positionLoop();
</code>

## ctracker methods

init() - called first, no args
start(vid) - attaches ctracker to a video
draw() - draws to canvas(canvasInput) - draws to canvas element 
getCurrentPosition() - gives an array of length 71 with landmarks (look up github page for which point refers to which landmark)
    0-14, jawline
    15-18, right eyebrow, rtl
    19-22, left eyebrow, ltr
    23-26, ltrb of left eye, respectively
    27, left eye center 
    28-31, rtlb of right eye
    32, right eye center 
    33, t nose
    34 - 40, nostril sides and bottom ltr
    41, nose bridge m
    42 - 43, nostril top
    44-50 top of upper lip, ltr
    51-55 bottom of bottom lip, rtl
    56-58 top of bottom lip, ltr
    59-61 bottom of top lip, rtl
    62 - bridge, bottom
    63-66 - ul,ur,br,bl of left eye
    67-70 - ur, ul, bl, br of right eye 

## canvas 2d, rendering makeup 

1. draw a path
    methods:
        beginPath() - call this first
        closePath() - automatically draws a line from current position to start of path
        stroke() - draws shape by stroking outline (call this last?)
        fill() - draws solid shape filling in outline 
        moveTo(x,y) - moves the pencil to point 
        lineTo(x,y) - draws straight line 
        arcTo(x1,y1,x2,y2, radius) - draws arc with
        quadraticCurveTo(cplx, cply, x, y) - where cpl is control point
        bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y) - has 2 control points
2.  svg path newPath2D('<!-- svg path -->')
3.  colors 
    setting pencil and paintbrush
        1.  ctx.fillStyle = ...
        2.  ctx.strokeStyle = ...
        3.  transparency - use rgba i.e. ctx.fillStyle = 'rgba(255, 0, 0, 0.5')
            or ctx.globalAlpha = ...
4.  line props 
    a.  lineWidth = 0-10 (pixels?)
    b.  linejoin - affects appearance of corners, bevel, round, miter
5.  gradients
    a.  ctx.createLinearGradient(x1, y1, x2, y2)
    b.  ctx.createRadialGradient(x1, y1, r1, x2, y2, r2)
    c.  gradient.addColorStop(position, color) - position is relative position 0.0 to 1.0 set on a gradient object returned from above 
## definitions 

