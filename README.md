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
## definitions 

