<script>
	let time = 0
  let prioObj = 1
  let prioImSp = 1
  let weightObj = 0
  let weightImSp = 0
  let obj = 1
  let imSp = 1
  let imSpImSp = 1
  let prioSum = 0
  let object = []

  const advanceTime = () => {
    imSp += weightImSp*imSpImSp
    obj += weightObj*imSp
    time++
  }

  const updatePrioSum = () => {
    prioSum = prioObj + prioImSp
    weightObj = prioObj/prioSum
    weightImSp = prioImSp/prioSum
  }
</script>

<main>
	<div class="container">
    <h3>Base Values</h3>
    <div>Obj = 1 | Obj imPs = 1 | Obj imPs imPs = 1</div>
    <h3>Priorities</h3>
    <div>Improvement Speed</div>
    <input type="number" bind:value={prioImSp} on:input={updatePrioSum}>
    <div>Objective</div>
    <input type="number" bind:value={prioObj} on:input={updatePrioSum}>
    <h4>Weights:</h4>
    <div>ImSp: {weightImSp}</div>
    <div>Obj: {weightObj}</div>
    <h3>Current Status</h3>
    <button class="btn-dark" on:click={advanceTime}>Time +1</button>
    <div>Time: {time}</div>
    <div>imSp: {imSp}</div>
    <div>obj: {obj}</div>
	</div>
</main>

<style>

</style>