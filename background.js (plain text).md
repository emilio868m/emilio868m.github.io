(function () {

var canvas = document.getElementById("bg");

var ctx = canvas.getContext("2d");

var t = 0;

var w = 0;

var h = 0;

var allLayers = \[

{ y: 0.55, amp: 26, freq: 0.006, speed: 0.012, color: "rgba(56, 189, 248, 0.35)" },

{ y: 0.65, amp: 32, freq: 0.005, speed: 0.016, color: "rgba(14, 165, 233, 0.40)" },

{ y: 0.75, amp: 38, freq: 0.004, speed: 0.020, color: "rgba(2, 132, 199, 0.50)" },

{ y: 0.85, amp: 44, freq: 0.0035, speed: 0.024, color: "rgba(3, 105, 161, 0.65)" }

\];

var layers = allLayers;

var step = 10;

function resize() {

var dpr = Math.min(window.devicePixelRatio || 1, 2);

w = window.innerWidth;

h = window.innerHeight;

canvas.width = w \* dpr;

canvas.height = h \* dpr;

ctx.setTransform(dpr, 0, 0, dpr, 0, 0);

var small = w < 600;

layers = small ? allLayers.slice(1) : allLayers;

step = small ? 16 : 10;

}

window.addEventListener("resize", resize);

resize();

function frame() {

var sky = ctx.createLinearGradient(0, 0, 0, h);

sky.addColorStop(0, "#0f172a");

sky.addColorStop(1, "#1e3a5f");

ctx.fillStyle = sky;

ctx.fillRect(0, 0, w, h);

layers.forEach(function (l, i) {

var dir = i % 2 ? -1 : 1;

ctx.beginPath();

ctx.moveTo(0, h);

for (var x = 0; x <= w + step; x += step) {

var y = h \* l.y

- Math.sin(x \* l.freq + t \* l.speed \* dir) \* l.amp
- Math.sin(x \* l.freq \* 2.3 + t \* l.speed \* 1.4) \* (l.amp \* 0.3);

ctx.lineTo(x, y);

}

ctx.lineTo(w, h);

ctx.closePath();

ctx.fillStyle = l.color;

ctx.fill();

});

t += 1;

requestAnimationFrame(frame);

}

if (!window.matchMedia("(prefers-reduced-motion: reduce)").matches) {

frame();

}

})();
