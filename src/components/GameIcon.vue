<template>
<div>
   <i class="fa fa-circle fa-4x zoomIn text-primary" v-if="iconName === 'circle'"></i>
   <i class="fa fa-times fa-4x zoomIn text-danger"  v-else-if="iconName === 'cross'"></i>
   <img src="../assets/logo.png" alt="logo" class="img-fluid zoomIn" v-else-if="iconName === 'empty'" />
</div>
</template>

<script>
export default {
    props: {
        iconName: {
        type: String,
        required: true
        }
    }
}
</script>


<style>
img {
  width: 80px;
}

@keyframes zoomIn {
  from {
    opacity: 0;
    transform: scale3d(0.3, 0.3, 0.3);
  }

  50% {
    opacity: 1;
  }
}

.zoomIn {
  animation-name: zoomIn;
  animation-duration: 1s;
}
</style>
