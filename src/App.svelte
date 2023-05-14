<script>
  import "./tailwind.css";
  import MainWindow from "./routes/MainWindow.svelte";

  import { Router, Link, Route } from "svelte-navigator";
</script>

<Router>
  <div>
    <Route path="/">
      <MainWindow />
    </Route>
  </div>
</Router>
