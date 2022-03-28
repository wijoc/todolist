<template>
    <div class="fixed-top">
        <nav class="navbar navbar-expand-lg navbar-light bg-light sticky-top fixed-top">
            <div class="container-fluid">
                <a class="navbar-brand" href="#">{{ brand }}</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                        <li class="nav-item">
                            <router-link class="nav-link active" to="/" @click="tasksList()">Task List</router-link>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#">Link</a>
                        </li>
                        <li class="nav-item">
                        <a class="nav-link disabled">Disabled</a>
                        </li>
                    </ul>
                    <div class="d-flex">
                        <a class="btn btn-outline-secondary" id="navbarDropdown" @click.left="logout()">
                            <i class="bi bi-door-open-fill"></i> Logout
                        </a>
                    </div>
                </div>
            </div>
        </nav>
    </div>
</template>

<script>
export default {
  name: 'TheNavbar',
  props: {
    brand: String
  },
  methods: {
    tasksList () {
      this.$emit('info', 'list')
    },
    logout () {
      var confirmLogout = confirm('Log Out?')
      if (confirmLogout === true) {
        this.$emit('logout', confirmLogout)
      } else {
        alert('Canceled!')
      }
    }
  }
}
</script>
