<script>
    import Breadcrumb from "sveltestrap/src/Breadcrumb.svelte";
    import BreadcrumbItem from "sveltestrap/src/BreadcrumbItem.svelte";
    import Card from "sveltestrap/src/Card.svelte";
    import CardBody from "sveltestrap/src/CardBody.svelte";
    import CardHeader from "sveltestrap/src/CardHeader.svelte";
    import CardFooter from "sveltestrap/src/CardFooter.svelte";
    import Row from "sveltestrap/src/Row.svelte";
    import Col from "sveltestrap/src/Col.svelte";
  
    import { onMount } from "svelte";
  
    const getData = () => {
        fetch("https://api.myuplink.com/v2/systems/me?page=1&itemsPerPage=10", {
            headers: {accept: "application/json", Authorization: `Bearer ${sessionStorage.getItem("access_token")}`}
        }).then(resp=>resp.json()).then(resp=>systems=resp.systems)
    }

    onMount(()=>{
        getData();
    })
  
    let systems = [];
  </script>
  
  <h1 class="mt-4">Systems</h1>
  <Breadcrumb class="mb-4">
    <BreadcrumbItem>
      <a href=".">Dashboard</a>
    </BreadcrumbItem>
    <BreadcrumbItem active>Systems</BreadcrumbItem>
  </Breadcrumb>
  
  {#each systems as system}
  <Card class="mb-4">
    <CardHeader>{system.name}</CardHeader>
    <CardBody>
      {#each system.devices as device}
      <Row>
        <Col>Device ID</Col>
        <Col>{device.id}</Col>
      </Row>
      <Row>
        <Col>Connection state</Col>
        <Col>{device.connectionState}</Col>
      </Row>
      <Row>
        <Col>Current Firmware Version</Col>
        <Col>{device.currentFwVersion}</Col>
      </Row>
      <Row>
        <Col>Serial number</Col>
        <Col>{device.product.serialNumber}</Col>
      </Row>
      {/each}
    </CardBody>
  </Card>
  {/each}
  
