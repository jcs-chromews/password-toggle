<script>
  import { onMount } from "svelte";

  var show = false;

  onMount(() => {
    var eye = document.getElementById("eye");
    disable(); // disabled by default

    //var fileds = document.getElementsByTagName();

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
    }

    function disable() {
      eye?.classList.add("grayscale");
      eye?.classList.remove("no-grayscale");
    }
  });
</script>

<main>
  <img id="eye" src="./logo.png" alt="eye" width="100px" />
</main>

<style>
  :global(.no-grayscale) {
    filter: grayscale(0%);
    transition: all 0.8s ease;
  }

  :global(.grayscale) {
    filter: grayscale(100%);
    transition: all 0.8s ease;
  }
</style>
