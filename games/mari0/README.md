---
layout: game
title: "Mari0"
folder: games/mari0
gamefile: game.js
description: "Mari0 is a fan-made video game that combines elements of the video games Super Mario Bros and Portal."
download: false
---

<center>
    <div>
        <canvas id="canvas" oncontextmenu="event.preventDefault()"></canvas>
        <canvas id="loadingCanvas" oncontextmenu="event.preventDefault()" width="800" height="600"></canvas>
    </div>
    <br>
    <p>By Stabyourself.net</p>
    <p><a href="http://stabyourself.net/mari0/" target="_blank">Original Download Page</a></p>
</center>

<script type='text/javascript'>
    window.addEventListener("keydown", function(e) {
        // space and arrow keys
        if ([32, 37, 38, 39, 40].indexOf(e.keyCode) > -1) {
            e.preventDefault();
        }
    }, false);

    var loadingContext = document.getElementById('loadingCanvas').getContext('2d');

    function drawLoadingText(text) {
        var canvas = loadingContext.canvas;

        loadingContext.fillStyle = 'white';
        loadingContext.fillRect(0, 0, canvas.scrollWidth, canvas.scrollHeight);

        loadingContext.font = '2em arial';
        loadingContext.textAlign = 'center'
        loadingContext.fillStyle = 'black';
        loadingContext.fillText(text, canvas.scrollWidth / 2, canvas.scrollHeight / 2);

        loadingContext.fillText("Powered By Emscripten.", canvas.scrollWidth / 2, canvas.scrollHeight / 4);
        loadingContext.fillText("Powered By LÖVE.", canvas.scrollWidth / 2, canvas.scrollHeight / 4 * 3);
    }

    var Module = {
        arguments: ['./'],
        printErr: console.error.bind(console),
        canvas: (function() {
            var canvas = document.getElementById('canvas');

            // As a default initial behavior, pop up an alert when webgl context is lost. To make your
            // application robust, you may want to override this behavior before shipping!
            // See http://www.khronos.org/registry/webgl/specs/latest/1.0/#5.15.2
            canvas.addEventListener("webglcontextlost", function(e) {
                alert('WebGL context lost. You will need to reload the page.');
                e.preventDefault();
            }, false);

            return canvas;
        })(),
        memoryInitializerPrefixURL: '',
        setStatus: function(text) {
            if (text) {
                drawLoadingText(text);
            } else if (Module.didSyncFS && Module.remainingDependencies === 0) {
                Module.callMain(Module.arguments);
                document.getElementById('loadingCanvas').style.display = 'none';
                document.getElementById('canvas').style.display = 'block';
            }
        },
        didSyncFS: false,
        totalDependencies: 0,
        remainingDependencies: 0,
        monitorRunDependencies: function(left) {
            this.remainingDependencies = left;
            this.totalDependencies = Math.max(this.totalDependencies, left);
            Module.setStatus(left ? 'Preparing... (' + (this.totalDependencies - left) + '/' + this.totalDependencies + ')' : 'All downloads complete.');
        }
    };
    Module.setStatus('Downloading...');
    window.onerror = function(event) {
        // TODO: do not warn on ok events like simulating an infinite loop or exitStatus
        Module.setStatus('Exception thrown, see JavaScript console');
        Module.setStatus = function(text) {
            if (text) Module.printErr('[post-exception status] ' + text);
        };
    };
</script>
<script type="text/javascript" src="game.js"></script>
<script async type="text/javascript" src="love.js"></script>