<template>
    <div class="container" style="max-width: 800px">
        <h2 class="text-center mt-5">Облік Відвідувачів</h2>
        <!-- <div class="d-flex mt-5">
            <input type="text" v-model="Name" placeholder="Enter Name" class="w-50 form control" @keydown.enter="addTask" />
            <input type="text" v-model="Surname" placeholder="Enter Surname" class="w-50 form control" />
            <button class="btn btn-warning rounded-0" @click="addTask">Submit</button>
        </div> -->
        <!--Modal-->
        

        <Teleport to="body">
            <!-- use the modal component, pass in the prop -->
            <modal :show="showModal" @close="showModal = false">
                <template #header>
                    <h3>Додати Відвідувача</h3>
                </template>
                <template #body>
                    <div class="d-flex mt-4 flex-column gap-4 justify-content-center" style="height: 130px;">
                        <input type="text" v-model="Name" placeholder="Enter Name" class="w-100 form control"
                            @keydown.enter="addTask" />
                        <input type="text" v-model="Surname" placeholder="Enter Surname" class="w-100 form control" />
                    </div>
                </template>
                <template #footer>
                    <button class="btn btn-warning bg-success" @click="addTask">✔️</button>
                    <button class="btn btn-warning bg-danger" @click="showModal = false"> ❌</button>
                </template>
            </modal>
        </Teleport>
        <!--Table-->
        <table class="table table-striped">
            <thead>
                <tr>
                    <th scope="col"><button id="show-modal" @click="showModal = true" class="addTaskClass">+</button></th>
                    <th scope="col">Name</th>
                    <th scope="col">Surname</th>
                    <th scope="col">Time</th>
                    <th scope="col" class="text-center">Edit</th>
                    <th scope="col" class="text-center">Delete</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="task in tasks" :key="task.id">
                    <td></td>
                    <td>{{ task.name }}</td>
                    <td>{{ task.surname }}</td>
                    <td>{{ task.time }}</td>
                    <td class="text-center" @click="console.log(task)">✏️</td>
                    <td class="text-center" @click="deleteTask(task.id)">🗑️</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import Modal from './Modal.vue'
export default {
    name: 'TodoApp',
    components: {
        Modal
    },
    data() {
        return {
            editedTask: null,
            Name: "",
            Surname: "",
            tasks: [],
            showModal: false
        }
    },
    methods: {
        addTask() {
            let taskId = Math.floor(Math.random() * (8884421 - 285414) + 13)
            let taskTime = new Date().toISOString().replace('-', '/').split('T')[0].replace('-', '/');
            let newTask = {
                id: taskId,
                name: this.Name,
                surname: this.Surname,
                time: taskTime,
            }

            this.tasks.push(newTask)
            this.Name = ""
            this.Surname = ""
        },
        editTask(task) {
            console.log(task)
        },
        deleteTask(taskId) {
            this.tasks = this.tasks.filter(t => t.id !== taskId)
        },
    },
}

</script>

<style>
.modal {
    position: fixed;
    z-index: 999;
    top: 20%;
    left: 50%;
    width: 300px;
    margin-left: -150px;
}

.addTaskClass {
    background: none;
    border: none;
    font-weight: bold;
}
</style>