
<script setup>
import { ref, onBeforeMount } from 'vue'
const
  checked = ref({}),

  nodes = [
    {
      "id": "root",
      "label": "Tree Root",
      "children": [
        {
          id: 0,
          label: "Project Status",
          children: [
            {
              "id": "tree-1",
              "label": "Project 1",
              "children": [
                {
                  "id": "tree-1-1",
                  "label": "Project 1 - Phase 1",
                  "issues": [
                    {
                      "id": "q-1-1",
                      "checked": [],
                      "directions": "Select all that applies",
                      "ans": [
                        {
                          "value": "a-1",
                          "label": "status: project phase on-time, on-budget"
                        },
                        {
                          "value": "a-2",
                          "label": "status: some milestones late; project phase ok"
                        },
                        {
                          "value": "a-3",
                          "label": "status: some milestones late; project phase late"
                        },
                        {
                          "value": "a-4",
                          "label": "status: project phase running over budget"
                        }
                      ]
                    },
                  ]
                },
                {
                  "id": "tree-1-2",
                  "label": "Project 1 - Phase 2",
                  "issues": [
                    {
                      "id": "q-1-2",
                      "checked": [],
                      "directions": "Select all that applies",
                      "ans": [
                        {
                          "value": "a-1",
                          "label": "status: project phase on-time, on-budget"
                        },
                        {
                          "value": "a-2",
                          "label": "status: some milestones late; project phase ok"
                        },
                        {
                          "value": "a-3",
                          "label": "status: some milestones late; project phase late"
                        },
                        {
                          "value": "a-4",
                          "label": "status: project phase running over budget"
                        }
                      ]
                    }
                  ]
                }
              ]
            },
            {
              "id": "tree-200",
              "label": "Project 2",
              "children": [
                {
                  "id": "tree-2-1",
                  "label": "Project 2 - Phase 1",
                  "issues": [
                    {
                      "id": "q-2-1",
                      "checked": [],
                      "directions": "Select all that applies",
                      "ans": [
                        {
                          "value": "a-1",
                          "label": "status: project phase on-time, on-budget"
                        },
                        {
                          "value": "a-2",
                          "label": "status: some milestones late; project phase ok"
                        },
                        {
                          "value": "a-3",
                          "label": "status: some milestones late; project phase late"
                        },
                        {
                          "value": "a-4",
                          "label": "status: project phase running over budget"
                        }
                      ]
                    }
                  ]
                },
                {
                  "id": "tree-2-2",
                  "label": "Project 2 - Phase 2",
                  "issues": [
                    {
                      "id": "q-2-2",
                      "checked": [],
                      "directions": "Select all that applies",
                      "ans": [
                        {
                          "value": "a-1",
                          "label": "status: project phase on-time, on-budget"
                        },
                        {
                          "value": "a-2",
                          "label": "status: some milestones late; project phase ok"
                        },
                        {
                          "value": "a-3",
                          "label": "status: some milestones late; project phase late"
                        },
                        {
                          "value": "a-4",
                          "label": "status: project phase running over budget"
                        }
                      ]
                    }
                  ]
                }
              ]
            }
          ]

        }
      ]
    }
  ]

onBeforeMount(() => {
  //nodes = await fetchData(); //nodes.value = ...?

  let deepIds = function (children) {
    return children.some(function (e) {
      if (e.issues) {
        for (let q of e.issues) checked[q.id] = [] //checked[q.id]=ref([])
      }
      else if (e.children) return deepIds(e.children)
    })
  }
  //const checked={}
  deepIds(nodes[0].children)
  console.log('checked', checked)

})
</script>

<template>
  <q-page padding>

    <div id="main">
      Questions:
      <ol type="A">
        <li>Why [un]checking one box of the group [de]selects all?</li>
        <li>Why this message "quasar.esm.js:23614 q-option-group: model should be array in your case"?</li>
        <li>What is the right way of doing this?</li>
        <li>How can I have multiple-choice-style letters (e.g. A,B,C,D) before the Answer checkbox?</li>
      </ol>
      <div class="q-pa-md q-gutter-sm">
        <q-tree v-model:expanded="expanded" v-model:selected="selected" v-model:ticked="ticked" :nodes="nodes"
          node-key="label" tick-strategy="leaf-filtered" default-expand-all>
          <template v-slot:default-header="prop">
            <div class="row items-center">
              <div class="text-weight-bold text-primary">{{ prop.node.label }}</div>
            </div>
          </template>

          <template v-slot:default-body="prop">
            <q-card v-for="q in prop.node.issues" :key="q.id" class="my-card q-mb-md" bordered flat>
              <q-card-section>
                {{ q.id }})
                {{ q.directions }}
              </q-card-section>

              <q-separator inset />

              <q-card-section>
                Answers:
                <div class="q-pa-md">
                  <q-option-group v-model='checked[q.id]' :options="q.ans" type="checkbox" />
                </div>

              </q-card-section>
            </q-card>

          </template>
        </q-tree>
      </div>

    </div>

  </q-page>
</template>

