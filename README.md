# 128-1-mux
#include "Mux.h"

using namespace admux;

Mux signal_01_1 (Pinset (8, 7, 6, 5, 4, 3, 2)); // 1x16 MUX for signal 1
//Mux signal_01_2 (Pinset (37, 39, 41)); // 1x8 MUX for signal 1

int offset_1[8] = {-2, 0, 2, 0, 0, -1, -1, 2}, offset_2[8] = {-3, 1, 1, 1, -1, -2, 0, 3};

//int vehicle[13] = {20, 21, 10, 59, 58, 57, 40, 83, 100, 8, 91, 25, 11};
int pin;
int j;
int data;

int get__8__channel(int pin) {
  int x = pin % 8;
  if ((pin / 32) % 2) {
    return (x - offset_2[x]);
  } else {
    return (x - offset_1[x]);
  }
}

int get__16__channel(int pin) {
  return (pin / 8);
}

int get__128__channel(int pin) {
  return (get__8__channel(pin) << 4) | (get__16__channel(pin));
}

void setup() {

  Serial.begin(9600);
  int pin= 32;
  //for (pin = 0; pin < 128; ++pin) {
    //Serial.println(get__128__channel(pin));
  //}

  signal_01_1.channel(get__128__channel(pin));
  //signal_01_2.channel(get__8__channel(0));
  j=get__128__channel(pin);
  Serial.println(j);
  Serial.println(get__8__channel(pin));
  Serial.println(get__16__channel(pin));

}

void loop() {
  signal_01_1.channel(j);
    data = signal_01_1.read();

}
