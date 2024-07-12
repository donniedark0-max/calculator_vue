<template>
<div class="app-container" @keyup="handleKeyup">
    <div class="title">
        Calculadora
    </div>
    <div class="calculator-container">
      <div class="calculator">
        <div class="limit-message" v-if="exceededLimit">
            Se ha alcanzado el límite de caracteres
        </div>
        <div class="display" :style="{ fontSize: displayFontSize }">
          {{ displayValue }}
        </div>
        <div class="buttons">
            <!-- Primera fila -->
            <div class="button-row ">
                <button class="wide-button b1" @click="clear()">AC</button>
                <button class="b1" @click="toggleSign()">+/-</button>
                <button class="b1" @click="addPercentage()">%</button>
                <button class="b2" @click="addToDisplay('/')">÷</button>
           </div>

           <!-- Segunda fila -->
            <div class="button-row">
                <button @click="addToDisplay('7')">7</button>
                <button @click="addToDisplay('8')">8</button>
                <button @click="addToDisplay('9')">9</button>
                <button class="b2" @click="addToDisplay('*')">x</button>
            </div>

           <!-- Tercera fila -->
            <div class="button-row">
                <button @click="addToDisplay('4')">4</button>
                <button @click="addToDisplay('5')">5</button>
                <button @click="addToDisplay('6')">6</button>
                <button class="b2" @click="addToDisplay('-')">-</button>
            </div>

           <!-- Cuarta fila -->
            <div class="button-row">
                <button @click="addToDisplay('1')">1</button>
                <button @click="addToDisplay('2')">2</button>
                <button @click="addToDisplay('3')">3</button>
                <button class="b2" @click="addToDisplay('+')">+</button>
            </div>

           <!-- Quinta fila -->
            <div class="button-row">
                <button class="wide-button zero" @click="addToDisplay('0')">0</button>
                <button @click="addToDisplay('.')">.</button>
                <button class="b2" @click="calculate()">=</button>
            </div>
        </div>
      </div>
      <div class="side-message">
        <p>学 然后</p> 
        <p>知 不足</p>
      </div>
    </div>
    <footer class="footer">
        Creado por dark0
    </footer>
</div>  
  </template>
  <script>
  export default {
  data() {
    return {
      displayValue: '0',
      maxDisplayLength: 10, // Longitud máxima del display antes de reducir el tamaño de la fuente
      displayFontSize: '4rem', // Tamaño inicial de la fuente del display
      exceededLimit: false // Indicador para mostrar mensaje de límite de caracteres excedido
    };
  },
  watch: {
    displayValue() {
      if (this.displayValue.length > 29) {
        this.exceededLimit = true;
      } else {
        this.exceededLimit = false;
      }
    }
  },
  methods: {
    handleKeyup(event) {
      const key = event.key;
      if (key >= '0' && key <= '9') {
        this.addToDisplay(key);
      } else if (key === '.') {
        this.addToDisplay('.');
      } else if (key === '+') {
        this.addToDisplay('+');
      } else if (key === '-') {
        this.addToDisplay('-');
      } else if (key === '*') {
        this.addToDisplay('*');
      } else if (key === '/') {
        this.addToDisplay('/');
      } else if (key === 'Enter') {
        this.calculate();
      } else if (key === 'Escape') {
        this.clear();
      }
    },
    addToDisplay(value) {
      if (this.displayValue === '0' && value !== '.') {
        this.displayValue = value;
      } else {
        this.displayValue += value;
      }

      // Ajustar el tamaño de la fuente si se excede la longitud máxima
      if (this.displayValue.length > this.maxDisplayLength && this.displayFontSize !== '1.5rem') { 
        this.displayFontSize = `${2 - (this.displayValue.length - this.maxDisplayLength) * 0.05}rem`;
      }
    },
    clear() {
      this.displayValue = '0';
      this.displayFontSize = '4rem'; // Restaurar tamaño de fuente por defecto al limpiar
      this.exceededLimit = false; // Ocultar el mensaje al limpiar
    },
    toggleSign() {
      if (this.displayValue !== '0') {
        this.displayValue = this.displayValue.startsWith('-') ? this.displayValue.substr(1) : '-' + this.displayValue;
      }
    },
    addPercentage() {
      if (this.displayValue !== '0') {
        this.displayValue = String(parseFloat(this.displayValue) / 100);
      }
    },
    calculate() {
      try {
        this.displayValue = eval(this.displayValue).toString();
        this.displayFontSize = '4rem'; 
        this.exceededLimit = false;
      } catch (error) {
        this.displayValue = 'Error';
        this.displayFontSize = '4rem'; 
        this.exceededLimit = false; 
      }
    }
  },
  mounted() {
    window.addEventListener('keyup',this.handleKeyup);
  },
  beforeUnmount() {
    window.removeEventListener('keyup', this.handleKeyup);
  }
};
  </script>
  
  <style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Zhi+Mang+Xing&display=swap');

  .app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
}

.calculator-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }

.title {
  text-align: center;
  color:white;
  font-size: 5rem;
  font-weight: bold;
  margin-bottom: 80px;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
  
.calculator {
    width: 400px;
    background-color: rgb(51, 51, 51);
    border-radius: 10px;
    padding: 20px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    position: relative; 
  }

.limit-message {
  position: absolute;
  top: -30px; 
  left: 50%;
  transform: translateX(-50%);
  color: rgb(143, 76, 76);
  padding: 5px 10px;
  border-radius: 5px;
  font-size: 1rem;
  display: inline-block;
  white-space: nowrap; /* Evita el salto de línea */
  z-index: 10; /* Asegura que esté encima del display y los botones */
  font-weight: 700;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
  
  .display {
    height: 80px;
    background-color: rgb(51, 51, 51);
    color: white;
    text-align: end;
    justify-content: end;
    align-items: center;
    padding: 0 10px;
    font-size: 4rem;
    margin-bottom: 10px;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  
  .buttons {
    display: grid;
    gap: 12px;
  }
  
  .button-row {
    display: flex;
    justify-content: space-between;
  }
  
  .buttons button {
    padding: 10px;
    font-size: 1.5rem;
    background-color: #cfcfcf;
    border: none;
    border-radius: 100px;
    cursor: pointer;
    min-width: 8vh;
    min-height: 8vh;
  }

  .buttons .zero {
    width:19.35vh;
    text-align:start;
    padding: 0px 31.5px 0px;
  }

  .buttons .b1 {
    background-color: #222222 ;
    color: white;

  }

  .buttons .b2 {
    background-color: #e27116 ;
    color: white;
  }
  .buttons button:hover {
    background-color: #ccc;
  }
  
  .wide-button {
    grid-column: span 2;
  }

.side-message {
  position: absolute;
  right: 190px; 
  font-size: 3.5rem;
  color: rgb(237, 184, 184);
  font-family: 'Zhi Mang Xing';
}
  footer {
  text-align: center;
  padding-top: 80px;
  font-size: 1rem;
  color: gray;
}
@media (max-width: 1108px) {
  .side-message {
    display: none;
  }
}
  </style>