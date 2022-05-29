<template>
    <footer role="footer" class="footer">
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

    .footer__logo {
        width: 30%;
    }

    .footer__social-icons {
        display: flex;
    }

    .footer__social-icons img {
        padding: 0 var(--top-small) 0 0;
        width: 25%;
    }

    /* Medium screen devices (968px and above) */
    @media screen and (min-width: 968px) {
        .footer__logo {
            width: 10%;
        }

        .footer__social-icons img {
            padding: 0 var(--top-xsmall) 0 var(--top-xsmall);
            width: 30%;
        }
    }
</style>