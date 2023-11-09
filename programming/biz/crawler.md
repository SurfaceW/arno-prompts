# Crawlers Assistant

- author: arno
- date: 2023-11-10
- version: 0.1.0
- models: gpt4-32k would be the best or claude 100k

## Description

An assistant that help you to write crawlers.

## Prompt Structure

```md
# Role

You are a programming assistant use `typescript` and `nodejs` to write crawlers.

* you can use the common crawler framework or library related to node.js to write crawlers.
* this time, you have to parse a sample html file and extract the data I want.
* the given html fragment would be like the following example.

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- head do not need to extract -->
  </head>
  <body
    class="widget_frame_base tgme_webpreview emoji_image thin_box_shadow tme_mode no_transitions"
  >
    <header class="tgme_header search_collapsed">
      <div class="tgme_container">
        <div class="tgme_header_search">
          <form class="tgme_header_search_form" action="/s/DEXTNewPairsBot">
            <svg class="tgme_header_search_form_icon" width="20" height="20" viewBox="0 0 20 20">
              <g fill="none" stroke="#7D7F81" stroke-width="1.4">
                <circle cx="9" cy="9" r="6"></circle>
                <path d="M13.5,13.5 L17,17" stroke-linecap="round"></path>
              </g>
            </svg>
            <input
              class="tgme_header_search_form_input js-header_search"
              placeholder="Search"
              name="q"
              autocomplete="off"
              value=""
            />
          </form>
        </div>
        <div class="tgme_header_right_column">
          <section class="tgme_right_column">
            <div class="tgme_channel_info">
              <div class="tgme_channel_info_header">
                <i class="tgme_page_photo_image bgcolor4" data-content="DB"
                  ><img
                    src="https://cdn4.cdn-telegram.org/file/q8Y9Tq83zSdwFP1-br0GL6pqfu7rR8bEUzdJ5jNlsJlbSklELoiyiQMl8jA86u6c46Ou7ZOrElkEAQV2RFBmkUTDYeOwR2Pr0Efk3dfof6ge48yPV-9v_oLQyHdrtxGN0PfKktbWB8NywsUWlxuRASPWoM0Q-qQP79OibnlU07hh87PeQwDFQW96smCzL78PFFdVzqGPGFb5cRC9uKH8hQErh_FVg-4r38Wyoq_s2-SsJtpPs9G5bLSUQGyR7sPQXu51eS2iYnNdvcY1kIytp7avEAgemESbmu7GEgkO2c5WXgaEjJYsXrswPKWJSTn2n46iysqclRziSfOv7SEPGQ.jpg"
                /></i>
                <div class="tgme_channel_info_header_title_wrap">
                  <div class="tgme_channel_info_header_title">
                    <span dir="auto">DEXT Live New Pairs Bot [Ethereum Blockchain]</span>
                  </div>
                  <div class="tgme_channel_info_header_labels"></div>
                </div>
                <div class="tgme_channel_info_header_username">
                  <a href="https://t.me/DEXTNewPairsBot">@DEXTNewPairsBot</a>
                </div>
              </div>
              <div class="tgme_channel_info_counters">
                <div class="tgme_channel_info_counter">
                  <span class="counter_value">27.4K</span>
                  <span class="counter_type">subscribers</span>
                </div>
                <div class="tgme_channel_info_counter">
                  <span class="counter_value">6</span>
                  <span class="counter_type">photos</span>
                </div>
                <div class="tgme_channel_info_counter">
                  <span class="counter_value">208K</span>
                  <span class="counter_type">links</span>
                </div>
              </div>
              <div class="tgme_channel_info_description">
                Follow all new Pairs in real time on Dextools:<br /><a
                  href="https://www.dextools.io/app/ether/pool-explorer"
                  target="_blank"
                  rel="noopener"
                  >https://www.dextools.io/app/ether/pool-explorer</a
                >
              </div>
              <a
                class="tgme_channel_download_telegram"
                href="//telegram.org/dl?tme=1d9cb169e1fe7b4c4f_15438699716097403248"
              >
                <svg
                  class="tgme_channel_download_telegram_icon"
                  width="21px"
                  height="18px"
                  viewBox="0 0 21 18"
                >
                  <g fill="none">
                    <path
                      fill="#ffffff"
                      d="M0.554,7.092 L19.117,0.078 C19.737,-0.156 20.429,0.156 20.663,0.776 C20.745,0.994 20.763,1.23 20.713,1.457 L17.513,16.059 C17.351,16.799 16.62,17.268 15.88,17.105 C15.696,17.065 15.523,16.987 15.37,16.877 L8.997,12.271 C8.614,11.994 8.527,11.458 8.805,11.074 C8.835,11.033 8.869,10.994 8.905,10.958 L15.458,4.661 C15.594,4.53 15.598,4.313 15.467,4.176 C15.354,4.059 15.174,4.037 15.036,4.125 L6.104,9.795 C5.575,10.131 4.922,10.207 4.329,10.002 L0.577,8.704 C0.13,8.55 -0.107,8.061 0.047,7.614 C0.131,7.374 0.316,7.182 0.554,7.092 Z"
                    ></path>
                  </g></svg
                >Download Telegram
              </a>
              <div class="tgme_footer">
                <div class="tgme_footer_column">
                  <h5><a href="//telegram.org/faq">About</a></h5>
                </div>
                <div class="tgme_footer_column">
                  <h5><a href="//telegram.org/blog">Blog</a></h5>
                </div>
                <div class="tgme_footer_column">
                  <h5><a href="//telegram.org/apps">Apps</a></h5>
                </div>
                <div class="tgme_footer_column">
                  <h5><a href="//core.telegram.org/">Platform</a></h5>
                </div>
              </div>
            </div>
          </section>
        </div>
        <div class="tgme_header_info">
          <a
            class="tgme_channel_join_telegram"
            href="//telegram.org/dl?tme=1d9cb169e1fe7b4c4f_15438699716097403248"
          >
            <svg
              class="tgme_channel_join_telegram_icon"
              width="19px"
              height="16px"
              viewBox="0 0 19 16"
            >
              <g fill="none">
                <path
                  fill="#ffffff"
                  d="M0.465,6.638 L17.511,0.073 C18.078,-0.145 18.714,0.137 18.932,0.704 C19.009,0.903 19.026,1.121 18.981,1.33 L16.042,15.001 C15.896,15.679 15.228,16.111 14.549,15.965 C14.375,15.928 14.211,15.854 14.068,15.748 L8.223,11.443 C7.874,11.185 7.799,10.694 8.057,10.345 C8.082,10.311 8.109,10.279 8.139,10.249 L14.191,4.322 C14.315,4.201 14.317,4.002 14.195,3.878 C14.091,3.771 13.926,3.753 13.8,3.834 L5.602,9.138 C5.112,9.456 4.502,9.528 3.952,9.333 L0.486,8.112 C0.077,7.967 -0.138,7.519 0.007,7.11 C0.083,6.893 0.25,6.721 0.465,6.638 Z"
                ></path>
              </g></svg
            >Join
          </a>
          <a class="tgme_header_link" href="https://t.me/DEXTNewPairsBot">
            <i class="tgme_page_photo_image bgcolor4" data-content="DB"
              ><img
                src="https://cdn4.cdn-telegram.org/file/q8Y9Tq83zSdwFP1-br0GL6pqfu7rR8bEUzdJ5jNlsJlbSklELoiyiQMl8jA86u6c46Ou7ZOrElkEAQV2RFBmkUTDYeOwR2Pr0Efk3dfof6ge48yPV-9v_oLQyHdrtxGN0PfKktbWB8NywsUWlxuRASPWoM0Q-qQP79OibnlU07hh87PeQwDFQW96smCzL78PFFdVzqGPGFb5cRC9uKH8hQErh_FVg-4r38Wyoq_s2-SsJtpPs9G5bLSUQGyR7sPQXu51eS2iYnNdvcY1kIytp7avEAgemESbmu7GEgkO2c5WXgaEjJYsXrswPKWJSTn2n46iysqclRziSfOv7SEPGQ.jpg"
            /></i>
            <div class="tgme_header_title_wrap">
              <div class="tgme_header_title">
                <span dir="auto">DEXT Live New Pairs Bot [Ethereum Blockchain]</span>
              </div>
              <div class="tgme_header_labels"></div>
            </div>
            <div class="tgme_header_counter">27.4K subscribers</div>
          </a>
        </div>
      </div>
    </header>
    <main class="tgme_main" data-url="/DEXTNewPairsBot">
      <div class="tgme_container">
        <section class="tgme_channel_history js-message_history">
          <div class="tgme_widget_message_centered js-messages_more_wrap">
            <a
              href="/s/DEXTNewPairsBot?before=208634"
              class="tme_messages_more js-messages_more"
              data-before="208634"
            ></a>
          </div>
          <div class="tgme_widget_message_wrap js-widget_message_wrap">
            <div
              class="tgme_widget_message text_not_supported_wrap js-widget_message"
              data-post="DEXTNewPairsBot/208649"
              data-view="eyJjIjotMTI5ODU1NjgxNiwicCI6MjA4NjQ5LCJ0IjoxNjk5NTUxNzgzLCJoIjoiYjA1YTc2Nzg2ZDhiODNhMzJmIn0"
            >
              <div class="tgme_widget_message_user">
                <a href="https://t.me/DEXTNewPairsBot"
                  ><i class="tgme_widget_message_user_photo bgcolor4" data-content="D"
                    ><img
                      src="https://cdn4.cdn-telegram.org/file/q8Y9Tq83zSdwFP1-br0GL6pqfu7rR8bEUzdJ5jNlsJlbSklELoiyiQMl8jA86u6c46Ou7ZOrElkEAQV2RFBmkUTDYeOwR2Pr0Efk3dfof6ge48yPV-9v_oLQyHdrtxGN0PfKktbWB8NywsUWlxuRASPWoM0Q-qQP79OibnlU07hh87PeQwDFQW96smCzL78PFFdVzqGPGFb5cRC9uKH8hQErh_FVg-4r38Wyoq_s2-SsJtpPs9G5bLSUQGyR7sPQXu51eS2iYnNdvcY1kIytp7avEAgemESbmu7GEgkO2c5WXgaEjJYsXrswPKWJSTn2n46iysqclRziSfOv7SEPGQ.jpg" /></i
                ></a>
              </div>
              <div class="tgme_widget_message_bubble">
                <i class="tgme_widget_message_bubble_tail">
                  <svg class="bubble_icon" width="9px" height="20px" viewBox="0 0 9 20">
                    <g fill="none">
                      <path
                        class="background"
                        fill="#ffffff"
                        d="M8,1 L9,1 L9,20 L8,20 L8,18 C7.807,15.161 7.124,12.233 5.950,9.218 C5.046,6.893 3.504,4.733 1.325,2.738 L1.325,2.738 C0.917,2.365 0.89,1.732 1.263,1.325 C1.452,1.118 1.72,1 2,1 L8,1 Z"
                      ></path>
                      <path
                        class="border_1x"
                        fill="#d7e3ec"
                        d="M9,1 L2,1 C1.72,1 1.452,1.118 1.263,1.325 C0.89,1.732 0.917,2.365 1.325,2.738 C3.504,4.733 5.046,6.893 5.95,9.218 C7.124,12.233 7.807,15.161 8,18 L8,20 L9,20 L9,1 Z M2,0 L9,0 L9,20 L7,20 L7,20 L7.002,18.068 C6.816,15.333 6.156,12.504 5.018,9.58 C4.172,7.406 2.72,5.371 0.649,3.475 C-0.165,2.729 -0.221,1.464 0.525,0.649 C0.904,0.236 1.439,0 2,0 Z"
                      ></path>
                      <path
                        class="border_2x"
                        d="M9,1 L2,1 C1.72,1 1.452,1.118 1.263,1.325 C0.89,1.732 0.917,2.365 1.325,2.738 C3.504,4.733 5.046,6.893 5.95,9.218 C7.124,12.233 7.807,15.161 8,18 L8,20 L9,20 L9,1 Z M2,0.5 L9,0.5 L9,20 L7.5,20 L7.5,20 L7.501,18.034 C7.312,15.247 6.64,12.369 5.484,9.399 C4.609,7.15 3.112,5.052 0.987,3.106 C0.376,2.547 0.334,1.598 0.894,0.987 C1.178,0.677 1.579,0.5 2,0.5 Z"
                      ></path>
                      <path
                        class="border_3x"
                        d="M9,1 L2,1 C1.72,1 1.452,1.118 1.263,1.325 C0.89,1.732 0.917,2.365 1.325,2.738 C3.504,4.733 5.046,6.893 5.95,9.218 C7.124,12.233 7.807,15.161 8,18 L8,20 L9,20 L9,1 Z M2,0.667 L9,0.667 L9,20 L7.667,20 L7.667,20 L7.668,18.023 C7.477,15.218 6.802,12.324 5.64,9.338 C4.755,7.064 3.243,4.946 1.1,2.983 C0.557,2.486 0.52,1.643 1.017,1.1 C1.269,0.824 1.626,0.667 2,0.667 Z"
                      ></path>
                    </g>
                  </svg>
                </i>
                <div class="tgme_widget_message_author accent_color">
                  <a class="tgme_widget_message_owner_name" href="https://t.me/DEXTNewPairsBot"
                    ><span dir="auto">DEXT Live New Pairs Bot [Ethereum Blockchain]</span></a
                  >
                </div>

                <div class="tgme_widget_message_text js-message_text" dir="auto">
                  New pair at Uniswap v2 <br /><br />YUMMY (YUMMY/WETH)<br /><br />Initial
                  Liquidity: &#036;3,027<br /><br />Token contract:<br />0xb86fe55c407d4c07968e999879f78caeb0be53a8<br /><br />DEXTools:<br /><a
                    href="https://www.dextools.io/app/ether/pair-explorer/0x181cb8501164405bf07b1716334e5295285ed90d"
                    target="_blank"
                    rel="noopener"
                    >https://www.dextools.io/app/ether/pair-explorer/0x181cb8501164405bf07b1716334e5295285ed90d</a
                  >
                </div>
                <a
                  class="tgme_widget_message_link_preview"
                  href="https://www.dextools.io/app/ether/pair-explorer/0x181cb8501164405bf07b1716334e5295285ed90d"
                >
                  <i
                    class="link_preview_right_image"
                    style="
                      background-image: url('https://cdn4.cdn-telegram.org/file/M_7JtEVbNCbjkWfSONkseIEQCqbWJjgSika8coalz6Aos9UhnFotPYWlosveBOdwL8YB0dZyVtsw0TINlwAcQ9x-Plzg4XkF0IYNzbHV4jsU7wmfnaupI6OsmFN04QvYfkHXzjUOn1mGmliR6RIcI5LfS_TkJTZ50fLqadsQEc3PZQ8DuyVqgG-OZMoyzrX4eo9NjjsG3Ojs-Y-fcgdI73_Z3vNKQKn4yv0csiwqy81EglTNXBYxSThhfu0mEfSCqZgtQ5sMEDCaCX1jWWsQzLrn5_8O_anGW9K6dntCFDJNvGVPHF4JYMQMI4-yrL1c9Isvr8xdsATKtrJPkNpvMw.jpg');
                    "
                  ></i>
                  <div class="link_preview_site_name accent_color" dir="auto">DEXTools.io</div>

                  <div class="link_preview_description" dir="auto">
                    DEXTools, the gateway to DEFI, real-time charts, history and all token info from
                    blockchain.
                  </div>
                </a>
                <div class="tgme_widget_message_footer compact js-message_footer">
                  <div class="tgme_widget_message_info short js-message_info">
                    <span class="tgme_widget_message_views">44</span
                    ><span class="copyonly"> views</span
                    ><span class="tgme_widget_message_meta"
                      ><a
                        class="tgme_widget_message_date"
                        href="https://t.me/DEXTNewPairsBot/208649"
                        ><time datetime="2023-11-09T17:35:20+00:00" class="time">17:35</time></a
                      ></span
                    >
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="tgme_widget_message_wrap js-widget_message_wrap">
            <div
              class="tgme_widget_message text_not_supported_wrap js-widget_message"
              data-post="DEXTNewPairsBot/208652"
              data-view="eyJjIjotMTI5ODU1NjgxNiwicCI6MjA4NjUyLCJ0IjoxNjk5NTUxNzgzLCJoIjoiYjRlZTQwMTcwMjg1YjUxYTc0In0"
            >
              <div class="tgme_widget_message_user">
                <a href="https://t.me/DEXTNewPairsBot"
                  ><i class="tgme_widget_message_user_photo bgcolor4" data-content="D"
                    ><img
                      src="https://cdn4.cdn-telegram.org/file/q8Y9Tq83zSdwFP1-br0GL6pqfu7rR8bEUzdJ5jNlsJlbSklELoiyiQMl8jA86u6c46Ou7ZOrElkEAQV2RFBmkUTDYeOwR2Pr0Efk3dfof6ge48yPV-9v_oLQyHdrtxGN0PfKktbWB8NywsUWlxuRASPWoM0Q-qQP79OibnlU07hh87PeQwDFQW96smCzL78PFFdVzqGPGFb5cRC9uKH8hQErh_FVg-4r38Wyoq_s2-SsJtpPs9G5bLSUQGyR7sPQXu51eS2iYnNdvcY1kIytp7avEAgemESbmu7GEgkO2c5WXgaEjJYsXrswPKWJSTn2n46iysqclRziSfOv7SEPGQ.jpg" /></i
                ></a>
              </div>
              <div class="tgme_widget_message_bubble">
                <i class="tgme_widget_message_bubble_tail">
                  <svg class="bubble_icon" width="9px" height="20px" viewBox="0 0 9 20">
                    <g fill="none">
                      <path
                        class="background"
                        fill="#ffffff"
                        d="M8,1 L9,1 L9,20 L8,20 L8,18 C7.807,15.161 7.124,12.233 5.950,9.218 C5.046,6.893 3.504,4.733 1.325,2.738 L1.325,2.738 C0.917,2.365 0.89,1.732 1.263,1.325 C1.452,1.118 1.72,1 2,1 L8,1 Z"
                      ></path>
                      <path
                        class="border_1x"
                        fill="#d7e3ec"
                        d="M9,1 L2,1 C1.72,1 1.452,1.118 1.263,1.325 C0.89,1.732 0.917,2.365 1.325,2.738 C3.504,4.733 5.046,6.893 5.95,9.218 C7.124,12.233 7.807,15.161 8,18 L8,20 L9,20 L9,1 Z M2,0 L9,0 L9,20 L7,20 L7,20 L7.002,18.068 C6.816,15.333 6.156,12.504 5.018,9.58 C4.172,7.406 2.72,5.371 0.649,3.475 C-0.165,2.729 -0.221,1.464 0.525,0.649 C0.904,0.236 1.439,0 2,0 Z"
                      ></path>
                      <path
                        class="border_2x"
                        d="M9,1 L2,1 C1.72,1 1.452,1.118 1.263,1.325 C0.89,1.732 0.917,2.365 1.325,2.738 C3.504,4.733 5.046,6.893 5.95,9.218 C7.124,12.233 7.807,15.161 8,18 L8,20 L9,20 L9,1 Z M2,0.5 L9,0.5 L9,20 L7.5,20 L7.5,20 L7.501,18.034 C7.312,15.247 6.64,12.369 5.484,9.399 C4.609,7.15 3.112,5.052 0.987,3.106 C0.376,2.547 0.334,1.598 0.894,0.987 C1.178,0.677 1.579,0.5 2,0.5 Z"
                      ></path>
                      <path
                        class="border_3x"
                        d="M9,1 L2,1 C1.72,1 1.452,1.118 1.263,1.325 C0.89,1.732 0.917,2.365 1.325,2.738 C3.504,4.733 5.046,6.893 5.95,9.218 C7.124,12.233 7.807,15.161 8,18 L8,20 L9,20 L9,1 Z M2,0.667 L9,0.667 L9,20 L7.667,20 L7.667,20 L7.668,18.023 C7.477,15.218 6.802,12.324 5.64,9.338 C4.755,7.064 3.243,4.946 1.1,2.983 C0.557,2.486 0.52,1.643 1.017,1.1 C1.269,0.824 1.626,0.667 2,0.667 Z"
                      ></path>
                    </g>
                  </svg>
                </i>
                <div class="tgme_widget_message_author accent_color">
                  <a class="tgme_widget_message_owner_name" href="https://t.me/DEXTNewPairsBot"
                    ><span dir="auto">DEXT Live New Pairs Bot [Ethereum Blockchain]</span></a
                  >
                </div>
                <div class="tgme_widget_message_text js-message_text" dir="auto">
                  New pair at Uniswap v2 <br /><br />Jujutsu Kaisen (JUJU/WETH)<br /><br />Initial
                  Liquidity: &#036;4,042<br /><br />Token contract:<br />0xdd466e9c060694aff06abdde57615282d7239562<br /><br />DEXTools:<br /><a
                    href="https://www.dextools.io/app/ether/pair-explorer/0x807df86b367411fb31ba88c97d2baa2f9575f52e"
                    target="_blank"
                    rel="noopener"
                    >https://www.dextools.io/app/ether/pair-explorer/0x807df86b367411fb31ba88c97d2baa2f9575f52e</a
                  >
                </div>
                <a
                  class="tgme_widget_message_link_preview"
                  href="https://www.dextools.io/app/ether/pair-explorer/0x807df86b367411fb31ba88c97d2baa2f9575f52e"
                >
                  <i
                    class="link_preview_right_image"
                    style="
                      background-image: url('https://cdn4.cdn-telegram.org/file/M_7JtEVbNCbjkWfSONkseIEQCqbWJjgSika8coalz6Aos9UhnFotPYWlosveBOdwL8YB0dZyVtsw0TINlwAcQ9x-Plzg4XkF0IYNzbHV4jsU7wmfnaupI6OsmFN04QvYfkHXzjUOn1mGmliR6RIcI5LfS_TkJTZ50fLqadsQEc3PZQ8DuyVqgG-OZMoyzrX4eo9NjjsG3Ojs-Y-fcgdI73_Z3vNKQKn4yv0csiwqy81EglTNXBYxSThhfu0mEfSCqZgtQ5sMEDCaCX1jWWsQzLrn5_8O_anGW9K6dntCFDJNvGVPHF4JYMQMI4-yrL1c9Isvr8xdsATKtrJPkNpvMw.jpg');
                    "
                  ></i>
                  <div class="link_preview_site_name accent_color" dir="auto">DEXTools.io</div>

                  <div class="link_preview_description" dir="auto">
                    DEXTools, the gateway to DEFI, real-time charts, history and all token info from
                    blockchain.
                  </div>
                </a>
                <div class="tgme_widget_message_footer compact js-message_footer">
                  <div class="tgme_widget_message_info short js-message_info">
                    <span class="tgme_widget_message_views">9</span
                    ><span class="copyonly"> views</span
                    ><span class="tgme_widget_message_meta"
                      ><a
                        class="tgme_widget_message_date"
                        href="https://t.me/DEXTNewPairsBot/208652"
                        ><time datetime="2023-11-09T17:42:39+00:00" class="time">17:42</time></a
                      ></span
                    >
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!-- more repeated blocks -->
        </section>
      </div>
    </main>
    <script src="//telegram.org/js/jquery.min.js"></script>
    <script src="//telegram.org/js/jquery-ui.min.js"></script>
    <script src="//telegram.org/js/tgwallpaper.min.js?3"></script>
    <script src="//telegram.org/js/tgsticker.js?31"></script>

    <script src="//telegram.org/js/widget-frame.js?62"></script>
    <script src="//telegram.org/js/telegram-web.js?14"></script>
    <script>
      TWeb.init();
    </script>

    <script
      defer
      src="https://static.cloudflareinsights.com/beacon.min.js/v84a3a4012de94ce1a686ba8c167c359c1696973893317"
      integrity="sha512-euoFGowhlaLqXsPWQ48qSkBSCFs3DPRyiwVu3FjR96cMPx+Fr+gpWRhIafcHwqwCqWS42RZhIudOvEI+Ckf6MA=="
      data-cf-beacon='{"rayId":"8237db16991ace78","version":"2023.10.0","token":"08a46637120a404a963395dd86986b4f"}'
      crossorigin="anonymous"
    ></script>
  </body>
</html>
```

Now your task are:

* extract the structure form the following content 

<div class="tgme_widget_message_text js-message_text" dir="auto">New pair at Uniswap v2
<br/><br/>Face ID .  (FACE/WETH)<br/><br/>Initial Liquidity: &#036;7,239<br/><br/>Token contract:<br/>0x4c2bdfcf877e2ed7aae6d6066757277af4998c7c<br/><br/>DEXTools:<br/><a
	href="https://www.dextools.io/app/ether/pair-explorer/0x3b54cf596754c92b1a2b8588c03cda48bd145422"
	target="_blank"
	rel="noopener">https://www.dextools.io/app/ether/pair-explorer/0x3b54cf596754c92b1a2b8588c03cda48bd145422</a>
</div>

* the crawler will finally output with **json** like:

```json
[{
  "name": "Face ID . (FACE/WETH)",
  "initialLiquidity": 7239,
  "tokenContract": "0x4c2bdfcf877e2ed7aae6d6066757277af4998c7c",
  "dexToolAddress": "https://www.dextools.io/app/ether/pair-explorer/0x3b54cf596754c92b1a2b8588c03cda48bd145422"
}
  // more repeated
]
```

Now write the code to try to implement such feature.

```

## Examples and Instances

### GPT-4-32K

Response with:

```md
To perform web scraping in Node.js, a popular library is `cheerio` which is a fast, flexible, and lean implementation of core jQuery designed specifically for the server.

First, let's install the required libraries. You can do this by running the following commands in your terminal:

```sh
npm install axios cheerio
```

Here is a simple Node.js script that uses `axios` to download the HTML content and `cheerio` to parse the HTML:

```typescript
import axios from 'axios';
import cheerio from 'cheerio';

async function extractDataFromHtml() {
  try {
    // replace this url with the actual url you want to scrape
    const url = 'http://example.com';
    const { data: html } = await axios.get(url);
    const $ = cheerio.load(html);

    const results: { name: string; initialLiquidity: number; tokenContract: string; dexToolAddress: string }[] = [];

    $('.tgme_widget_message_text').each((index, element) => {
      const text = $(element).text().split('\n');
      const name = text[0].split(' ')[4];
      const initialLiquidity = Number(text[2].split(' ')[2].replace(',', ''));
      const tokenContract = text[4];
      const dexToolAddress = $('a', element).attr('href');

      results.push({ name, initialLiquidity, tokenContract, dexToolAddress });
    });

    console.log(JSON.stringify(results, null, 2));
  } catch (error) {
    console.error(error);
  }
}

extractDataFromHtml();
```

Please replace `'http://example.com'` with the actual URL you want to scrape.

This script will print the result as a JSON array in the console. Each item in the array is an object that contains `name`, `initialLiquidity`, `tokenContract`, and `dexToolAddress`.

The `extractDataFromHtml` function uses `axios` to send a GET request to the given URL and receive the HTML content. Then, it uses `cheerio.load` to parse the HTML content. After that, it uses the `$` function (which is similar to jQuery's `$` function) to select the `.tgme_widget_message_text` elements. For each selected element, it extracts the desired data and adds it to the `results` array.

You can adjust the script according to your needs. For example, you may need to adjust the selectors or the data extraction logic depending on the actual structure of the HTML.
```


## Reference

version: 

- `0.0.1`: first bite use gpt-4-32k to perform the task


[Reference]

- [ref content]()