# Arduino-sound-sensor-project-
It‘s an arduino sound sensor that can turn on or off a led by high frequency sound like a clap.
const int SOUND_PIN = A0;   // sound sensor analog output
const int LED_PIN   = 13;   // output to toggle (built-in LED)

const int THRESHOLD   = 600;   // raise if it triggers on noise, lower if it misses claps (0-1023)
const unsigned long DEBOUNCE_MS = 300;  // ignore further claps for this long after one is detected

bool ledState = false;
unsigned long lastClapTime = 0;

void setup() {
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);
  Serial.begin(9600);
  Serial.println("Clap to toggle. Open Serial Monitor to see readings.");
}

void loop() {
  int level = analogRead(SOUND_PIN);

  if (level > THRESHOLD && millis() - lastClapTime > DEBOUNCE_MS) {
    lastClapTime = millis();
    ledState = !ledState;
    digitalWrite(LED_PIN, ledState ? HIGH : LOW);
    Serial.print("Clap! LED is now ");
    Serial.println(ledState ? "ON" : "OFF");
  }
}
