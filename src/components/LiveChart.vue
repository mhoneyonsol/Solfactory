<template>
  <div :class="{'om': true, 'hidden': !isVisible}">
    <div class="tradingview-widget-container">
      <div class="tradingview-widget-container__widget"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isVisible: false
    };
  },
  mounted() {
    if (!document.querySelector('#tradingview-widget-script')) {
      const script = document.createElement('script');
      script.id = 'tradingview-widget-script';
      script.src = 'https://s3.tradingview.com/external-embedding/embed-widget-advanced-chart.js';
      script.async = true;
      script.innerHTML = JSON.stringify({
        autosize: true,
        symbol: "CRYPTO:SOLUSD",
        interval: "D",
        timezone: "Etc/UTC",
        theme: "dark",
        style: "1",
        locale: "fr",
        hide_side_toolbar: false,
        allow_symbol_change: true,
        calendar: false,
        support_host: "https://www.tradingview.com"
      });
      script.onload = this.applyIframeStyles;
      this.$el.querySelector('.tradingview-widget-container__widget').appendChild(script);
    }

    // Show the div after 5 seconds
    setTimeout(() => {
      this.isVisible = true;
    }, 3000);
  },
  methods: {
    applyIframeStyles() {
      const iframe = this.$el.querySelector('.tradingview-widget-container__widget iframe');
      if (iframe) {
        iframe.style.borderRadius = '23px';
      }
    }
  }
};
</script>

<style scoped>
.tradingview-widget-container {
    height: 600px;
    width: 100%;
    margin-top: 10%;
}

iframe#tradingview-widget-script {
    user-select: none;
    box-sizing: border-box;
    display: block;
    height: 100%;
    width: 100%;
    border-radius: 23px!important;
    filter: drop-shadow(2px 4px 6px black);
}

.tradingview-widget-container__widget {
    height: 600px;
}

.om {
    width: 60%; 
    height: 90vh; 
    margin-left: auto; 
    margin-right: auto;
    transition: opacity 1s ease-in-out;
    opacity: 1;
}

.om.hidden {
    opacity: 0;
}

@media only screen and (max-width: 600px) {
    .om {
        width: 100%!important;
        height: inherit!important;
    }

    .tradingview-widget-container__widget {
        height: 350px!important;
    }

    .modal-content[data-v-5ea95aee] {
        margin: inherit!important;
        width: 92%!important;
        margin-top: 60px!important;
    }
}
</style>
