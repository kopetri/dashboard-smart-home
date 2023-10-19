<script>
    import Breadcrumb from "sveltestrap/src/Breadcrumb.svelte";
    import BreadcrumbItem from "sveltestrap/src/BreadcrumbItem.svelte";
    import Card from "sveltestrap/src/Card.svelte";
    import CardBody from "sveltestrap/src/CardBody.svelte";
    import CardHeader from "sveltestrap/src/CardHeader.svelte";
    import CardText from "sveltestrap/src/CardText.svelte";
    import CardSubtitle from "sveltestrap/src/CardSubtitle.svelte";
    import Row from "sveltestrap/src/Row.svelte";
  
    import DashboardCard from "../components/DashboardCard.svelte";
    import CustomCard from "../components/CustomCard.svelte";
    import Table from "../components/Table.svelte";
    import OutdoorTemperature from "../components/Visualizations/OutdoorTemperature.svelte";
    import BarChart from "../components/Charts/BarChart.svelte";

    import { onMount } from "svelte";

    const getData = () => {
        fetch("https://api.myuplink.com/v2/systems/me?page=1&itemsPerPage=10", {
            headers: {accept: "application/json", Authorization: `Bearer ${sessionStorage.getItem("access_token")}`}
        }).then(resp=>resp.json()).then(resp=>systems=resp.systems)
    }

    onMount(()=>{
        getData();
    })
  
    let title = "SB Admin Svelte";
    let systems = [];
  </script>
  
  <svelte:head>
    <title>{title}</title>
  </svelte:head>
  <h1 class="mt-4">Dashboard</h1>
  <Breadcrumb class="mb-4">
    <BreadcrumbItem active>Dashboard</BreadcrumbItem>
  </Breadcrumb>
  <Row>
    {#each systems as system}
    <div class="col-xl-3 col-md-6">
        <DashboardCard cardTitle={system.name} cardColor="primary" href="systems" />
      </div>
    {/each}    
  </Row>
  {#each systems as system}
    {#each system.devices as device}
    <Row>
        <div class="col-xl-6">
          <CustomCard cardTitle="Area Chart Example" cardIcon="fas fa-chart-area">
            <OutdoorTemperature device={device}/>
          </CustomCard>
        </div>
        <div class="col-xl-6">
          <CustomCard cardTitle="Bar Chart Example" cardIcon="fas fa-chart-bar">
            <BarChart />
          </CustomCard>
        </div>
      </Row>
    {/each}
  {/each}
  <CustomCard cardTitle="DataTable Example" cardIcon="fas fa-table">
    <Table />
  </CustomCard>
  