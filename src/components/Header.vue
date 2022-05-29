<template>
    <header role="header" class="header">
        <div class="header__logo">
            <img src="/images/svg/logo.svg" alt="logo-yummy">
        </div>

        <!-- Hamburger haven't functionality -->
        <!-- <div class="header__hamburger">
            <img src="/images/svg/hamburger.svg" alt="hamburger-icon">
        </div> -->
    </header>
</template>

<style>
    .header {
        position: fixed;
        top: 0;
        left: 0;
        z-index: 300;
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;
        box-shadow: 0 0 5px 0;
        background-color: var(--header);
        padding: 0 var(--padding-small) 0 var(--padding-small);
    }
</style>