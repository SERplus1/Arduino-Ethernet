/*
  Web Server

 A simple web server that shows the value of the analog input pins.
 using an Arduino Wiznet Ethernet shield.

 Circuit:
 * Ethernet shield attached to pins 10, 11, 12, 13
 * Analog inputs attached to pins A0 through A5 (optional)

 created 18 Dec 2009
 by David A. Mellis
 modified 9 Apr 2012
 by Tom Igoe
 modified 02 Sept 2015
 by Arturo Guadalupi
 MODIFICADO NUEVAMENTE POR SERGIO HIDALGO 19/06/2018

Voltimetro de hasta 55vdc
Circuito 
r1 esta conectado al positivo
r2 esta conectado al negativo
el valor analogico se toma en la union de las dos resistencias
Con 10 muestras 

 */
//incluyo para el voltimetro
// número de muestras analógicas a tomar para la lectura
#define NUM_SAMPLES 20
int sum = 0;                    // suma de muestras hechas
unsigned char sample_count = 0; // numero de la muestra
float voltage = 0.0;            // voltaje calculado

#include <SPI.h>
#include <Ethernet.h>

// Enter a MAC address and IP address for your controller below.
// The IP address will be dependent on your local network:
byte mac[] = {
  0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED
};
IPAddress ip(192, 168, 1, 7);

// Initialize the Ethernet server library
// with the IP address and port you want to use
// (port 80 is default for HTTP):
EthernetServer server(80);

//ESTO CAMBIA PARA VOLTIMETRO
//float v1 = 5.033; // valor real de la alimentacion de Arduino, Vcc
//float r1 = 977000; // 1M 979900
//float r2 = 100820; // 100K 100110
//float temperature = 0.0;
//LO ANTERIOR VOLTIMETRO
//ACA AGREGO LO DEL RELAY
// Relay state and pin
String relay1State = "Off";
const int relay = 7;

// Client variables 
char linebuf[80];
int charcount=0;
//HASTA ACA LO DEL RELAY

void setup() {
  //ACA AGREGO LO DEL RELAY
    // Relay module prepared 
    pinMode(relay, OUTPUT);
    digitalWrite(relay, HIGH);
    //HASTA ACA LO DEL RELAY
  //SIRVE PARA QUE NO MOLESTE LA SD CARD ARROJANDO 255.255.255.255
  pinMode(4, OUTPUT);
  digitalWrite(4, HIGH);

  // Open serial communications and wait for port to open:
  Serial.begin(9600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }


  // start the Ethernet connection and the server:
  Ethernet.begin(mac, ip);
  server.begin();
  Serial.print("server is at ");
  Serial.println(Ethernet.localIP());
  //CAMBIA A VOLTIMETRO
    
    /*
    Serial.println("DC VOLTMETER");
    Serial.print("Maximum Voltage: ");
    Serial.print((int)(v1 / (r2 / (r1 + r2))));
    Serial.println("V");
    Serial.println("--------------------");
    Serial.println(""); 
    delay(2000);  */
    //temperature = (float)tempData / 20.0;
    //Serial.print("Temperature: ");
    //Serial.print(temperature);
    //SI LO ANTERIOR
}
//se agrega un void para el webserver

// Display dashboard page with on/off button for relay
// It also print Temperature in C and F
void dashboardPage(EthernetClient &client) {
  client.println("<!DOCTYPE HTML><html><head>");
  client.println("<meta name=\"viewport\" content=\"width=device-width, initial-scale=1\"></head><body>");                                                             
  client.println("<h3>Arduino Web Server - <a href=\"/\">Refresh</a></h3>");
  // Generates buttons to control the relay
  client.println("<h4>Relay 1 - State: " + relay1State + "</h4>");
  // If relay is off, it shows the button to turn the output on          
  if(relay1State == "Off"){
    client.println("<a href=\"/relay1on\"><button>ON</button></a>");
  }
  // If relay is on, it shows the button to turn the output off         
  else if(relay1State == "On"){
    client.println("<a href=\"/relay1off\"><button>OFF</button></a>");                                                                    
  }
  client.println("</body></html>"); 
}
//hasta aca el webserver


void loop() {
  // listen for incoming clients
  EthernetClient client = server.available();
  if (client) {
    Serial.println("new client");
    //Esto agrgue para VOLTIMETRO
    // tomar un número de muestras analógicas y sumarlas
    while (sample_count < NUM_SAMPLES) {
        sum += analogRead(A5);
        sample_count++;
        delay(10);
    }
    // calcular la tensión de utilizar 5,0 para una tensión 5.0V 
    // ADC de referencia
    // 5.033V es la tensión de referencia calibrada
    voltage = ((float)sum / (float)NUM_SAMPLES * 5.033) / 1024.0;
    // Envia voltaje para su visualización en tensión Serial Monitor 
    // multiplicado por 11 cuando se utiliza divisor de tensión que 
    // divide por 11.
    // 11.132 es el valor calibrado del división de tensión
    //Tengo que cambiar lo de abajo para leer por http
    /*
    Serial.print(voltage * 10.73);
    Serial.println (" V");
    sample_count = 0;
    sum = 0;
    */
    /*
    float v = (analogRead(5) * v1) / 1024.0;
    float v2 = v / (r2 / (r1 + r2));//relacion de divisor 10.69 o 10.73
    //LO DE ARRIBA VOLTIMETRO */
    //agrego cosas para el webserver
    memset(linebuf,0,sizeof(linebuf));
    charcount=0;
    //hasta aca lo del webserver


    // an http request ends with a blank line
    boolean currentLineIsBlank = true;
    while (client.connected()) {
      if (client.available()) {
        char c = client.read();
        Serial.write(c);
        //lo del webserver
        //read char by char HTTP request
        linebuf[charcount]=c;
        if (charcount<sizeof(linebuf)-1) charcount++;
        //hasta aca lo del webserver
        // if you've gotten to the end of the line (received a newline
        // character) and the line is blank, the http request has ended,
        // so you can send a reply
        if (c == '\n' && currentLineIsBlank) {
          //agrego lo del webserver

            //hasta aca lo del webserver



        /*
            // send a standard http response header 
            client.println("HTTP/1.1 200 OK");
             client.println("Content-Type: text/html");
             client.println("Connection: close");  // the connection will be closed after completion of the response
             client.println("Refresh: 5");  // refresh the page automatically every 5 sec
             client.println();
             client.println("<!DOCTYPE HTML>");
             client.println("<html>");
             // output the value of each analog input pin
            //ACA se agrega el VOLTIMETRO
            
            client.print("VDC: ");
            client.print(voltage * 10.73);
            client.println (" Voltios");
            client.println("<br />");//salta de linea
            sample_count = 0;
            sum = 0;
        */
            /*
             client.print("VDC: ");
             client.println(v2);
             client.println("voltios");
             client.println("<br />");
             delay(1000);*/
            //client.print("Temperature: ");
             //client.print(temperature);
             //ESTO DE ARRIBA CAMBIA A VOLTIMETRO
        /*
            for (int analogChannel = 0; analogChannel < 6; analogChannel++) {
            int sensorReading = analogRead(analogChannel);
            client.print("analog input ");
            client.print(analogChannel);
            client.print(" is ");
            client.print(sensorReading);
            client.println("<br />");
          }
          client.println("</html>");
        */
            //agrego esto del webserver
        dashboardPage(client);
            //lo anterior  del webserver
          break;
        }
        if (c == '\n') {
            //agrego esto del webserver
            if (strstr(linebuf,"GET /relay1off") > 0){
            digitalWrite(relay, HIGH);
            relay1State = "Off";
          }
          else if (strstr(linebuf,"GET /relay1on") > 0){
            digitalWrite(relay, LOW);
            relay1State = "On";
          }
          // you're starting a new line
          currentLineIsBlank = true;
          memset(linebuf,0,sizeof(linebuf));
          charcount=0;          
          //lo de arriba webserver
            /* comento esto para agregar lo de arriba
          // you're starting a new line
          currentLineIsBlank = true;
          */
        } else if (c != '\r') {
          // you've gotten a character on the current line
          currentLineIsBlank = false;
        }
      }
    }
    // give the web browser time to receive the data
    delay(1);
    // close the connection:
    client.stop();
    Serial.println("client disconnected");
  }
}

