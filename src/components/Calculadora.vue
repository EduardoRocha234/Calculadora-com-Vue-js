<template>
  <div class="calculadora">
  <div  class="telaDaCalculadora">{{valorDigitado ||'0' }} {{ simboloDaOperação }} </div>
  <div @click="limpar" class="botao">C</div>
  <div @click="sinalDeMenos" class="operador">+/-</div>
  <div @click="porcentagem" class="operador">%</div>
  <div @click="dividir" class="operador">÷</div>
  <div @click="adicionarNumero('7')" class="botao">7</div>
  <div @click="adicionarNumero('8')" class="botao">8</div>
  <div @click="adicionarNumero('9')" class="botao">9</div>
  <div @click="multiplicar" class="operador">X</div>
  <div @click="adicionarNumero('4')" class="botao">4</div>
  <div @click="adicionarNumero('5')" class="botao">5</div>
  <div @click="adicionarNumero('6')" class="botao">6</div>
  <div @click="subtrair" class="operador">-</div>
  <div @click="adicionarNumero('1')" class="botao">1</div>
  <div @click="adicionarNumero('2')" class="botao">2</div>
  <div @click="adicionarNumero('3')" class="botao">3</div>
  <div @click="somar" class="operador">+</div>
  <div @click="adicionarNumero(0)" class="zero">0</div>
  <div @click="ponto" class="botao">.</div>
  <div @click="resultado" class="operador">=</div>

  </div>
</template>

<script>
export default {
  data: function() {
    return {
      valorDigitado: '',
      valorAnterior: null,
      clicouNoOperador: false,
      calculo: null,
      simboloDaOperação: ''
    }
  },
  methods: {
    limpar() {
      this.valorDigitado = '';
      this.simboloDaOperação = ''
    },
    //para adicionar o numeros na tela
      adicionarNumero(numero) {
        if (this.clicouNoOperador) {
          this.valorDigitado = '';
          this.clicouNoOperador = false;
        }
  
        this.valorDigitado = `${this.valorDigitado}${numero}`;
      },

    sinalDeMenos() {
      //verifica se o primeiro inice  tem o sinal se tiver retorna o segundo indice
      //que seria o número q está a frente do sinal, se n tiver ele retorna com o sinal
      this.valorDigitado = this.valorDigitado.charAt(0) === '-'
        ? this.valorDigitado.slice(1)
        : `-${this.valorDigitado}`;
    },

    //ele verifica se á o ponto na operação abaixo pois indexOf retorna -1 caso o mesmo
    //não esteja presente. sendo assim se for iggual a -1 significa que não á ponto
    //e então ele adiciona o ponto a tela da calculadora
      ponto() {
        if (this.valorDigitado.indexOf('.') === -1) {
          this.adicionarNumero('.');
        }
      },
      
    porcentagem() {
      this.valorDigitado = `${parseFloat(this.valorDigitado) / 100}`;
    },

    //operação para quando clicar o botão o valor atual vire o valor antigo para 
    //que tenha os dois valores para a operação
    setarValor() {
      this.valorAnterior = this.valorDigitado;
      this.clicouNoOperador = true;
    },

    dividir() {
      this.simboloDaOperação = '÷'
      this.calculo = (num1, num2) => num1 / num2;
      this.setarValor();
    },
    multiplicar() {
      this.simboloDaOperação = 'x'

      this.calculo = (num1, num2) => num1 * num2;
      this.setarValor();
    },
    subtrair() {
      this.simboloDaOperação = '-'
      this.calculo = (num1, num2) => num1 - num2;
      this.setarValor();
    },
    somar() {
      this.simboloDaOperação = '+'

      this.calculo = (num1, num2) => num1 + num2;
        this.setarValor();
    },
     resultado() {
       this.simboloDaOperação = ''
       this.valorDigitado = `${this.calculo(
               parseFloat(this.valorAnterior),
               parseFloat(this.valorDigitado),
      )}`;
      this.valorAnterior = null;
    },
    },
  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


.calculadora {
  background-color: rgba(14, 187, 144, 0.945);
  margin: auto  auto 45%;
  float: center;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  width: 395px;
  border-radius:  30px;

}

.telaDaCalculadora {
  height: 350px;
  width: 357px;
  border-radius: 175px;
  grid-column: 1 / 5;
  background-color: rgb(8, 230, 163);
  color: rgb(0, 0, 0);
  text-align: center;
  padding: 15px;
  border-color: rgb(18, 136, 106);
  border: 2px solid rgb(47, 121, 102);
  height: 50px;
  font-size: 40px;
}
.zero {
  height: 45px;
  width: 157px;
  border-radius: 48px;
  font-size: 35px;
  padding: 22px 15px 15px 15px;
  grid-column: 1 / 3;
  text-align: center;
  border: 1px solid rgb(35, 58, 49);
  background-color: rgb(177, 255, 232);
}
.zero:hover {
  background-color: rgb(137, 243, 213);
}

.botao {
  height: 48px;
  width: 48px;
  border-radius: 24px;
  font-size: 30px;
  padding: 22px 15px 15px 22px;
  text-align: center;
  border: 1px solid rgb(0, 0, 0);
  background-color: rgb(177, 255, 236);
  margin: auto;
  
}

.botao:hover{
  background-color: rgb(137, 243, 213);
}
.operador {
  margin: auto;
  height: 48px;
  width: 48px;
  border-radius: 24px;
  color: #fff;
  padding: 18px 15px 15px 18px;
  text-align: center;
  font-size: 30px;
  border: 1px solid rgb(57, 92, 78);
  background-color: rgb(18, 136, 106);
}

.operador:hover{
  background-color: rgb(47, 121, 102);
}

</style>
