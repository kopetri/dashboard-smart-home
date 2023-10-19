<script>
    import AreaChart from "../Charts/AreaChart.svelte";
    import { onMount } from "svelte";

    export let device;

    const getData = () => {
        fetch(`https://api.myuplink.com/v2/devices/${device.id}/points?parameters=sh-outdoorTemp`, {
            headers: {
                "accept": "application/json", 
                "Accept-Language": "en-US",
                "Authorization": `Bearer ${sessionStorage.getItem("access_token")}`}
        }).then(resp=>resp.json()).then(resp=>console.log(resp))
    }

    onMount(()=>{
        getData();
    })

</script>
<AreaChart />