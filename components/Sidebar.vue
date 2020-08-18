<template>
      <div class="sidebar">
        <div class="sidebar-backdrop"></div>
        <transition name="slide">
            <div class="sidebar-panel">
                <slot></slot>
            </div>
        </transition>
    </div>
    </template>
    <style scoped>
    .slide-enter-active,
    .slide-leave-active
    {
        transition: transform 0.2s ease;
    }

    .slide-enter,
    .slide-leave-to {
        transform: translateX(-100%);
        transition: all 150ms ease-in 0s
    }

    .sidebar-backdrop {
        width: 100vw;
        height: 100vh;
        position: fixed;
    }

    .sidebar-panel {
        overflow-y: auto;
        background: #f0f0f0;
        box-shadow: 3px 7px 7px #b9b9b9;
        position: fixed;
        left: 3%;
        padding: 2rem 30px 2rem 2px;
        width: 180px;
    }
    </style>