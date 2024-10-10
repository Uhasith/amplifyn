<script setup lang="ts">
import { ref, computed } from 'vue';
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select';
import { Loader2, Calculator } from 'lucide-vue-next';
import { Button } from '@/components/ui/button';
import { toast } from 'vue-sonner';

const isLoading = ref<boolean>(false);
const nodes = ref<any>([
  { value: 'A', name: 'A' },
  { value: 'B', name: 'B' },
  { value: 'C', name: 'C' },
  { value: 'D', name: 'D' },
  { value: 'E', name: 'E' },
  { value: 'F', name: 'F' },
  { value: 'G', name: 'G' },
  { value: 'H', name: 'H' },
  { value: 'I', name: 'I' },
  { value: 'J', name: 'J' },
]);

const selectedFromNode = ref<any>(null);
const selectedToNode = ref<any>(null);
const result = ref<any>(null);

const calculateDistance = () => {
  if (!selectedFromNode.value || !selectedToNode.value) {
    toast('Validation Error', {
      description: 'Please select both from and to nodes',
    });
    return;
  }
  result.value = null;
  isLoading.value = true;

  // Add a delay to simulate an API call
  setTimeout(() => {
    isLoading.value = false;
    result.value = 10;
  }, 3000);
}

// Filter nodes for "From Node" based on selectedToNode
const filteredFromNodes = computed(() => {
  return nodes.value.filter((node: any) => node.value !== selectedToNode.value);
});

// Filter nodes for "To Node" based on selectedFromNode
const filteredToNodes = computed(() => {
  return nodes.value.filter((node: any) => node.value !== selectedFromNode.value);
});
</script>

<template>
  <div class="flex flex-1 items-center justify-center flex-col">
    <div class="bg-[#1154A3] w-full h-[50vh] flex flex-col items-center justify-center">
      <h1 class="font-poppins text-4xl text-white font-semibold mb-4">Dijiktra’s Algorithm Calculator</h1>
      <p class="text-lg text-white font-normal">Discovering Optimal Routes Through Nodes Using Dijkstra's Method</p>
    </div>
    <div class="bg-[#E7F3FF] w-full h-[50vh] flex justify-center">
      <div class="bg-white shadow-[0px_8px_22px_2px_#0000001F] rounded-lg flex -mt-32 h-full w-[40%]">
        <div class="w-1/2 flex flex-col justify-evenly px-10">
          <h2 class="text-[#1154A3] text-xl font-semibold">Select Path</h2>
          <div>
            <div class="w-[270px]">
              <label for="from_node" class="block mb-2 text-sm font-medium text-[#3B3C3F]">From Node</label>
              <Select v-model="selectedFromNode">
                <SelectTrigger>
                  <SelectValue placeholder="Select a node" />
                </SelectTrigger>
                <SelectContent>
                  <SelectGroup>
                    <SelectItem v-for="node in filteredFromNodes" :value="node.value" :label="node.name"
                      :key="'from' + node.value">
                      {{ node.name }}
                    </SelectItem>
                  </SelectGroup>
                </SelectContent>
              </Select>
            </div>
          </div>

          <div>
            <div class="w-[270px]">
              <label for="to_node" class="block mb-2 text-sm font-medium text-[#3B3C3F]">To Node</label>
              <Select v-model="selectedToNode">
                <SelectTrigger>
                  <SelectValue placeholder="Select a node" />
                </SelectTrigger>
                <SelectContent>
                  <SelectGroup>
                    <SelectItem v-for="node in filteredToNodes" :value="node.value" :label="node.name"
                      :key="'to' + node.value">
                      {{ node.name }}
                    </SelectItem>
                  </SelectGroup>
                </SelectContent>
              </Select>
            </div>
          </div>

          <div class="flex gap-3">
            <Button variant="outline" :disabled="isLoading"
              @click="result = null; selectedFromNode = null; selectedToNode = null"
              class="px-4 py-3 border border-[#DA753C] rounded-lg text-[#DA753C] hover:text-[#DA753C]/90">
              Clear
            </Button>
            <Button :disabled="isLoading" @click="calculateDistance"
              class="flex gap-2 items-center justify-center text-white bg-[#DA753C] hover:bg-[#DA753C]/90 px-4 py-3 rounded-lg">
              Calculate
              <Loader2 v-if="isLoading" class="w-4 h-4 mr-2 animate-spin" />
              <Calculator v-else class="w-4 h-4 mr-2" />
            </Button>
          </div>
        </div>
        <div v-if="result == null" class="w-1/2 flex justify-center items-center">
          <img src="/src/assets/image.png" alt="logo" class="w-[60%]">
        </div>
        <div v-else class="w-1/2 flex flex-col gap-5 bg-[#F2F3F6] p-10 rounded-lg">
          <h2 class="text-[#1154A3] text-xl font-semibold">Result</h2>
          <div class="bg-white rounded w-full h-full p-5">
            <p class="text-[#5A5B5D] text-md">From Node : {{ selectedFromNode }}</p>
            <p class="text-[#5A5B5D] text-md">To Node : {{ selectedToNode }}</p>
            <p class="text-[#5A5B5D] text-md">Total Distance : {{ result }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
