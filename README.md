[![Typing SVG](https://readme-typing-svg.demolab.com?font=Lacquer&size=18&pause=1000&width=435&lines=ea%C3%AD%2C+me+chamo+gabriel+e+eu+trabalho+com+dados)](https://git.io/typing-svg)

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Morphing Text Effect</title>

<style>
  body {
    margin: 0;
    height: 100vh;
    background-color: #0d0d0d;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }

  #container {
    position: relative;
    filter: url(#threshold) blur(0.6px);
  }

  svg {
    position: absolute;
    width: 100%;
    height: 100%;
  }

  text {
    fill: #00ccff;
    font-family: "Arial Black", sans-serif;
    font-size: 100px;
    text-anchor: middle;
    dominant-baseline: middle;
    x: 50%;
    y: 50%;
    transform: translateY(0.35em);
  }
</style>
</head>

<body>
  <div id="container">
    <svg viewBox="0 0 800 300">
      <filter id="threshold">
        <feColorMatrix in="SourceGraphic" type="matrix"
          values="1 0 0 0 0  
                  0 1 0 0 0  
                  0 0 1 0 0  
                  0 0 0 255 -140" />
      </filter>
      <text id="text1">Loading</text>
      <text id="text2">Morphing</text>
    </svg>
  </div>

<script>
const elts = {
	text1: document.getElementById("text1"),
	text2: document.getElementById("text2")
};

// Os textos que você quer que mudem
const texts = [
	"Welcome",
	"to",
	"my",
	"GitHub",
	"Profile",
	"Gabriel"
];

const morphTime = 1;
const cooldownTime = 0.25;

let textIndex = texts.length - 1;
let time = new Date();
let morph = 0;
let cooldown = cooldownTime;

elts.text1.textContent = texts[textIndex % texts.length];
elts.text2.textContent = texts[(textIndex + 1) % texts.length];

function doMorph() {
	morph -= cooldown;
	cooldown = 0;
	let fraction = morph / morphTime;
	if (fraction > 1) {
		cooldown = cooldownTime;
		fraction = 1;
	}
	setMorph(fraction);
}

function setMorph(fraction) {
	elts.text2.style.filter = `blur(${Math.min(8 / fraction - 8, 100)}px)`;
	elts.text2.style.opacity = `${Math.pow(fraction, 0.4) * 100}%`;

	fraction = 1 - fraction;
	elts.text1.style.filter = `blur(${Math.min(8 / fraction - 8, 100)}px)`;
	elts.text1.style.opacity = `${Math.pow(fraction, 0.4) * 100}%`;

	elts.text1.textContent = texts[textIndex % texts.length];
	elts.text2.textContent = texts[(textIndex + 1) % texts.length];
}

function doCooldown() {
	morph = 0;
	elts.text2.style.filter = "";
	elts.text2.style.opacity = "100%";
	elts.text1.style.filter = "";
	elts.text1.style.opacity = "0%";
}

function animate() {
	requestAnimationFrame(animate);
	let newTime = new Date();
	let shouldIncrementIndex = cooldown > 0;
	let dt = (newTime - time) / 1000;
	time = newTime;
	cooldown -= dt;
	if (cooldown <= 0) {
		if (shouldIncrementIndex) textIndex++;
		doMorph();
	} else {
		doCooldown();
	}
}
animate();
</script>
</body>
</html>

