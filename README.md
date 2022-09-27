// Plataforma-Ferroviaria




#include <Wire.h>
#include <LiquidCrystal_I2C.h>

#define clk 2
#define dt 3
#define btn 4

LiquidCrystal_I2C lcd(0x27,16,2);


int state_clk_old;
int state_btn_old;
int count=0;

byte flecha_01[8] = {B10000, B11000, B11100, B11111, B11111, B11100, B11000, B10000}; //flecha_01
byte flecha_02[8] = {B01000, B01100, B01110, B11111, B11111, B01110, B01100, B01000}; //flecha_02
byte flecha_03[8] = {B00010, B00110, B01110, B11111, B11111, B01110, B00110, B00010}; //flecha_03

void setup() {
  Serial.begin(9600);
  lcd.init();
  lcd.backlight();
  pinMode (clk,INPUT);
  pinMode (dt,INPUT);
  pinMode (btn, INPUT_PULLUP);
  state_clk_old = digitalRead(clk);
  state_btn_old = digitalRead(btn);
  lcd.createChar(0, flecha_01);
  lcd.createChar(1, flecha_02);
  lcd.createChar(2, flecha_03);

  menu_01(); //no se como pasar por ellos con el encoder
  menu_02();
//  submenu02();
  menu_03();
  menu_04();
  menu_05();
  menu_06();
  menu_07();
}

void loop() {
  int state_btn = digitalRead(btn);

  encoder();
    
}

void encoder(){
    int state_clk = digitalRead(clk);
    int state_dt = digitalRead(dt);
    
    if(state_clk_old == HIGH && state_clk == LOW){
      if(state_dt == LOW){
        count--;
      }else{
        count++;
      }
      }
   delay(20);
   state_clk_old = state_clk;
  }

void menu_01(){ //este menu solo se mostrara en el encendido o despues de reset.
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.write(1);
//  lcd.write(1);
  lcd.setCursor(2,0);
  lcd.print("TRENMANIACOS");
  lcd.setCursor (15,0);
  lcd.write(2);
//  lcd.write(2);
  lcd.setCursor(0,1);
  lcd.print("ROTONDA DIGITAL");
  delay(5000); //Los delay estan puestos porque no se pasar de menus.
}

void menu_02(){
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("INICIAR DESTINO");
  lcd.setCursor(0,1);
  lcd.write(1);
  lcd.setCursor(1,1);
  lcd.print("VIA NUMERO"); //clik en encoder y pasa a los guiones adjuntos.
  lcd.setCursor(12,1);
  lcd.print("_");
  lcd.blink(); //estos guiones bajos parpadean para seleccionar la via. Con el giro del encoder la elegimos y con el clik la fijamos. 
  delay(2000);
  lcd.noBlink();

}

void menu_03(){
  lcd.clear();
  lcd.setCursor(01,0);
  lcd.print("GIRO DERECHA");
  lcd.setCursor(1,1);
  lcd.print("GIRO IZQUIERDA");
  delay(2000);
}

void menu_04(){
  lcd.clear();
  lcd.setCursor(1,0);
  lcd.print("INICIAR GIRO");
  lcd.setCursor(1,1);
  lcd.print("RESET POSICION");
    delay(2000);
}

void menu_05(){
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("MOVIENDO MOTOR");
  lcd.setCursor(0,1);
  lcd.print("ESPERE......");
    delay(2000);
}

void menu_06(){
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("VIA SELECCIONADA");
  lcd.setCursor(0,1);
  lcd.print("NUMERO");
  lcd.setCursor(8,1);
  lcd.print("XX"); // AQUI DEBE APARECER EL NUMERO DE VIA QUE HAYAMOS SELECCIONADO EN Menu_02
  delay(2000);
}

void menu_07(){
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("    SERVICIO");
  lcd.setCursor(0,1);
  lcd.print("   FINALIZADO"); //AQUI DEBE REGRESAR AL menu_02
    delay(2000);
}

 

  
