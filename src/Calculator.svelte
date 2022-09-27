<script>
	let total = '';
	let state = null;
	let output = '';
	let calculations = '';
	let operand = '';

	const ops = ['+', '-', 'x', '/'];

	function resolveState() {
		if (calculations.slice(-1) == '=') {
			calculations = output;
		}
		console.log(output);
		switch (state) {
			// case 'equals':
			// 	calculations = calculations + '=';

			case 'add':
				total = parseFloat(operand) + parseFloat(output);
				calculations = calculations + '+';
				output = '0';
				break;
			case 'subtract':
				total = parseFloat(operand) - parseFloat(output);
				calculations = calculations + '-';
				output = '0';
				break;
			case 'multiply':
				total = parseFloat(operand) * parseFloat(output);
				calculations = calculations + 'x';
				output = '0';
				break;
			case 'divide':
				total = parseFloat(operand) / parseFloat(output);
				calculations = calculations + '/';
				output = '0';
				break;
			default:
				total = parseFloat(output);
				output = '0';
				break;
		}
	}

	function setOperation(operation) {
		state = operation;
		operand = output;
		resolveState();
	}

	function setValue(value) {
		if (output.toString() == '0' || state == 'equal') {
			output = '';
		}
		if (state == 'equal') {
			state = null;
			calculations = '';
		}

		if (value == 'C') {
			total = 0;
			state = null;
			output = '';
			calculations = '';
			return;
		}
		// if (output.toString().search('.') != -1) {
		// 	return;
		// }
		calculations = calculations + value;
		output = output + value;
	}
	function equal() {
		resolveState();
		console.log(calculations.slice(-1));
		if (ops.includes(calculations.slice(-1))) {
			calculations = calculations.slice(0, -1) + '=';
		}

		output = total;
		state = 'equal';
		total = 0;
	}
</script>

<div class="calculator">
	<div class="screen">
		<input
			class="calculations"
			type="text"
			bind:value={calculations}
			readonly="true"
		/>
		<input class="output" type="text" bind:value={output} readonly="true" />
	</div>
	<p>{total}</p>
	<div class="buttons">
		<div class="operations">
			<button
				on:click={() => {
					setOperation('add');
				}}>+</button
			>
			<button
				on:click={() => {
					setOperation('subtract');
				}}>-</button
			>
			<button
				on:click={() => {
					setOperation('multiply');
				}}>&times;</button
			>
			<button
				on:click={() => {
					setOperation('divide');
				}}>&divide;</button
			>
		</div>
		<div class="numbers">
			<div>
				<button
					class="number"
					on:click={() => {
						setValue(7);
					}}>7</button
				>
				<button
					class="number"
					on:click={() => {
						setValue(8);
					}}>8</button
				>
				<button
					class="number"
					on:click={() => {
						setValue(9);
					}}>9</button
				>
			</div>
			<div>
				<button
					class="number"
					on:click={() => {
						setValue(4);
					}}>4</button
				>
				<button
					class="number"
					on:click={() => {
						setValue(5);
					}}>5</button
				>
				<button
					class="number"
					on:click={() => {
						setValue(6);
					}}>6</button
				>
			</div>
			<div>
				<button
					class="number"
					on:click={() => {
						setValue(1);
					}}>1</button
				>
				<button
					class="number"
					on:click={() => {
						setValue(2);
					}}>2</button
				>
				<button
					class="number"
					on:click={() => {
						setValue(3);
					}}>3</button
				>
			</div>
			<div>
				<button
					class="number"
					on:click={() => {
						setValue(0);
					}}>0</button
				>
				<button
					class="number"
					on:click={() => {
						setValue('.');
					}}>.</button
				>
				<button
					class="number"
					on:click={() => {
						setValue('C');
					}}>C</button
				>
			</div>
		</div>
		<div class="equal">
			<button on:click={equal}>=</button>
		</div>
	</div>
</div>

<style>
	.screen {
		display: flex;
		flex-direction: column;
		border: solid black;
	}
	.calculations,
	.output {
		border-style: none;
	}

	.number {
		/* background-color: blue; */
		/* display: flex;  */
		padding: 1rem;
		border-radius: 0.2rem;
	}

	.numbers {
		/* background-color: red; */
		display: inline-flexbox;
		/* justify-items: space-between; */
		padding: 1rem;
		margin: 1rem;
	}
</style>
