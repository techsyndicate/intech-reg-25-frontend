<script>
	import { Notyf } from "../node_modules/notyf";
	const events = [
		[
			"App Development",
			"Web Development",
			"Game Development",
			"Encryptid | Cryptic Hunt X CTF",
		],
		["3D Design", "2D Design", "UI Design", "A/V editing"],
		["Group Discussion", "Crossword", "Gaming", "Photography"],
		["Competitive Programming", "Film Making", "Hardware", "Cubing"],
		["Quiz", "Canva (Jr)", "Scratch (Jr)", "Paint 3D (Jr)"],
		["Adobe Express (Jr)", "PPT Making (Jr)"]
	];

	let selected = [];
	let notyf = new Notyf();

	function removeSpace() {
		var email = window.document.getElementById("email").value;
		email = email.replace(/\s/g, "");
		window.document.getElementById("email").value = email;
	}

	function submit() {
		console.log("hey");
		var vehicle1 = document.getElementById("vehicle1").checked;
		var name = window.document.getElementById("name").value;
		var dob = window.document.getElementById("dob").value;
		var email = window.document.getElementById("email").value.trim();
		var parentemail = window.document
			.getElementById("parentemail")
			.value.trim();
		var phone = window.document.getElementById("phone").value;
		var adno = window.document.getElementById("adno").value;
		var classs = window.document.getElementById("class").value;
		var section = window.document.getElementById("section").value;
		if (!vehicle1) {
			notyf.error("Please ask your parents to consent for Discord!");
			return;
		}
		if (
			!(
				name &&
				dob &&
				email &&
				phone &&
				adno &&
				classs &&
				section &&
				parentemail
			)
		) {
			notyf.error("Please fill all the fields");
			return;
		}
		if (
			new Date(dob).getFullYear() < 2000 ||
			new Date(dob).getFullYear() > 2016
		) {
			notyf.error("Please enter a valid date of birth");
			return;
		}
		if (phone.length != 10) {
			notyf.error("Please enter a valid phone number");
			return;
		}
		// email regex
		var re =
			/^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
		if (!re.test(email)) {
			notyf.error("Please enter a valid email");
			return;
		}
		if (email.includes('@ais.amity.edu') == false) {
			notyf.error("Please enter a valid school email");
			return;
		}
		if (!re.test(parentemail)) {
			notyf.error("Please enter a valid email");
			return;
		}
		if (adno.length < 4 || adno.length > 5) {
			notyf.error("Please enter a valid admission number");
			return;
		}
		if (classs < 7 || classs > 12) {
			notyf.error("Please enter a valid class");
			return;
		}
		var validSections = [
			"a",
			"b",
			"c",
			"d",
			"e",
			"f",
			"g",
			"h",
			"i",
			"j",
			"s1",
			"s2",
			"sync",
			"synconnect",
			"synchro",
		];
		if (validSections.includes(section.toLowerCase()) == false) {
			notyf.error("Please enter a valid section");
			return;
		}
		window.document.getElementById("reg-container").style.display = "none";
		window.document.getElementById("reg-container-2").style.display ="flex";
	}

	function select(string) {
		var x = document.getElementById(string);
		x.style.transition = "all 500ms ease";
		if (x.style.color == "rgb(255, 255, 255)") {
			x.style.border = "3px solid #16e16e";
			x.style.color = "#16e16e";
			selected.push(string);
		} else {
			x.style.color = "rgb(255, 255, 255)";
			x.style.border = "3px solid #2C2A2A";
			var index = selected.indexOf(string);
			if (index > -1) {
				selected.splice(index, 1);
			}
		}
		console.log(selected);
	}

	async function register() {
		var name = window.document.getElementById("name").value;
		var dob = window.document.getElementById("dob").value;
		var email = window.document.getElementById("email").value;
		var parentemail = window.document
			.getElementById("parentemail")
			.value.trim();
		var phone = window.document.getElementById("phone").value;
		var adno = window.document.getElementById("adno").value;
		var classs = window.document.getElementById("class").value;
		var section = window.document.getElementById("section").value;
		if (selected.length == 0) {
			notyf.error("Please select atleast one field");
			return;
		}
		var body = JSON.stringify({
			name: name,
			dob: dob,
			email: email,
			phone: phone,
			adno: adno,
			grade: classs,
			section: section,
			selected: selected,
			parentemail: parentemail,
		});
		window.document.getElementById("regis").disabled = true;
		await fetch("https://intech-25-backend.onrender.com/register", {
			method: "POST",
			headers: {
				"Content-Type": "application/json",
			},
			body: body,
		}).then(async (response) => {
			response = await response.json();
			console.log(response);
			if (
				response.hasOwnProperty("success") &&
				response.success == false
			) {
				notyf.error(response.msg);
				window.document.getElementById("regis").disabled = false;
				return;
			}
			window.document.getElementById("reg-container-2").style.display =
				"none";
			window.document.getElementById("regMail").innerText = email;
			window.document.getElementById("thank-you").style.display = "flex";
		});
	}
</script>

<main>
	<script src="https://cdn.jsdelivr.net/npm/notyf@3/notyf.min.js"></script>
	<div class="nav">
		<img
			src="https://github.com/techsyndicate/website/blob/main/public/assets/images/ts.png?raw=true"
		/>
	</div>
	<div id="reg-container" class="reg-container">
		<h1 class="title">Register</h1>
		<div class="reg-form">
			<div class="reg-form-div-container">
				<div class="reg-form-div ">
					<p for="name">Name</p>
					<input
						id="name"
						type="text"
						name="name"
						placeholder="John Doe"
						style="font-size:1.25rem;"
					/>
				</div>
				<div class="reg-form-div">
					<p for="dob">Date Of Birth</p>
					<input
						id="dob"
						type="date"
						name="dob"
						style="color:rgba(255,255,255,0.44); font-size: 1.25rem;"
					/>
				</div>
			</div>
			<div class="reg-form-div-container">
				<div class="reg-form-div">
					<p for="email">School Email</p>
					<input
						type="text"
						id="email"
						name="email"
						on:input={removeSpace}
						placeholder="john.doe@ais.amity.edu"
						style="font-size: 1.25rem;"
					/>
				</div>
				<div class="reg-form-div">
					<p for="parentemail">Parent Email</p>
					<input
						type="text"
						id="parentemail"
						name="parentemail"
						placeholder="william.doe@gmail.com"
						style="font-size: 1.25rem;"
					/>
				</div>
			</div>
			<div class="reg-form-div-container">
				<div class="reg-form-div">
					<p for="phone">Phone Number</p>
					<input
						type="number"
						id="phone"
						name="phone"
						placeholder="9889889889"
						style="font-size: 1.25rem;"
					/>
				</div>
				<div class="reg-form-div">
					<p for="adno">Admission Number</p>
					<input
						type="number"
						id="adno"
						name="adno"
						placeholder="1111"
						style="font-size: 1.25rem;"
					/>
				</div>
			</div>
			<div class="reg-form-div-container">
				<div class="reg-form-div">
					<p for="class">Grade/Class</p>
					<input
						type="number"
						id="class"
						name="class"
						placeholder="7-12"
						min="7"
						max="12"
						style="font-size: 1.25rem;"
					/>
				</div>
				<div class="reg-form-div">
					<p for="section">Section</p>
					<input
						type="text"
						id="section"
						name="section"
						placeholder="A"
						style="font-size: 1.25rem;"
					/>
				</div>
			</div>
			<div class="reg-form-div-container-parent">
				<div id="parent">

					<input
					type="checkbox"
					id="vehicle1"
					name="vehicle1"
					value="Bike"
					class="age-consent"
					/>
					<span class="color-changer"></span>
					<label style="color: #FFF; font-size: 1rem;" for="vehicle1">
						I consent for my child if above 13 to join the discord
						server of inTech (filled by a parent).
					</label>
				</div>
				<br />
				<div class="inpt-sub-div-1" style="padding-left: 1vw;">
					<button class="inpt-sub" on:click={submit}>Continue</button>
				</div>
			</div>
		</div>
	</div>
	<div id="reg-container-2" class="reg-container">
		<h1 class="title">Choose Interested Fields</h1>
		<div class="reg-form-2">
			{#each events as event}
				<div class="reg-form-div-container">
					{#each event as eventEl}
						<button
							style="color: rgb(255, 255, 255); border: #2C2A2A solid 3px; width: 16vw; padding:0; padding-top:0.7vw; padding-bottom:0.7vw; "
							class="event-but"
							on:click={() => select(eventEl)}
							id={eventEl}>{eventEl}</button
						>
					{/each}
				</div>
			{/each}
		</div>
		<div class="inpt-sub-div">
			<button id="regis" class="inpt-sub" on:click={register}
				>Submit</button
			>
		</div>
	</div>
	<div id="thank-you" class="reg-container">
		<h1 class="title-thank title">Thank You</h1>
		<div class="reg-form-2">
			<p class="thank-you-p">
				Your registration is complete. You have been emailed the details
				at <span id="regMail"></span>
				to proceed further. <br />
				Make sure to check your spam mail
			</p>
		</div>
	</div>
</main>

<style>
	@import url("https://fonts.googleapis.com/css2?family=Outfit:wght@100;200;300;400;500;600;700;800;900&display=swap");
	:global(body) {
		margin: 0;
		padding: 0;
	}
	
	.reg-form input:focus {
		transition: all 500ms;
		transition-timing-function: ease-in-out;
		border: 2px solid white;
		outline: none;
	}

	.reg-form-div-container-parent {
		/* display: flex; */
		font-size: 1rem;
		width: 90%;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		gap: 1vw;
	}

	.reg-form-div-container-parent input {
		width: 2vw !important;
		color: #16e16e;
	}
	#parent {
		display: flex;
		align-items: center;
		gap: 1vw;
		margin-left: -1vw;
	}

	.event-but {
		background-color: #0d0c0c;
		padding: 15px 32px;
		text-align: center;
		text-decoration: none;
		display: inline-block;
		font-size: 16px;
		margin: 1vw;
		cursor: pointer;
		border-radius: 10px;
		width: 20vw;
		font-family: "Outfit", sans-serif;
	}

	#reg-container {
		margin-bottom: 50px;
		display: flex;
	}
	
	#reg-container-2 {
		display: none;
	}
	
	#thank-you {
		display: none;
	}
	#regMail {
		text-decoration: underline;
	}

	.thank-you-p {
		font-size: 1.5rem;
		font-family: "Outfit", sans-serif;
		color: #fff;
		text-align: center;
	}

	.nav img {
		width: 4vw;
		margin-top: 2.5vw;
		margin-left: 2.5vw;
	}

	.title {
		color: #16e16e;
		width: 80vw;
		font-size: 4rem;
		font-weight: 600;
		font-family: "Outfit", sans-serif;
		margin-top: 0;
	}
	.title-thank {
		text-align: center;
	}
	:global(body) {
		background-color: #0d0c0c;
		margin: 0;
		font-family: "Outfit", sans-serif;
	}

	.inpt-sub-div-1 {
		/* width: 60vw;  */
		display: flex;
		/* justify-content: flex-end;  */
	}

	.inpt-sub-div {
		width: 80vw;
		display: flex;
		justify-content: flex-end;
	}

	.inpt-sub {
		margin-top: 15px;
		width: 18vw !important;
		padding-top: 0.8vw;
		padding-bottom: 0.8vw;
		background-color: #16e16e;
		font-size: 1.5rem;
		border-radius: 9px;
		font-family: "Outfit", sans-serif;
		cursor: pointer;
		font-weight: 400;
	}

	.reg-form {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		margin-bottom: 0;
	}

	.reg-form p {
		width: 40vw;
		color: #fff;
		font-size: 1.5rem;
		font-family: "Outfit", sans-serif;
	}

	.reg-form-div {
		display: flex;
		flex-direction: column;
		margin-bottom: 1vw;
		margin-left: 2.9vw;
		width: 40vw;
	}

	.reg-form-div-container {
		display: flex;
		width: 80vw;
		justify-content: center;
		font-family: "Outfit", sans-serif;
	}

	.reg-form input {
		height: 40px;
		width: 30vw;
		border-radius: 7px;
		padding: 1.3vw;
		background-color: #0d0c0c;
		border: #2c2a2a solid 2px;
		font-size: 1rem;
		color: #fff;
		font-family: "Outfit", sans-serif;
	}

	input[type="date"]::-webkit-calendar-picker-indicator {
		filter: invert(1);
		color: #fff;
	}

	input[type=checkbox] {
		/* appearance: none; */
		transform: scale(0.9);
		accent-color: #16e16e;
		background-color: #000;
		margin-top: 0.5vw;
	}
	.color-changer {
		position: absolute;
		top: 20px;
		left: 0;
		height: 10px;
		width: 10px;
		background-color: #000;
	}
	.reg-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	@media screen and (max-width: 1020px) {
		.reg-container {
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;
		}	
		.event-but {
			width: 80vw;
			margin: 1vw auto;
			font-size: 1.5rem;
		}

		#reg-container-2 .title {
			font-size: 3rem;
		}

		h1 {
			width: 85vw;
			font-size: 20vw;
		}

		.nav img {
			width: 17vw;
			margin: 10vw;
		}

		.reg-form p {
			font-size: 2rem;
			margin: 0;
			margin-bottom: 5px;
			width: 90vw;
		}

		.reg-form-div-container {
			width: 101%;
			flex-direction: column;
			display: flex;
			justify-content: center;
		}

		.reg-form input {
			width: 80vw;
			height: 15vw;
			font-size: 2rem;
		}

		.inpt-sub {
			width: 40vw !important;
			height: 15vw;
			font-size: 2rem;
		}

		.inpt-sub-div,
		.inpt-sub-div-1 {
			width: 80vw;
			margin-bottom: 50px;
			display: flex;
			flex-direction: row;
			align-items: center;
			justify-content: center;
		}

		.reg-form-div {
			margin-bottom: 5vw;
		}

		.thank-you-p {
			font-size: 2rem;
			text-align: center;
			width: 80vw;
		}

		.reg-form-div-container-parent {
			flex-direction: column;
			align-items: center;
		}

		.reg-form-div-container-parent input {
			width: 50px !important;
			float: left;
			margin-bottom: 1vw;
		}

		.reg-form-div-container-parent label {
			font-size: 1.2rem !important;
			margin-left: 10px;
			text-align: left;
		}

		.reg-form-div-container-parent {
			width: 85vw;
			margin-bottom: 50px;
		}

		#parent {
			align-items: center;
			display: flex;
			margin: 0;
			justify-content: center;
		}
	}
</style>
