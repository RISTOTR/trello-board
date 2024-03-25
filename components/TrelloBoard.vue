<script setup lang="ts">
import { nanoid } from 'nanoid';
import type { Column, Task } from '~/types';
import TrelloBoardTask from './TrelloBoardTask.vue';
import DragHandle from './DragHandle.vue';
import draggable from 'vuedraggable'
import NewTask from './NewTask.vue';

const columns = useLocalStorage<Column[]>("Trelloboard", [
  {
    id: nanoid(),
    title: "Backlog",
    tasks: [
      {
        id: nanoid(),
        title: "Create marketing landing page",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Develop cool new feature",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Fix page nav bug",
        createdAt: new Date(),
      }
    ],
  },
  {
    id: nanoid(),
    title: "Selected for Dev",
    tasks: [
      {
        id: nanoid(),
        title: "Create VN LP",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Desing new landing page",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Create Backend for new lP",
        createdAt: new Date(),
      }
    ],
  },
  {
    id: nanoid(),
    title: "In Progress",
    tasks: [
      {
        id: nanoid(),
        title: "Create FOREVER landing page",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Buttons landing page",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Fix tracking bug",
        createdAt: new Date(),
      }
    ],
  },
  {
    id: nanoid(),
    title: "In QA",
    tasks: [
      {
        id: nanoid(),
        title: "Create",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Develop ",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Fix",
        createdAt: new Date(),
      }
    ],
  },
  {
    id: nanoid(),
    title: "Done",
    tasks: [
      {
        id: nanoid(),
        title: "New comparison site tech earphones",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Interstitial earphones",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "tracking interstitial",
        createdAt: new Date(),
      }
    ],
  }
],
{
    serializer: {
      read: (value) => {
        return JSON.parse(value).map((column: Column) => {
          column.tasks = column.tasks.map((task: Task) => {
            task.createdAt = new Date(task.createdAt);
            return task;
          });
          return column;
        });
      },
      write: (value) => JSON.stringify(value),
    },
  }
);
const alt = useKeyModifier("Alt");

function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: "",
    tasks: [],
  }
  columns.value.push(column);
    nextTick(() => {
      (document.querySelector(".column:last-of-type .title-input") as HTMLInputElement).focus()
    })
  }
</script>
<template>
  <div class="flex items-start verflow-x-auto gap-4">
    <draggable
    v-model="columns"
    group="columns"
    :animation="150"
    handle=".drag-handle"
    item-key="id"
    class="flex gap-4 items-start"
    >
    <template #item="{element: column}: {element: Column}">
      <div 
      class="column bg-gray-950 p-5 rounded min-w-[250px]"
      >
        <header class="text-white font-bold mb-4">
          <DragHandle />
          <input
          class="title-input bg-transparent focus:bg-white rounded px-1 w-4/5"
          @keyup.enter="($event.target as HTMLInputElement).blur()"
          @keydown.backspace="
          column.title === '' 
          ? (columns = columns.filter((c) => c.id !== column.id))
          : null
          "
          type="text"
          v-model="column.title"
          />
        </header>
        <draggable
          v-model="column.tasks"
          :group="{ name: 'tasks', pull: alt ? 'clone' : true}"
          :animation="150"
          handle=".drag-handle"
          item-key="id"
        >
        <template #item="{element: task} : {element: Task}">
          <div>
            <TrelloBoardTask :task="task" @delete="column.tasks = column.tasks.filter(t=> t.id !== $event)"/>
          </div>
        </template>
        </draggable>
        <footer>
          <NewTask @add="column.tasks.push($event)" />
        </footer>
      </div>
    </template>
    </draggable>
   <button
   @click="createColumn"
   class="create-column-btn text-white whitespace-nowrap p-2 rounded"
   >
   + Add another Column
  </button>
  </div>
</template>


<style>
.create-column-btn {
  background: #00DC82;
}
.column {
  border: 1px solid rgb(51 65 85)
}
</style>