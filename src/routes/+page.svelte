<script>
  import { onMount } from "svelte";

  var show = false;

  onMount(() => {
    var inputs = document.getElementsByTagName("input");
    var fields = [];
    for (let i = 0; i < inputs.length; ++i) {
      if (inputs[i].type == "password") {
        fields.push(inputs[i]);
      }
    }

    var eye = document.getElementById("eye");
    disable(); // disabled by default
    var count = document.getElementById("count");

    count.innerHTML = fields.length;

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

      for (let i = 0; i < fields.length; ++i) {
        fields[i].type = "";
      }
    }

    function disable() {
      eye?.classList.add("grayscale");
      eye?.classList.remove("no-grayscale");

      for (let i = 0; i < fields.length; ++i) {
        fields[i].type = "password";
      }
    }
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
    transition: all 0.8s ease;
  }

  :global(.grayscale) {
    filter: grayscale(100%);
    transition: all 0.8s ease;
  }

  #count {
    color: green;
    font-weight: bold;
    font-size: 1.8em;
  }
</style>
