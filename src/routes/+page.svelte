<script>
	import { goto } from '$app/navigation';

	let myIP = '';
	let partnerIP = '';
	let initial = true;

	$: myIPValid = regex.test(myIP);
	$: partnerIPValid = regex.test(partnerIP);
	$: isError = !myIPValid || !partnerIPValid;

	const regex = new RegExp(/^((25[0-5]|(2[0-4]|1\d|[1-9]|)\d)\.?\b){4}$/);

	const handleNext = () => {
		initial = false;
		if (isError) return;
		goto(`/${myIP}?compare=${partnerIP}`);
	};
</script>

<div class="flex items-center justify-center h-screen w-full">
	<div class="w-full max-w-xs">
		<h1 class="text-white text-2xl font-bold mb-6 text-center">LoL Observer Syncer</h1>
		<form class="bg-gray-800 shadow-md rounded px-8 pt-6 pb-8 mb-4">
			<div class="mb-4">
				<label class="block text-white text-sm font-bold mb-2" for="username">Your IP</label>
				<input
					class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
					class:dark:border-red-500={!initial && isError && !myIPValid}
					class:border-red-500={!initial && isError && !myIPValid}
					class:mb-4={initial && !isError && myIPValid}
					id="username"
					type="text"
					bind:value={myIP}
					placeholder="127.0.0.1"
				/>
				{#if !initial && isError && !myIPValid}
					<p class="text-red-500 text-xs italic">Please enter a valid IP address.</p>
				{/if}
			</div>
			<div class="mb-6">
				<label class="block text-white text-sm font-bold mb-2" for="password">
					Your Partners IP
				</label>
				<input
					class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
					class:border-red-500={!initial && isError && !partnerIPValid}
					class:dark:border-red-500={!initial && isError && !partnerIPValid}
					class:mb-4={initial && !isError && partnerIPValid}
					id="password"
					type="text"
					bind:value={partnerIP}
					placeholder="123.45.78.9"
				/>
				{#if !initial && isError && !partnerIPValid}
					<p class="text-red-500 text-xs italic">Please enter a valid IP address.</p>
				{/if}
			</div>
			<div class="flex items-center justify-between">
				<button
					class="w-full bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
					type="button"
					on:click={handleNext}
				>
					Next
				</button>
			</div>
		</form>
		<p class="text-center text-gray-500 text-xs">Made by <a href="https://cinzya.gg">Cinzya</a></p>
	</div>
</div>
