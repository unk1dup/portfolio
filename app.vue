<template>
  <div class="min-h-screen bg-black text-white">
    <!-- Particles and audio remain unchanged -->
    <ClientOnly>
      <Particles
        id="tsparticles"
        :options="{
          background: {
            color: {
              value: 'transparent'
            }
          },
          fpsLimit: 60,
          particles: {
            color: {
              value: '#ffffff'
            },
            links: {
              color: '#ffffff',
              distance: 150,
              enable: true,
              opacity: 0.2,
              width: 1
            },
            move: {
              enable: true,
              speed: 2
            },
            number: {
              density: {
                enable: true,
                area: 800
              },
              value: 80
            },
            opacity: {
              value: 0.3
            },
            size: {
              value: { min: 1, max: 3 }
            }
          }
        }"
      />
    </ClientOnly>

    <!-- Background Music -->
    <audio ref="audioPlayer" loop autoplay>
      <source src="your-background-music.mp3" type="audio/mpeg">
    </audio>

    <!-- Main content -->
    <div class="container mx-auto px-4 py-16 relative z-10">
      <!-- Info Bar -->
      <div class="max-w-2xl mx-auto mb-8 flex justify-between items-center bg-gray-950/50 backdrop-blur-lg rounded-lg p-4 border border-gray-800">
        <div class="flex items-center space-x-4">
          <div class="text-sm">
            <div class="font-medium text-blue-400">Moscow Time</div>
            <div class="text-gray-300">{{ moscowTime }}</div>
          </div>
        </div>
        <div class="flex items-center space-x-4">
          <div class="text-sm text-right">
            <div class="font-medium text-blue-400">Temperature</div>
            <div class="text-gray-300">{{ weather.temp }}°C</div>
          </div>
        </div>
      </div>

      <!-- Now Playing -->
      <div v-if="currentTrack" class="max-w-2xl mx-auto mb-8 bg-gray-950/50 backdrop-blur-lg rounded-lg p-4 border border-gray-800">
        <div class="text-sm">
          <div class="font-medium text-blue-400">Now Playing</div>
          <div class="text-gray-300">{{ currentTrack }}</div>
        </div>
      </div>

      <!-- Profile section -->
      <div class="max-w-2xl mx-auto text-center backdrop-blur-sm">
        <div class="mb-8">
          <img
            src="https://via.placeholder.com/150"
            alt="Profile Picture"
            class="w-32 h-32 rounded-full mx-auto mb-4 border-4 border-gray-800 hover:border-blue-500/50 transition-all duration-300"
          />
          <h1 class="text-3xl font-bold mb-2 text-blue-400">Your Name</h1>
          <p class="text-gray-400 mb-6">
            Your bio description goes here. Tell people about yourself and what you do.
          </p>
        </div>

        <!-- Social links -->
        <div class="grid gap-4 max-w-md mx-auto mb-8">
          <a
            href="#"
            class="bg-gray-950/50 hover:bg-gray-900/70 backdrop-blur-lg transition-all duration-300 rounded-lg p-4 flex items-center justify-center space-x-3 border border-gray-800 hover:border-blue-500/50"
          >
            <span class="text-lg text-gray-300 hover:text-blue-400">Instagram</span>
          </a>
          <a
            href="#"
            class="bg-gray-950/50 hover:bg-gray-900/70 backdrop-blur-lg transition-all duration-300 rounded-lg p-4 flex items-center justify-center space-x-3 border border-gray-800 hover:border-blue-500/50"
          >
            <span class="text-lg text-gray-300 hover:text-blue-400">Twitter</span>
          </a>
          <a
            href="#"
            class="bg-gray-950/50 hover:bg-gray-900/70 backdrop-blur-lg transition-all duration-300 rounded-lg p-4 flex items-center justify-center space-x-3 border border-gray-800 hover:border-blue-500/50"
          >
            <span class="text-lg text-gray-300 hover:text-blue-400">LinkedIn</span>
          </a>
          <a
            href="#"
            class="bg-gray-950/50 hover:bg-gray-900/70 backdrop-blur-lg transition-all duration-300 rounded-lg p-4 flex items-center justify-center space-x-3 border border-gray-800 hover:border-blue-500/50"
          >
            <span class="text-lg text-gray-300 hover:text-blue-400">GitHub</span>
          </a>
        </div>

        <!-- Projects Button -->
        <div class="mt-8">
          <button
            @click="showProjects = !showProjects"
            class="bg-gradient-to-r from-blue-900/70 to-blue-800/70 hover:from-blue-800/90 hover:to-blue-700/90 backdrop-blur-lg transition-all duration-300 rounded-lg px-8 py-4 text-lg font-medium border border-gray-800 hover:border-blue-500/50"
          >
            My Projects
          </button>
        </div>

        <!-- Projects Modal -->
        <div v-if="showProjects" class="fixed inset-0 bg-black/90 backdrop-blur-sm z-50 flex items-center justify-center p-4">
          <div class="bg-gray-950/90 rounded-lg p-8 max-w-2xl w-full max-h-[80vh] overflow-y-auto border border-gray-800">
            <div class="flex justify-between items-center mb-6">
              <h2 class="text-2xl font-bold text-blue-400">My Projects</h2>
              <button @click="showProjects = false" class="text-gray-400 hover:text-blue-400">
                Close
              </button>
            </div>
            <div class="grid gap-6">
              <div v-for="project in projects" :key="project.name" class="bg-gray-900/50 rounded-lg p-6 border border-gray-800">
                <h3 class="text-xl font-semibold mb-2 text-blue-400">{{ project.name }}</h3>
                <p class="text-gray-400 mb-4">{{ project.description }}</p>
                <a :href="project.link" target="_blank" class="text-blue-400 hover:text-blue-300">View Project →</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Particles from 'vue3-particles'

const audioPlayer = ref(null)
const showProjects = ref(false)
const moscowTime = ref('')
const weather = ref({ temp: '' })
const currentTrack = ref(null)

const projects = [
  {
    name: 'Project 1',
    description: 'Description of your first project goes here.',
    link: '#'
  },
  {
    name: 'Project 2',
    description: 'Description of your second project goes here.',
    link: '#'
  },
  // Add more projects as needed
]

// Update Moscow time every second
const updateMoscowTime = () => {
  const now = new Date()
  const moscowOffset = 3 // UTC+3
  const utc = now.getTime() + (now.getTimezoneOffset() * 60000)
  const moscowDate = new Date(utc + (3600000 * moscowOffset))
  moscowTime.value = moscowDate.toLocaleTimeString('ru-RU', { hour: '2-digit', minute: '2-digit' })
}

// Fetch weather data
const fetchWeather = async () => {
  try {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=Belgorod&units=metric&appid=YOUR_API_KEY`
    )
    const data = await response.json()
    weather.value = {
      temp: Math.round(data.main.temp)
    }
  } catch (error) {
    console.error('Error fetching weather:', error)
    weather.value = { temp: '--' }
  }
}

onMounted(() => {
  // Set initial volume
  if (audioPlayer.value) {
    audioPlayer.value.volume = 0.2
  }

  // Start Moscow time updates
  updateMoscowTime()
  setInterval(updateMoscowTime, 1000)

  // Fetch weather initially and every 5 minutes
  fetchWeather()
  setInterval(fetchWeather, 300000)

  // Example of setting current track (you would need to implement your own music tracking system)
  currentTrack.value = 'Your Current Song - Artist'
})
</script>

<style>
body {
  font-family: 'Poppins', sans-serif;
}

/* Custom scrollbar for projects modal */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: rgba(0, 0, 0, 0.3);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: rgba(59, 130, 246, 0.2);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(59, 130, 246, 0.3);
}
</style>