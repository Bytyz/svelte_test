<svelte:options tag = "one-select" />

<script>

</script>

<div class="select-block">
  <h1>Pure CSS Select</h1>
  <div class="select">
    <select name="slct" id="slct">
      <option selected disabled>Choose an option</option>
      <option value="1">Pure CSS</option>
      <option value="2">No JS</option>
      <option value="3">Nice!</option>
    </select>
  </div>
</div>

<style>
  .select-block {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: #f39c12;
  height: 300px;
}
  h1 {
  margin: 0 0 0.25em;
}
/* Reset Select */
select {
  -webkit-appearance: none;
  -moz-appearance: none;
  -ms-appearance: none;
  appearance: none;
  outline: 0;
  box-shadow: none;
  border: 0 !important;
  background: #2c3e50;
  background-image: none;
}
/* Remove IE arrow */
select::-ms-expand {
  display: none;
}
/* Custom Select */
.select {
  position: relative;
  display: flex;
  width: 20em;
  height: 3em;
  line-height: 3;
  background: #2c3e50;
  overflow: hidden;
  border-radius: .25em;
}
select {
  flex: 1;
  padding: 0 .5em;
  color: #fff;
  cursor: pointer;
}
/* Arrow */
.select::after {
  content: '\25BC';
  position: absolute;
  top: 0;
  right: 0;
  padding: 0 1em;
  background: #34495e;
  cursor: pointer;
  pointer-events: none;
  -webkit-transition: .25s all ease;
  -o-transition: .25s all ease;
  transition: .25s all ease;
}
/* Transition */
.select:hover::after {
  color: #f39c12;
}

</style>