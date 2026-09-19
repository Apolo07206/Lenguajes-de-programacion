
En JavaScript, el ritual **switch–case** es una especie de puesto de control para decisiones múltiples. En lugar de encadenar `if` tras `if`, declaras una sola entrada y luego comparas contra distintos “casos”. Cada caso es una posible ruta que el código puede tomar.

ejemplo 


let fruta = "mango"

switch (fruta) {
  case "manzana":
    console.log("Crujiente y roja")
    break
  case "mango":
    console.log("Dulce y tropical")
    break
  case "banana":
    console.log("Amarilla y práctica")
    break
  default:
    console.log("No conozco esa fruta")
}




ejemplo


const dia = parseInt(prompt("Digite un número del 1 al 7 para ver el día de la semana"));

switch (dia) {
  case 1:
    console.log("Es lunes");
    break;

  case 2:
    console.log("Es martes");
    break;

  case 3:
    console.log("Es miércoles");
    break;

  case 4:
    console.log("Es jueves");
    break;

  case 5:
    console.log("Es viernes");
    break;

  case 6:
    console.log("Es sábado");
    break;

  case 7:
    console.log("Es domingo");
    break;

  default:
    console.log("Opción no válida");
    break;
}
