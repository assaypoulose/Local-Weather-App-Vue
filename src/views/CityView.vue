<template>
    <div>
        <Suspense>
            <AsyncView />
            <template #fallback>
                <p>Loading...</p>
            </template>
        </Suspense>
    </div>
</template>

<script setup>
import AsyncView from '../components/AsyncView.vue';


</script>
