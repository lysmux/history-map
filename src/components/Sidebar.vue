<script setup lang="ts">
    import type { Place } from '../types/Place.d'
    
    defineProps<{
      isVisible: boolean;
      places: Place[];
    }>();
    
    const emit = defineEmits<{
      (event: 'toggle-sidebar'): void;
      (event: 'select-place', place: Place): void;
    }>();
    
    const handlePlaceClick = (place: Place) => {
      emit('select-place', place);
    };
    </script>
    
    <template>
      <div class="sidebar-container" :class="{ 'sidebar-hidden': !isVisible }">
        <div class="sidebar">
          <ul>
            <li
              v-for="place in places"
              :key="place.id"
              @click="handlePlaceClick(place)"
            >
              {{ place.name }}
            </li>
          </ul>
        </div>
    
        <button @click="emit('toggle-sidebar')" class="toggle-button">
          <span class="arrow" :class="{ 'arrow-collapsed': isVisible }"></span>
        </button>
      </div>
    </template>
    
    <style scoped>
    /* WW2-era styling with larger text and wider sidebar */
    .sidebar-container {
      position: fixed;
      top: 60px;
      left: 0;
      z-index: 1000;
      transition: transform 0.3s ease;
    }
    
    .sidebar-hidden {
      transform: translateX(-350px); /* Adjusted for wider sidebar */
    }
    
    .sidebar {
      width: 450px; /* Increased sidebar width */
      background-color: rgb(226, 232, 240); /* White background */
      color: black; /* Black text */
      height: calc(100vh - 60px);
      overflow-y: auto;
      font-family: 'Arial', serif; /* Vintage serif font */
      font-size: 26px; /* Increased font size */
      font-weight: bold; /* Bold text */
      border-right: 2px solid #ccc; /* Add a subtle border */
      box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1); /* Add a shadow for depth */
    }
    
    .toggle-button {
      position: absolute;
      right: -20px;
      top: 50%;
      transform: translateY(-50%);
      width: 20px;
      height: 60px;
      background-color: black; /* Black background */
      border: none;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 0 8px 8px 0;
      padding: 0;
      outline: none;
      z-index: 1001;
    }
    
    .arrow {
      width: 0;
      height: 0;
      border-top: 10px solid transparent;
      border-bottom: 10px solid transparent;
      border-left: 10px solid white; /* White arrow */
      transition: transform 0.3s ease;
    }
    
    .arrow-collapsed {
      transform: rotate(180deg);
    }
    
    ul {
      list-style-type: none;
      padding: 0;
      margin: 0;
    }
    
    li {
      padding: 20px; /* Increased padding for larger text */
      cursor: pointer;
      border-bottom: 1px solid #eee; /* Add a subtle separator */
      transition: background-color 0.3s ease;
    }
    
    li:hover {
      background-color: #f5f5f5; /* Light gray hover effect */
    }
    
    /* Add a vintage paper texture to the sidebar */
    .sidebar::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxwYXR0ZXJuIGlkPSJwYXR0ZXJuXzEiIHBhdHRlcm5Vbml0cz0idXNlclNwYWNlT25Vc2UiIHg9IjAiIHk9IjAiIHdpZHRoPSI1MCIgaGVpZ2h0PSI1MCI+PHJlY3Qgd2lkdGg9IjUwIiBoZWlnaHQ9IjUwIiBmaWxsPSIjZmZmZmZmIi8+PHJlY3Qgd2lkdGg9IjEiIGhlaWdodD0iNTAiIGZpbGw9IiNlZWVlZWUiLz48cmVjdCB3aWR0aD0iNTAiIGhlaWdodD0iMSIgZmlsbD0iI2VlZWVlZSIvPjwvcGF0dGVybj48cmVjdCB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiBmaWxsPSJ1cmwoI3BhdHRlcm5fMSkiLz48L3N2Zz4=');
      opacity: 0.2; /* Subtle paper texture */
      pointer-events: none; /* Ensure clicks pass through */
    }
    </style>