<main>
  <div>
    <img id="eye" src="./logo.png" alt="eye" width="80px" />
    <span id="count">0</span>
  </div>
</main>

<script>
  import { onMount } from "svelte";

  const query = { active: true, currentWindow: true };

  onMount(() => {
    var show = false;

    var eye = document.getElementById("eye");
    var count = document.getElementById("count");

    eye?.addEventListener("click", function () {
      show = !show;

      if (show) {
        enable();
      } else {
        disable();
      }
    });

    function enable() {
      eye?.classList.add("no-grayscale");
      eye?.classList.remove("grayscale");

      chrome.tabs.query(query, function (tab) {
        chrome.scripting
          .executeScript({
            target: { tabId: tab[0].id },
            func: () => {
              var inputs = document.getElementsByTagName("input");

              var inner_count = 0;

              for (let i = 0; i < inputs.length; ++i) {
                if (inputs[i].type == "password") {
                  inputs[i].type = "text";
                  inputs[i].setAttribute("password-toggle", "on");
                  ++inner_count;
                }
              }

              return inner_count;
            },
          })
          .then((result) => {
            count.innerHTML = result[0].result;
          });
      });
    }

    function disable() {
      eye?.classList.add("grayscale");
      eye?.classList.remove("no-grayscale");

      chrome.tabs.query(query, function (tab) {
        chrome.scripting
          .executeScript({
            target: { tabId: tab[0].id },
            func: () => {
              var inputs = document.getElementsByTagName("input");

              var inner_count = 0;

              for (let i = 0; i < inputs.length; ++i) {
                if (inputs[i].getAttribute("password-toggle") == "on") {
                  inputs[i].type = "password";
                  inputs[i].setAttribute("password-toggle", "off");
                  ++inner_count;
                }
              }

              return inner_count;
            },
          })
          .then((result) => {
            count.innerHTML = result[0].result;
          });
      });
    }

    chrome.tabs.query(query, function (tab) {
      chrome.scripting
        .executeScript({
          target: { tabId: tab[0].id },
          func: () => {
            var inputs = document.getElementsByTagName("input");
            var inner_count = 0;
            var show = false;

            for (let i = 0; i < inputs.length; ++i) {
              let input = inputs[i];
              let mark = input.getAttribute("password-toggle");
              let is_on = mark == "on";
              let is_pass = input.type == "password";

              if (is_pass || mark) {
                if (is_on) {
                  show = true;
                }
                ++inner_count;
              }
            }

            return [inner_count, show];
          },
        })
        .then((result) => {
          count.innerHTML = result[0].result[0];
          show = result[0].result[1];

          if (!show) {
            disable();
          }
        });
    });
  });
</script>

<style>
  :global(body) {
    background-color: #343434;
  }

  :global(.no-grayscale) {
    filter: grayscale(0%);
    transition: all 0.4s ease;
  }

  :global(.grayscale) {
    filter: grayscale(100%);
    transition: all 0.4s ease;
  }

  #count {
    position: absolute;
    color: #11ffaa;
    font-weight: bold;
    font-size: 1.8em;
    float: right;
    right: 10px;
  }
</style>
