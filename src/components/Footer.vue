<template>
    <footer class="footer">
        <div class="footer__logo">
            <img src="/images/svg/logo.svg" alt="logo-icon">
        </div>

        <div class="footer__social-icons">
            <img src="/images/svg/instagram.svg" alt="instagram-icon">
            <img src="/images/svg/facebook.svg" alt="facebook-icon">
            <img src="/images/svg/twitter.svg" alt="twitter-icon">
        </div>
    </footer>
</template>

<style>
    .footer {
        padding: 0 var(--top-small) 0 var(--top-small);
        background-color: var(--footer);
        position: fixed;
        bottom: 0;
        width: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .footer__social-icons {
        display: flex;
    }

    .footer__social-icons img {
        padding: 0 var(--top-small) 0 var(--top-small);
    }
</style>