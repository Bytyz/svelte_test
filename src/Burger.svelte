<svelte:options tag = "burger-menu" />

<script>
	export let name = "test burger";
  console.log (name)
</script>

<section class="navigation">
	<h1>Burger Menu </h1>

	<input id="burger" type="checkbox" />
	
	<label for="burger">
		<span></span>
		<span></span>
		<span></span>
	</label>
	
	<nav>    
	  <ul>
		<li><a href="/test/public/index.html">index</a></li>
		<li><a href="/test/public/main.html">main</a></li>
		<li><a href="/test/public/bounce.html">bounce</a></li>
	  </ul>  
	</nav>
</section>

<style>
  
.navigation {
  display: flex;
  align-items: center;
  height: 80px;
}
h1 {
  z-index: 6;
  font-size: 20px;
  font-weight: 900;
  font-family: sans-serif;
  text-transform: uppercase;
}
input + label {
  position: fixed;
  top: 40px;
  right: 40px;
  height: 20px;
  width: 15px;
  z-index: 5;
}
input + label span {
  position: absolute;
  width: 100%;
  height: 2px;
  top: 50%;
  margin-top: -1px;
  left: 0;
  display: block;
  background: #020304;
  transition: 0.5s;
}
input + label span:first-child {
  top: 3px;
}
input + label span:last-child {
  top: 16px;
}
label:hover {
  cursor: pointer;
}
input:checked + label span {
  opacity: 0;
  top: 50%;
}
input:checked + label span:first-child {
  opacity: 1;
  transform: rotate(405deg);
}
input:checked + label span:last-child {
  opacity: 1;
  transform: rotate(-405deg);
}
input ~ nav {
  background: #fbdb5b;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100px;
  z-index: 3;
  transition: 0.5s;
  transition-delay: 0.5s;
  overflow: hidden;
}
input ~ nav > ul {
  text-align: center;
  position: absolute;
  top: 35%;
  left: 20%;
  right: 20%;
}
input ~ nav > ul > li {
  opacity: 0;
  transition: 0.5s;
  transition-delay: 0s;
  list-style: none;
}
input ~ nav > ul > li > a {
  text-decoration: none;
  text-transform: uppercase;
  color: #020304;
  font-weight: 700;
  font-family: sans-serif;
  display: block;
  padding: 10px;
}
input:checked ~ nav {
  height: 200px;
  transition-delay: 0s;
}
input:checked ~ nav > ul > li {
  opacity: 1;
  transition-delay: 0.5s;
}
</style>