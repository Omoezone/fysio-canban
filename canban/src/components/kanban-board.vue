<template>
  <h1>W's simple Kanban</h1>
  <!-- Tilføj nye tasks, til backlog-->
  <br>
  <div>
    <input v-model="newTask" type="text" placeholder="Task Titel" /><br/>
    <input v-model="newTodoDescription" type="text" placeholder="Task Beskrivelse" /><br/><br/>
    <button @click="addTask">Tilføj Task</button>
  </div>
  <br>

  <div style="display: flex; border: solid black 2px; height: 100%; margin:1rem">
    <div style="background-color: gray; width: 100%;">
      <div style="border: solid black 2px;" >
        <h1>Backlog</h1>
      </div>
      <div style="border: solid black 2px; height: 30rem;">
        <draggable v-model="backlogArr" group="kanban" @start="drag=true" @end="drag=false" item-key="id">
          <template #item="{element}">
            <div style="height: 4rem; padding-top: 1rem; border-bottom: solid black 1px;">
              <span style="margin-left: 1.5rem; border-bottom: solid black 1px; font-weight: bold;">
                {{element.navn}}
              </span>
              <button @click="deleteTask('backlogArr', element.id)" style="float: right; background: none; border: none;">
                <img class="fas fa-times" alt="x-missing" style="height: 1rem; width: 1rem;">
              </button>
              <br>
              <span style="margin-left: 1.5rem;">
                {{element.beskrivelse}}
              </span>
            </div>
          </template>
        </draggable>
      </div>
    </div>

    <div style="background-color: red; width: 100%;">
      <div style="border: solid black 2px;">
        <h1>Todo</h1>
      </div>
      <div style="border: solid black 2px; height: 30rem;">
        <draggable v-model="todoArr" group="kanban" @start="drag=true" @end="drag=false" item-key="id">
          <template #item="{element}">
            <div style="height: 4rem; padding-top: 1rem; border-bottom: solid black 1px;">
              <span style="margin-left: 1.5rem; border-bottom: solid black 1px; font-weight: bold;">
                {{element.navn}}
              </span>
              <button @click="deleteTask('todoArr', element.id)" style="float: right; background: none; border: none;">
                <img class="fas fa-times" alt="x-symbol" style="height: 1rem; width: 1rem;">
              </button>
              <br>
              <span style="margin-left: 1.5rem;">
                {{element.beskrivelse}}
              </span>
            </div>
          </template>
        </draggable>
      </div>
    </div>

    <div style="background-color: yellow; width: 100%;">
      <div style="border: solid black 2px;">
        <h1>In progress</h1>
      </div>
      <div style="border: solid black 2px; height: 30rem;">
        <draggable v-model="inProgressArr" group="kanban" @start="drag=true" @end="drag=false" item-key="id">
          <template #item="{element}">
            <div style="height: 4rem; padding-top: 1rem; border-bottom: solid black 1px;">
              <span style="margin-left: 1.5rem; border-bottom: solid black 1px; font-weight: bold;">
                {{element.navn}}
              </span>
              <button @click="deleteTask('inProgressArr', element.id)" style="float: right; background: none; border: none;">
                <img class="fas fa-times" alt="x-symbol" style="height: 1rem; width: 1rem;">
              </button>
              <br>
              <span style="margin-left: 1.5rem;">
                {{element.beskrivelse}}
              </span>
            </div>
          </template>
        </draggable>
      </div>
    </div>

    <div style="background-color: greenyellow; width: 100%;">
      <div style="border: solid black 2px;">
        <h1>Finished</h1>
      </div>
      <div style="border: solid black 2px; height: 30rem;">
        <draggable v-model="finishedArr" group="kanban" @start="drag=true" @end="drag=false" item-key="id">
          <template #item="{element}">
            <div style="height: 4rem; padding-top: 1rem; border-bottom: solid black 1px;">
              <span style="margin-left: 1.5rem; border-bottom: solid black 1px; font-weight: bold;">
                {{element.navn}}
              </span>
              <button @click="deleteTask('finishedArr', element.id)" style="float: right; background: none; border: none;">
                <img class="fas fa-times" alt="x-symbol" style="height: 1rem; width: 1rem;">
              </button>
              <br>
              <span style="margin-left: 1.5rem;">
                {{element.beskrivelse}}
              </span>
            </div>
          </template>
        </draggable>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable';
import '@fortawesome/fontawesome-free/css/all.css';


export default {
  components: {
    draggable,
  },
  data() {
    return {
      backlogArr: [
        { id: 1, navn: "Tilføj ting i backlog", beskrivelse: "Backlog-ception?" },
        { id: 2, navn: "Tilføje kryds til task", beskrivelse: "Så de kan slettes nemt og forståeligt" },
        { id: 3, navn: "Klam baggrundsfarve", beskrivelse: "Ændrer den dog"  },
        { id: 4, navn: "Tilføj pæn css", beskrivelse: "Hvad er gode componenter?"  }
      ],
      todoArr: [
        { id: 1, navn: "Tilbehør", beskrivelse: "Flere features?" },
        { id: 2, navn: "Fix x knap", beskrivelse: "Hvad er den bar? -->" },
        { id: 3, navn: "Ikke flere firkanter", beskrivelse: "BorderRadius er en ting" },
      ],
      inProgressArr: [
        { id: 6, navn: "Brok over draggable", beskrivelse: "lav opslag om version konflikter"},
        { id: 7, navn: "Prøv at sælg dette til trello", beskrivelse: "Salesman level 100"  },
        { id: 8, navn: "Kig på persistency", beskrivelse: "Gem kanban til næste gang"},
      ],
      finishedArr: [
        { id: 9, navn: "Opsæt komponenter", beskrivelse: "Behøver egentlig kun et komponent?"  },
        { id: 10, navn: "Få det til at virke", beskrivelse: "Drag og opret korrekt"  }
      ],
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        const newTask = {
          id: this.backlogArr.length + 1,
          navn: this.newTask.trim(),
          beskrivelse: this.newTodoDescription.trim(),
        };
        this.backlogArr.push(newTask);
        this.newTask = ""; 
        this.newTodoDescription = "";
      }
    },
    deleteTask(arrayName, taskId) {
      const targetArray = this[arrayName];
      const index = targetArray.findIndex((task) => task.id === taskId);
      if (index !== -1) {
        targetArray.splice(index, 1);
      }
    }
  },
};
</script>