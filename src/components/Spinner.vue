<script>

export default  {
    data() {
    return {

    }
    },
    }
</script>

<template>
    <div class="loading-spinner"></div>
</template>

<style scoped lang="scss">
    .loading-spinner {
        pointer-events: none;
        width: 24px;
        height: 24px;

        border: 3px solid transparent;
        border-color: #fff;
        border-top-color: rgba(255, 255, 255, 0);
        border-radius: 50%;
        animation: loadingspin 1s linear infinite;
    }

    @keyframes loadingspin {
        100% {
                transform: rotate(360deg)
        }
    }
</style>