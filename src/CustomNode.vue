<script lang="ts" setup>
// @ts-nocheck
import { Handle, Position, useNodeId, useVueFlow} from "@vue-flow/core";
import { ref } from "vue";

const nodeId = useNodeId();
const {updateNodeInternals, onConnect, onConnectStart,findNode } = useVueFlow();

const letters = ref([])
const repeatCount = ref(1)

/**
 * Generates letters, repeats if entire alphabet has been used
 */
function generateLetter() {
  const alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
  const unusedLetter = alphabet.split('').find(letter => 
    !letters.value.includes(letter.repeat(repeatCount.value))
  )
  
  if (unusedLetter) {
    letters.value.push(unusedLetter.repeat(repeatCount.value))
  } else {
    repeatCount.value++
    letters.value.push('A'.repeat(repeatCount.value))

  }
}

/**
 * Adds a new handleId to the array of handles and updates node internals
 */
function addHandle(){
  const handleId = generateLetter();

  updateNodeInternals([nodeId])
}

/**
 * Removes the first item in the handles array and updates node internals
 */
function removeHandle()  {
  if (letters.value.length > 0) {
    letters.value.shift()
  }
  updateNodeInternals([nodeId])
}

onConnectStart(({ nodeId, handleId, handleType }) => {
  const node = findNode(nodeId);
  console.log("\"From\" Node id:", nodeId, ", Source handle id:", handleId)
});

</script>

<template>
  <div class="custom-node">
    <div>
      <p style="font-size: 12px;">Node {{ nodeId }}</p>
    </div>
    <div v-for="(handleId, index) in letters" :key="handleId">
      <div style="  display: flex;flex-direction: column; justify-content: space-between;">
        <div>
        <Handle class="top-handle" :id="handleId" :position="Position.Top"/>
      </div>
        <div style="position: relative; right: 10px; top:-10px;">{{ handleId }}</div>
      </div>
    </div>
      <button class="increment nodrag" @click="addHandle()">Add handle</button>
      <button class="increment nodrag" @click="removeHandle()">Remove handle</button>
    </div>
</template>

<style>
.top-handle{
  position: relative;
  width: 12px;
  height: 12px;
  right:10px;
}
.custom-node {
  min-width: 100px;
  gap: 4px;
  padding: 8px;
  background: transparent;
  border: 1px solid black;
  border-radius: 4px;
}

.increment {
  border-radius: 4px;
  background: #42b983;
  font-size: 10px;
  color: #fff;
  cursor: pointer;
  border: none;
  margin-right: 3px;
}

.increment:hover {
  box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
}
</style>
