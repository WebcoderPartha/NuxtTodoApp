<template>
  <div class="min-h-[600px] bg-gray-700 pb-5">
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
    <div v-if="todolist.length > 0" class=" w-full max-w-[800px] p-6 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-900 dark:border-gray-700 mx-auto">
        <h3 class="w-full text-3xl font-bold leadi text-center text-white border-b mb-5">Todo List</h3>
 
      <div  v-for="todo in todolist" class=" bg-sky-900 py-2 px-5 text-center mb-2 rounded-2xl">
        <div class="grid grid-cols-4 gap-4">
          <div class="col-span-3">
            <h5 v-if="todo.status" class="mb-2 text-2xl tracking-tight text-white">{{ todo.todo }}</h5>
            <h5 v-else class="mb-2 text-2xl tracking-tight text-red-700 line-through">{{ todo.todo }}</h5>
          </div>
          <div>
            <button   @click="taskDone"  class="text-fuchsia-950">
            
                <svg :data-todo="todo.todo" :data-id="todo.id" v-if="todo.status" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                <path  stroke-linecap="round" stroke-linejoin="round" d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
        
            </button>
            &nbsp;
            <button class="text-red-900">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M9.75 9.75l4.5 4.5m0-4.5l-4.5 4.5M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
            </button>
          </div>
        </div>
      </div>
  
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

const {data:tododatalist, refresh} = await useFetch('http://localhost:5000/todo', {method:'GET'})
todolist.value = tododatalist

const taskDone = async (e) => {
  const buttonData = {
    todo: e.target.getAttribute('data-todo'),
    status: false,
    id : e.target.getAttribute('data-id'),
  }
  console.log(buttonData)
  await useFetch('http://localhost:5000/todo/'+buttonData.id, {
    method:'PUT',
    body: JSON.stringify(buttonData)
  })
  refresh()

}

    
</script>

<style lang="scss" scoped></style>
