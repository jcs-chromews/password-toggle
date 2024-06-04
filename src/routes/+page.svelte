<script>
  import { onMount } from "svelte";

  const query = { active: true, currentWindow: true };

  onMount(() => {
    var show = false;

    var eye = document.getElementById("eye");
    disable(); // disabled by default
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

              for (let i = 0; i < inputs.length; ++i) {
                if (inputs[i].type == "password") {
                  inputs[i].type = "text";
                  inputs[i].setAttribute("password-toggle", "mark");
                }
              }
            },
          })
          .then(() => console.log("[password-toggle] show password"));
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

              for (let i = 0; i < inputs.length; ++i) {
                if (inputs[i].getAttribute("password-toggle") == "mark") {
                  inputs[i].type = "password";
                }
              }
            },
          })
          .then(() => console.log("[password-toggle] hide password"));
      });
    }

    //count.innerHTML = inner_count;
  });
</script>

<main>
  <div>
    <img id="eye" src="./logo.png" alt="eye" width="80px" />
    <span id="count">0</span>
  </div>
</main>

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
    color: green;
    font-weight: bold;
    font-size: 1.8em;
  }
</style>
