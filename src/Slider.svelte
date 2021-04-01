<svelte:options tag="slider-main" />

<script>
  var maxRotate = 180;

var slider, content, prev, next;
var slides = [];
var styles = [];
var currentIndex = 0;

// These will get set automatically after DOM content is rendered
var sliderWidth = 800;
var slideWidth = 300;
var perspective = 1000;


/*
 * Wrapping Array
 * This is a simple class that takes slices of an array, but wraps
 * back around to the beginning when it reaches the end
 * Ex:
 * var wa = new WrappingArray([0, 1, 2, 3, 4])
 * wa.slice(0) // outputs [0, 1, 2, 3, 4]
 * wa.slice(3) // outputs [2, 3, 4, 0, 1]
*/
function WrappingArray (list) {
  this.list = Array.prototype.slice.call(list)
  Object.defineProperty(this, 'length', {
    get: function () {
      return this.list.length;
    }
  })
}

WrappingArray.prototype.slice = function slice (start, end) {
  var l = this.length,
      s = start,
      e = end || l + s,
      res = [];
  while (s < e) {
    res.push(this.list[((s % l) + l) % l]);
    s++;
  }
  return res;
}

WrappingArray.prototype.map = function map () {
  return Array.prototype.map.apply(this.list, arguments);
}

// Initialize values and add event listeners (for next/prev buttons)
function init () {
  var shadowroot = document.querySelector('slider-main').shadowRoot;

  slides = new WrappingArray(shadowroot.querySelectorAll('.slide'));
  slider = shadowroot.querySelector('.slider');
  content = shadowroot.querySelector('.slider-content');
  next = shadowroot.querySelector('.next');
  prev = shadowroot.querySelector('.prev');

  console.log(content)

  
  

  next.addEventListener('click', incSlides);
  prev.addEventListener('click', decSlides);

  sliderWidth = slider.getBoundingClientRect().width;
  slideWidth = slides.list[0].getBoundingClientRect().width;
  perspective = +(window.getComputedStyle(content).getPropertyValue('perspective').replace(/px/, ''));
  calculateStyles();
  sliderToIndex();
}

// The styles list is calculated once, then used by the wrapping array of slides
// to update the elements' styles
function calculateStyles() {
  var rotateInterval = maxRotate / slides.length;
  var translateInterval = sliderWidth * 2 / slides.length;
  var center = Math.floor(slides.length / 2);
  var slideOffset = sliderWidth / 2 - slideWidth / 2;

  styles = slides.map(function(slide, idx) {
    var offset = (idx + center) % slides.length - center;
    var translateX = (offset * translateInterval) + slideOffset;
    var translateZ = -Math.abs(offset * perspective / 2);
    return {
      rotate: -offset * rotateInterval,
      translateX: translateX,
      translateZ: translateZ,
      opacity: 1 - Math.abs(2 * offset / slides.length * 0.75)
    }
  })
}

// Set currentIndex (of the slider) to index and update styles
function sliderToIndex (idx) {
  currentIndex = idx || currentIndex;
  var els = slides.slice(currentIndex);
  els.forEach(function (el, i) {
    var style = styles[i];
    el.style.transform = 'translate3d(' +
      style.translateX + 'px, 0, ' +
      style.translateZ + 'px)' +
      ' rotateY(' + style.rotate + 'deg)';
    el.style.opacity = style.opacity;
  })
}

function incSlides () {
  sliderToIndex(currentIndex + 1);
}

function decSlides () {
  sliderToIndex(currentIndex - 1);
}

// Initialize and update
document.addEventListener('DOMContentLoaded', init);
</script>



<section class="wrap">
  <div class="slider">
    <div class="slider-content">
      <div class="slide slide-0"><h1>Slide 0</h1></div>
      <div class="slide slide-1"><h1>Slide 1</h1></div>
      <div class="slide slide-2"><h1>Slide 2</h1></div>
      <div class="slide slide-3"><h1>Slide 3</h1></div>
      <div class="slide slide-4"><h1>Slide 4</h1></div>
      <div class="slide slide-5"><h1>Slide 5</h1></div>
      <div class="slide slide-6"><h1>Slide 6</h1></div>
    </div>
    <div class="slider-content-fade"></div>
    <div class="controls">
      <div class="prev">&laquo;</div>
      <div class="next">&raquo;</div>
    </div>
  </div>
</section>

<style>
  .wrap {
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  height: 100%;
}

.slider {
  display: flex;
  align-items: center;
  position: relative;
  min-height: 400px;
  height: 400px;
  width: 960px;
  overflow: hidden;
}

.slider-content {
  display: flex;
  flex: 0 0 100%; /* for IE */
  -ms-flex: 0 0 100%; /* for IE */
  align-items: center;
  position: absolute;
  perspective: 700px;
  transform-style: preserve-3d;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  height: 400px;
}

.slide {
  position: absolute;
  min-height: 200px;
  border: 1px solid black;
  width: 300px;
  display: flex;
  flex: 0 0 100%;
  -ms-flex: 0 0 100%; /* for IE */
  align-items: center;
  justify-content: center;
  transition: transform 320ms ease-in-out, opacity 360ms linear;
  transform-origin: center center;
  transform: translate3d(250px, 0, 0);
  background-color: white;
  transform-style: preserve-3d;
}

.controls {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  z-index: 10;
  pointer-events: none;
}

.prev, .next {
  min-height: 44px;
  width: 44px;
  border-radius: 50%;
  background-color: grey;
  pointer-events: auto;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  font-size: 20px;
}
</style>