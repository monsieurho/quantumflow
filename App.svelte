<script>
  import Button from "./Button.svelte";
  import jQuery from "jquery";
  import * as THREE from "three";
  import SimplexNoise from "simplex-noise";
  import { afterUpdate } from "svelte";
  import About from "./About.svelte";

  setTimeout(function() {
    let jQuerycanvas = jQuery("#blob canvas"),
      canvas = jQuerycanvas[0],
      renderer = new THREE.WebGLRenderer({
        canvas: canvas,
        context: canvas.getContext("webgl2"),
        antialias: true,
        alpha: true
      }),
      simplex = new SimplexNoise();

    renderer.setSize(jQuerycanvas.width(), jQuerycanvas.height());
    renderer.setPixelRatio(window.devicePixelRatio || 1);

    let scene = new THREE.Scene();
    let camera = new THREE.PerspectiveCamera(
      45,
      jQuerycanvas.width() / jQuerycanvas.height(),
      0.1,
      1000
    );

    camera.position.z = 5;

    let geometry = new THREE.SphereGeometry(0.8, 128, 128);

    let material = new THREE.MeshPhongMaterial({
      color: 0xffffff,
      shininess: 100
    });

    let lightTop = new THREE.DirectionalLight(0xff0000, 0.5);
    lightTop.position.set(0, 500, 200);
    lightTop.castShadow = true;
    scene.add(lightTop);

    let lightTop2 = new THREE.DirectionalLight(0x00ff00, 0.4);
    lightTop.position.set(0, 0, 200);
    lightTop.castShadow = true;
    scene.add(lightTop2);

    let lightBottom = new THREE.DirectionalLight(0x0000ff, 0.95);
    lightBottom.position.set(0, 0, 0);
    lightBottom.castShadow = true;
    scene.add(lightBottom);

    let lightBottom2 = new THREE.DirectionalLight(0xffff00, 0.25);
    lightBottom.position.set(0, -500, 400);
    lightBottom.castShadow = true;
    scene.add(lightBottom2);

    let ambientLight = new THREE.AmbientLight(0x798296);
    scene.add(ambientLight);

    let sphere = new THREE.Mesh(geometry, material);

    scene.add(sphere);

    let update = () => {
      let time = performance.now() * 0.00001 * 100 * 1,
        spikes = 1.8 * 1;

      for (let i = 0; i < sphere.geometry.vertices.length; i++) {
        let p = sphere.geometry.vertices[i];
        p.normalize().multiplyScalar(
          1 +
            0.3 * simplex.noise3D(p.x * spikes, p.y * spikes, p.z * spikes + time)
        );
      }

      sphere.geometry.computeVertexNormals();
      sphere.geometry.normalsNeedUpdate = true;
      sphere.geometry.verticesNeedUpdate = true;
    };

    function animate() {
      update();
      renderer.render(scene, camera);
      requestAnimationFrame(animate);
    }

    requestAnimationFrame(animate);
  }, 0);
</script>

<style>
  * {
    box-sizing: border-box;
  }

  .header-quantum {
    position: fixed;
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    padding: 20px;
  }
  .logo-quantum img {
    width: 150px;
  }
  .links-quantum ul {
    display: flex;
    list-style: none;
  }
  .links-quantum li {
    margin-right: 35px;
    font-size: 20px;
  }
  .links-quantum li a {
    color: #000;
    text-decoration: none;
  }

  .crypto-scraper {
    position: fixed;
    bottom: 35px;
    right: 35px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .crypto-scraper svg {
    margin: 0px;
    margin-bottom: -7px;
  }
  .crypto-scraper a {
    font-size: 20px;
    color: #000;
    text-decoration: none;
    margin-right: 20px;
  }

  .hero-container {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    overflow: hidden;
    background-color: #fff;
    background-image: url();
  }

  .hero-text-container {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-items: center;
    align-items: center;
    position: absolute;
    margin-top: -30px;
    color: #111;
  }

  .hero-text-container h1 {
    font-size: 65px;
    margin-bottom: 0px;
    text-align: center;
  }
  .hero-text-container p {
    font-size: 45px;
    text-align: center;
    max-width: 1000px;
    margin-top: 0px;
  }

  #blob {
    position: relative;
    top: 50px;
  }
  #blob canvas {
    width: 100vw;
    height: 100vh;
    margin-top: -7%;
  }
  @media (max-width: 1200px) {
    #blob canvas {
      margin-top: -10%;
      width: 100vw;
      height: 100vh;
    }
  }
  @media (max-width: 600px) {
    #blob canvas {
      width: 100vw;
      height: 100vh;
    }
  }
  .arrow-down-hero {
    position: absolute;
    bottom: 0px;
  }
</style>
  <!-- HEADER STARTS -->
<div class="header-quantum">
  <div class="logo-quantum">
    <img alt="logo" src="https://assets.codepen.io/4625073/logo-quantum-vectors.png" />
  </div>
    <div class="links-quantum">
      <ul>
        <li><a href="test">About</a></li>
        <li><a href="test">Clients</a></li>
        <li><a href="test">Contact us</a></li>
      </ul>
  </div>
</div>
<!-- HEADER ENDS -->

<!-- FIXED FOOTER STARTS -->

  <div class="crypto-scraper">
    <a href="test"><svg xmlns="http://www.w3.org/2000/svg" width="40" viewBox="0 0 24 24"><path d="M8.326 6h.721l.493 1.851.458-1.851h.727l-.832 2.752v1.878h-.716v-1.878l-.851-2.752zm3.297 1.187c-.56 0-.931.37-.931.917v1.675c0 .602.314.916.931.916.511 0 .913-.342.913-.916v-1.675c0-.535-.398-.917-.913-.917zm.266 2.563c0 .186-.095.323-.266.323-.176 0-.277-.144-.277-.323v-1.589c0-.187.086-.326.265-.326.194 0 .278.134.278.326v1.589zm2.364-2.535v2.593c-.077.097-.25.256-.374.256-.135 0-.169-.093-.169-.23v-2.619h-.635v2.855c0 .337.103.61.443.61.192 0 .459-.1.734-.426v.377h.635v-3.416h-.634zm1.101 6.929c-.224 0-.271.158-.271.382v.329h.535v-.329c.001-.221-.046-.382-.264-.382zm-2.351.02l-.125.099v2.027l.144.115c.099.05.242.054.31-.034.034-.046.052-.121.052-.224v-1.68c0-.11-.021-.193-.064-.25-.074-.096-.211-.107-.317-.053zm2.413-2.598c-1.301-.089-5.533-.088-6.833 0-1.406.097-1.572.947-1.583 3.184.011 2.233.175 3.087 1.583 3.184 1.3.088 5.532.089 6.833 0 1.407-.096 1.573-.946 1.584-3.184-.011-2.233-.175-3.087-1.584-3.184zm-6.162 5.344h-.681v-3.77h-.705v-.64h2.091v.64h-.705v3.77zm2.424 0h-.605v-.359c-.111.132-.228.233-.348.302-.326.187-.773.183-.773-.477v-2.72h.604v2.494c0 .131.032.219.161.219.118 0 .282-.151.355-.244v-2.47h.605v3.255zm2.328-.675c0 .403-.15.716-.553.716-.222 0-.406-.081-.575-.292v.25h-.61v-4.409h.61v1.42c.137-.167.322-.304.538-.304.443 0 .59.374.59.815v1.804zm2.235-.876h-1.157v.614c0 .244.021.455.265.455.255 0 .271-.172.271-.455v-.226h.622v.244c0 .627-.269 1.007-.906 1.007-.577 0-.873-.421-.873-1.007v-1.46c0-.565.373-.957.919-.957.58 0 .86.369.86.957v.828zm-4.241-13.359c5.514 0 10 4.486 10 10s-4.486 10-10 10-10-4.486-10-10 4.486-10 10-10zm0-2c-6.627 0-12 5.373-12 12s5.373 12 12 12 12-5.373 12-12-5.373-12-12-12z"/></svg></a>
    
    <a href="test"><svg xmlns="http://www.w3.org/2000/svg" width="40" viewBox="0 0 24 24"><path d="M12 2c5.514 0 10 4.486 10 10s-4.486 10-10 10-10-4.486-10-10 4.486-10 10-10zm0-2c-6.627 0-12 5.373-12 12s5.373 12 12 12 12-5.373 12-12-5.373-12-12-12zm6.5 8.778c-.441.196-.916.328-1.414.388.509-.305.898-.787 1.083-1.362-.476.282-1.003.487-1.564.597-.448-.479-1.089-.778-1.796-.778-1.59 0-2.758 1.483-2.399 3.023-2.045-.103-3.86-1.083-5.074-2.572-.645 1.106-.334 2.554.762 3.287-.403-.013-.782-.124-1.114-.308-.027 1.14.791 2.207 1.975 2.445-.346.094-.726.116-1.112.042.313.978 1.224 1.689 2.3 1.709-1.037.812-2.34 1.175-3.647 1.021 1.09.699 2.383 1.106 3.773 1.106 4.572 0 7.154-3.861 6.998-7.324.482-.346.899-.78 1.229-1.274z"/></svg></a>
    
    
    <a href="test"><svg xmlns="http://www.w3.org/2000/svg" width="40"  viewBox="0 0 24 24"><path d="M12 2.02c5.514 0 10 4.486 10 10s-4.486 10-10 10-10-4.486-10-10 4.486-10 10-10zm0-2c-6.627 0-12 5.373-12 12s5.373 12 12 12 12-5.373 12-12-5.373-12-12-12zm0 12.55l-5.992-4.57h11.983l-5.991 4.57zm0 1.288l-6-4.629v6.771h12v-6.771l-6 4.629z"/></svg></a>
  </div>

<!-- FIXED FOOTER ENDS -->


<!-- ------ HERO CONTAINER STARTS ----- -->
<div class="hero-container">
<div id="blob">
    <canvas></canvas>
</div> 
      <div class="hero-text-container">
    <h1>Web3 Frontend Studio</h1>
        <p style="display:none;">We are a design and development studio that works with tomorrow's web3 organizations to create blockchain-native web apps</p>
</div>
  <div class="arrow-down-hero">
    <svg fill="#000" viewBox="0 0 23 40" width="33" xmlns="http://www.w3.org/2000/svg"><path d="m.833984 29.1667 10.833316 10.8333 10.8333-10.8333-1.1783-1.1784-8.8217 8.8217v-36.80999905h-1.6666v36.80999905l-8.82168-8.8217z" fill="#000" opacity="1"/></svg>
  </div>
</div>
<!-- ------ HERO CONTAINER ENDS ----- -->  


<About />

