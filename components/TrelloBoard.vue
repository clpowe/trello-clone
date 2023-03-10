<script setup lang="ts">
import { nanoid } from 'nanoid'
import type { Column, Task } from '@/types'
import draggable from 'vuedraggable'

const columns = ref<Column[]>([
    {
        id: nanoid(),
        title: 'Backlog',
        tasks: [
            {
                id: nanoid(),
                title: 'Create marketing landing page',
                createdAt: new Date(),
            },
            {
                id: nanoid(),
                title: 'Develop cool new feature',
                createdAt: new Date(),
            },
            {
                id: nanoid(),
                title: 'Fix a bug',
                createdAt: new Date(),
            },
        ],
    },
    {
        id: nanoid(),
        title: 'Selected for Dev',
        tasks: [],
    },
    {
        id: nanoid(),
        title: 'In Progress',
        tasks: [],
    },
    {
        id: nanoid(),
        title: 'QA',
        tasks: [],
    },
    {
        id: nanoid(),
        title: 'Completed',
        tasks: [],
    },
])
</script>

<template>
    <div>
        <draggable
            v-model="columns"
            group="columns"
            :animation="150"
            handle=".drag-handle"
            item-key="id"
            class="flex gap-4 overflow-x-auto items-start"
        >
            <template #item="{ element: column }: { element: Column }">
                <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
                    <header class="font-bold mb-4">
                        <DragHandle />
                        {{ column.title }}
                    </header>
                    <draggable
                        v-model="column.tasks"
                        group="tasks"
                        handle=".drag-handle"
                        :animation="150"
                        item-key="id"
                    >
                        <template #item="{ element: task }: { element: Task }">
                            <div>
                                <TrelloBoardTask
                                    :task="task"
                                    @delete="
                                        column.tasks = column.tasks.filter(
                                            (t) => t.id != $event
                                        )
                                    "
                                />
                            </div>
                        </template>
                    </draggable>
                    <footer>
                        <NewTask @add="column.tasks.push($event)" />
                    </footer>
                </div>
            </template>
        </draggable>
    </div>
</template>
