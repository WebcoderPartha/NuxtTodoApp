<template>
    <div class="h-[600px] bg-gray-700">
        <section class="p-6 dark:text-gray-100">
		<form novalidate="" class="container w-full max-w-xl p-8 mx-auto space-y-6 rounded-md shadow dark:bg-gray-900">
			<h2 class="w-full text-3xl font-bold leadi text-center">Add Todo</h2>
			<div>
				<label for="name" class="block mb-1 ml-1">Write todo</label>
				<input id="name" type="text" placeholder="Type your text" required="" class="block w-full p-2 rounded focus:outline-none focus:ring focus:ri focus:ri dark:bg-gray-800">
			</div>
			<!-- <div>
				<label for="email" class="block mb-1 ml-1">Email</label>
				<input id="email" type="email" placeholder="Your email" required="" class="block w-full p-2 rounded focus:outline-none focus:ring focus:ri focus:ri dark:bg-gray-800">
			</div>
			<div>
				<label for="message" class="block mb-1 ml-1">Message</label>
				<textarea id="message" type="text" placeholder="Message..." class="block w-full p-2 rounded autoexpand focus:outline-none focus:ring focus:ri focus:ri dark:bg-gray-800"></textarea>
			</div> -->
			<div>
				<button type="submit" class="w-full px-4 py-2 font-bold rounded shadow focus:outline-none focus:ring hover:ring focus:ri dark:bg-violet-400 focus:ri hover:ri dark:text-gray-900">Add Todo</button>
			</div>
		</form>
	</section>
        
    </div>
</template>

<script setup>

</script>

<style lang="scss" scoped>

</style>