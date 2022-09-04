<main>
	<h2>Convert a Java SQL string to plain SQL</h2>

	<textarea placeholder="Enter SQL string here..." bind:value={sqlString} autofocus></textarea>

	<button on:click={convert}>To SQL!</button>
	<button class="warning" on:click={clear}>Clear</button>
</main>

<script>
	let sqlString;

	function clear(event) {
		sqlString = "";
	}

	function convert(event) {
		let workingString = sqlString;

		if (workingString == null || workingString.length <= 0) {
			// Abort - nothing in the string!
			alert("Your string is empty.  Please hang up, and try again.")
		}

		sqlString = parse(workingString);
	}

	function parse(workingString) {
		let newString = '';
		let workingStrLen = workingString.length;

		// Parser state
		let addThisChar = true;
		let inAString = false;
		for (let i = 0; i < workingStrLen; i++) {
			addThisChar = true;
			let thisChar = consume(workingString);

			if (thisChar == '"') {
				// Beginning a string - or maybe ending one!
				inAString = !inAString;

				// Since this is a Java String char and not
				// part of the actual SQL statement, don't add
				// it to our final string.
				addThisChar = false;
			} else if (!inAString) {
				// We are not inside of the Java string containing
				// the actual SQL, so keep skipping until we reach
				// the next string.
				addThisChar = false;
			}

			// Remove the first char
			if (workingString.length > 1) {
				workingString = workingString.substring(1);
			}

			// Add the character, depending on the state
			if (addThisChar) {
				newString += thisChar;
			}
		}
		return newString;
	}

	function consume(s) {
		return s[0];
	}
</script>