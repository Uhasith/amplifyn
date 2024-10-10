<template>
    <div class="min-h-screen flex flex-col bg-[#FFFFFF]">
        <Toaster />
        <RouterView />
    </div>
</template>

<script setup lang="ts">
import { RouterView } from 'vue-router';
import { Toaster } from '@/components/ui/sonner';
</script>
