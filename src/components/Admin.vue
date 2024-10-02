<script setup>

</script>

<template>
    <div class="bg-gray-800 min-h-screen p-10 font-poppins text-white mt-10">
        <h1 class="text-3xl font-bold mb-8 text-center">Admin Panel</h1>
    
        <!-- Toggle section buttons -->
        <div class="flex justify-center gap-4 mb-8">
          <button class="bg-teal-500 px-4 py-2 rounded hover:bg-teal-600" @click="toggleEditing('home')">Edit Home</button>
          <button class="bg-teal-500 px-4 py-2 rounded hover:bg-teal-600" @click="toggleEditing('certifications')">Manage Certifications</button>
          <button class="bg-teal-500 px-4 py-2 rounded hover:bg-teal-600" @click="toggleEditing('resume')">Edit Resume</button>
        </div>
    
        <!-- Render based on active section -->
        <Home v-if="sectionEditing === 'home'" :data="dataStore.home" @update="updateSection('home', $event)" />
        <Certification v-if="sectionEditing === 'certifications'" :data="dataStore.certifications" @update="updateSection('certifications', $event)" />
        <Resume v-if="sectionEditing === 'resume'" :data="dataStore.resume" @update="updateSection('resume', $event)" />
    </div>
</template>
