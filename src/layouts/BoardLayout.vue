<template>
    <div>
        <Navbar brand="ToDoList" />
        <main class="mt-5">
            <slot />
        </main>
    </div>
</template>

<script>
import Navbar from '@/components/board/TheNavbar.vue'

export default {
  name: 'BoardLayout',
  components: {
    Navbar
  }
}
</script>
