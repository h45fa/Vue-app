<template>
    <div class="container" style="max-width: 800px">
        <h2 class="text-center mt-5">Облік Відвідувачів</h2>
        <Teleport to="body">
            <modal :show="showModal" @close="showModal = false">
                <template #header>
                    <h3>Додати Відвідувача</h3>
                </template>
                <template #body>
                    <div class="d-flex mt-4 flex-column gap-4 justify-content-center" style="height: 130px;">
                        <input type="text" v-model="Name" placeholder="Enter Name" class="w-100 form control" />
                        <input type="text" v-model="Surname" placeholder="Enter Surname" class="w-100 form control" />
                    </div>
                </template>
                <template #footer>
                    <button class="btn btn-warning bg-success" @click="addTask(task)">✔️</button>
                    <button class="btn btn-warning bg-danger" @click="showModal = false"> ❌</button>
                </template>
            </modal>
        </Teleport>
        <table class="table table-striped" id="myTable">
            <thead>
                <tr>
                    <th scope="col"><button id="show-modal" @click="showModal = true" class="addTaskClass">+</button></th>
                    <th scope="col" @click="sortTable(1)">Name</th>
                    <th scope="col" @click="sortTable(2)">Surname</th>
                    <th scope="col" @click="sortTable(3)">Time</th>
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
                    <td class="text-center" @click="editTask(task)">✏️</td>
                    <td class="text-center" @click="deleteTask(task.id)">🗑️</td>
                </tr>
                <Teleport to="body">
                    <!-- use the modal component, pass in the prop -->
                    <EditM :showEdit="showEditModal" @close="showEditModal = false" v-for="task in tasks" :key="task.id">
                        <template #header>
                            <h3>Додати Відвідувача</h3>
                        </template>
                        <template #body>
                            <div class="d-flex mt-4 flex-column gap-4 justify-content-center" style="height: 130px;">
                                <input type="text" v-model="task.name" placeholder="Enter Name"
                                    class="w-100 form control" />
                                <input type="text" v-model="task.surname" placeholder="Enter Surname"
                                    class="w-100 form control" />
                                <!-- <h2>{{ task.name }}</h2>
                                <h2>{{ task.surname }}</h2> -->
                            </div>
                        </template>
                        <template #footer>
                            <button class="btn btn-warning bg-success" @click="editTaskSubmit(task)">✔️</button>
                            <button class="btn btn-warning bg-danger" @click="showEditModal = false"> ❌</button>
                        </template>
                    </EditM>
                </Teleport>
            </tbody>
        </table>
    </div>
</template>

<script>
import Modal from './Modal.vue';
import EditM from './Editmodal.vue';
import axios from 'axios';
export default {
    name: 'TodoApp',
    components: {
        Modal,
        EditM
    },
    data() {
        return {
            editedTask: null,
            Name: "",
            Surname: "",
            tasks: [],
            showModal: false,
            showEditModal: false
        }
    },
    async mounted() {
        await this.getData();
    },
    methods: {
        getData() {
            axios
                .get("https://63d81aa75dbd72324433552c.mockapi.io/test")
                .then((response) => (this.tasks = response.data))
                .then((response) =>
                (this.tasks = this.tasks.map((task) => {
                    let newTask = {
                        id: task.id,
                        name: task.name,
                        surname: task.surname,
                        time: task.time,
                    }
                    return newTask
                })))
        },
        addTask() {
            let taskId = Math.floor(Math.random() * (8884421 - 285414) + 13)
            let taskTime = new Date().toISOString().replace('-', '/').split('T')[0].replace('-', '/');
            let newTask = {
                id: taskId,
                name: this.Name,
                surname: this.Surname,
                time: taskTime,
            }
            console.log(newTask)
            axios.post('https://63d81aa75dbd72324433552c.mockapi.io/test', newTask)

            this.tasks.push(newTask)
            this.Name = ""
            this.Surname = ""
        },
        editTask(task) {
            this.showEditModal = true
            this.task = task
            console.log(task)
        },
        editTaskSubmit(task) {
            let newTask = {
                id: task.id,
                name: this.name,
                surname: this.surname,
                time: task.time,
            }
            this.showEditModal = false
            // this.tasks.push(newTask)
        },
        deleteTask(taskId) {
            this.tasks = this.tasks.filter(t => t.id !== taskId)
        },
        sortTable(n) {
            var table, rows, switching, i, x, y, shouldSwitch, dir, switchcount = 0;
            table = document.getElementById("myTable");
            switching = true;
            dir = "asc";
            while (switching) {
                switching = false;
                rows = table.rows;
                for (i = 1; i < (rows.length - 1); i++) {
                    shouldSwitch = false;
                    x = rows[i].getElementsByTagName("TD")[n];
                    y = rows[i + 1].getElementsByTagName("TD")[n];
                    if (dir == "asc") {
                        if (x.innerHTML.toLowerCase() > y.innerHTML.toLowerCase()) {
                            shouldSwitch = true;
                            break;
                        }
                    } else if (dir == "desc") {
                        if (x.innerHTML.toLowerCase() < y.innerHTML.toLowerCase()) {
                            shouldSwitch = true;
                            break;
                        }
                    }
                }
                if (shouldSwitch) {
                    rows[i].parentNode.insertBefore(rows[i + 1], rows[i]);
                    switching = true;
                    switchcount++;
                } else {
                    if (switchcount == 0 && dir == "asc") {
                        dir = "desc";
                        switching = true;
                    }
                }
            }
        }
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