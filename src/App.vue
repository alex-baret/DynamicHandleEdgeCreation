<script lang="ts" setup>
import { h, ref, markRaw } from "vue";
import { Background } from "@vue-flow/background";
import { VueFlow, useVueFlow, type Node, type Edge } from "@vue-flow/core";
import CustomNode from "./CustomNode.vue";

const { onConnect, addEdges } = useVueFlow();

const nodes = ref<Node[]>([
  { id: "1", type: "input", label: "Node 1", position: { x: 250, y: 5 } },
  { id: "2", type: "output", label: "Node 2", position: { x: 100, y: 100 } },
  { id: "3", type: "custom", label: "Node 3", position: { x: 400, y: 100 } },
]);

const edges = ref<Edge[]>([]);

const nodeTypes = {
  custom: markRaw(CustomNode),
};

onConnect((params) => {
  addEdges([params]);
});
</script>

<template>
  <div style="height: 100vh">
    <VueFlow
      v-model:nodes="nodes"
      v-model:edges="edges"
      :nodeTypes="nodeTypes"
      fit-view-on-init
      class="vue-flow-basic-example"
      :default-zoom="1.5"
      :min-zoom="0.2"
      :max-zoom="4"
    >
      <Background pattern-color="#aaa" :gap="8" />
    </VueFlow>
  </div>
</template>
