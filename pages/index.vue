<template>
  <div class="h-[600px] bg-gray-700">
    <section class="p-6 dark:text-gray-100">
      <form
        @submit.prevent="todoHandle"
        novalidate=""
        class="container w-full max-w-xl p-8 mx-auto space-y-6 rounded-md shadow dark:bg-gray-900"
      >
        <h2 class="w-full text-3xl font-bold leadi text-center">Add Todo</h2>
        <div>
          <label for="name" class="block mb-1 ml-1">Write todo</label>
          <input
            id="name"
            v-model="todo.todo"
            type="text"
            placeholder="Type your text"
            required=""
            class="block w-full p-2 rounded focus:outline-none focus:ring focus:ri focus:ri dark:bg-gray-800"
          />
          <span class="text-red-900">{{ error.error }}</span>
        </div>
    
        <div>
          <button
            v-if="isPending"
            disabled
            type="submit"
            class="w-full px-4 py-2 font-bold rounded shadow focus:outline-none focus:ring hover:ring focus:ri dark:bg-violet-400 focus:ri hover:ri dark:text-gray-900"
          >
            Adding Todo
          </button>
          <button
            v-else
            type="submit"
            class="w-full px-4 py-2 font-bold rounded shadow focus:outline-none focus:ring hover:ring focus:ri dark:bg-violet-400 focus:ri hover:ri dark:text-gray-900"
          >
            Add Todo
          </button>
        </div>
      </form>
    </section>

    <!-- Todo List -->
    <div v-if="todolist.length > 0" class=" w-full max-w-xl p-6 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-900 dark:border-gray-700 mx-auto">
        <h3 class="w-full text-3xl font-bold leadi text-center text-white border-b">Todo List</h3>
 
        <h5 v-for="todo in todolist" class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">{{ todo.todo }}</h5>
  
    </div>

  </div>
</template>

<script setup>
// Site meta add
useHead({
  title: "Todo App",
});

const todo = useState(() => ({
  todo: "",
}));
const todolist = useTodoList()

const error = useState(() => ({
  error: "",
}));

const isPending = ref(false);

const todoHandle = async (e) => {
    isPending.value = true;

  if (todo.value.todo.length === 0) {
    error.value.error = "Field must not be empty!";
  } else {
    const todoData = {
      todo: todo.value.todo,
      status: true,
    };
    const { data } = await useFetch("http://localhost:5000/todo", {
      method: "POST",
      body: JSON.stringify(todoData),
      headers: {
        "Content-Type": "application/json",
      },
    });

    isPending.value = false;
    (error.value.error = ""), (todo.value.todo = "");
    e.target.reset();
    const {data:tododatalist} = await useFetch('http://localhost:5000/todo', {method:'GET'})
    todolist.value = tododatalist
  }
};



const {data:tododatalist} = await useFetch('http://localhost:5000/todo', {method:'GET'})
todolist.value = tododatalist
 console.log(todolist.value)

    
</script>

<style lang="scss" scoped></style>
