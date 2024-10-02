<template>
    <div class="p-6 bg-gray-800 min-h-screen text-white">
      <h1 class="text-2xl font-bold tracking-tight text-gray-900 sm:text-4xl dark:text-white mt-10">Admin Panel</h1>
  
      <!-- Tabs to navigate between different sections -->
      <div class="flex space-x-4 mb-6">
        <button @click="currentTab = 'home'" :class="{ 'bg-teal-600': currentTab === 'home' }" class="bg-gray-700 hover:bg-teal-700 text-white font-bold py-2 px-4 rounded">
          Home Settings
        </button>
        <button @click="currentTab = 'certificates'" :class="{ 'bg-teal-600': currentTab === 'certificates' }" class="bg-gray-700 hover:bg-teal-700 text-white font-bold py-2 px-4 rounded">
          Certificate Settings
        </button>
        <button @click="currentTab = 'resume'" :class="{ 'bg-teal-600': currentTab === 'resume' }" class="bg-gray-700 hover:bg-teal-700 text-white font-bold py-2 px-4 rounded">
          Resume Settings
        </button>
      </div>
  
      <!-- Home Settings -->
      <div v-if="currentTab === 'home'">
        <h2 class="text-xl font-bold mb-4">Edit Home Page Settings</h2>
        <form @submit.prevent="saveHomeChanges">
          <div class="mb-4">
            <label for="image" class="block text-sm font-medium text-white">Profile Image URL</label>
            <input v-model="home.image" type="text" id="image" class="mt-1 block w-full p-2 bg-gray-700 text-white rounded" />
          </div>
          <div class="mb-4">
            <label for="name" class="block text-sm font-medium text-white">Name</label>
            <input v-model="home.name" type="text" id="name" class="mt-1 block w-full p-2 bg-gray-700 text-white rounded" />
          </div>
          <div class="mb-4">
            <label for="description" class="block text-sm font-medium text-white">Description</label>
            <textarea v-model="home.description" id="description" rows="4" class="mt-1 block w-full p-2 bg-gray-700 text-white rounded"></textarea>
          </div>
          <div class="mb-4">
            <label for="github" class="block text-sm font-medium text-white">GitHub Link</label>
            <input v-model="home.github" type="text" id="github" class="mt-1 block w-full p-2 bg-gray-700 text-white rounded" />
          </div>
          <div class="mb-4">
            <label for="linkedin" class="block text-sm font-medium text-white">LinkedIn Link</label>
            <input v-model="home.linkedin" type="text" id="linkedin" class="mt-1 block w-full p-2 bg-gray-700 text-white rounded" />
          </div>
          <button type="submit" class="bg-teal-600 hover:bg-teal-700 text-white font-bold py-2 px-4 rounded">
            Save Home Changes
          </button>
        </form>
      </div>
  
      <!-- Certificate Settings -->
      <div v-if="currentTab === 'certificates'">
        <h2 class="text-xl font-bold mb-4">Edit Certificate Page Settings</h2>
        
        <ul class="space-y-6">
          <li v-for="(certificate, index) in certificates" :key="index" class="bg-gray-700 p-4 rounded-lg">
            <h3 class="text-lg font-semibold mb-2">Certificate {{ index + 1 }}</h3>
  
            <label class="block text-sm font-medium text-white">Title</label>
            <input v-model="certificate.title" type="text" class="mt-1 block w-full p-2 bg-gray-600 text-white rounded mb-4" />
  
            <label class="block text-sm font-medium text-white">Image URL</label>
            <input v-model="certificate.image" type="text" class="mt-1 block w-full p-2 bg-gray-600 text-white rounded mb-4" />
  
            <label class="block text-sm font-medium text-white">Link</label>
            <input v-model="certificate.link" type="text" class="mt-1 block w-full p-2 bg-gray-600 text-white rounded mb-4" />
  
            <!-- Remove Button -->
            <button @click="removeCertificate(index)" class="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded">
              Remove Certificate
            </button>
          </li>
        </ul>
  
        <button @click="addCertificate" class="mt-6 bg-teal-600 hover:bg-teal-700 text-white font-bold py-2 px-4 rounded">
          Add Certificate
        </button>
  
        <button @click="saveCertificateChanges" class="mt-6 bg-teal-600 hover:bg-teal-700 text-white font-bold py-2 px-4 rounded">
          Save Certificate Changes
        </button>
      </div>
  
      <!-- Resume Settings -->
      <div v-if="currentTab === 'resume'">
        <h2 class="text-xl font-bold mb-4">Edit Resume Page Settings</h2>
  
        <div v-for="(image, index) in resumeImages" :key="index" class="mb-4">
          <label class="block text-sm font-medium text-white">Resume Image {{ index + 1 }} URL</label>
          <input v-model="resumeImages[index]" type="text" class="mt-1 block w-full p-2 bg-gray-700 text-white rounded" />
  
          <!-- Remove Button for Resume Images -->
          <button @click="removeResumeImage(index)" class="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded mt-4">
            Remove Resume Image
          </button>
        </div>
  
        <button @click="addResumeImage" class="bg-teal-600 hover:bg-teal-700 text-white font-bold py-2 px-4 rounded">
          Add Resume Image
        </button>
  
        <button @click="saveResumeChanges" class="bg-teal-600 hover:bg-teal-700 text-white font-bold py-2 px-4 rounded mt-4">
          Save Resume Changes
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  // Current active tab
  const currentTab = ref('home');
  
  // Home data
  const home = ref({
    image: '/ron.png',
    name: 'Ron Anthony Sy',
    description: `As a fourth year computer science student at New Era University, my objective is to acquire more programming experience and improve my coding abilities. As I advance academically, I plan to investigate collaborative projects, seminars, and apprenticeships that will provide me with hands-on experience and an understanding of industry standards. Due to my profound curiosity and eagerness to acquire knowledge, I am confident in my capacity to adapt to challenges and thrive in a dynamic and interactive environment.`,
    github: 'https://github.com/rnnthnysy',
    linkedin: 'https://www.linkedin.com/in/ron-anthony-sy/'
  });
  
  // Certificates data in a list format
  const certificates = ref([
    { title: 'Fortinet Certified Associate in Cybersecurity', link: 'https://drive.google.com/file/d/1PGjCxTZ9KO0YVJ5wOma6s7XjQLs3OZnz/view?usp=drive_link', image: '/Fortinet Certified Associate in Cybersecurity-1.png' },
    { title: 'FCA - FortiGate 7.4 Operator Self-Paced', link: 'https://drive.google.com/file/d/1USgVyXc0OHiSC41HgYrVcJBZaLqmpLr4/view?usp=drive_link', image: '/FCA - FortiGate 7.4 Operator Self-Paced-1.png' },
    { title: 'FCF - Technical Introduction to Cybersecurity 1.0 Self-Paced', link: 'https://drive.google.com/file/d/12h8NptOIcZoMDdTFxZyBBKHWi8uKN0-x/view?usp=drive_link', image: '/FCF - Technical Introduction to Cybersecurity 1.0 Self-Paced-1.png' },
    { title: 'Enhancing Digital Protection Cybersecurity for All', link: 'https://drive.google.com/file/d/1FfzyfjsnlgL-j5LlFCDuTSined1U7Np5/view?usp=drive_link', image: '/Enhancing Digital Protection Cybersecurity for All-1.png' },
    { title: 'Fortinet Certified Fundamentals in Cybersecurity', link: 'https://drive.google.com/file/d/1MAEc4xVa-PuQ4UWkALIv1vJPa0x2Pk36/view?usp=drive_link', image: '/Fortinet Certified Fundamentals in Cybersecurity-1.png' },
    { title: 'FCF - Getting Started in Cybersecurity 2.0 Self-Paced', link: 'https://drive.google.com/file/d/1HRj5qMuuM9IAjtUk6OG4Qs7D1rVk2VmC/view?usp=drive_link', image: '/FCF - Getting Started in Cybersecurity 2.0 Self-Paced-1.png' },
    { title: 'FCF - Introduction to the Threat Landscape 2.0 Self-Paced', link: 'https://drive.google.com/file/d/1vN9ytrfw95zJiUxZVVIKsgQCoOxtXywp/view?usp=drive_link', image: '/FCF - Introduction to the Threat Landscape 2.0 Self Paced-1.png' },
    { title: 'Cyber Security Bootcamp', link: 'https://drive.google.com/file/d/1FbdQK0RPr6DfZFztXdd4FcPKDrwxfhnM/view?usp=drive_link', image: '/Cyber Security Bootcamp-1.png' },
    { title: 'Cybersecurity Workshop - CTF', link: 'https://drive.google.com/file/d/1wAN4DlExeWl1rXumVXiGrKb6xyQJRbb7/view?usp=drive_link', image: '/certificate1.png' },
    { title: 'Python', link: 'https://drive.google.com/file/d/1zEv7uCDIEYCbCgMPTa80LL_85s1HQ43W/view?usp=drive_link', image: '/Python.png' },
    { title: 'Intro to Programming', link: 'https://drive.google.com/file/d/1LlIPp4I04kQQ5irBVcw7hw-DGwl2PnBi/view?usp=drive_link', image: '/Intro to Programming.png' },
    { title: 'Data Analytics Essentials', link: 'https://drive.google.com/file/d/19AURAS57TwHbkAoTjd-QtVXITIcWT6ZQ/view?usp=drive_link', image: '/Data Analytics Essential-1.png' },
    { title: 'Business Analytics with Excel', link: 'https://drive.google.com/file/d/1-0zlC1q5KSwFh7h0B2eHwtPakAq9CewA/view?usp=drive_link', image: '/Business Analytics with Excel.png' }
  ]);
  
  // Resume images data
  const resumeImages = ref([
    '/Ron Anthony Sy-CV-1.png',
    '/Ron Anthony Sy-CV-2.png'
  ]);
  
  // Functions to handle form submissions
  const saveHomeChanges = () => {
    console.log('Home page settings saved:', home.value);
  };
  
  const saveCertificateChanges = () => {
    console.log('Certificates saved:', certificates.value);
  };
  
  const saveResumeChanges = () => {
    console.log('Resume images saved:', resumeImages.value);
  };
  
  // Add a new certificate
  const addCertificate = () => {
    certificates.value.push({ title: '', link: '', image: '' });
  };
  
  // Remove a certificate
  const removeCertificate = (index) => {
    certificates.value.splice(index, 1);
  };
  
  // Add a new resume image
  const addResumeImage = () => {
    resumeImages.value.push('');
  };
  
  // Remove a resume image
  const removeResumeImage = (index) => {
    resumeImages.value.splice(index, 1);
  };
  </script>
  
