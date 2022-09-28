<script>
	let total = 0;
	let state = null;
	let output = '';
	let calculations = '';
	let operand = '';

	const ops = ['+', '-', 'x', '/'];

	function resolveState() {
		if (calculations.slice(-1) == '=') {
			calculations = output;
		}
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
		if (total === 0) {
			operand = output;
		} else {
			operand = total;
		}
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
		// console.log(calculations);
		console.log(calculations.toString().slice(-1));
		if (ops.includes(calculations.toString().slice(-1))) {
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
	<!-- <p>{total}</p> -->
	<div class="buttons">
		<div class="operations">
			<button
				class="button-style"
				on:click={() => {
					setOperation('add');
				}}>+</button
			>
			<button
				class="button-style"
				on:click={() => {
					setOperation('subtract');
				}}>-</button
			>
			<button
				class="button-style"
				on:click={() => {
					setOperation('multiply');
				}}>&times;</button
			>
			<button
				class="button-style"
				on:click={() => {
					setOperation('divide');
				}}>&divide;</button
			>
		</div>
		<div class="numbers">
			<div>
				<button
					class="button-style"
					on:click={() => {
						setValue(7);
					}}>7</button
				>
				<button
					class="button-style"
					on:click={() => {
						setValue(8);
					}}>8</button
				>
				<button
					class="button-style"
					on:click={() => {
						setValue(9);
					}}>9</button
				>
			</div>
			<div>
				<button
					class="button-style"
					on:click={() => {
						setValue(4);
					}}>4</button
				>
				<button
					class="button-style"
					on:click={() => {
						setValue(5);
					}}>5</button
				>
				<button
					class="button-style"
					on:click={() => {
						setValue(6);
					}}>6</button
				>
			</div>
			<div>
				<button
					class="button-style"
					on:click={() => {
						setValue(1);
					}}>1</button
				>
				<button
					class="button-style"
					on:click={() => {
						setValue(2);
					}}>2</button
				>
				<button
					class="button-style"
					on:click={() => {
						setValue(3);
					}}>3</button
				>
			</div>
			<div>
				<button
					class="button-style"
					on:click={() => {
						setValue(0);
					}}>0</button
				>
				<button
					class="button-style"
					on:click={() => {
						setValue('.');
					}}>.</button
				>
				<button
					class="button-style"
					on:click={() => {
						setValue('C');
					}}>C</button
				>
			</div>
		</div>
		<div class="equal">
			<button class="button-style" on:click={equal}>=</button>
		</div>
	</div>
</div>

<style>
	.calculator {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 50%;
		padding: 2rem;
		border-radius: 1rem;
		background-color: grey;
		box-shadow: rgba(45, 35, 66, 0.4) 0 2px 4px,
			rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #d6d6e7 0 -8px 0 inset;
	}

	.screen {
		display: flex;
		flex-direction: column;
		border: solid darkgray;
		border-radius: 0.5rem;
		padding: 0;
		background-color: white;
	}
	.calculations,
	.output {
		border-style: none;
		padding: 0rem;
		margin-right: 1rem;
		text-align: right;
	}

	.output {
		font-size: 2rem;
		font-weight: bold;
		padding: 0;
		/* border: red solid; */
	}

	.calculations {
		opacity: 0.7;
		margin-top: 0.2rem;
	}

	.calculator .buttons {
		display: flex;
		flex-wrap: wrap;
	}

	.calculator .buttons .operations {
		display: flex;
		justify-content: space-between;
		width: 100%;
	}

	.calculator .buttons .operations button {
		width: 24%;
	}

	.calculator .buttons .numbers {
		width: 75%;
	}

	.calculator .buttons .numbers > div {
		display: flex;
		justify-content: space-between;
	}

	.calculator .buttons .numbers > div button {
		width: 32%;
	}

	.calculator .equal {
		flex: 1;
	}

	.calculator .equal button {
		margin-left: 5%;
		width: 95%;
		height: 95%;
		background-color: lightblue;
	}

	.calculator .buttons {
		outline: none;
		border-radius: 0.2rem;
		padding-top: 1rem;
	}

	.button-style {
		align-items: center;
		appearance: none;
		background-color: #fcfcfd;
		border-radius: 4px;
		border-width: 0;
		box-shadow: rgba(45, 35, 66, 0.4) 0 2px 4px,
			rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #d6d6e7 0 -3px 0 inset;
		box-sizing: border-box;
		color: #36395a;
		cursor: pointer;
		display: inline-flex;
		height: 48px;
		justify-content: center;
		line-height: 1;
		list-style: none;
		overflow: hidden;
		padding-left: 16px;
		padding-right: 16px;
		position: relative;
		text-align: left;
		text-decoration: none;
		transition: box-shadow 0.15s, transform 0.15s;
		user-select: none;
		-webkit-user-select: none;
		touch-action: manipulation;
		white-space: nowrap;
		will-change: box-shadow, transform;
		font-size: 1.7rem;
	}

	/* .button-style:focus {
		box-shadow: #d6d6e7 0 0 0 1.5px inset, rgba(45, 35, 66, 0.4) 0 2px 4px,
			rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #d6d6e7 0 -3px 0 inset;
	}

	.button-style:hover {
		box-shadow: rgba(45, 35, 66, 0.4) 0 4px 8px,
			rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #d6d6e7 0 -3px 0 inset;
		transform: translateY(-2px);
	} */

	.button-style:active {
		box-shadow: #d6d6e7 0 3px 7px inset;
		transform: translateY(2px);
	}
</style>
