<template>
  <div id="app">
  <h1>Sagittale PREVIEW</h1>
  <div class="container">
    <object type="image/svg+xml" data="/sagittale-ant.svg" ref="svgElement"></object>
    <div class="side">
      <h2>Épaule</h2>
      <button :class="{ red: true, active: activeJarret === '5.8' }" @click="rotateElement('ant-Z', '5.8', '#d03431', '2pt')">2</button>
      <button :class="{ orange: true, active: activeJarret === '3.5' }" @click="rotateElement('ant-Z', '3.5', '#ea9c2b', '2pt')">1</button>
      <button :class="{ green: true, active: activeJarret === '0' }" @click="rotateElement('ant-Z', '0', '#5cb85c', '2pt')">0</button>
      <button :class="{ orange: true, active: activeJarret === '-3.5' }" @click="rotateElement('ant-Z', '-3.5', '#ea9c2b', '2pt')">-1</button>
      <button :class="{ red: true, active: activeJarret === '-5.8' }" @click="rotateElement('ant-Z', '-5.8', '#d03431', '2pt')">-2</button>
      
      
      <h2>Carpe</h2>
      <button :class="{ red: true, active: activeCarpe === '14.14' }" @click="rotateElement('ant-A', '14.14', '#d03431', '2pt')">2</button>
      <button :class="{ orange: true, active: activeCarpe === '5' }" @click="rotateElement('ant-A', '5', '#ea9c2b', '2pt')">1</button>
      <button :class="{ green: true, active: activeCarpe === '0' }" @click="rotateElement('ant-A', '0', '#5cb85c', '2pt')">0</button>
      <button :class="{ orange: true, active: activeCarpe === '-5' }" @click="rotateElement('ant-A', '-5', '#ea9c2b', '2pt')">-1</button>
      <button :class="{ red: true, active: activeCarpe === '-14.14' }" @click="rotateElement('ant-A', '-14.14', '#d03431', '2pt')">-2</button>
      
      <h2>Boulet</h2>
      <button :class="{ red: true, active: activeBoulet === '36' }" @click="rotateElement('ant-B', '36', '#d03431', '2pt')">3</button>
      <button :class="{ red: true, active: activeBoulet === '12' }" @click="rotateElement('ant-B', '12', '#d03431', '2pt')">2</button>
      <button :class="{ orange: true, active: activeBoulet === '7.5' }" @click="rotateElement('ant-B', '7.5', '#ea9c2b', '2pt')">1</button>
      <button :class="{ green: true, active: activeBoulet === '0' }" @click="rotateElement('ant-B', '0', '#5cb85c', '2pt')">0</button>
      <button :class="{ orange: true, active: activeBoulet === '-7.5' }" @click="rotateElement('ant-B', '-7.5', '#ea9c2b', '2pt')">-1</button>
      <button :class="{ red: true, active: activeBoulet === '-12' }" @click="rotateElement('ant-B', '-12', '#d03431', '2pt')">-2</button>
      <button :class="{ red: true, active: activeBoulet === '-36' }" @click="rotateElement('ant-B', '-36', '#d03431', '2pt')">-3</button>
      
      <h2>Pieds</h2>
      <button :class="{ red: true, active: activePieds === '12' }" @click="rotateElement('ant-C', '12', '#d03431', '2pt')">2</button>
      <button :class="{ orange: true, active: activePieds === '7.5' }" @click="rotateElement('ant-C', '7.5', '#ea9c2b', '2pt')">1</button>
      <button :class="{ green: true, active: activePieds === '0' }" @click="rotateElement('ant-C', '0', '#5cb85c', '2pt')">0</button>
      <button :class="{ orange: true, active: activePieds === '-7.5' }" @click="rotateElement('ant-C', '-7.5', '#ea9c2b', '2pt')">-1</button>
      <button :class="{ red: true, active: activePieds === '-12' }" @click="rotateElement('ant-C', '-12', '#d03431', '2pt')">-2</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container{display: flex}
.side{margin-left: 2rem}
button{margin-right: 0.5rem}
.red:focus{background-color: #ce3431}
.orange:focus{background-color: #e89a2b}
.green:focus{background-color: #5cb65c}

.active.red{background-color: #ce3431}
.active.orange{background-color: #e89a2b}
.active.green{background-color: #5cb65c}
</style>

<script>
export default {
  data() {
    return {
      rotationZ: 0,
      rotationA: 0,
      activeJarret: null,
      activeCarpe: null,
      activeBoulet: null,
      activePieds: null,
      angleJarret: 0,
      angleCarpe: 0,
      angleBoulet: 0,
      anglePieds: 0,
      totalAngle: 0,
    };
  },
  methods: {
    changeColor(elementId, color) {
      const svgDocument = this.$refs.svgElement.contentDocument;
      const element = svgDocument.getElementById(elementId);
      if (element) {
        element.style.fill = color;
        console.log(`Couleur de ${elementId} changée en ${color}`);
      }
    },
    rotateElement(elementId, angle, strokeColor, strokeWidth) {
      const svgDocument = this.$refs.svgElement.contentDocument;
      console.log(`Rotation de ${elementId} de ${angle} degrés avec couleur ${strokeColor} et épaisseur de trait ${strokeWidth}`);

      const element = svgDocument.getElementById(elementId);
      const pivot = svgDocument.getElementById(elementId + '-join');
      const dev = svgDocument.getElementById(elementId + '-dev');
      if (elementId.includes('ant-Z')) {
        this.activeJarret = angle;
      } else if (elementId.includes('ant-A')) {
        this.activeCarpe = angle;
      } else if (elementId.includes('ant-B')) {
        this.activeBoulet = angle;
      } else if (elementId.includes('ant-C')) {
        this.activePieds = angle;
      }
      if (element) {
        if (pivot) {
          const cx = pivot.cx.baseVal.value;
          const cy = pivot.cy.baseVal.value;
          console.log(`Centre de rotation : (${cx}, ${cy})`);
          element.setAttribute('transform', `rotate(${angle}, ${cx}, ${cy})`);
          console.log(`Attribut 'transform' appliqué : ${element.getAttribute('transform')}`);

          pivot.style.stroke = strokeColor;
          pivot.style.strokeWidth = strokeWidth;
        } else {
          console.log(`Pivot introuvable pour ${elementId}`);
        }

        if (dev) {
          dev.style.stroke = strokeColor;
          dev.style.strokeWidth = strokeWidth;
        } else {
          console.log(`Dev introuvable pour ${elementId}`);
        }

        const devTop = svgDocument.getElementById(elementId + '-dev-top');
        if (devTop) {
          devTop.style.stroke = strokeColor;
          devTop.style.strokeWidth = strokeWidth;
        } else {
          console.log(`Dev-Top introuvable pour ${elementId}`);
        }

        // Mise à jour des rotations et vérification des conditions
        if (elementId === 'ant-Z') {
          this.rotationZ = parseFloat(angle);
          console.log(this.rotationZ)
          console.log(this.rotationA)
        } else if (elementId === 'ant-A') {
          this.rotationA = parseFloat(angle);
          console.log(this.rotationZ)
          console.log(this.rotationA)
        }

        this.checkRotations();
      } else {
        console.log(`Élément principal introuvable pour ${elementId}`);
      }
      if (elementId.includes('ant-Z')) {
        this.angleJarret = parseFloat(angle);
      } else if (elementId.includes('ant-A')) {
        this.angleCarpe = parseFloat(angle);
      } else if (elementId.includes('ant-B')) {
        this.angleBoulet = parseFloat(angle);
      } else if (elementId.includes('ant-C')) {
        this.anglePieds = parseFloat(angle);
      }

      // Recalcul de totalAngle
      this.totalAngle = this.angleJarret + this.angleCarpe + this.angleBoulet + this.anglePieds;

      // Application de la rotation à ant-mask-foot
      this.applyRotationToMaskFoot();
    },

    applyRotationToMaskFoot() {
      const svgDocument = this.$refs.svgElement.contentDocument;
      const maskFoot = svgDocument.getElementById('ant-mask-foot');
      const pivot = svgDocument.getElementById('ant-C-join');

      if (maskFoot && pivot) {
        const inverseAngle = -this.totalAngle;
        const cx = pivot.cx.baseVal.value;
        const cy = pivot.cy.baseVal.value;

        maskFoot.setAttribute('transform', `rotate(${inverseAngle}, ${cx}, ${cy})`);
      }
    },
    checkRotations() {
      const svgDocument = this.$refs.svgElement.contentDocument;
      const antNormal = svgDocument.getElementById('ant-normal');
      const antExt2 = svgDocument.getElementById('ant-ext-2');

      if (this.rotationZ === 5.8 && this.rotationA === -14.14) {
        if (antNormal) antNormal.style.display = 'none';
        if (antExt2) {
          antExt2.style.display = 'block';
          antExt2.style.fill = '#c6c5c4';
          antExt2.style.stroke = '#c3c1c1';
          antExt2.style.stroke = '2pt';
        }
      } else {
        if (antNormal) antNormal.style.display = 'block';
        if (antExt2) antExt2.style.display = 'none';
      }
    }
  }
}
</script>


