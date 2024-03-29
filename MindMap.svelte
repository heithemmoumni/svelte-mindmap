<script>
  import {
    forceCollide,
    forceLink,
    forceManyBody,
    forceSimulation,
    select,
    zoom,
    zoomIdentity
  } from "d3";

  import {
    d3Connections,
    d3Nodes,
    d3Drag,
    d3PanZoom,
    onTick
  } from "./utils/d3";
  import './app.css'
  import { getDimensions, getViewBox } from "./utils/dimensions";
  import subnodesToHTML from "./utils/subnodesToHTML";
  import nodeToHTML from "./utils/nodeToHTML";
  import { onMount, beforeUpdate, afterUpdate } from "svelte";

  export let editable = false;
  export let _simulation = null;

  export let map = {
    title: null,
    nodes: [],
    connections: []
  };

  let mountPoint;

  function prepareNodes() {
    const render = node => {
      node.html = nodeToHTML(node);
      node.nodesHTML = subnodesToHTML(node.nodes);

      const dimensions = getDimensions(node.html, {}, "mindmap-node");
      node.width = dimensions.width;
      node.height = dimensions.height;

      const nodesDimensions = getDimensions(
        node.nodesHTML,
        {},
        "mindmap-subnodes-text"
      );
      node.nodesWidth = nodesDimensions.width;
      node.nodesHeight = nodesDimensions.height;
    };

    map.nodes.forEach(node => render(node));
  }

  /**
   * Add new class to nodes, attach drag behevior,
   * and start simulation.
   */
  function prepareEditor(svg, conns, nodes, subnodes) {
    nodes
      .attr("class", "mindmap-node mindmap-node--editable")
      .on("dbclick", node => {
        node.fx = null;
        node.fy = null;
      });

    nodes.call(d3Drag(_simulation, svg, nodes));

    // Tick the simulation 100 times
    for (let i = 0; i < 100; i += 1) {
      _simulation.tick();
    }

    setTimeout(() => {
      _simulation
        .alphaTarget(0.5)
        .on("tick", () => onTick(conns, nodes, subnodes));
    }, 200);
  }

  /**
   * Render mind map unsing D3
   */

  function renderMap() {
    const svg = select(mountPoint);

    // Clear the SVG in case there's stuff already there.
    svg.selectAll("*").remove();

    // Add subnode group
    svg.append("g").attr("id", "mindmap-subnodes");

    prepareNodes();

    // Bind data to SVG elements and set all the properties to render them
    const connections = d3Connections(svg, map.connections);
    const { nodes, subnodes } = d3Nodes(svg, map.nodes);

    nodes.append("title").text(node => node.note);

    // Bind nodes and connections to the simulation
    _simulation
      .nodes(map.nodes)
      .force("link")
      .links(map.connections);

    if (editable) {
      prepareEditor(svg, connections, nodes, subnodes);
    }

    // Tick the simulation 100 times
    for (let i = 0; i < 100; i += 1) {
      _simulation.tick();
    }

    onTick(connections, nodes, subnodes);

    svg
      .attr("viewBox", getViewBox(nodes.data()))
      .call(d3PanZoom(svg))
      .on("dbClick.zoom", null);
  }

  onMount(() => {
    _simulation = forceSimulation()
      .force("link", forceLink().id(node => node.text))
      .force("charge", forceManyBody())
      .force("collide", forceCollide().radius(100));

    renderMap();
  });

  afterUpdate(() => {
    zoom().transform(select(mountPoint), zoomIdentity);
    renderMap();
  });
</script>

<div>
  {#if map.title}
    <h1>{map.title}</h1>
  {/if}
  <svg class="mindmap-svg" bind:this={mountPoint} />
</div>
