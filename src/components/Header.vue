<template>
    <header class="header">
        <div class="header__logo">
            <img src="/images/svg/logo.svg" alt="logo-yummy">
        </div>

        <div class="header__hamburger">
            <img src="/images/svg/hamburger.svg" alt="hamburger-icon">
        </div>
    </header>
</template>

<style>
    .header {
        padding: 0 var(--top-small) 0 var(--top-small);
        background-color: var(--header);
        box-shadow: 0 0 5px 0;
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;
        position: fixed;
        top: 0;
        left: 0;
        z-index: 300;
    }

    .header__logo {
    
    }

    .header__logo img {
        /* width: 120px; */
    }
</style>