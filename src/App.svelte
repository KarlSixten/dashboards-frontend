<script>
    import { Router, Route } from 'svelte-routing'
    import Dashboard from "./pages/Dashboard/Dashboard.svelte";

    export let url = "";
</script>

<Router {url}>
    <div>
        <Route path="/">
            <Dashboard group="sales"></Dashboard>
        </Route>
        <Route path="/sales">
            <Dashboard group="sales"></Dashboard>
        </Route>
        <Route path="/outreach">
            <Dashboard group="outreach"></Dashboard>
        </Route>
    </div>
</Router>